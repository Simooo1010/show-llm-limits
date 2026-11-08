# SPEC — <Product name>

<!-- TEMPLATE for the TARGET project, not for this central playbook. Fill all required sections; remove unused placeholders. Number sections so decisions and hand-offs can cite them. -->

## 1. Goal (required)
<Who uses it, on what device, to do what. What "done" looks like for v1.>

## 2. Data (required)
Source: <system, database/API name>.
Names below are copied **exactly** from the live system.

| Property / field | Type | Options (exact labels) | App reads | App writes |
|---|---|---|---|---|
| <Name> | <type> | <A, B, C> | yes | no |

If no external data system exists, say so explicitly.

## 3. <Screens / features — free-form>

## 4. <Edge cases — free-form>

## 5. Build order (required)
Each step must be testable and reviewable.

1. **Step 1 — <name>:** <scope>. Check: <what owner verifies>.
2. **Step 2 — <name>:** …

## 6. Workflow settings (required)
- **Playbook source/ref:** <repository URL and immutable commit SHA actually read>.
- **Approval mode:** <Strict | Milestone | Autonomous>; default Strict.
- **Owner:** <name or role>.
- **Test device(s):** <real devices>.
- **Supported UI:** <themes and sizes>.
- **Allowed external writes:** <exact resources/fields, or none>.
- **Approved dependencies:** <names or separately maintained target-repo file>.
- **Production merge:** owner approval required unless written delegation specifies who, when and conditions.
- **Project commands:** lint: <command>; build: <command>; tests: <command>.
- **Design system:** <location or none>.

## 7. Decision log (required)
| Date | Decision | Decided by | § |
|---|---|---|---|
| <YYYY-MM-DD> | Spec v1 approved; playbook commit <SHA> | <owner> | all |

Record significant changes (including a playbook-version upgrade) before implementing the changed scope.
