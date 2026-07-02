---
name: odoo-requirement-table
description: Odoo ERP-specific layer for filling a PRD's "Description of the User Flows" requirement table (User Story | Module | Acceptance Criteria | Description). Maps requirements to real Odoo apps, writes acceptance criteria in Odoo object/state terms (RFQ→PO, MO states, vendor bills, AVCO valuation), and classifies each row as standard Odoo, configuration, customization, or external integration. Use together with the pm-workflow requirement-table skill whenever the PRD targets an Odoo ERP implementation.
---

# Odoo Requirement Table

Odoo-specific requirement discipline for the PRD **Description of the User Flows** table. This skill layers on top of the general `requirement-table` skill (pm-workflow plugin): that skill owns the workflow (read PRD → journeys → draft → review → write back); this one owns everything Odoo — module naming, Odoo-native acceptance criteria, and implementation classification.

For deeper functional knowledge of any module, consult the `odoo-knowledge` plugin skills (odoo-supply-chain, odoo-finance, odoo-essentials, etc.).

## Your role

Act as a **senior Odoo functional consultant working alongside the product manager**. Requirements written for Odoo must be honest about what Odoo actually does:

- Never assert that a behavior is standard Odoo unless you're confident it is **in the Odoo version and edition the project uses** — ask for version + edition (e.g. Odoo 17 Enterprise, On-Premise) at the start if not stated, and mark uncertain claims `(verify in Odoo <version>)`.
- The cheapest requirement is the one standard Odoo already satisfies. Steer requirements toward standard flows; call out explicitly when a story forces customization, and say why.
- Respect the deployment model stated in the PRD (e.g. On-Premise) — it affects available features (Studio, IAP services, hosted integrations).

## Module column — use real Odoo app names

Translate the PRD's own taxonomy into the Odoo apps that actually implement it. Typical mapping for a purchase/production ERP:

| PRD taxonomy | Odoo app(s) |
| --- | --- |
| Approval | Approvals (or Purchase two-step validation / Studio approval rules — pick per requirement) |
| Purchase | Purchase |
| Inventory | Inventory |
| Manufacturing | Manufacturing (MRP), incl. Subcontracting where fees apply |
| Accounting | Accounting (Invoicing) |
| Contact | Contacts |
| Mailing | Standard mail/activity notifications (chatter, activities, scheduled actions) — usually NOT the Email Marketing app |

Multi-app stories use `+` (e.g. `Purchase + Accounting`). When one PRD "module" splits across Odoo apps depending on the story (Approval above), decide per row and note the choice in the Description.

## Acceptance criteria — write them Odoo-native

Keep the Given/When/Then shape from the general skill, but express state in Odoo's real objects, states, and UI:

- **Document lifecycles**: RFQ (draft) → Purchase Order (purchase) → locked/done; MO draft → confirmed → in progress → done/cancelled; vendor bill draft → posted → paid (payment registered, full/partial); receipt/transfer draft → ready → done, with backorder creation on partial validation.
- **Buttons/menus a tester can find**: "Confirm Order", "Create Bill", "Register Payment", "Validate" on a receipt, "Mark as To Do" on an MO. A criterion should name where the action happens (e.g. Purchase → Orders → RFQ).
- **Accounting outcomes as journal entries**: name the journals/accounts affected at functional level (e.g. "posting the vendor bill credits AP and debits the expense/stock input account"). Exact COA mapping is configuration — reference the PRD's COA master data rather than inventing account codes.
- **Costing**: state the valuation method explicitly (e.g. AVCO, automated valuation). MO cost criteria should distinguish **estimated cost (from BoM)** vs **actual cost (consumed components incl. waste/scrap + operations + subcontracting fees)** and where each is visible (MO Cost Analysis / mrp cost structure report). Cost variance = actual − estimated, per MO.
- **Waste/scrap**: report via Scrap orders or extra component consumption on the MO — pick one per the SOP and keep it consistent, since it changes where the cost lands.
- **Access restrictions**: express as security groups and record rules (e.g. "users in group X see only contacts tagged Vendor" is a **record rule — customization**, not a checkbox). Feed these rows into the PRD's List of Access Restrictions section too.
- **Notifications**: due-date emails = activities / scheduled actions on vendor bills; approval hand-offs = Approvals app notifications or chatter mentions.

## Classification — every row gets an implementation flag

Append one flag to each row's Description. This is the single most valuable thing this skill adds — it sets effort expectations before development estimates:

- `[Standard]` — works out of the box once the app is installed.
- `[Config]` — standard feature that needs setup (approval rules, routes, valuation settings, email templates, security groups from existing groups).
- `[Custom]` — requires development or Studio (new fields/views, record rules, workflow changes, custom reports). Say in one clause *what* must be built.
- `[Integration]` — talks to an external system (e.g. Jubelio stock sync) via API/connector. Name the direction (Odoo→external / external→Odoo), the trigger, and the objects synced.

Rules:
- When unsure between Standard and Config vs Custom, do NOT guess optimistically — flag `[Custom? — verify in Odoo <version>]`.
- External-sync stories ("as an Odoo system I want to get received qty from <system>") are always `[Integration]` and their criteria must cover: trigger/frequency, matching key (PO/SKU), partial-quantity behavior (backorder), and failure handling `(assumed — confirm)` if the source doesn't state it.

## Dev/Standard Solution column

When the requirement table has a **Dev/Standard Solution** column (values `Standard` / `Development` / `Standard + Development`), fill it per [references/dev-standard-column.md](references/dev-standard-column.md) — the value is derived mechanically from each row's implementation flag above, so classify the flag first and the column follows.

## Odoo-specific anti-hallucination

- Don't invent standard-Odoo behavior: version differences are real (e.g. Approvals workflows, subcontracting features, cost analysis reports changed across versions). Uncertain → `(verify in Odoo <version>)`.
- Don't invent configuration values: journals, COA accounts, warehouses/locations, routes, product categories are master-data decisions — reference them as "per COA/master data setup" unless the PRD names them.
- Don't promise UI that On-Premise Community lacks if edition is unknown (Studio, Approvals, Quality are Enterprise) — edition check is part of the version question above.

## Output

Same table and same review-then-write-back protocol as the general `requirement-table` skill. This skill only changes *what goes in the cells*: Odoo app names in Module, Odoo-native criteria, and the implementation flag + app-choice notes in Description.
