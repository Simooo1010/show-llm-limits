# PROBE (PROPOSED, NOT RUN) — do limit data and hooks exist inside a Claude Code cloud VM?

**Status: APPROVED by the owner on 2026-11-08 ("probe approved", recorded as D-027a). The approval covers this exact script and prompt only. Any change needs new approval. No agent runs it: the owner runs it in a throwaway repo and pastes the output back. Not yet run.**

## Question it answers
Inside a Claude Code cloud session:
1. Does the status line run, and does its stdin JSON contain `rate_limits`?
2. Which hook events fire (`SessionStart`, `UserPromptSubmit`, `PreToolUse`, `Stop`, `StopFailure`)? What top-level keys do their payloads carry? (R4)
3. Do any environment variable **names** hint at limit data?

## Safety design
- **Throwaway repo, created by the owner.** Name suggestion: `llm-limits-probe`, private, empty. The owner pastes the two files below. **No agent pushes to any of the owner's repos.**
- The probe logs **key names only**, except for the `rate_limits` subtree (percent and reset time, which is the data we want to find). It never logs prompt text, tool input or output, transcript content or paths.
- It reads **no** credential files. It prints **no** environment variable values (names only).
- It makes **no** network request. It writes one log file inside the VM (`/tmp/probe-log.jsonl`). The VM is thrown away after the session.
- A session with exactly one repository is required, because repo hooks load only in single-repo sessions.
- Cost: one short cloud session. It uses a little of the Claude Pro allowance.

## File 1 — `.claude/settings.json`
```json
{
  "statusLine": { "type": "command", "command": "node .claude/probe.mjs statusLine" },
  "hooks": {
    "SessionStart":     [{ "hooks": [{ "type": "command", "command": "node .claude/probe.mjs SessionStart" }] }],
    "UserPromptSubmit": [{ "hooks": [{ "type": "command", "command": "node .claude/probe.mjs UserPromptSubmit" }] }],
    "PreToolUse":       [{ "matcher": "*", "hooks": [{ "type": "command", "command": "node .claude/probe.mjs PreToolUse" }] }],
    "Stop":             [{ "hooks": [{ "type": "command", "command": "node .claude/probe.mjs Stop" }] }],
    "StopFailure":      [{ "hooks": [{ "type": "command", "command": "node .claude/probe.mjs StopFailure" }] }]
  }
}
```

## File 2 — `.claude/probe.mjs`
```js
// Probe: logs which event fired and the KEY NAMES of its JSON input.
// Logs values only for the rate_limits subtree. Never throws. Never blocks.
import { appendFileSync } from "node:fs";

const event = process.argv[2] ?? "unknown";
let raw = "";
process.stdin.setEncoding("utf8");
process.stdin.on("data", (c) => (raw += c));
process.stdin.on("end", () => {
  const entry = { t: new Date().toISOString(), event };
  try {
    const j = JSON.parse(raw);
    entry.keys = Object.keys(j);
    if (j.rate_limits !== undefined) entry.rate_limits = j.rate_limits;
    entry.has_rate_limits = j.rate_limits !== undefined;
  } catch {
    entry.parse_error = true;
    entry.raw_length = raw.length;
  }
  try { appendFileSync("/tmp/probe-log.jsonl", JSON.stringify(entry) + "\n"); } catch {}
  if (event === "statusLine") process.stdout.write("probe");
  process.exit(0);
});
```

## Prompt to give the cloud session (it is the only thing the agent does)
> Run exactly these three shell commands and show me the full output. Do nothing else. Do not run any other command.
> 1. `cat /tmp/probe-log.jsonl`
> 2. `env | cut -d= -f1 | grep -i -E 'claude|limit|rate|usage|quota' | sort`
> 3. `node --version`

Then the owner sends a second short message ("say hello") and asks the agent to run command 1 again, so `UserPromptSubmit`, `Stop` and a second status-line call are visible.

## How to read the result
| Observation | Meaning |
|---|---|
| `statusLine` entry present, `has_rate_limits: true` | The cloud VM exposes limits through the status line. Best case for D-015. |
| `statusLine` entry present, no `rate_limits` | The status line runs but has no limit data in cloud. Bridge still needed. |
| No `statusLine` entry | Status line does not run in cloud. Bridge still needed. |
| `PreToolUse` / `Stop` entries present | Those hooks fire in cloud, so enforcement (D-017) is possible there. |
| No hook entries at all | Repo hooks did not load (check the session had exactly one repo). |

## What the owner does (about 5 minutes)
1. Create a private empty repo `llm-limits-probe` and add the two files above.
2. Start a Claude Code cloud session on that repo only.
3. Paste the prompt. Copy the output back to the Session 2 chat (it has no secrets by design; read it before pasting).
4. Delete the repo afterwards.
