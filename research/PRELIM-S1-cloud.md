# PRELIM (Session 1b) — limits for cloud agents (Claude Code web, Codex cloud)
**Status: PRELIMINARY, verified in S1b on 2026-11-08.** Two independent passes: (1) the Session 1 subagent file (earlier version of this file), (2) a Session 1b Sonnet subagent. The main agent re-read the primary pages for every ★ claim. Legend: ✅ = main agent read it in the primary page on 2026-11-08 (page dates are not shown on these docs). **SUB** = reported by a subagent, not re-read by the main agent. No endpoint was called, no credential file was read. Not legal advice.

## 1. Can a cloud agent see its limits?
- ✅★ **No documented in-cloud readout** of used % or reset time for either provider. Nothing found in `code.claude.com/docs/en/claude-code-on-the-web`, `.../cloud-environments`, `.../settings`. Whether the status-line `rate_limits` field or `/usage` works in a cloud VM is **undocumented** (the statusline page does not mention cloud). **Test needed** (probe hook, owner approval).
- ✅★ **Allowance is account-level and shared.**
  - Claude: "cloud sessions share rate limits with all other Claude and Claude Code usage within your account." Parallel tasks use proportionally more.
  - Codex: "Local messages and cloud chats share your plan's usage allowance." and "Cloud tasks may use more of your allowance than local messages." (`learn.chatgpt.com/docs/pricing.md`).
  - **Consequence:** a **local** collector measures the same pool that cloud agents burn. The cloud agent only needs the *number*, delivered by a bridge.
- ✅ Own container check (this session is a Claude Code cloud session): env var names show `CLAUDE_CODE_REMOTE=true`, `CLAUDE_CODE_ENTRYPOINT=remote`, `CLAUDE_CODE_REMOTE_SESSION_ID`. **No variable carries limit data.** No user `settings.json`, no repo `.claude/`. A platform Stop hook script exists, so hooks do run there. Values of secrets were not printed.
- Hook payloads carry no `used_percentage`; `rate_limit` is only a `StopFailure` matcher (from Session 1 research).
- At the limit, Claude scheduled runs: ✅ "Without usage credits, additional runs are rejected until your usage window resets." What an interactive cloud session shows at the limit: NOT FOUND. Codex: the active turn may finish (pricing page ✅), cloud-specific behavior NOT FOUND.

## 2. What works in cloud
| Feature | Claude Code cloud | Codex cloud |
|---|---|---|
| Repo `CLAUDE.md` / `AGENTS.md` | CLAUDE.md ✅ yes | AGENTS.md: legacy page says used for lint/test commands (SUB); current page unclear |
| Repo hooks (`.claude/settings.json`) | ✅★ **Yes, only in a session with ONE repository, outside a project's threads.** Not in project threads, not in multi-repo sessions. `SessionStart` hooks run on start and resume, default timeout 600 s, no cloud-only scoping (guard with `CLAUDE_CODE_REMOTE`). PreToolUse/Stop/StopFailure in cloud: not explicitly documented. | **Not found** (SUB; medium confidence) |
| User-level `~/.claude/settings.json`, user hooks, `settings.local.json`, plugins | ✅ **Not read / not installed** | Personal skills not synced (SUB) |
| Repo `.mcp.json`, skills, agents, commands | Yes in single-repo sessions (SUB for each item; settings page ✅ for settings) | Repo skills yes (SUB); MCP in cloud tasks NOT FOUND |
| claude.ai connectors (remote MCP) | Yes; traffic goes through Anthropic servers, not the session network (SUB) | — |
| Scheduled runs | ✅★ **Scheduled tasks (formerly routines), research preview.** Triggers: schedule (recurring **or once at a specific future time**), API (`POST …/fire`, beta header `experimental-cc-routine-2026-04-01`, claude.ai users only), GitHub events. Each run is a new cloud session. Limits ✅: 100 scheduled runs/h per account; 30/h per task for run-now, API fires, re-runs. ✅★ **`/schedule` is not available inside a cloud session**: tasks are created from the web UI or a local CLI. So a cloud agent **cannot create its own resume task**. Runs draw subscription usage like interactive sessions (SUB). Min interval 1 h (SUB). | "Scheduled tasks"/Automations page exists, **not read**; unknown |
| Send a message into an existing cloud session | ✅ `claude -p "msg" --cloud <session-id>` from any machine signed in with `claude auth login`; works on expired sessions by re-provisioning; returns `{ok, session_id, url}` with `--output-format json` | n/a found |

