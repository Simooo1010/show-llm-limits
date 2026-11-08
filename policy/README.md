# policy/

Future home of the **agent behavior policy**: the rules that turn limit data into guidance and actions for agents (for example "stop at 95% used", "resume after the window resets", "split work into steps").

Planned layout (proposal — not final until the spec is approved):
- `default.yaml` — policy shipped with the tool. Written from `research/R6-behavior-catalogue.md`.
- The **user's edits live outside this repository**, in the user's own config/data directory, so updates to the default never overwrite them.
- A schema and validation rules (Step 2 of the build order).

Principles already decided by the owner: **every behavior is user-editable**, and the policy considers both the provider limits and the user's wishes.

Status: **empty — no policy exists yet.** See `docs/SPEC.md` §3.2 for the illustrative shape.
