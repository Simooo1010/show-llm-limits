# PRELIM (Session 1) — Claude Code limit data sources
**Status: PRELIMINARY.** Gathered 2026-11-08 by a Sonnet research subagent from public web pages. The main agent verified only the items marked ✅ in the primary source. Everything else must be re-verified in Session 2 (R1, R4, R5). No endpoint was called. No credential file was read.

| # | Source | Official? | Auth | Key facts (names verbatim) | Risk |
|---|---|---|---|---|---|
| 1 | `/usage` screen | Documented (code.claude.com/docs/en/costs) | claude.ai login | Plan bars (5-hour session, weekly). API users: token counts + local dollar estimate. Falls back to "last-known usage" (≤60 min) if the request fails. No machine-readable output. | Medium (UI changes) |
| 2 | **Status line JSON on stdin** ✅ | **Documented** (code.claude.com/docs/en/statusline) | none (local) | `rate_limits.five_hour.used_percentage`, `rate_limits.seven_day.used_percentage` (0–100); `rate_limits.five_hour.resets_at`, `rate_limits.seven_day.resets_at` (Unix epoch seconds). `rate_limits.spend_limit.*` behind a Claude apps gateway. Also `cost.*`, `context_window.used_percentage`, `session_id`, `transcript_path`. | Low–medium |
| 2a | Presence rules ✅ | same page | — | `rate_limits` appears **only for claude.ai Pro, Max, Ultra** subscribers, **only after the first API response** in a session. Each window may be absent; a window is dropped after its `resets_at`. API-key users get no `rate_limits`. | — |
| 3 | Hooks | Documented (code.claude.com/docs/en/hooks) | none | Common input: `session_id`, `transcript_path`, `cwd`, `permission_mode`, `hook_event_name`. **No rate-limit fields** found in common input. `StopFailure` hook with matcher `rate_limit` fires on a limit hit but **cannot block or resume** (output ignored). `PreToolUse` and `Stop` hooks can block. | Low |
| 4 | Local transcripts `~/.claude/projects/<project>/<session-id>.jsonl` | Location documented; **line schema undocumented** | file access | `message.usage`: `input_tokens`, `output_tokens`, `cache_creation_input_tokens`, `cache_read_input_tokens`. One API response can appear on several lines (double counting up to ~1.9x). **No limit % or reset time.** | Medium–high |
| 5 | API-key response headers | Documented (platform.claude.com/docs/en/api/rate-limits) | API key | `anthropic-ratelimit-requests-*`, `anthropic-ratelimit-tokens-*`, `-input-tokens-*`, `-output-tokens-*` (`limit`/`remaining`/`reset`). API keys are **out of v1 scope** (D-007). | Low |
| 6 | `GET https://api.anthropic.com/api/oauth/usage` | **UNDOCUMENTED** | Claude Code OAuth token | Community tools use it; heavily rate limited (persistent 429s reported). Not called. **Likely violates the credential clause below.** | **High** |
| 7 | `claude.ai/api/organizations/{orgId}/usage` | **UNDOCUMENTED** | web session cookies | Behind Cloudflare; fragile. **Rejected.** | High |
| 8 | `rate_limit_event` in stream output | **UNDOCUMENTED**, single secondary source (Paseo discussion) | none | Claimed `rate_limit_info.unifiedWindows` with `five_hour`/`seven_day`. Unverified. | Unknown |

## Semantics (documented: support.claude.com/en/articles/11647753)
- Session limit resets **5 hours** after the first message; weekly limits reset weekly at a fixed per-account time. Per-model-family limits exist (Opus, Sonnet messages seen in `code.claude.com/docs/en/errors`). Verified Ultra accounts: no 5-hour limit. Usage credits can continue work past the limit.
- Messages (verbatim): `You've hit your session limit · resets 3:45pm`; `You've hit your weekly limit · resets Mon 12:00am`; warning `You've used 85% of your session limit · resets 3:45pm`.
- Allowance is shared across Claude Code, claude.ai chat and Cowork (Team/Enterprise docs; individual-plan claim is secondary).
- **Limits change often** (secondary sources list several changes in 2025–2026). The tool must read limits live, never hardcode them.

## Resume mechanisms
- **Built in:** interactive sessions signed in with claude.ai can wait and continue after reset (v2.1.234+), setting "Continue automatically at usage limit" in `/config`. Desktop app has "Auto-continue when limits reset". → **Overlaps with our "resume" feature.**
- `/loop` + cron tools: fire only while Claude Code runs and is idle; recurring tasks expire after 7 days.
- Headless: `claude -p`; `--output-format json` gives usage and cost; `CLAUDE_CODE_RETRY_WATCHDOG=1` retries 429/529 indefinitely (not for spend limits).
- OS scheduler + `claude -p --resume`: not documented as a limit-resume path.

## Not found / unknown
Official docs for `/api/oauth/usage`; whether `Stop` fires on a limit hit; whether `claude -p --output-format stream-json` documents `rate_limit_event`; official transcript schema; per-plan token quotas (not published); whether `statusLine` runs in headless or cloud sessions.

## Impact on SPEC
The status line is an **official, token-free, local** source for 5-hour and weekly used % on Claude (Pro/Max/Ultra). A tool can be (or wrap) the status-line command and write snapshots locally. Blind spots: headless runs and anything where the status line does not run. See SPEC §2.
