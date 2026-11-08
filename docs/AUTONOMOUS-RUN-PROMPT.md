# AUTONOMOUS-RUN-PROMPT — backup copy of the scheduled-window prompt

> This is a **backup copy** for humans and for rebuilding the schedule. It is not an authority: the authority is the owner's request on 2026-11-08 (D-028) and the prompt stored in the scheduled tasks named `S2 window …`. Change the schedule only when the owner asks.

## Schedule (D-028)
- One window = **20 minutes of work**, then stop. Next window starts **5 h 10 min later** (start to start: 5 h 30 min).
- First window: **2026-11-08 17:00 Europe/Rome**. Then 22:30, 04:00, 09:30, 15:00, 20:30, 02:00, 07:30, and so on.
- Implemented as fresh-session one-shot scheduled tasks. Each window tops up the queue so that at least 8 are pending.

## Prompt (as stored in each scheduled task)
```
AUTONOMOUS WINDOW RUN for repository Simooo1010/show-llm-limits. The owner asked for this schedule on 2026-11-08 (decision D-028). The owner is usually not present. You work alone, inside the rules below.

TIME BUDGET: run `date -u` first and note the time. Work at most 20 minutes of wall-clock time. Start wrapping up at minute 17. Never run past minute 20. A short, clean checkpoint is better than unfinished work.

SETUP: `git fetch origin`. Check out branch `docs/s2-autonomous` (create it from origin/main only if it does not exist). Do all work and commits on that branch. Never force-push. Never rewrite history. Never merge a PR. Run the WORKFLOW §0 preflight: read in full AGENTS.md, memory.md, docs/workflow/WORKFLOW.md, docs/SPEC.md, docs/OPEN-QUESTIONS.md (including the section "Owner queue (unattended runs)"), docs/RESEARCH-PLAN.md, the Session 2 section of docs/SESSIONS.md, and research/PRELIM-S1-*.md. Report READY or BLOCKED in your first message. If BLOCKED, write why into the owner queue and stop.

GOAL: Session 2 research (docs and research only). The spec is NOT approved, so: no product code, no new dependencies, no LICENSE-style files that the spec says come later. Take the next unfinished item of docs/RESEARCH-PLAN.md in this priority: R3, R1, R2, R5, R7, R4, R6, R8, R9. After all are done, prepare spec v1 text as PROPOSALS in docs (session 3 preparation). Never write "Spec approved" and never claim the owner approved anything. One item, or one clear slice of an item, per window. Apply every owner decision D-001 to D-028 in docs/SPEC.md §7. Output files: research/R<n>-<slug>.md using the template in docs/RESEARCH-PLAN.md; keep research/INDEX.md current; add a row to the "Autonomous log" table in memory.md (UTC start, UTC end, what was done, what is next, idle yes/no).

RESEARCH RULES: use at most three parallel Sonnet subagents (research only, medium effort). Cite sources with URL and date retrieved. Verify every decisive claim yourself in the primary page. Say NOT FOUND when not found. Never read credential or auth files. Never call undocumented endpoints with any token. Never run the cloud probe (D-027) unless the owner approved the exact script after seeing it: prepare the script in research/, put it in the owner queue, and wait. Never use Astra. Max effort High. If R3 rates the only exact source RED, stop collector design and queue it for the owner. If the provider signals a usage limit, stop at a clean checkpoint, commit, record the resume point in memory.md.

OWNER QUEUE: anything that needs the owner (approval, answer, choice, merge, probe script, package approval, spec approval) goes into docs/OPEN-QUESTIONS.md, section "Owner queue (unattended runs)", as a row: ID, question, your recommendation, what it blocks, default if no answer. At the start of every window, read the queue for rows with a line starting `ANSWER:` and apply those answers (record each as a `SPEC:` decision row in docs/SPEC.md §7). While a row is unanswered, continue with other work that does not depend on it. Never guess a material decision.

END OF WINDOW: commit with a clear message ending with the attribution lines given in your system reminder. Push with a normal `git push -u origin docs/s2-autonomous` (retry up to 4 times on network errors only). If no open PR exists from docs/s2-autonomous to main, open one as DRAFT using the repo PR template if any; otherwise only push. Do not mark it ready and do not merge it.

SCHEDULE TOP-UP (do this last, even if you did little work): call list_triggers with include_completed true and find the scheduled tasks named like "S2 window <date time>". Count the pending (enabled, not yet fired) ones. If fewer than 8 are pending, create one more at a time with create_trigger: name "S2 window <YYYY-MM-DD HH:MM Rome>", run_once_at = (latest pending run_once_at + 5 hours 30 minutes) written as local time with [Europe/Rome], create_new_session_on_fire true, initiation human_schedule, and prompt = the exact stored prompt of an existing "S2 window" task (read it with get_trigger; if none can be read, use docs/AUTONOMOUS-RUN-PROMPT.md from branch docs/s2-autonomous). Repeat until 8 are pending. Never change or delete other scheduled tasks. Idle rule: if this window and the two windows before it (see the Autonomous log) all had nothing to do except waiting for the owner, do NOT top up; instead add an owner-queue row saying the windows stopped and why, and stop. Any answered owner-queue row counts as new work.

STOP CONDITIONS: stop immediately and queue the question if the task would need to write to any repository or service other than this one, spend money, change settings or permissions, or contradict the written spec.
```

## How the owner controls it
- **Answer a queued question:** tell any interactive session, or add a line `ANSWER: <text>` under the row in `docs/OPEN-QUESTIONS.md`. The next window applies it.
- **Pause or stop:** tell any interactive session "stop the S2 windows". It disables or deletes the `S2 window …` scheduled tasks.
- **Merge:** windows never merge. The owner merges the draft PR from `docs/s2-autonomous` when ready.
