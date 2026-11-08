# Setup for owner — show-llm-limits

Checklist of things only the owner can do. Never paste secret values into chat or Git.

## Now
- [ ] Review the S0 skeleton (branch `claude/focused-feynman-bx04fi`). Merge it into the default branch, or ask for changes. *(The remote was empty; see `memory.md`.)*
- [ ] Start Session 1 with the prompt in `docs/SESSIONS.md`.
- [ ] Be ready to answer `docs/OPEN-QUESTIONS.md`: where your agents run, which plans/accounts you have, your OS, your risk appetite for data sources.

## Before research (Session 2)
- [ ] Decide if the research agent may look at *your own* local agent files for **structure only** (for example, field names in a log). Default: **no**. Approval is per action.
- [ ] Do not give any API key or token to a research session.

## Before build
- [ ] Approve the spec (write "Spec approved").
- [ ] Choose license and name (Q-05, Q-01).
- [ ] Approve the dependency list.
- [ ] Choose a browser for the dashboard check.

## Before public release
- [ ] Authorize production release (WORKFLOW §6, no delegation exists).
- [ ] Confirm no personal usage data or secrets are in the repository (`.gitignore` helps; check anyway).
