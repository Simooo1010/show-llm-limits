# OPEN QUESTIONS — agenda for Session 1 (define details)

Ask the owner in grouped rounds of up to four questions (use the structured-question tool if the host has one). Each question has a **recommendation**. Record each answer in `docs/SPEC.md` §7 and update the spec. Mark answered items below with the date and decision ID.

Status: `OPEN` unless stated.

## Round 1 — scope and reality (ask first; they change research)
| ID | Question | Recommendation | Why it matters |
|---|---|---|---|
| Q-03 | **Where do your agents run?** Local CLI on your computer, cloud sessions (like Claude Code on the web), or both? | Local first. Treat cloud as a later bridge. | Local collectors cannot read files inside a cloud container. This decides the architecture. |
| Q-04 | **Which accounts and plans?** Claude (Pro / Max / Team / API key) and OpenAI (ChatGPT Plus / Pro / Team / API key)? One account each or several? | Subscription limits (usage windows) are the core. API-key rate limits are a different problem; defer. | Subscription and API limits are different mechanisms with different data. |
| Q-08 | **Risk appetite for data sources.** If the only exact source is an undocumented endpoint that needs your login token, do you want it (a) never, (b) opt-in and off by default, or (c) on? | (a) or (b). Start with official/local sources and decide after R3. | Legal/ToS risk and account-ban risk for you and for every public user. |
| Q-07 | Which OS and shell do you use? Which must v1 support? | Owner's OS first; others after v1. | Paths of local files and install steps differ. |

## Round 2 — product behavior
| ID | Question | Recommendation | Why it matters |
|---|---|---|---|
| Q-09 | **May agents edit the behavior policy themselves?** (never / propose only / edit within bounds / fully) | Propose only. The user approves edits. | Controls autonomy and abuse risk. A policy an agent can rewrite is not a limit. |
| Q-11 | **How should an agent "resume in 5 hours"?** Self-schedule (provider scheduler, OS cron), or only write a checkpoint and wait for you or an automation to restart it? | Checkpoint plus a documented restart recipe per provider; automate after R4. | Depends on what each provider allows. |
| Q-14 | **Several agents on one account at once** — do you need combined view and per-agent attribution in v1? | Combined view yes; per-agent attribution best-effort. | Affects store design. |
| Q-13 | **How long to keep history?** | 90 days by default, user-configurable, local only. | Storage and privacy. |

## Round 3 — project and release
| ID | Question | Recommendation | Why it matters |
|---|---|---|---|
| Q-02 | **Stack.** Playbook default is Next.js on Vercel, but this tool is local-first. Options: (a) TypeScript on Node: CLI + local dashboard server, SQLite, Vitest; (b) Python; (c) Go/Rust single binary. | (a) TypeScript on Node, **no Vercel, no hosting**. Fast to build, same language for CLI and dashboard, easy `npx` install. Confirm after R8. | Sets dependencies the owner must approve (WORKFLOW §7). |
| Q-05 | **License** for a free public tool. | MIT (simple) or Apache-2.0 (adds patent grant). Pick after R3 and R7 (license compatibility). | Public release needs one. Do not add a LICENSE file before the decision. |
| Q-06 | **Distribution:** npm/`npx`, GitHub releases, or both? | npm + GitHub, after v1 works. | Release step. |
| Q-01 | **Name.** Keep `show-llm-limits`? Names with "Claude" or "Codex" may raise trademark issues. | Keep a neutral name. R3 checks. | Public naming. |

## Round 4 — process
| ID | Question | Recommendation | Why it matters |
|---|---|---|---|
| Q-10 | **Approval mode:** Strict, Milestone or Autonomous? | **Strict** through Step 2 (policy engine). Then **Milestone** if gates are green. Autonomous only for steps with strong automated tests. | You want highly autonomous sessions; the workflow allows it only inside a recorded mode. |
| Q-12 | **Test device and dashboard checks:** which browser/device will you use to check the dashboard? Which window sizes? | One desktop browser, one theme, two sizes. | Needed for gates 4–5. |

## Answers log
| Date | ID | Answer | Recorded in |
|---|---|---|---|
| — | — | (none yet) | — |
