# WORKFLOW — provider-neutral coding workflow (embedded, project-adapted copy)

> **Provenance.** Embedded copy of `Simooo1010/claude-project-playbook` `docs/WORKFLOW.md` at commit `e9abfefc9fa120504918e464f4bf2a7dcadf0570`. The owner chose to embed it so the rules can be adapted for this project (decision D-001 in `docs/SPEC.md` §7). Every local change from upstream is listed in [`ADAPTATIONS.md`](ADAPTATIONS.md). Sections without an entry there are unchanged. **This copy is now the governing method for this repository.**

Canonical development method for Claude Code, Codex and any other coding agent working on **this** repository (`Simooo1010/show-llm-limits`).

`AGENTS.md` (repository root) is the entrypoint. [`PROVIDER-ADAPTERS.md`](PROVIDER-ADAPTERS.md) has implementation examples; this file remains authoritative for the method.

---

## 0. Activation and startup preflight (embedded mode)

**[ADAPTED — see ADAPTATIONS.md A-1.]** The playbook lives inside the target repository. The owner adopted it for this project as a **standing authorization** (D-001), so no per-session adoption prompt is needed. This repository's `AGENTS.md` / `CLAUDE.md` are the activation. Agents must still read the files; a file that exists is not a file that was read.

On **every new session** and **every implementation step**, before implementation or delegation:

1. **Read in full:** `AGENTS.md`, `memory.md`, this file, `docs/SPEC.md` (including the decision log), and the section of `docs/SESSIONS.md` for the current session. Read `docs/OPEN-QUESTIONS.md` and `docs/RESEARCH-PLAN.md` when the session needs them. A search excerpt or a prior-chat summary is not proof of reading.
2. **Record the versions:** the Git commit of this repository (`git rev-parse HEAD`) and the **upstream playbook SHA** recorded in `ADAPTATIONS.md`. Do not silently adopt newer upstream changes. Record an upgrade as a decision.
3. **Determine state:** current phase, **spec approval status**, approval mode (default **Strict**), allowed external writes, dependencies approved, and release authority. The spec in `docs/SPEC.md` is the only source for these.
4. **Verify capabilities that actually exist** in this environment: repository access, build/test tools, Git/PR operations, subagents, model selection, reasoning effort and review tools. Choose a compliant plan under §9.
5. **Give a concise preflight report:** repository and commit, files actually read, phase/step, approval mode, proposed model/effort/delegation where controllable, verified capabilities, blockers. Say clearly **READY** or **BLOCKED** for the requested action.

If a required file cannot be read, a source conflicts, an action is outside authorized scope, or a mandatory gate is impossible, **stop the affected action** and report the exact issue. Never fake compliance. Higher-priority platform/safety constraints and explicit owner permissions still apply.

Persist critical decisions in `docs/SPEC.md` §7 (decision log) and the session summary in `memory.md` (§13). This repository must never contain secrets, tokens or personal usage data.

---

## 1. Principles

1. **Written source of truth.** Product spec, applicable rules and decisions live in the target repo. A material decision in chat is written into the target's `docs/SPEC.md` decision log when editing is authorized (otherwise reported for recording).
2. **Small verified steps.** Each step is buildable, testable and reviewable on its own branch when Git operations are authorized.
3. **Owner checks behavior.** Every UI step ends with a preview and clear checks on the real test device; an agent may not claim to have used a device it cannot access.
4. **Honest reporting.** Failing checks and skipped work are named. "Done" means verified to the stated quality gates, not merely code written.
5. **Cold start each session and step.** Re-read the pinned playbook and applicable target spec; follow §0.
6. **Minimal sufficient capability.** Use only helpful subagents, the least expensive adequate models and appropriate effort without weakening verification (§9).

---

## 2. Phase 1 — Specification (no code yet)

