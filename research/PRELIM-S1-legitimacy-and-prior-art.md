# PRELIM (Session 1) — legitimacy, terms, naming, prior art
**Status: PRELIMINARY. Not legal advice.** Gathered 2026-11-08 by a Sonnet subagent. ✅ = quote verified by the main agent on the primary page on 2026-11-08. Everything else needs re-verification in Session 2 (R3, R7). Document effective dates were mostly not retrieved.

## Anthropic
- ✅ `code.claude.com/docs/en/legal-and-compliance`: "Anthropic does not permit third-party developers to offer Claude.ai login into their own applications, or to route requests through Free, Pro, Max, or Ultra plan credentials on behalf of their users. Moreover, developers may not collect, store, or intermediate Claude.ai credentials or session tokens". Also: "Anthropic reserves the right to take measures to enforce these restrictions and may do so without prior notice."
- ✅ Same page: "OAuth authentication is intended exclusively for purchasers of … subscription plans and is designed to support ordinary use of Claude Code and other native Anthropic applications."
- ✅ Same page: "you can't use the Claude Code or Anthropic names or logos as part of your own product, feature, or company name".
- Consumer Terms §3 (not verified by main agent): bars access "through automated or non-human means" except via API key or where Anthropic "explicitly permit[s] it"; bars sharing credentials; bars "bypassing any of our systems or protective measures".
- Usage Policy (not verified): bars services "that route requests through consumer subscriptions" and intentionally bypassing "restrictions".
- Enforcement (secondary, The Register 2026-02-20): tools spoofing the Claude Code harness were blocked.
- ✅ Statusline `rate_limits` is an official local data feed.

## OpenAI
- Terms of Use, effective 1 Jan 2026 (not verified by main agent): "You may not share your account credentials…"; bars "circumvent any rate limits or restrictions or bypass any protective measures"; bars programmatic extraction of data or output.
- Codex auth docs: "treat ~/.codex/auth.json like a password: it contains access tokens"; "API keys are still the recommended default for automation".
- No official statement found on third-party reuse of Codex ChatGPT tokens, nor on reading Codex local logs. A forum post claiming it is supported is unofficial.
- Brand page: OpenAI/ChatGPT/GPT names not allowed in product names. No explicit rule on "Codex".

## Preliminary ratings (to be confirmed)
| Source | Rating | Reason |
|---|---|---|
| Claude status-line `rate_limits` / local files | **GREEN** | Official local feed; no credential, no request to the service |
| Codex local rollout files | **GREEN/AMBER** | Local read; no clause found against it; undocumented format |
| Codex app-server (Codex uses its own login) | **AMBER, likely acceptable** | Official interface; we touch no token; confirm in R3 |
| Any tool that reads an OAuth token and calls a usage endpoint (Claude `api/oauth/usage`, Codex `wham/usage`) | **RED for Anthropic; AMBER for OpenAI** | Anthropic: "collect, store, or intermediate" credentials. High ban risk for every public user |
| Pause/resume helper that waits for the **official** reset | **GREEN** if worded and built as waiting; **never** rotate accounts/tokens | Evasion wording in both terms could be misread |
| Name containing Claude, Anthropic, OpenAI, ChatGPT, GPT | **RED** | Brand rules. `show-llm-limits` has none of these words. Use descriptive text ("for Claude Code and Codex users") with a non-affiliation notice |

## Prior art (secondary, from a subagent; star counts and dates not re-checked)
| Tool | License / lang | Data source | Feeds agents? |
|---|---|---|---|
| ccusage (github.com/ccusage/ccusage) | MIT / Rust; ~19k stars | Local JSONL/SQLite logs, many CLIs incl. Claude Code + Codex | Partly: Claude Code `statusline` command (beta) |
| CodexBar (github.com/steipete/CodexBar) | MIT / Swift (macOS) | Claude OAuth API / cookies / CLI; Codex OAuth API / CLI | CLI bundled; MCP unverified |
| Claude-Code-Usage-Monitor (Maciek-roboblog) | MIT / Python | local logs + status-line | No |
| Claude-Code-Usage-Monitor (CodeZeno) | MIT / Rust | reads local sign-in credentials, calls services | No |
| AIBattery (KyleNesium) | MIT / Swift | local dirs + ChatGPT usage endpoint | Unverified |
| lpm, Lunavect, stream-deck-ai-limits | unverified | — | — |

**Gap:** none checked clearly gives agents their own limits **plus a user-editable policy plus agent-readable history**, on Windows, for local **and** cloud agents. Several tools use the token approach that Anthropic's text prohibits.

## Not found / unknown
Effective dates of Consumer Terms/Usage Policy; Anthropic Commercial Terms and Trademark Guidelines (not read); OpenAI Services Agreement (not read); explicit wording on scheduled resume, unattended runs, or several agents on one subscription; trademark position on "Codex" and bare "Claude"; details of the smaller prior-art tools.
