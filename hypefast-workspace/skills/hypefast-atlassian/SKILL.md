---
name: hypefast-atlassian
description: The concrete Atlassian workspace values for the Hypefast (hypefast-it) account — cloudId, site, Confluence spaces, Jira projects/boards, the PRD-template source page, and known people. Use these as the ready-made config whenever a pm-workflow skill (confluence-create-page, jira-create-ticket, requirement-table, success-criteria) runs against the Hypefast account, so you skip runtime discovery. This is account-specific template data — ignore it for any other organization's Atlassian account.
---

# Hypefast Atlassian Workspace

Concrete, ready-to-use configuration for Hypefast's Atlassian workspace. The
pm-workflow skills in the `product-knowledge` repo are **organization-agnostic** —
they hardcode nothing. When working on the Hypefast (`hypefast-it`) account, use
the values below directly instead of running discovery (`getAccessibleAtlassianResources`,
`getConfluenceSpaces`, `getVisibleJiraProjects`).

> Scope: this is *account template* knowledge, not a workflow. It only applies to
> the Hypefast account. If the current work targets a different Atlassian
> organization, ignore this file and let the pm-workflow skill discover or ask.

## Atlassian account

| Value | |
| --- | --- |
| **Site** | `hypefast-it.atlassian.net` |
| **cloudId** | `a6661705-a333-4449-8206-6a19abf3d70f` |

If the cloudId ever fails, re-discover it with `getAccessibleAtlassianResources`
and update this file.

## Confluence

| Space key | Name | spaceId |
| --- | --- | --- |
| `TECHPRODUC` | Tech-Product | `33166` |

- **PRD template source page:** "Project Documentation and Requirements Overview"
  — space `TECHPRODUC`, page `1554907138`. This is the canonical Hypefast PRD
  template captured into `confluence-create-page/prd-template.html`. If it
  changes, re-fetch with `getConfluencePage(cloudId, pageId, contentFormat:"html")`
  and overwrite that template file.
- **Wiki base URL:** `https://hypefast-it.atlassian.net/wiki` (prefix for
  `_links.webui` values).

## Jira

| Project key | Name | Board |
| --- | --- | --- |
| `ERP` | ERP (Odoo, Hypefast/Bohopanna) | https://hypefast-it.atlassian.net/jira/software/c/projects/ERP/boards/98 |
| `ASP` | AI Stuff Playground | https://hypefast-it.atlassian.net/jira/software/projects/ASP/boards/97 |

- **Browse URL pattern:** `https://hypefast-it.atlassian.net/browse/<ISSUE-KEY>`.
- `ERP` is company-managed / classic; `ASP` is team-managed. Both link a
  Story/Task to its Epic via the system `parent` field
  (`{"parent": {"key": "<EPIC-KEY>"}}`).
- For any other project, resolve with `getVisibleJiraProjects(searchString:...)`
  and confirm the issue type exists in its `issueTypes`.

## People

| Name | Role | accountId |
| --- | --- | --- |
| Arbi Ramadhan | Account owner / PM | `712020:7608e2cc-3fca-42ce-afac-619fcaa12f9f` |

Resolve any other name → accountId with `lookupJiraAccountId`.

## Hypefast PRD-template quirks

The Hypefast PRD template (above) has traits the pm-workflow skills should expect
on this account:

- The **OBJECTIVE** goal statement is rendered as **images**, unreadable via the
  Confluence API. Fall back to the readable **Problem Statement** subsection just
  below it, and confirm with the user before relying on it.
- Work from the PRD **page text only** — never fetch the linked SOP Google
  Docs/Sheets or embedded images; ask the user to paste any needed detail.
