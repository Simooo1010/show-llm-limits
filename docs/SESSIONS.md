# SESSIONS — roadmap and kickoff prompts

The project is built in **linked sessions**. The links are: this repository, `memory.md` (state and next action), `docs/SPEC.md` (truth), and the embedded workflow. A session starts cold: it must re-read these files (WORKFLOW §0). Chat history is not a source of truth.

## Roadmap
| Session | Goal | Allowed work | Output | Exit condition |
|---|---|---|---|---|
| **S0 — Setup** (done) | Embed workflow, create structure and draft spec | Docs only | This repository skeleton | Owner reviews |
| **S1 — Define details** (in progress: Rounds 1–2 done, partial pre-research done) | Interview the owner; close the open questions; sharpen spec v0 | Docs only | Updated `docs/SPEC.md` (still DRAFT), answers log in `docs/OPEN-QUESTIONS.md` | Questions Round 1–4 answered or consciously deferred |
| **S2 — Research** | R1–R9 in `docs/RESEARCH-PLAN.md`: legitimacy, legality, data sources, how agents should use limits, behavior catalogue | Docs and research only; no product code; no real credentials | `research/R*.md`, `research/INDEX.md`, draft `policy/default.yaml` | Every R-item done, or marked blocked with reason |
| **S3 — Reconcile and approve spec** | Merge research into spec v1: final data names, build order, dependencies, license, commands | Docs only | Spec v1 for owner review | **Owner writes "Spec approved"**; recorded in §7 |
| **S4+ — Build** | One build-order step per session (or more in Milestone/Autonomous mode) | Product code per approved step | Branch + draft PR + five-part hand-off per step | Gates in WORKFLOW §6 |
| **Review/QA sessions** | Independent review (gate 3), final QA (gate 4–5), release | Review, fixes in separate commits | Review record | Owner authorizes release |

S1 and S2 can be the same chat if the owner prefers. Their goals and outputs stay separate. S2 may change what S1 decided. S3 reconciles it.

## Session rules
- One session, one goal. Do not start later-session work unless the owner says so.
- Each session works on its own branch. Cloud sessions use the branch the host assigns. Record it in `memory.md`.
- End every session by updating `memory.md` (state, decisions, open questions, **exact next action**). Commit and push. Open a draft PR when the host allows it.
- Unattended or automated sessions follow WORKFLOW §13.4: no guessing on material decisions; write the question down and stop the affected work.
- Usage limits of the build sessions: stop at a clean checkpoint when the provider signals a limit. Record the resume point. (Later this tool's own guidance replaces this manual rule.)

## Kickoff prompts (copy as is)

### Session 1 — Define details
> Work in `Simooo1010/show-llm-limits`. This repository embeds its governing workflow. Run the WORKFLOW §0 preflight now: read in full `AGENTS.md`, `memory.md`, `docs/workflow/WORKFLOW.md`, `docs/SPEC.md`, `docs/SESSIONS.md`, and `docs/OPEN-QUESTIONS.md`. Report READY or BLOCKED. This is **Session 1 — Define details**: docs only, no product code. Interview me with the questions in `docs/OPEN-QUESTIONS.md` in grouped rounds (use the structured-question tool if you have it; give your recommendation for each). Challenge weak assumptions. After each round, record my answers in `docs/OPEN-QUESTIONS.md` and the spec decision log (`SPEC:` commits), and update `memory.md`. End with the exact next action and the Session 2 kickoff prompt, adjusted to my answers.

### Session 1b — Define details (continued)
> Work in `Simooo1010/show-llm-limits`. Run the WORKFLOW §0 preflight: read in full `AGENTS.md`, `memory.md`, `docs/workflow/WORKFLOW.md`, `docs/SPEC.md`, `docs/SESSIONS.md`, `docs/OPEN-QUESTIONS.md` and the three `research/PRELIM-S1-*.md` files. Report READY or BLOCKED. This is **Session 1b**: docs only, no product code. Goal: **zero open doubts** before Session 2. Do in order: (1) **Verify** the cloud research that Session 1 finished but did not verify (`research/PRELIM-S1-cloud.md`; check the ★ claims in the primary pages; do not redo it from zero). Original scope: what limit data a Claude Code web/cloud session and a Codex cloud task can see; whether hooks, status line, MCP, scheduled tasks work there; network and persistence limits; bridge patterns (private-repo snapshot, env secret, MCP connector). Use one Sonnet subagent, cite sources with dates, verify decisive claims yourself. Save as `research/PRELIM-S1-cloud.md`. (2) Ask me the remaining questions in grouped rounds with recommendations: Q-08b, Q-15, Q-16, Q-17, Q-18, then Q-14, Q-10, Q-12, Q-01, Q-02, Q-05, Q-06, and my exact plan tiers. (3) Record answers (answers log + `SPEC:` decision rows), update the spec, update `memory.md`. (4) Run a **doubt check**: list every remaining ambiguity in the spec; ask until none is left or each is consciously deferred. End with the Session 2 kickoff prompt adjusted to my answers.

### Session 2 — Research
> Work in `Simooo1010/show-llm-limits`. Run the WORKFLOW §0 preflight: read in full `AGENTS.md`, `memory.md`, `docs/workflow/WORKFLOW.md`, `docs/SPEC.md`, `docs/OPEN-QUESTIONS.md` (with my answers), and `docs/RESEARCH-PLAN.md`. Report READY or BLOCKED. This is **Session 2 — Research**: execute R1–R9 as written. Follow the research rules exactly: cite sources with dates; no credentials, no real accounts, no calls to undocumented endpoints with my tokens unless I approve that exact action; use at most three parallel Sonnet subagents; verify decisive claims yourself in the primary sources. Priority is R3 (is this legitimate and legal?) and R6 (the complete behavior catalogue, including my examples: stop at 95%, resume after reset, split work into steps, user-editable). Write outputs to `research/`, draft `policy/default.yaml`, propose spec changes as `SPEC:` commits, update `memory.md`. Stop and report if R3 finds a RED source.

### Session 3 — Reconcile and approve spec
> Work in `Simooo1010/show-llm-limits`. Run the §0 preflight (read `AGENTS.md`, `memory.md`, the workflow, `docs/SPEC.md`, all `research/R*.md`). This is **Session 3**: turn the draft spec into **spec v1**: exact external data names from research, final build order, approved dependencies, license, commands, approval mode, test device. Show me the changes as a short summary with diagrams where they help. Do not write product code. Wait for me to write "Spec approved". Then record it in §7.

### Build step (Session 4+)
> Work in `Simooo1010/show-llm-limits`. Proceed with Step N. Run the §0 preflight first. Follow WORKFLOW §4, §6, §9, §12 and §13. Provide the five-part hand-off (§5). Respect the active approval mode in `docs/SPEC.md` §6. Update `memory.md` at the end.