1. **Interview.** Inspect existing target materials first. Ask the owner grouped questions (up to four per round when structured questioning is supported), with a recommended option where useful. Resolve material ambiguities before implementation, not trivial matters that the spec already answers.
2. **Check reality.** Read real external schema/API information where authorized; copy names and option labels **exactly** and identify fields that may be written.
3. **Write/update the target's `docs/SPEC.md`**, following `docs/workflow/SPEC.template.md` or an existing equivalent. Required: goal, data, build order, decision log, workflow settings (including the upstream playbook SHA). Do not overwrite a valid existing spec; reconcile it.
4. **List owner-only setup** (accounts, secrets, connectors, device installs) in the target's chosen tracking document or hand-off, without pasting secret values.
5. **Record project settings** (approval mode, devices, commands, scope, dependencies and release authority) in the target's spec/instruction file.
6. **Owner approves the spec** explicitly. Record approval. **No implementation code before approval**, unless the owner expressly authorizes a smaller diagnostic-only task that does not implement the product.

Later material spec changes: a separate `SPEC:` commit where Git is available, plus a decision-log entry. Resume implementation only after the change is approved as appropriate.

---

## 3. Approval modes

Select and record the mode in the **target** project's written settings; default **Strict**. Every mode requires owner approval for production merge unless a written target-project delegation specifies approver, date, scope and conditions.

| Mode | Agent may start the next step when… | Owner reviews |
|---|---|---|
| **Strict** | Owner explicitly approved the completed current step | Every step on the preview |
| **Milestone** | Current step passed mandatory automated gates and falls inside the current approved milestone | At each milestone |
| **Autonomous** | Current step passed mandatory automated gates and remains within approved spec/scope | At end of build order, or when blocked |

In **all modes**, stop when reality contradicts the spec, a new unapproved dependency is needed, an unaddressed material decision appears, or a failure cannot be fixed without changing scope. "Autonomous" never means unrestricted access, automatic Astra use or bypassing quality gates.

---

## 4. Phase 2 — Implementation step loop

For each authorized build-order step:

1. **Re-read and preflight (§0).** Read this workflow, `memory.md`, the relevant spec section. This project has no design system in v1 (D-002).
2. **Clarify** remaining material uncertainty before coding.
3. **Branch.** Where Git write tools are authorized, use one branch per step; split larger steps into independently verifiable parts. If branching/PR tools are unavailable, state the deviation and follow the owner-approved alternative; do not claim branches/PRs exist.
4. **Build** in this order when appropriate: data layer → pure logic (no I/O, unit-tested) → UI rendering view models. Every displayed business rule/derived number must have meaningful tests. Adapt the order explicitly when the architecture requires it.
5. **Verify** (§6 gate 1; other gates as applicable). Fix and retest.
6. **Push and create a draft PR** when authorized and supported; configure actual monitoring only if available (§8).
7. **Hand-off** (§5), with evidence and a clear indication of incomplete gates.
8. **Wait** as the active approval mode requires. Owner feedback leads to changes on the same branch and a fresh hand-off.
9. **Merge** only when all applicable gates and release permissions are satisfied. Then begin the next step.

---

## 5. Hand-off at the end of every step

Always provide five parts, in this order:

```text
Step N — <name> · Preview: <verified URL, or "not available">

What to check (on <device>, or "owner verification pending")
1. <action> → <expected outcome>
2. ...

Not in this step
- <expected feature deferred to Step M>

Checks
- lint: PASS/FAIL/NOT RUN with evidence
- build: PASS/FAIL/NOT RUN with evidence
- tests: PASS/FAIL/NOT RUN with counts/evidence
- extra QA gates and review: PASS/PENDING/BLOCKED
- playbook: <SHA>; models/effort/subagents used where observable

Open questions / next
- <decision needed, with recommendation>
- Next: Step N+1 — <name>; permitted only after <approval condition>
```

Use plain-language device checklists; one action per line. Never invent a preview URL, test count, model choice, review or device result.

---

## 6. Quality gates

| # | Gate | When | Required |
|---|---|---|---|
| 1 | Lint + build + unit tests pass | Before every push / step completion | Always where applicable; otherwise explain and approve the equivalent |
| 2 | Real-data run using representative **authorized** real inputs; safe redacted/synthetic test fixtures for regression tests | Before approving a step that handles external data | Always for external-data features |
| 3 | **Independent** code review by an agent/person **who did not write the code**; fixes in separate `Fixes from the code review` commit where Git is available | Before production | Always |
| 4 | Final QA walkthrough of supported screens on the real test device; fixes in `Fixes from the final QA` commit where available | Before production | Always for device/UI apps |
| 5 | Visual check with screenshots of each screen/theme/size specified for the target | Before production; also UI-heavy steps | Always for UI apps |

