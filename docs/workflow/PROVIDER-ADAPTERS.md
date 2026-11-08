# Provider adapters — implementation examples, not a second policy (embedded copy)

> **Adapted copy.** Upstream commit `e9abfef…` (see `ADAPTATIONS.md` A-4). The text is unchanged except this notice. In this repository the "target repository" is `show-llm-limits` itself, and the playbook is embedded in `docs/workflow/`. Example config files below are **not installed**; installing any of them needs an owner-approved decision.

**Canonical policy:** [WORKFLOW.md §9](WORKFLOW.md). This file maps the **same requirements** onto specific interfaces. The selected model IDs, effort controls, available subagents and limits can change. Verify them in the actual session; never claim an example file is installed in another repository merely because this reference repository contains it.

## Provider-neutral rule

The application repository is the **target**. This repository is the **reference**. Explicitly ask the agent to read the playbook at a pinned commit; do not rely on cross-repository auto-discovery. Capture the preflight report and require the step loop, review and release gates regardless of provider.

| Work tier | Claude examples | Codex / ChatGPT examples | Default effort |
|---|---|---|---|
| Cheap scoped extraction / read-only discovery | Haiku | Luna | Low |
| Medium complexity / bounded changes | Sonnet | Terra | Medium |
| Implementation / integration / hard debugging | Opus (or Fable if available) | Sol | Medium; High if justified |
| Restricted exceptional escalation | highest approved provider model/setting | **Astra only after explicit single-task approval** | Up to High unless separately authorized |

These are **role examples**, not claims of equivalence or permanent availability. If the product uses different model IDs, use its actual supported selector. Never silently raise effort or substitute a restricted model.

## Codex

Codex recognizes `AGENTS.md` in its **active repository instruction scope**. The AGENTS.md stored in this separate reference repo does not automatically govern the target. Tell Codex explicitly to fetch and follow the pinned playbook when working in the target repo.

Where the Codex runtime supports project subagent configuration, the following is an **optional example for the TARGET repo** (or an equivalent user/session setting), not a file to be installed here:

```toml
# TARGET REPOSITORY: .codex/config.toml
[agents]
enabled = true
max_concurrent_threads_per_session = 3
default_subagent_model = "gpt-6-luna"
default_subagent_reasoning_effort = "low"
```

```toml
# TARGET REPOSITORY: .codex/agents/reviewer.toml
name = "reviewer"
description = "Independent reviewer for correctness, security and regressions."
model = "gpt-6.1-sol"
model_reasoning_effort = "high"
developer_instructions = """
Independently review work written by another agent.
Do not modify code. Check the diff, specification, tests, security and regressions.
Report precise findings and evidence; never approve by assumption.
"""
```

Check that **these exact model IDs, config fields and subagent features** are supported in the running Codex version/environment before installing the optional example. They do not enforce an Astra ban on models chosen through unrelated UI or session controls; approval and runtime settings must be checked separately.

Docs: https://developers.openai.com/codex/multi-agent and https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex

## Claude Code

Claude Code uses `CLAUDE.md` instructions (and, in supported versions, `AGENTS.md`). For the central reference repository, `CLAUDE.md` imports `@AGENTS.md`. **Do not assume either file in a second repository is automatically inherited by a target repository.** For external-reference use, explicitly read the pinned playbook in each session.

Claude Code can define specialized agents under `.claude/agents/` in the **target** project or user configuration, with supported model/effort fields. If optional agents are configured, bind their scopes to §9's tiers and permissions; avoid unrestricted automatic delegation. The independent reviewer must not be the implementer.

Some Claude cloud/multi-repository session types do not honor the same local project settings and hooks as local Claude Code. Verify effective settings and available tools in that environment. Do not assume a local hook can protect cloud execution.

Docs: https://code.claude.com/docs/en/sub-agents and https://code.claude.com/docs/en/memory and https://code.claude.com/docs/en/settings

## ChatGPT Work, Projects, ordinary chat, and other agents

Use project/workspace instructions **when available** to point new sessions at this reference repo, but always run WORKFLOW §0 preflight. A connected GitHub repository or an attached Markdown file is a source to inspect, not proof of automatic instruction enforcement. In ordinary chat without code execution/Git write tools, the assistant can plan or review but must not claim it built, tested or merged anything. Host-selected models cannot necessarily be overridden by text instructions; disclose that limitation.

For any other coding agent, translate roles into supported tools and model/effort tiers, or work without subagents if they are unavailable. Missing delegation features never justify bypassing independent review or mandatory release checks. Distinguish what was performed from what still requires the owner or another environment.

## Enforcement caveat

Natural-language instructions are **not hard security boundaries**. Use actual runtime permissions, model restrictions where supported, CI, branch protection, tests and human approvals. If the provider cannot enforce a constraint such as "Astra requires approval" mechanically, require a per-task explicit approval in the interaction and disclose the residual limitation.
