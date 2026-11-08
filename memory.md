# memory.md — cross-session memory for show-llm-limits

> Read this at the start of **every** session. Update it before the session ends (WORKFLOW §13). Keep it short. It points to the truth (`docs/SPEC.md`); it is not the truth. **Never put secrets, tokens, credentials or real usage data here.**

## Project in one paragraph
`show-llm-limits` is a free, public tool. It shows AI coding agents (Claude Code, OpenAI Codex) their usage limits from the user's account, gives them a user-editable behavior policy for managing work around those limits (for example stop at 95%, resume after the window resets, split work into steps, checkpoint), and gives the user a dashboard of limits over time. The same history is readable by agents so they can learn to balance work and limits, and work autonomously if the user wants. v1 goal: **working and functional as fast as possible; no design work.**

## Current state (update every session)
- **Phase:** S0 done. **S1 Define details is partly done (Rounds 1–2 answered; pre-research on Claude Code, Codex and legitimacy done; cloud research done in `research/PRELIM-S1-cloud.md`, unverified). Continue in S1b.**
- **Spec:** `docs/SPEC.md` is **DRAFT v0, NOT APPROVED**. **No product code is allowed.**
- **Approval mode:** Strict (default).
- **Repository:** `Simooo1010/show-llm-limits`. `main` was created at the S0 commit `9d95e4e` (owner said "merge also"). S1 work is on branch `claude/focused-feynman-bx04fi` with a draft PR into `main`. **Merge that PR before S1b, or start S1b on that branch.**
- **Upstream playbook:** `Simooo1010/claude-project-playbook` @ `e9abfefc9fa120504918e464f4bf2a7dcadf0570`, embedded in `docs/workflow/` with local changes in `ADAPTATIONS.md`. The playbook repo was read-only for S0 and was not changed.
- **Stack / dependencies / license / name:** not decided (Q-01, Q-02, Q-05). Owner OS: Windows native.

## Owner decisions so far (details in `docs/SPEC.md` §7)
- D-001 Embed and follow the playbook always; owner may adapt it.
- D-002 No design now; fast and functional.
- D-003 Free for everyone, at least for the owner's personal use. License open.
- D-004 Session plan S0 → S1 define → S2 research → S3 approve spec → build.
- D-005 No code before spec approval.
- D-006 **Cloud agents are first-class in v1**, not only local.
- D-007 Targets: local CLI + cloud + automation; Claude Pro/Max + ChatGPT Plus/Pro; Windows native first; no API keys/Team/Enterprise/IDE-specific in v1.
- D-008 Agents only propose policy edits. D-009 Resume = checkpoint + restart recipe. D-010 Retention 90 days.
- D-011 PROVISIONAL: token-based sources "opt-in, off by default" — to reconfirm (Q-08b).

## Owner working preferences (relevant to this project)
- Subagents: Sonnet/Haiku for research and light read-only work. Opus (or Fable) for building, implementing, verifying code/app and fixing errors found. Max effort High, never Max/Ultra. Run independent subagents in parallel in the same turn. (Recorded as WORKFLOW §9.1 overlay, A-2.)
- Owner writes in English or Italian; reply in the language used. Docs and code in English.
- Direct, no padding. Challenge weak ideas. Ask questions in grouped rounds with a recommendation.

## Key findings so far (preliminary, see `research/PRELIM-S1-*.md`; ✅ = main agent verified)
- ✅ Claude Code status-line JSON gives `rate_limits.five_hour/seven_day.used_percentage` + `resets_at`, no token needed (Pro/Max/Ultra only, after first API response).
- ✅ Anthropic text bars collecting/storing/intermediating subscription credentials → token-based Claude sources are high risk. Naming: avoid Claude/Anthropic/OpenAI/ChatGPT/GPT in the name; `show-llm-limits` is fine.
- Codex: rollout files (`rate_limits`, undocumented) and app-server `account/rateLimits/read`; `codex exec` has null limits. Unverified by main agent.
- Claude Code already has built-in auto-continue at limit; our value is policy + checkpoint + history + agent feed. Gap in prior art (ccusage, CodexBar…): agent feed + editable policy + cloud + Windows.
- Cloud: no documented in-cloud readout; limits are account-level and shared; best bridge candidate = local collector → snapshot in a user-owned private repo attached to the session + SessionStart hook (Claude); Codex cloud has no repo hooks (advice only). Routines allow one-shot future runs (Claude cloud resume).
- Windows differ by plan; read live.

## Known risks and unknowns (to resolve in research)
- **Legitimacy of sources (R3):** using a subscription login token outside the official client, or undocumented endpoints, may break provider terms. Prefer official/local sources.
- **No verified data source yet (R1, R2).** Do not state any field name as fact until research verifies it.
- **Cloud agents cannot read local files (Q-03).**
- **Enforce vs advise (R4):** unknown which behaviors can be enforced by hooks.
- **Provider limits change without notice (R5).**

## Next action
1. Owner merges the S1 draft PR into `main` (or starts S1b on the S1 branch).
2. Start **Session 1b** with the prompt in `docs/SESSIONS.md`: verify the cloud research (`research/PRELIM-S1-cloud.md`, check ★ claims), ask Q-08b, Q-15–Q-18, Q-14, Q-10, Q-12, Q-01, Q-02, Q-05, Q-06 and plan tiers, then run the doubt check.

## Session log (append only; newest last)
| Session | Date | Branch | Summary | Next |
|---|---|---|---|---|
| S0 Setup | 2026-11-08 | `claude/focused-feynman-bx04fi` | Embedded and adapted the playbook (A-1…A-5). Created AGENTS.md/CLAUDE.md, memory.md, draft SPEC v0 (goal, components, policy shape, edge cases, build order), OPEN-QUESTIONS (Q-01…Q-14), RESEARCH-PLAN (R1–R9), SESSIONS with kickoff prompts, folder stubs. No product code. | S1 |
| S1 Define (partial) | 2026-11-08 | `claude/focused-feynman-bx04fi` | Created `main` at S0 commit. Round 1–2 answered (D-006…D-011). 3 Sonnet subagents: Claude Code, Codex, legitimacy/prior art; main agent verified statusline fields and Anthropic credential clause. Cloud research agent launched but **not finished**. Wrote `research/PRELIM-S1-*.md`, updated spec and questions. No code. | S1b |