Every fixed bug gets a regression test when technically feasible. Do not store production secrets, access tokens or personal data unnecessarily in fixtures, artifacts, screenshots or logs. Redact sensitive samples; get permission before running actions against real data.

If the agent cannot use the device or browser, the **owner** must conduct the required checks and report evidence. The gate remains PENDING until verified; do not claim completion. If tests/tools are missing, report the exact missing verification and seek an authorized equivalent, never silently mark PASS. An agent cannot independently review its own implementation just by adopting a different persona.

---

## 7. Safety and scope

- Never commit or print secrets. Only harmless variable names go into `.env.example`; never client-expose server secrets.
- Use exact external field and option names from the approved spec. Unexpected schema → stop and report; don't guess or rename.
- Write only to exact approved external resources/fields. No other create/update/delete operations.
- Paginate external queries when supported/needed; handle rate limits and incomplete results honestly.
- No invented product data outside labeled test fixtures.
- Ask before adding any dependency not approved in the target's project settings; record who approved it and when. Lint/test development tooling exceptions require a documented reason and remain subject to security rules.
- Do not change other repositories or production services merely because they are visible or connected.
- Never relax host permissions, security rules, tests or user approval gates to make a check appear green.

---

## 8. Git and PR conventions

- Branch per step: `step-<n>-<short-name>`; docs: `docs/<name>`; spec commits begin with `SPEC:`.
- Commit messages name the step and user-visible change.
- PR titles describe user-visible behavior. Every pushed step branch gets a **draft PR** where supported/authorized.
- **PR monitoring is conditional on actual capabilities.** If an ongoing subscription, automation or active session can monitor a PR, configure it with consent and explain its scope. Otherwise report that the PR is **not actively monitored**, and do not claim to work between sessions.
- Resolve CI failures and review comments before merge, rerunning appropriate checks; never disable or skip tests merely to obtain green status.
- Do not rewrite history on branches where someone else works.

---

## 9. Subagents, models, reasoning, and budget

### 9.1 Provider-neutral model selection

Choose the **lowest-cost model and lowest supported reasoning effort that can reliably achieve the required outcome**, considering complexity, ambiguity, risk and available verification. Correctness and safety are non-negotiable.

| Work / capability tier | Claude examples | Codex / ChatGPT examples | Default effort |
|---|---|---|---|
| Read-only file discovery, simple extraction, low-risk mechanical work | Haiku | Luna | Low |
| Scoped analysis, UI inspection, straightforward edits with good tests | Sonnet | Terra | Medium |
| Architecture, implementation, integration, difficult debugging and substantive verification | Opus (or Fable if available) | Sol | Medium; High where justified |
| Exceptional unresolved or consequential problem | Strongest separately approved provider configuration | **Astra: restricted, explicit approval** | Up to High unless separate policy change |

These examples are **not cross-provider performance equivalences** or guaranteed selectable models. Resolve actual model identifiers against the active provider/session; never invent or misreport the model used. A small well-tested code change may use a lighter model; critical research may require a stronger one.

**Project overlay (owner policy for this repository) [ADAPTED — ADAPTATIONS.md A-2].** The table above is the default. For this project the owner sets these stricter assignments. They narrow the table; they never widen it:

- **Research, read-only discovery, light or low-risk tasks:** Sonnet or Haiku (Codex: Terra or Luna).
- **Building, implementing, verifying code and the app, and fixing the errors found:** Opus, or Fable if available (Codex: Sol). Anything that needs precision.
- **Maximum effort: High.** Never Max, XHigh or Ultra (§9.2).
- **Independent subagents run in parallel in the same turn** when their tasks do not depend on each other (§9.4).
- Astra stays restricted (§9.3).

### 9.2 Reasoning effort

