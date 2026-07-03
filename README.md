# projects

Complementary **project knowledge** for Hypefast PM work — context about the projects themselves, consumed alongside the workflow skills in [`hypefast-arbi/product-knowledge`](https://github.com/hypefast-arbi/product-knowledge).

## How this repo is meant to be used

- **Knowledge, not workflow.** This repo tells Claude (and teammates) what a project *is* — its domain, platform, modules, integrations, and project-specific requirement conventions. It does not define how PM deliverables get produced.
- **The "how" lives in product-knowledge.** Creating PRDs, filling requirement tables, writing success criteria, filing backlog tickets — all of that is the `pm-workflow` plugin in product-knowledge. When any of those skills runs against a specific project, it pulls context **from here** first.
- **Layer skills, not replacements.** Skills in this repo that look like workflow (e.g. `odoo-requirement-table`) are platform-specific *layers*: they plug project/platform knowledge into a pm-workflow skill and never replace or bypass it.
- **Rule for Claude:** treat this repo strictly as supplementary context to know more about a project. Never drive a deliverable from this repo alone — start from the matching pm-workflow skill and enrich it with the knowledge here.

## Layout

One folder per project/domain, each an installable Claude plugin (`.claude-plugin/plugin.json` + `skills/`):

| Folder | Plugin | What it knows |
|---|---|---|
| `odoo-erp/` | odoo-knowledge | Odoo functional knowledge — Essentials, Finance, HR, Productivity, Sales, Services, Supply Chain |
| `product-skills/` | product-skills | Odoo-specific requirement-table layer: module mapping, Odoo-native acceptance criteria, standard/config/custom/integration flags |
| `hypefast-workspace/` | hypefast-workspace | Hypefast account template — concrete Atlassian cloudId/site, Confluence spaces, Jira projects/boards, PRD-template source, known people (`hypefast-atlassian`), and repo locations/git conventions (`hypefast-repos`). The pm-workflow skills read this to skip runtime discovery on the `hypefast-it` account |

Add a new folder as each new project starts, following the same plugin structure.

**Account templates live here, not in product-knowledge.** The pm-workflow skills
are organization-agnostic and hardcode no site, cloudId, space, or project. The
concrete values for a given Atlassian account belong in an account-template plugin
here (like `hypefast-workspace`); a teammate on a different account just installs
their own template — or lets the skills discover/ask at runtime.

## Install

```bash
# add this repo as a marketplace (once per machine)
claude plugin marketplace add hypefast-arbi/projects

# install the knowledge you need
claude plugin install odoo-knowledge@projects
claude plugin install product-skills@projects
claude plugin install hypefast-workspace@projects   # Hypefast account template
```

To update later: `claude plugin marketplace update projects`.
