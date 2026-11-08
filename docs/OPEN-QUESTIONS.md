# OPEN QUESTIONS — agenda for Session 1 (define details)

Ask the owner in grouped rounds of up to four questions (use the structured-question tool if the host has one). Each question has a **recommendation**. Record each answer in `docs/SPEC.md` §7 and update the spec. Mark answered items below with the date and decision ID.

Status: `OPEN` unless the **Answers log** (bottom) shows an answer. **Answered in Session 1:** Q-03, Q-04, Q-07, Q-09, Q-11, Q-13 (and Q-08 provisionally). **Still open:** Q-01, Q-02, Q-05, Q-06, Q-10, Q-12, Q-14, plus the new Q-08b, Q-15–Q-18 at the bottom. Q-03's old "cloud later" recommendation is **withdrawn**: cloud is first-class (D-006).

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
| 2026-11-08 | Q-03 | Local CLI + cloud sessions + automation (headless/CI). IDE extension not selected. Mid-session: "we have to focus also on cloud, not only local". | D-006, D-007 |
| 2026-11-08 | Q-04 | Claude subscription (Pro/Max) + ChatGPT subscription (Plus/Pro) for Codex. No API keys, no Team/Enterprise. Exact tiers not given (ask in S1b). | D-007 |
| 2026-11-08 | Q-07 | Windows (native). | D-007 |
| 2026-11-08 | Q-08 | "Opt-in, off by default" — before seeing research. **Provisional.** | D-011 |
| 2026-11-08 | Q-09 | Agents propose only; user approves. | D-008 |
| 2026-11-08 | Q-11 | Checkpoint + restart recipe; automate after research. | D-009 |
| 2026-11-08 | Q-13 | 90 days. | D-010 |
| 2026-11-08 | Cloud bridge | Owner chose "decide after research", then said cloud must be a focus. Tracked as Q-16. | D-006 |

## New questions raised in Session 1 (ask in S1b; each has my recommendation)
| ID | Question | Recommendation | Why it matters |
|---|---|---|---|
| Q-08b | **Narrow the token rule.** Research: Anthropic's published text bars collecting/storing/intermediating subscription credentials; usage endpoints are undocumented. Options: (a) never use token-based sources for Claude; for Codex only the app-server (Codex holds the login); (b) opt-in for both; (c) as you decided before. | **(a).** The status line gives Claude limits with no token. A public tool that risks every user's account is not "free for everyone". | Public ban risk. Confirms or replaces D-011. |
| Q-15 | **Reuse or build?** ccusage (MIT, large) already reads local logs and has a status-line command; CodexBar is macOS-only. Options: (a) own collectors, own small core; (b) depend on ccusage as a library/CLI for token history; (c) fork. | **(a)** for limits and policy (the gap), and read ccusage's code/license in S2 before deciding on token history. | Dependencies need your approval; maintenance load. |
| Q-16 | **Cloud bridge.** Preliminary cloud research is done (`research/PRELIM-S1-cloud.md`). Options: (a) local tool pushes a small snapshot file to your own private repo attached to cloud sessions; a `SessionStart` hook and CLAUDE.md/AGENTS.md make the agent read it; (b) env secret (stale, poor); (c) hosted remote MCP (breaks local-first, costs hosting); (d) provider-native (not available). | **(a)**, with `observed_at` and `unknown`-when-stale. Codex cloud has no repo hooks, so advice only there. Verify the ★ claims first. | D-006 makes it a v1 requirement. Needs your approval to push data to a repo. |
| Q-17 | **Enforce or advise?** Claude hooks (`PreToolUse`, `Stop`) can block; `StopFailure` cannot. Should the 95% rule be a hard block, or advice the agent may ignore? | Hard block by hook where the provider allows; advice elsewhere; user can switch per rule. | Real autonomy needs enforcement; wrong blocks waste work. |
| Q-18 | **Headless blind spot.** `codex exec` writes no limit data; Claude headless is unknown. Accept `unknown` + conservative policy, or require running through an interactive wrapper? | Accept `unknown` with conservative defaults; document it. | Affects automation users. |