- **Low:** bounded extraction, navigation, simple mechanical tasks.
- **Medium:** normal implementation, bounded changes, structured analysis.
- **High:** difficult debugging, architecture/security decisions, complex review or genuine ambiguity.

**High is the maximum allowed effort by default.** Never use Extra High, XHigh, Max or a higher equivalent without an explicit policy exception from the owner. Lower the effort when the task does not justify it. If an agent lacks effort controls, report that limitation, and do not claim a chosen level was enforced.

### 9.3 Astra — restricted use

Astra is **not** a default main-agent model and **must not** be automatically spawned as a subagent. It may be proposed only for:
1. A substantial issue that Sol at High has not adequately resolved; or
2. An exceptional, high-impact technical decision where stronger reasoning is justified *in advance*.

Before **each invocation**, explain the exact problem, why Sol/standard tier is insufficient, the **single bounded task**, and the expected benefit. Get **explicit owner approval for that invocation**. An approval does not persist to other tasks, subagents, sessions or projects. Never silently escalate to Astra, even if a provider offers it as a fallback. Apply similarly strict consent to another provider's unusually high-consumption restricted model if one is designated.

If the host selected Astra before this playbook was adopted and the agent cannot change the main model, **report the actual model and restriction**; do not pretend the policy changed the host selector. The owner decides whether to continue in that environment.

### 9.4 When and how to delegate

Use subagents **only when they pay off**: independent parallel work, large read-only sweeps, isolated context or independent review. A simple, dependent or single-file task normally stays with the main agent.

- Main agent retains responsibility for planning, coordination, integration, validation and owner approvals.
- Prefer **no more than three concurrent subagents** unless clear measured benefit justifies more.
- Parallelize independent tasks in the same turn when supported; sequence dependent tasks.
- Each delegation defines objective, relevant files, allowed operations, constraints, output format and completion criteria.
- Avoid parallel writes to the same files/shared mutable state without isolated worktrees and coordination.
- Wait for the needed subagent results; verify and integrate rather than blindly accepting summaries.
- All delegated agents inherit the **same applicable safety and scope constraints**; delegation cannot bypass approvals, model limits or testing gates.
- Subagents add context isolation but also consume additional usage; count that cost in the decision.

### 9.5 Independent review, availability and transparency

The gate-3 reviewer **must not be the implementer**. Use an independent subagent, separate agent/session with genuinely separate implementation responsibility, or a human reviewer. For high-risk code, favor a strong model at High. A self-review may be useful but **does not count** as the mandatory independent review.

Check actual model/effort/tool availability. Use approved equivalent tiers when the preferred one is missing and explain any material substitution; never silently substitute restricted or higher-consumption models. If no compliant option exists, mark the affected work BLOCKED. In hand-off, report the main agent and subagents/models/efforts **where observable**, and identify every unknown rather than inventing details.

---

## 10. Default stack (only when the target spec does not say otherwise)

- **Framework:** Next.js App Router, TypeScript strict.
- **Hosting:** Vercel preview deployments per branch; production `main` by default.
- **Source control:** target application repository on GitHub.
- **Agent environment:** any compatible local/cloud coding environment with the needed permissions; not necessarily Claude Code.
- **Testing:** Vitest for pure logic; adapt to project requirements.
- **Commands:** `npm run lint`, `npm run build`, `npm test`, `npm run dev` only when actually defined in the target.
- **Secrets:** environment/secret-manager storage, server-only use; never client-expose sensitive values.

The app's current approved stack outranks these **defaults**. Never install Next.js, Vercel or a test framework in an existing project just because this default is listed.

---

## 11. Communication

- Docs and code in English; reply in the owner's language. **[ADAPTED — ADAPTATIONS.md A-7.]** Write all English text for the owner in ASD-STE100 Simplified Technical English. Follow it fully and always (D-029). Keep technical precision and necessary detail.
- Owner-facing checklists and setup steps in simple language.
- Ask material clarification questions early with a recommended option when useful.
- Keep progress updates concise; durable records belong in target commits, PRs and spec/decision log.
- State what was **not** done, why, and which gates remain pending.

