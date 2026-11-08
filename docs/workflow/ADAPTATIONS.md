# ADAPTATIONS — local changes to the embedded playbook

Upstream: `https://github.com/Simooo1010/claude-project-playbook`, branch `main`.
**Pinned upstream commit:** `e9abfefc9fa120504918e464f4bf2a7dcadf0570` (read in full on 2026-11-08).

Rules for this file:
- Every change from upstream goes here with a reason. A change without an entry here is a defect.
- Do not pull newer upstream changes silently. To upgrade: read the upstream diff, record a decision in `docs/SPEC.md` §7, update the SHA above, and log each merged change.
- The owner may adapt any rule. An agent may propose a rule change, but only the owner approves it. Record approval in `docs/SPEC.md` §7.

## Files copied (from `docs/` upstream)
| Local file | Upstream file | Status |
|---|---|---|
| `WORKFLOW.md` | `docs/WORKFLOW.md` | Adapted (A-1, A-2, A-3, A-5, A-6) |
| `PROVIDER-ADAPTERS.md` | `docs/PROVIDER-ADAPTERS.md` | Notice only (A-4) |
| `HOW-TO-WORK-WITH-AGENTS.md` | `docs/HOW-TO-WORK-WITH-AGENTS.md` | Notice only (A-4) |
| `SPEC.template.md` | `docs/SPEC.template.md` | Unchanged |

Not copied: upstream `README.md`, `SETUP-FOR-OWNER.md` (replaced by this repository's own), `HOW-TO-WORK-WITH-CLAUDE.md` (legacy redirect), upstream `AGENTS.md`/`CLAUDE.md` (replaced by this repository's own, which add the ASD-STE100-inspired style rule from upstream `AGENTS.md`).

## Changes
| ID | Where | Change | Reason |
|---|---|---|---|
| A-1 | WORKFLOW §0 and §12 | Rewrote activation for embedded mode. The playbook is inside the target, adoption is a standing authorization (D-001). Preflight reads local files and records this repo's commit plus the upstream SHA. | Upstream §0 assumes two separate repositories. Here there is one. |
| A-2 | WORKFLOW §9.1 | Added "Project overlay" paragraph: research/light work on Sonnet/Haiku; build, verify and fix on Opus/Fable; max effort High; parallelize independent subagents. | Owner policy for this project. It narrows the upstream table and does not widen it. |
| A-3 | WORKFLOW §13 (new) | Added multi-session continuity: `memory.md` protocol, truth hierarchy, rules for automated sessions, dogfooding of usage limits, session-end checklist. | The project is built in many linked, partly automated sessions. |
| A-4 | `PROVIDER-ADAPTERS.md`, `HOW-TO-WORK-WITH-AGENTS.md` | Added a notice at the top. No other change. | Mark them as embedded copies. Upstream wording about "external playbook" is kept for easy diffing. |
| A-5 | WORKFLOW §4 step 1 | Replaced "design-system README when UI is touched" with "no design system in v1 (D-002)". | Owner decision: no design work now. |
| A-6 | WORKFLOW §2 step 3 | Template path changed to `docs/workflow/SPEC.template.md`; "playbook SHA" changed to "upstream playbook SHA". | Paths and wording for the embedded layout. |
| A-7 | WORKFLOW §11 and `AGENTS.md` | Changed the style rule from "about 80% of ASD-STE100" to "follow ASD-STE100 fully and always" for all English text for the owner. Code, commands, logs, API names and quotations stay out of scope. | Owner order on 2026-11-08 (D-029). |

## Intentionally unchanged (and why)
- §6 quality gates 4 and 5 (device walkthrough, screenshots) stay. The dashboard is a UI. For v1, "supported screens/themes/sizes" is defined in `docs/SPEC.md`, kept minimal because there is no design phase.
- §9.3 Astra restriction, §9.2 effort cap, §3 approval modes, §7 safety rules.