## 3. Network, secrets, persistence
- ✅ Network levels: **None, Trusted (default), Full, Custom** (own allowlist, optionally plus defaults). Session-scoped `/loop` tasks do not survive environment expiry; shell commands and subagents stop when the VM is reclaimed; conversation history is restored.
- ✅★ **GitHub proxy:** serves API requests only for repositories **attached to the session**; others get 403 "…is not enabled for this session", also for public repos' archives. GraphQL rejected (`gh pr`/`gh issue` fail; use REST `gh api repos/...`). Pushes of tags and branch deletes rejected. `raw.githubusercontent.com` (public files) is in the Trusted list.
- ✅★ **Environment variables are readable by the agent and by anyone who uses the environment** ("Anyone who uses the environment can read the values"). Do not put secrets there.
- ✅★ **Network secrets:** a key stored on the environment; Anthropic's agent proxy adds it to requests for listed hosts "after each request leaves the session's VM, so the key itself stays outside the VM". Sessions can reach those hosts **even if the access level would block them**. Never attached to GitHub, `api.anthropic.com`, package registries. Not on self-hosted environments. Static key per host.
- Persistence: fresh VM and fresh clone per session; setup script cached as filesystem snapshot if it finishes in about 5 min (cache expiry about 7 days); idle VM pauses, then may be reclaimed (✅ for expiry; cache details ✅ in docs headings, SUB for numbers).
- Codex cloud (SUB, legacy pages): internet off by default in agent phase, allowlist presets (None / Common dependencies / All); secrets setup-only and removed before the agent phase (legacy); the current guide mentions network secrets with a proxy placeholder; saved VM state recoverable up to 7 days. Docs say OpenAI plans to deprecate the legacy cloud experience.

## 4. Automation path (GitHub Actions) — flag for R3
SUB: `anthropics/claude-code-action` authenticates with `ANTHROPIC_API_KEY` or a `CLAUDE_CODE_OAUTH_TOKEN` made by `claude setup-token` (subscription). The docs present it as supported. This touches the credential clause in the legitimacy file. **R3 must check it before we design anything for CI.** Limit visibility inside the Action: NOT FOUND.

## 5. Bridge patterns (assessed, ranked for v1)
| # | Pattern | Verdict |
|---|---|---|
| 1 | **Provider-native only → `unknown`** | Always the fallback. Policy must be conservative when no bridge data exists. |
| 2 | **Snapshot file in a repo attached to the session** (local collector pushes; `SessionStart` hook + CLAUDE.md/AGENTS.md tell the agent to read it; agent re-fetches with `git fetch`) | ✅ feasible for Claude (single-repo sessions). Needs `observed_at`; stale if the owner's PC is off; data is low-sensitivity but lives in git history; no token ever in the file. A separate "limits" repo must be **attached to every session** (multi-repo sessions load **no** repo hooks, ✅). Best default: put the snapshot in the **same repo the agent works on** (e.g. a git-ignored-from-main branch or a file the collector commits) — see Q-16. Codex: no repo hooks found, so advice only. |
| 3 | **Local push into a running/expired session:** `claude -p "limits: …" --cloud <session-id>` | ✅ documented, official CLI, no network change. Claude only. Needs the session id; good for "wake after reset" or "warn now". |
| 4 | **User-owned HTTPS endpoint + network secret** | Technically cleanest and live (✅ secret mechanism). Needs hosting a tiny endpoint → conflicts with "local-first, no hosting" unless the user opts in. Codex: needs internet on + allowlist. |
| 5 | **Remote MCP connector** | Highest setup; Codex cloud MCP not found. Defer. |
| 6 | **Plain env var token** | Rejected: readable by agent and all environment users. |
| 7 | Issue/PR comments as message channel | Possible via REST on attached repos; commit-identity side effects and event loops. Defer. |

Existing tools: AI Meter (`github.com/dichiara19/ai-meter`) has an MCP server so a local agent reads its own limits; local Windows app; no cloud mention (SUB). Nothing found built for cloud agents.

## Corrections to the Session 1 version of this file
1. Env/secret was "poor". **Corrected:** plain env vars are bad, but **network secrets** are a real, documented mechanism (pattern 4).
2. "Repo hooks yes" now carries the **single-repository** condition (breaks pattern 2 if a limits repo is added as a second repo).
3. Open question "can an agent create a routine itself?" **Answered: no, not via `/schedule` inside a cloud session.** Resume in cloud therefore = routine created by the user (one-shot) or `claude -p --cloud <id>` from the local PC (pattern 3).
4. Added pattern 3 (documented message into a session).

## Not found / unknown (carry to S2/R4)
`rate_limits`/`/usage`/`/status` inside a cloud VM (probe test); PreToolUse/Stop hooks in cloud explicitly; what a cloud session prints at the limit (errors page not read); Codex scheduled tasks, current cloud hooks/MCP/AGENTS.md, codex-action; whether the `/fire` payload `text` field can carry limit data; daily routine caps (only third-party sources: 5/15/25 per plan, unconfirmed).