### 11.1 Visual explanations for coding work

Use visuals **frequently and proactively when they improve understanding**, especially for non-trivial software engineering. For complex architecture, component boundaries, dependencies, data movement, algorithms, state transitions, asynchronous execution, debugging, and before/after behavior, prefer to include a compact, accurate diagram alongside the explanation rather than relying on prose alone. Use the simplest sufficient form: Mermaid flowcharts, sequence diagrams or state diagrams in Markdown when supported; otherwise a readable text/ASCII diagram, table or annotated code excerpt. Explain key decisions and trade-offs in text as well.

- Before implementation, a visual can make the proposed design, interfaces, risks and sequence of work reviewable. During debugging, show the observed path, expected path and failure point where useful. At hand-off, summarize important changed flows or architecture visually when the step is complex.
- Base diagrams on actual inspected code, verified behavior or explicitly labeled proposals. Distinguish current state from planned state, and never invent integrations, dependencies, test results or runtime behavior.
- A small self-evident code edit, simple fix or routine status update does not need a diagram. Avoid repeated, decorative or low-information visuals; do not let presentation delay coding, review, testing or the mandatory hand-off.
- Consider interactive HTML explanations or small executable visualizations when static diagrams are insufficient and interaction materially helps inspect complex behavior. Build them only within the authorized scope and supported environment; do not add application features, persistent project files, dependencies, external API calls or expense merely to explain work without the required permission.
- Explainer videos and elaborate visual artifacts are opt-in: create them only when the owner explicitly requests them. No visual replaces source code, tests, logs, evidence, required reports, approval gates or accessibility checks.

---

## 12. Compliance and cross-session continuity

At every session start and step, execute §0 and report **READY or BLOCKED**. Before each hand-off, re-check:
- Correct repository and commit; upstream playbook SHA unchanged or upgrade recorded; required files genuinely read.
- Authorized spec, step, approval mode and external write scope.
- Actual models/efforts/subagents; Astra not used without task-specific permission.
- Tests, previews, review and device evidence, distinguishing PASS / FAIL / NOT RUN / PENDING.
- Git/PR/monitoring and production permissions, without invented background capabilities.

Use real tool results, logs, review records and explicit owner confirmation as evidence; words in a prompt are not proof that any check ran. When transitioning to a new chat, session or workspace, carry forward the hand-off summary through `memory.md`, **then re-read the source files**. Memory is a pointer to the truth, not the truth.

Instruction files express intended behavior but cannot enforce access controls or model selection in a host that offers no such controls. For stronger guarantees use host permissions, actual agent configuration, hooks where supported, CI checks, branch protection and owner approvals. Never represent a natural-language instruction as a technical enforcement guarantee.

---

## 13. Multi-session continuity and automation

**[ADDED — ADAPTATIONS.md A-3.]** This project is built in many sessions, some automated. These rules keep them consistent.

1. **`memory.md` is the cross-session memory.** Read it at the start of every session. Update it before the session ends: state, decisions made (with a pointer to `docs/SPEC.md` §7), open questions, and the next action. Keep it short. Do not store secrets or personal data.
2. **Truth hierarchy** when sources disagree: owner's latest explicit instruction → `docs/SPEC.md` (approved) → this workflow → `memory.md` → chat history. Report the conflict; do not choose silently.
3. **One session, one stated goal** (see `docs/SESSIONS.md`). A session does not start work that belongs to a later session unless the owner says so.
4. **Automated or unattended sessions** may run only inside an approved mode (§3) and an approved scope. When blocked or unsure, an unattended session writes the question to `docs/OPEN-QUESTIONS.md` and `memory.md`, then stops the affected work. It never guesses on a material decision, never widens its own permissions, and never changes the approval mode.
5. **Usage limits apply to the agents building this project.** Once the tool exists, the build sessions should use its own guidance (dogfooding). Until then, stop at a clean checkpoint when the provider signals a limit, and record the exact resume point in `memory.md`.
6. **Session end checklist:** work committed on the session branch; `memory.md` updated; open questions recorded; hand-off (§5) given if a step ran.
