# memory.md — cross-session memory for show-llm-limits

> Read this at the start of **every** session. Update it before the session ends (WORKFLOW §13). Keep it short. It points to the truth (`docs/SPEC.md`); it is not the truth. **Never put secrets, tokens, credentials or real usage data here.**

## Project in one paragraph
`show-llm-limits` is a free, public tool. It shows AI coding agents (Claude Code, OpenAI Codex) their usage limits from the user's account, gives them a user-editable behavior policy for managing work around those limits (for example stop at 95%, resume after the window resets, split work into steps, checkpoint), and gives the user a dashboard of limits over time. The same history is readable by agents so they can learn to balance work and limits, and work autonomously if the user wants. v1 goal: **working and functional as fast as possible; no design work.**

## Current state (update every session)
- **Phase:** S0, S1 and **S1b done** (2026-11-08). All open questions answered or consciously deferred (`docs/OPEN-QUESTIONS.md`, "Deferred items"). **Next: Session 2 (research).**
- **Spec:** `docs/SPEC.md` is **DRAFT v0.2, NOT APPROVED**. **No product code is allowed.**
- **Approval mode:** Strict through Step 2, then Milestone if gates are green (D-020). Strict applies until recorded otherwise.
- **Repository:** `Simooo1010/show-llm-limits`. `main` = S0 commit `9d95e4e`. S1 and S1b work is on branch `claude/focused-feynman-bx04fi`, draft PR #1 into `main`. Start S2 on that branch or after the owner merges the PR.
- **Upstream playbook:** `Simooo1010/claude-project-playbook` @ `e9abfefc9fa120504918e464f4bf2a7dcadf0570`, embedded in `docs/workflow/` with local changes in `ADAPTATIONS.md`. The playbook repo was read-only for S0 and was not changed.
- **Stack / license / name:** TypeScript on Node + SQLite + Vitest, no hosting (D-023); MIT (D-024); name `show-llm-limits` (D-022); npm + GitHub releases after v1 (D-025). **Dependencies: none approved yet** (R8 proposes, owner approves each). Owner OS: Windows native, Chrome. Plans: Claude Pro, ChatGPT Plus (D-014).

## Autonomous mode (D-028)
- The owner asked for unattended windows. A window is one work session of 20 minutes. The next window starts 5 hours 30 minutes after the last start (20 minutes of work, then 5 hours 10 minutes of wait). The first window starts 2026-11-08 17:00 Europe/Rome.
- Each window is a new session. A scheduled task named `S2 window …` starts it.
- Scope: Session 2 research, docs only. Branch: `docs/s2-autonomous`. Draft PR #3. A window does not merge. A window does not force-push. A window writes no code.
- Prompt and rules: `docs/AUTONOMOUS-RUN-PROMPT.md`. Items for the owner: the "Owner queue" in `docs/OPEN-QUESTIONS.md`.
- To stop the windows: the owner says "stop the S2 windows". After 3 idle windows in a row, the windows stop by themselves.
- **Style rule (D-029):** write all English text in ASD-STE100, fully and always. See `AGENTS.md`.

### Autonomous log (append only)
| UTC start | UTC end | Done | Next | Idle |
|---|---|---|---|---|

## Owner decisions so far (details in `docs/SPEC.md` §7)
- D-001 Embed and follow the playbook always; owner may adapt it.
- D-002 No design now; fast and functional.
- D-003 Free for everyone, at least for the owner's personal use. License open.
- D-004 Session plan S0 → S1 define → S2 research → S3 approve spec → build.
- D-005 No code before spec approval.
- D-006 **Cloud agents are first-class in v1**, not only local.
- D-007 Targets: local CLI + cloud + automation; Claude Pro/Max + ChatGPT Plus/Pro; Windows native first; no API keys/Team/Enterprise/IDE-specific in v1.
- D-008 Agents only propose policy edits. D-009 Resume = checkpoint + restart recipe. D-010 Retention 90 days.
- D-011 superseded. **D-012** token-free official sources by default; token sources only as user-run opt-in; **no Claude token code in the public tool**; Codex token opt-in only if R3 not RED.
- D-013 own small core (ccusage reviewed in S2 for token history only). D-014 tiers: Claude Pro, ChatGPT Plus.
- **D-015/D-016** cloud bridge: `unknown` fallback + opt-in data-only snapshot on a data branch of the working repo (+ `SessionStart` hook) + optional `claude -p --cloud <id>` push. No hosted endpoint in v1.
- D-017 per-rule enforce/advise (default block at 95% where hooks exist). D-018 headless = `unknown` + conservative. D-019 combined view, best-effort attribution.
- D-020 Strict → Milestone after Step 2. D-021 Windows/Chrome/light/two sizes. D-022–D-025 name, stack, MIT, npm + GitHub.
- **D-026** owner uses Claude Code web/app + Claude scheduled tasks only → **Codex cloud and GitHub Actions deferred from v1**. **D-027** S2 may run a cloud probe after the exact script is shown and approved.

