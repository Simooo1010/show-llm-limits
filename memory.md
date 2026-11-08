# memory.md — cross-session memory for show-llm-limits

> Read this at the start of **every** session. Update it before the session ends (WORKFLOW §13). Keep it short. It points to the truth (`docs/SPEC.md`); it is not the truth. **Never put secrets, tokens, credentials or real usage data here.**

## Project in one paragraph
`show-llm-limits` is a free, public tool. It shows AI coding agents (Claude Code, OpenAI Codex) their usage limits from the user's account, gives them a user-editable behavior policy for managing work around those limits (for example stop at 95%, resume after the window resets, split work into steps, checkpoint), and gives the user a dashboard of limits over time. The same history is readable by agents so they can learn to balance work and limits, and work autonomously if the user wants. v1 goal: **working and functional as fast as possible; no design work.**

## Current state (update every session)
- **Phase:** S0 Setup is done. Waiting for the owner to review and start S1.
- **Spec:** `docs/SPEC.md` is **DRAFT v0, NOT APPROVED**. **No product code is allowed.**
- **Approval mode:** Strict (default).
- **Repository:** `Simooo1010/show-llm-limits`. Remote was empty at S0 start. S0 branch: `claude/focused-feynman-bx04fi`.
- **Upstream playbook:** `Simooo1010/claude-project-playbook` @ `e9abfefc9fa120504918e464f4bf2a7dcadf0570`, embedded in `docs/workflow/` with local changes in `ADAPTATIONS.md`. The playbook repo was read-only for S0 and was not changed.
- **Stack / dependencies / license / name:** not decided (Q-01, Q-02, Q-05).

## Owner decisions so far (details in `docs/SPEC.md` §7)
- D-001 Embed and follow the playbook always; owner may adapt it.
- D-002 No design now; fast and functional.
- D-003 Free for everyone, at least for the owner's personal use. License open.
- D-004 Session plan S0 → S1 define → S2 research → S3 approve spec → build.
- D-005 No code before spec approval.

## Owner working preferences (relevant to this project)
- Subagents: Sonnet/Haiku for research and light read-only work. Opus (or Fable) for building, implementing, verifying code/app and fixing errors found. Max effort High, never Max/Ultra. Run independent subagents in parallel in the same turn. (Recorded as WORKFLOW §9.1 overlay, A-2.)
- Owner writes in English or Italian; reply in the language used. Docs and code in English.
- Direct, no padding. Challenge weak ideas. Ask questions in grouped rounds with a recommendation.

## Known risks and unknowns (to resolve in research)
- **Legitimacy of sources (R3):** using a subscription login token outside the official client, or undocumented endpoints, may break provider terms. Prefer official/local sources.
- **No verified data source yet (R1, R2).** Do not state any field name as fact until research verifies it.
- **Cloud agents cannot read local files (Q-03).**
- **Enforce vs advise (R4):** unknown which behaviors can be enforced by hooks.
- **Provider limits change without notice (R5).**

## Next action
1. Owner reviews the S0 skeleton (PR/branch) and merges or comments.
2. Start **Session 1 — Define details** with the prompt in `docs/SESSIONS.md`.

## Session log (append only; newest last)
| Session | Date | Branch | Summary | Next |
|---|---|---|---|---|
| S0 Setup | 2026-11-08 | `claude/focused-feynman-bx04fi` | Embedded and adapted the playbook (A-1…A-5). Created AGENTS.md/CLAUDE.md, memory.md, draft SPEC v0 (goal, components, policy shape, edge cases, build order), OPEN-QUESTIONS (Q-01…Q-14), RESEARCH-PLAN (R1–R9), SESSIONS with kickoff prompts, folder stubs. No product code. | S1 |
