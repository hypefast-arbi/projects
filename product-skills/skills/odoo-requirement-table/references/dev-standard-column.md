# Sub-skill: Dev/Standard Solution column

Fill the **Dev/Standard Solution** column of the Description of the User Flows table. The column answers one question for planning: *does this story need development, or does standard Odoo (installed apps + configuration) cover it?*

## Allowed values — exactly one per row

- `Standard` — standard Odoo covers it: out-of-the-box behavior OR configuration done in the UI without code (approval categories/sequences, security groups, payment terms, valuation settings, routes).
- `Development` — the story cannot ship without building something: custom fields/views/models, record rules, workflow changes, custom reports, or any external integration/connector.
- `Standard + Development` — the core mechanism is standard but a required part of the story needs building (e.g. standard MO cost analysis + a stored variance field; standard backorder behavior + an integration trigger).

Keep the cell to the bare value — no notes, no parentheses. Nuance stays in the Description's implementation flag.

## Derive it from the Description flag — don't re-reason

The Description column already carries an implementation flag per row (see SKILL.md Classification). Map mechanically:

| Description flag | Dev/Standard Solution |
| --- | --- |
| `[Standard]` | Standard |
| `[Config]` | Standard |
| `[Custom]` | Development |
| `[Integration]` | Development |
| Mixed flag (e.g. `[Standard; Custom for X]`, `[Config; Custom if Y]`, `[Standard behavior + Integration trigger]`) | Standard + Development |

Rules:

- **Config counts as Standard.** No code means no development, even when setup effort is real.
- **Record rules and scheduled actions with code count as Development** — they are authored in the UI but they are logic someone must write, test, and maintain.
- **Uncertain between Standard and Development → `Standard + Development`**, never an optimistic `Standard`. The column sets effort expectations; under-calling dev is the expensive mistake.
- **Consistency check:** if the value you want contradicts the row's Description flag, fix the flag (or the value) — the two must tell the same story.
