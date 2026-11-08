# PRELIM (Session 1) — Codex limit data sources
**Status: PRELIMINARY.** Gathered 2026-11-08 by a Sonnet subagent from public pages and issues. **Not verified by the main agent.** Re-verify in Session 2 (R2, R4, R5), ideally by reading `codex-rs` source. Docs host moved: `developers.openai.com/codex/*` redirects to `learn.chatgpt.com/docs/*`. No endpoint called; no credential file read.

| # | Source | Official? | Auth | Key facts (names verbatim) | Risk |
|---|---|---|---|---|---|
| 1 | `/status` in a CLI session; usage dashboard `chatgpt.com/codex/settings/usage` | Documented (pricing FAQ) | ChatGPT sign-in | Pricing page: "If you want to see your remaining limits during an active Codex CLI session, you can use `/status`." Output format not verified. Dashboard has no documented API. | Low (format unknown) |
| 2 | **Rollout files** `$CODEX_HOME/sessions/**/*.jsonl` (default `~/.codex/sessions`) | **Undocumented** (community posts, issues) | file access | `event_msg` lines with `payload.type == "token_count"` hold `info` (`total_token_usage`, `last_token_usage`, `model_context_window`) and `rate_limits`: `limit_id`, `limit_name`, `primary`, `secondary`, `credits`, `plan_type`, `rate_limit_reached_type`. `primary`/`secondary`: `used_percent`, `window_minutes`, `resets_at` (epoch s). Example windows: 300 and 10080 minutes. | Medium–high |
| 2a | Blind spot | issue #14728 | — | `codex exec` mode writes `rate_limits: null` in rollouts. Data is fresh only as of the last turn. | — |
| 3 | **app-server** JSON-RPC `account/rateLimits/read`; notification `account/rateLimits/updated` (`codex app-server --listen stdio://`) | Official docs for app-server; method seen in a **docs mirror**, not the official page | the user's ChatGPT login **held by Codex** (we handle no token) | Result: `rateLimits`, `rateLimitsByLimitId`; buckets have `limitId`, `limitName`, `primary`, `secondary`, `rateLimitReachedType`; windows `usedPercent`, `windowDurationMins`, `resetsAt`. Also `rateLimitResetCredits`. Experimental status unknown. | Medium (schema changing) |
| 4 | Hooks | Documented (`learn.chatgpt.com/docs/hooks`) | none | Events include `SessionStart`, `UserPromptSubmit`, `PreToolUse`, `PostToolUse`, `Stop`, `PostModelRequest` (has `token_usage`). **No rate-limit fields.** `transcript_path` may point to the rollout file. | Low |
| 5 | `notify` config | Documented | none | Only `agent-turn-complete`; no usage fields seen. | — |
| 6 | `codex exec --json` events | Documented | — | `thread.started`, `turn.started`, `turn.completed`, `turn.failed`, `item.*`, `error`. No `rate_limits`. | — |
| 7 | `https://chatgpt.com/backend-api/wham/usage` | **UNDOCUMENTED** | ChatGPT access token from `auth.json` | Polled by the CLI itself every 60 s (issue #10869). Schema not verified. **Not recommended.** | **High** |
| 8 | API-key headers | Documented | API key | `x-ratelimit-limit-requests`, `-tokens`, `remaining-*`, `reset-*`. API keys are out of v1 scope (D-007). | Low |

## Semantics (documented: pricing page, learn.chatgpt.com/docs/pricing)
- Plus/Business: local-message estimates "per five-hour period"; "Pro plans currently have no five-hour limit"; "Weekly limits may also apply". **Windows differ by plan — read `window_minutes`, never assume 5 h / weekly.**
- "Local messages and cloud chats share your plan's usage allowance." Code Review via GitHub is counted separately. Fast mode uses 2.5x, Ultrafast 8x of included usage.
- At the limit: "the agent will be able to continue working on that turn, subject to fair use limits." Credits can be bought.
- Plan/limit changes were frequent in 2026 (secondary source, low confidence).
- Reset time: use `resets_at`; do not derive from `window_minutes`.

## Resume mechanisms
`codex exec resume [SESSION_ID]` / `--last`; `codex resume`; `codex exec --json`; `CODEX_API_KEY=… codex exec` is the documented API-key automation path; `codex cloud list --json`; app "Scheduled tasks" (`learn.chatgpt.com/docs/automations`). Scheduling API and an official GitHub Action: not found.

## Not found / unknown
Exact `/status` output; limit-hit error text in exec JSON; whether `turn.completed` carries tokens; `wham/usage` schema; official page for `account/rateLimits/read`; rollout schema stability; whether cloud tasks write rollouts; whether IDE and CLI rollouts match.

## Impact on SPEC
Two local candidates for Codex: **rollout files** (undocumented, no token needed) and **app-server** (official interface, Codex holds the login). Headless `codex exec` has no limit data in rollouts: a blind spot for automation.
