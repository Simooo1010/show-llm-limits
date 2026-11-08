# How to work with coding agents — owner's guide (embedded copy)

> **Adapted copy.** Upstream commit `e9abfef…` (see `ADAPTATIONS.md` A-4). In this repository the playbook is **embedded**, so the "external reference" parts below are replaced by the short prompts in `docs/SESSIONS.md`. The rest (spec approval, step loop, review, release, resume) applies unchanged. Where text below says "playbook repository" or "do not copy", read it as: the embedded files in `docs/workflow/`.

Use **one target application repository plus this separate central reference repository**. Do **not** use "Use this template" and do not copy the playbook files into the application repo unless the owner later chooses to. The project spec, approvals, branches and code belong to the target repository; the general method remains here.

## 1. Activate the workflow in a new session

Select/open the application repository, and make `https://github.com/Simooo1010/claude-project-playbook` readable through the connected GitHub tools, an extra repository, or accessible source files. Then send:

> Work on **[TARGET_OWNER/TARGET_REPO]**. Adopt **https://github.com/Simooo1010/claude-project-playbook** as the governing workflow for this project; it is a reference repository, not the working repository. Do not copy its files. Read the full current `AGENTS.md`, `docs/WORKFLOW.md`, `docs/HOW-TO-WORK-WITH-AGENTS.md`, and `docs/PROVIDER-ADAPTERS.md` from the playbook repository. Record its exact commit SHA, then inspect the target repository, its own instruction files, current spec, decisions and open PRs. Execute WORKFLOW §0 startup preflight. Report the target repo, playbook SHA, sources actually read, approval mode, available tools/model/effort controls, and any blockers. Do not code before the applicable spec approval. Follow WORKFLOW in every step, especially §9. Do not auto-use Astra, and never claim compliance without evidence.

The agent should inspect the target's actual situation before asking questions. If a valid approved spec already exists, don't restart discovery unnecessarily: confirm it is current and correctly versioned first. If not, begin the spec interview using a few grouped questions with recommended options where supported, until **material** ambiguities are resolved.

## 2. Review and approve the spec

Read the project-specific `docs/SPEC.md` (or the equivalent agreed artifact). Confirm the build order, data names, allowed writes, test device, dependencies, approval mode and playbook commit. Reply **"Spec approved"** only after checking it. The agent records that decision. If you change direction, say **"Change of plan: …"** and require an updated spec/decision log **before** code.

## 3. During a step

- The agent re-checks the pinned playbook, target instructions and applicable spec section for **every step**.
- It executes only the authorized step and any permitted supporting work, using §9's model/subagent controls.
- It gives a preview and plain-language device checks when UI/device testing is relevant.
- You reply **"OK"** only after the required review in Strict mode; report bugs as "I did X, saw Y, expected Z."
- For Milestone or Autonomous modes, automated gates still apply and production requires separate authorization unless specifically delegated in writing.

**Next step prompt:** "Proceed with Step N in [target repo]. First re-read the pinned playbook and applicable spec; follow WORKFLOW §4, §6, §9 and §12. Provide the five-part hand-off. Respect our active approval mode."

**Review prompt:** "Perform independent review and remaining QA gates for Step N. Name the reviewer, what evidence was checked, what is still pending, and whether release is blocked."

**Release prompt:** "Report every WORKFLOW §6 gate with evidence. Do not merge to production until explicitly authorized under the target's written rules."

## 4. Resume or move to another chat/project/workspace

> Continue work on **[TARGET_OWNER/TARGET_REPO]** using **https://github.com/Simooo1010/claude-project-playbook** at the **previously recorded playbook commit SHA**. Re-read its mandatory instructions and the target's current spec, state and PRs. Run WORKFLOW §0 preflight. Report the last approved step, pending quality gates, outstanding owner decisions, and the next permissible action. Don't rely on memory of a previous chat as evidence.

A project/workspace may preserve *instructions to consult the playbook* between chats if the platform supports project instructions. It does **not** guarantee current GitHub access, automatic file reading, continuous supervision, persisted subagents, or model selection. Confirm those anew in every session.

When both repositories are selectable, the **target app repo** should be the working repo; this playbook should be a **reference/source**. Additional repository access is not an automatic instruction-loading mechanism. If the agent cannot fetch the full files, stop the affected work and provide the missing files through a supported mechanism.

## 5. Owner-side responsibilities

Supply real examples and exact names for external systems, decide approval boundaries, provide safe secret-management access, test previews on your devices, and explicitly approve production actions as required. Never paste secret values into chat.

The agent must not promise background PR watching, device testing, code review, model switching or usage limits that its actual tools cannot deliver. Ask for evidence.

## 6. Inspect the startup compliance report

The agent must report:
- **Target / playbook:** distinct repositories, immutable playbook SHA, files actually read.
- **Project state:** current step, spec approval, approval mode, relevant branches/PRs.
- **Capabilities:** git, build/test environment, browser/device, subagents, models and effort controls actually available.
- **Model policy:** proposed task-to-model/effort assignments, any substitution; Astra unapproved by default.
- **Decision:** READY for the permitted work or BLOCKED with exact reasons.

The preflight proves the agent located and read the instructions in that session; it is not a security guarantee. Use permissions, CI and protected branches for enforceable controls.
