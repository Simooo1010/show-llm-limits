# show-llm-limits — agent entrypoint

This is the **working repository** for `show-llm-limits`: a free tool that shows AI coding agents (Claude Code, OpenAI Codex) their usage limits, tells them how to manage work around those limits, and gives the user a dashboard of limits over time.

## Governing workflow — always follow it
The owner embedded the development playbook in this repository. **It governs every session, human-started or automated.**

1. At the start of every session and every step, run the preflight in `docs/workflow/WORKFLOW.md` §0. Read in full: this file, `memory.md`, `docs/workflow/WORKFLOW.md`, `docs/SPEC.md`, and your session's section in `docs/SESSIONS.md`.
2. Report **READY** or **BLOCKED** before doing project work.
3. **No product code before the owner approves `docs/SPEC.md`.** The spec status is in its header. While it says DRAFT, only documentation, research and decision work is allowed.
4. Before the session ends, update `memory.md` (WORKFLOW §13).

## Where things are
| Need | File |
|---|---|
| Cross-session memory and next action | `memory.md` |
| Method (steps, gates, models, approvals) | `docs/workflow/WORKFLOW.md` |
| Local changes to the upstream playbook | `docs/workflow/ADAPTATIONS.md` |
| Product spec + decision log | `docs/SPEC.md` |
| Questions for the owner | `docs/OPEN-QUESTIONS.md` |
| Research agenda and outputs | `docs/RESEARCH-PLAN.md`, `research/` |
| Session roadmap and kickoff prompts | `docs/SESSIONS.md` |
| Agent behavior policy (future) | `policy/` |

## Hard rules (summary — the workflow file is authoritative)
- Never commit or print secrets, tokens, credentials or real usage data. Never read the owner's credential files (for example provider auth files) unless the owner approves that exact action in the current session.
- Do not add a dependency that the spec does not approve. Ask first.
- Do not write to any repository or service other than this one.
- Use the model and effort policy in WORKFLOW §9 (including the project overlay). Maximum effort is High. Do not use Astra without approval for that single task.
- Report honestly: PASS / FAIL / NOT RUN / PENDING, with evidence. Do not claim a check ran if it did not.
- A material decision made in chat must be written to `docs/SPEC.md` §7.
- Docs and code in English. Reply in the owner's language.

## Communication style (from upstream `AGENTS.md`)
For owner-facing explanations, reasoning summaries, decisions and progress reports in English: use clear, short sentences and consistent terms, about 80% of the way to ASD-STE100 Simplified Technical English. Keep technical precision. This is a readability preference, not a claim of formal compliance. Do not apply it to code, commands, logs, API names or quotations. When the owner writes in another language, answer in that language with the same clarity.
Use a compact diagram when it helps explain a non-trivial design (WORKFLOW §11.1). No decorative visuals.
