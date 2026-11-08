# show-llm-limits

> **Status: pre-build. Setup and specification phase. No code yet.**

A free tool that shows AI coding agents — **Claude Code** and **OpenAI Codex** — their **usage limits**, tells them **how to manage work** around those limits, and gives you a **dashboard** of your limits over time.

Planned v1:
1. **Limits for agents:** an agent can ask for its current limits (used %, window, reset time, freshness) in machine-readable form.
2. **Behavior policy, fully editable by you:** for example *stop at 95% used*, *resume after the window resets*, *split work into steps*, *checkpoint before stopping*. A default policy ships with the tool.
3. **Dashboard:** graphs of limit consumption over time.
4. **History for agents:** agents can read past usage to balance work and limits — and work autonomously, if you choose.

Local-first. Read-only toward providers. No usage data leaves your machine (proposal; see spec).

*Not affiliated with, endorsed by, or sponsored by Anthropic or OpenAI. Product names belong to their owners.*

## How this project is run
- Development follows the embedded workflow in [`docs/workflow/WORKFLOW.md`](docs/workflow/WORKFLOW.md): spec first, small verified steps, owner approvals, independent review.
- Built in linked sessions: see [`docs/SESSIONS.md`](docs/SESSIONS.md). Continuity lives in [`memory.md`](memory.md).
- Source of truth for the product: [`docs/SPEC.md`](docs/SPEC.md) (draft, unapproved).

## Repository map
| Path | Purpose |
|---|---|
| `AGENTS.md`, `CLAUDE.md` | Agent entrypoints (always-follow rules) |
| `memory.md` | Cross-session memory and next action |
| `docs/SPEC.md` | Product spec and decision log |
| `docs/OPEN-QUESTIONS.md` | Questions for the owner (Session 1) |
| `docs/RESEARCH-PLAN.md` | Research agenda (Session 2) |
| `docs/SESSIONS.md` | Roadmap and kickoff prompts |
| `docs/workflow/` | Embedded, adapted playbook; changes logged in `ADAPTATIONS.md` |
| `research/` | Research outputs (empty) |
| `policy/` | Agent behavior policy (empty) |
| `SETUP-FOR-OWNER.md` | Owner-only actions |

## License
Not chosen yet (decision Q-05). Intent: free for everyone.
