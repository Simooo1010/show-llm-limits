# PRELIM (Session 1) — limits for cloud agents (Claude Code web, Codex cloud)
**Status: PRELIMINARY.** Gathered 2026-11-08 by a Sonnet subagent from official docs. **Not verified by the main agent.** Session 1b/2 must verify the decisive claims (marked ★) in the primary pages. No endpoint called, no credential read. The harness flagged the report for an instruction-shaped pattern ("settings-json"); it was a mention of settings files, not a directive. Nothing in it was acted on.

## 1. Can a cloud agent see its limits?
- ★ **No documented in-cloud readout** of 5-hour/weekly used % or reset time for Claude Code web or Codex cloud. The only documented numeric source is the local status-line JSON (`rate_limits.*`), which needs a local terminal.
- ★ **Limits are account-level and shared.** Claude: cloud sessions "share rate limits with all other Claude and Claude Code usage within your account" (`code.claude.com/docs/en/claude-code-on-the-web`). Codex: "Local messages and cloud chats share your plan's usage allowance"; "Cloud tasks may use more of your allowance than local messages" (`learn.chatgpt.com/docs/pricing.md`).
- **Consequence:** a **local** collector measures the same pool the cloud agents burn. The cloud agent only needs the *number*, delivered by a bridge.
- Hooks: `rate_limit` exists only as a `StopFailure` error type; no `used_percentage` in hook payloads.
- Unknown: whether `/usage` or `/status` works inside a Claude cloud session (the support page and the commands page disagree on `/status`); Codex cloud behavior at the limit.
- At the limit (Claude): "Usage limit reached · wrapping up" applies to a response in progress. Scheduled runs: "Without usage credits, additional runs are rejected until your usage window resets" (`routines.md`).
- Cloud env vars seen: `CLAUDE_CODE_REMOTE=true`, `CLAUDE_CODE_REMOTE_SESSION_ID`. No limit variable.

## 2. Features that work in cloud
| Feature | Claude Code cloud | Codex cloud |
|---|---|---|
| Repo `CLAUDE.md` / `AGENTS.md` | CLAUDE.md **yes** | AGENTS.md: only on the Legacy page; current page unclear |
| Repo hooks (`.claude/settings.json`) | **Yes, in a session with one repository**; `SessionStart` hooks run (example guarded by `CLAUDE_CODE_REMOTE`, 600 s default timeout) | **Not supported** as far as found: cloud allows "only Global MCP hooks" (medium confidence) |
| Repo `.mcp.json` servers | Yes, single-repo sessions | — |
| Skills / agents / commands in repo | Yes | Repo skills yes; personal skills not synced |
| Plugins in repo settings; `~/.claude/*`; `claude mcp add` local/user scope; `/plugin`, `/resume` | **No** | — |
| claude.ai connectors (remote MCP) | **Yes**, also in scheduled tasks; traffic goes through Anthropic servers, not the session network | — |
| Subagents | Yes | — |
| Scheduled runs | **Routines**: one-shot "once at a specific future time" or cron; faster than hourly rejected; each run is a new session; API `/fire` trigger (research preview, beta header); 100 runs/hour scheduled, 30/hour manual | Automations/Team Tasks "run in the cloud"; cron/one-shot details unknown |

## 3. Network and persistence
- Claude cloud network levels: None, Trusted (default), Full, Custom allowlist. GitHub access goes through a proxy scoped to **the repositories attached to the session**; other repos return 403, even public ones. Env vars are readable by anyone using the environment; values are read only at VM start/restore. Files persist while the VM is paused; the VM can be reclaimed. Background shells do not survive.
- Codex cloud: configurable internet access and domain allowlist; saved VM state recoverable up to 7 days; environment secrets removed before the agent phase.

## 4. Bridge patterns for limit data
| Pattern | Feasible? | Notes |
|---|---|---|
| (a) **Snapshot file in a repo attached to the session** (pushed by the local tool) | ★ Yes for Claude (repo cloned, hooks run). No doc forbids it. | Staleness: needs `observed_at`; if the user's computer is off, data goes stale. Limit data is low-sensitivity but lives in git history. Never put tokens in it. |
| (b) Environment variable / secret | Poor | Read only at VM start; no documented API to update from the user's machine. |
| (c) Remote (HTTPS) MCP server the user hosts or a connector | Plausible, untested | Local stdio MCP does not work in cloud. Needs hosting → breaks "local-first", adds cost/legal surface. |
| (d) Provider-native only | **Not available** | See §1. |

**Existing tools:** AI Meter (`github.com/dichiara19/ai-meter`) has an MCP server so Claude Code can read its own limits; it is a local Windows app, no cloud mention. No tool found built for cloud agents.

## Impact on SPEC / questions
- Strong candidate for Q-16: **local collector → snapshot file pushed to a user-owned private repo attached to cloud sessions → `SessionStart` hook + CLAUDE.md/AGENTS.md instruct the agent to read it**, with `observed_at` and an `unknown`-when-stale rule. Codex cloud lacks repo hooks, so there the bridge is instruction-only (AGENTS.md) — advice, not enforcement (Q-17).
- **Resume in cloud (Claude):** routines support one-shot future runs, so "resume after reset" can be a scheduled routine. Needs checking whether an agent can create one itself.
- The tool's real job for cloud: measure locally, publish a small safe snapshot, keep it fresh.

## Not found / unknown
Official in-cloud readout; `/usage` in cloud; Codex cloud limit-hit behavior; current Codex cloud AGENTS.md wording; repo hooks in Codex cloud; Codex cron/one-shot; Claude GitHub Action limit behavior; acceptability of a hosted MCP for this purpose.