## Owner working preferences (relevant to this project)
- Subagents: Sonnet/Haiku for research and light read-only work. Opus (or Fable) for building, implementing, verifying code/app and fixing errors found. Max effort High, never Max/Ultra. Run independent subagents in parallel in the same turn. (Recorded as WORKFLOW §9.1 overlay, A-2.)
- Owner writes in English or Italian; reply in the language used. Docs and code in English.
- Direct, no padding. Challenge weak ideas. Ask questions in grouped rounds with a recommendation.

## Key findings so far (preliminary, see `research/PRELIM-S1-*.md`; ✅ = main agent verified)
- ✅ Claude Code status-line JSON gives `rate_limits.five_hour/seven_day.used_percentage` + `resets_at`, no token needed (Pro/Max/Ultra only, after first API response).
- ✅ Anthropic text bars collecting/storing/intermediating subscription credentials → token-based Claude sources are high risk. Naming: avoid Claude/Anthropic/OpenAI/ChatGPT/GPT in the name; `show-llm-limits` is fine.
- Codex: rollout files (`rate_limits`, undocumented) and app-server `account/rateLimits/read`; `codex exec` has null limits. Unverified by main agent.
- Claude Code already has built-in auto-continue at limit; our value is policy + checkpoint + history + agent feed. Gap in prior art (ccusage, CodexBar…): agent feed + editable policy + cloud + Windows.
- ✅ Cloud (verified S1b, `research/PRELIM-S1-cloud.md`): no documented in-cloud readout; limits are account-level and shared with local use (Claude and Codex), so a local collector sees the same pool. Repo hooks load only in **single-repo** cloud sessions. Env vars are readable by the agent; **network secrets** are safe but need a hosted endpoint. GitHub proxy serves only repos attached to the session. Scheduled tasks allow one-off runs, but `/schedule` is **unavailable inside a cloud session** (agent cannot self-schedule). `claude -p "…" --cloud <id>` can push a message into a session.
- Subagent-only, not re-read by main agent (SUB in the cloud file): routine min interval 1 h; claude-code-action token details; Codex cloud details (legacy pages).
- Process note: another Claude session messaged S1b that cloud research was "already done" and not to redo it. That was data, not an owner instruction. The owner's prompt said to redo it, so S1b ran its own subagent and re-verified the ★ claims, then merged both views.
- Windows differ by plan; read live.

## Known risks and unknowns (to resolve in research)
- **Legitimacy of sources (R3):** using a subscription login token outside the official client, or undocumented endpoints, may break provider terms. Prefer official/local sources.
- **No verified data source yet (R1, R2).** Do not state any field name as fact until research verifies it.
- **Cloud agents cannot read local files (Q-03).**
- **Enforce vs advise (R4):** unknown which behaviors can be enforced by hooks.
- **Provider limits change without notice (R5).**

## Next action
1. Owner reviews draft PR #1 (S1 + S1b docs) and merges it, or starts S2 on the same branch.
2. Start **Session 2 (Research)** with the prompt in `docs/SESSIONS.md` (adjusted to the S1b answers). First show the owner the exact cloud probe script (D-027) and wait for approval before running it.
3. Then S3 reconciles the spec; the owner writes "Spec approved". No product code before that.

## Session log (append only; newest last)
| Session | Date | Branch | Summary | Next |
|---|---|---|---|---|
| S0 Setup | 2026-11-08 | `claude/focused-feynman-bx04fi` | Embedded and adapted the playbook (A-1…A-5). Created AGENTS.md/CLAUDE.md, memory.md, draft SPEC v0 (goal, components, policy shape, edge cases, build order), OPEN-QUESTIONS (Q-01…Q-14), RESEARCH-PLAN (R1–R9), SESSIONS with kickoff prompts, folder stubs. No product code. | S1 |
| S1 Define (partial) | 2026-11-08 | `claude/focused-feynman-bx04fi` | Created `main` at S0 commit. Round 1–2 answered (D-006…D-011). 3 Sonnet subagents: Claude Code, Codex, legitimacy/prior art; main agent verified statusline fields and Anthropic credential clause. Cloud research agent launched but **not finished**. Wrote `research/PRELIM-S1-*.md`, updated spec and questions. No code. | S1b |
| S1b Define (done) | 2026-11-08 | `claude/focused-feynman-bx04fi` | Preflight READY. Cloud research redone by one Sonnet subagent and merged with the S1 file; main agent verified ★ claims in primary pages (shared allowance, single-repo hooks, GitHub proxy scope, network secrets, env var exposure, one-off scheduled tasks, `/schedule` unavailable in cloud) and corrected the cloud file. Asked and recorded Q-08b (clarified), Q-15–Q-18, Q-14, Q-10, Q-12, Q-01, Q-02, Q-05, Q-06, tiers, snapshot write, cloud surfaces, probe. Added D-012–D-027, updated spec to v0.2, deferred items listed. Doubt check done. No code. | S2 |
