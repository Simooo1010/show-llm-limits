# OPEN QUESTIONS — agenda for Session 1 (define details)

Ask the owner in grouped rounds of up to four questions (use the structured-question tool if the host has one). Each question has a **recommendation**. Record each answer in `docs/SPEC.md` §7 and update the spec. Mark answered items below with the date and decision ID.

Status: **All questions are answered or consciously deferred (end of Session 1b, 2026-11-08).** Answers are in the **Answers log** (bottom) and in `docs/SPEC.md` §7 (D-012 to D-027). **Deferred items** are listed after the log. Q-03's old "cloud later" recommendation is **withdrawn**: cloud is first-class for Claude Code (D-006, narrowed by D-026).

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

## Answers log — Session 1b (2026-11-08)
| Date | ID | Answer | Recorded in |
|---|---|---|---|
| 2026-11-08 | Q-08b | First reply "Let's continue for both Claude and Codex" (ambiguous). Clarified after the Anthropic clause was shown: token-based sources only as a user-run opt-in, off by default; **Claude excluded from the public release**; Codex only if R3 is not RED. | D-012 (supersedes D-011) |
| 2026-11-08 | Q-15 | (a) Own small core. ccusage reviewed in S2 for token history only. | D-013 |
| 2026-11-08 | Tiers | Claude Pro. ChatGPT Plus. | D-014 |
| 2026-11-08 | Q-16 | Layered bridge: unknown fallback + repo snapshot + optional local push. | D-015 |
| 2026-11-08 | Snapshot write | Yes, opt-in per repo, off by default, data branch only. | D-016 |
| 2026-11-08 | Q-17 | Per-rule enforce/advise switch; default hard block at 95% where hooks exist. | D-017 |
| 2026-11-08 | Q-18 | Accept `unknown` + conservative defaults. | D-018 |
| 2026-11-08 | Q-14 | Combined view yes; per-agent attribution best-effort. | D-019 |
| 2026-11-08 | Q-10 | Strict to Step 2, then Milestone if gates green. | D-020 |
| 2026-11-08 | Q-12 | Windows, Chrome, light theme, two sizes. | D-021 |
| 2026-11-08 | Q-01 | Keep `show-llm-limits`. | D-022 |
| 2026-11-08 | Q-02 | TypeScript on Node, SQLite, Vitest, no hosting. | D-023 |
| 2026-11-08 | Q-05 | MIT. | D-024 |
| 2026-11-08 | Q-06 | npm + GitHub releases after v1. | D-025 |
| 2026-11-08 | Surfaces | Uses Claude Code web/app and Claude scheduled tasks. Does not use Codex cloud or GitHub Actions. | D-026 |
| 2026-11-08 | Probe | Allowed in S2 after the exact probe is shown and approved. | D-027 |

## Deferred items (conscious, with owner of the next step)
| Item | Default until decided | Decided in |
|---|---|---|
| Codex cloud tasks and GitHub Actions automation | Out of v1 (D-026). Revisit after v1. | After v1 |
| Codex token-based opt-in source | Not implemented until R3 rates it not RED (D-012). | S2 (R3), S3 |
| Whether snapshot push may target public repos | Refuse by default; user can override with a warning. | S3 |
| Snapshot branch shape: single latest commit (overwrite) vs history | Overwrite a single commit so git history does not collect usage data. | S3 / Step 7 |
| Exact packages (SQLite driver, chart library, CLI and YAML parsers) | None approved. | S2 (R8), then owner approves each |
| Probe script for `rate_limits`/`/usage` in a cloud VM | Not run. Owner approves the exact script first (D-027). | S2 |
| Whether Codex app-server works on Windows native and its schema stability | Unknown; verify in R2. | S2 |
| What a Claude cloud session prints at a limit; PreToolUse/Stop hooks in cloud | Unknown; advice-only in cloud until confirmed (D-017). | S2 (R4) |
