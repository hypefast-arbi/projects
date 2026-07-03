---
name: hypefast-repos
description: The concrete git repos in Arbi's Hypefast workspace — local clone paths, remotes, default branches, and per-repo git workflow (direct-to-main vs. feature-branch+PR). Use whenever a pm-workflow skill (e.g. product-spec-sync) needs to resolve a Hypefast repo location or git conventions instead of asking or guessing. Account-specific — ignore for any other organization.
---

# Hypefast Repositories

Concrete repo locations and git conventions for Arbi's Hypefast workspace, so
pm-workflow skills can resolve them instead of running discovery or asking.

> Scope: account-specific template data, not a workflow. Applies only when
> working in Arbi's `D:\Code\hypefast\` workspace.

| Repo | Remote | Typical local path | Default branch | Git workflow |
| --- | --- | --- | --- | --- |
| product-knowledge | `github.com/hypefast-arbi/product-knowledge` | `D:\Code\hypefast\product-knowledge` | `main` | Commit + push straight to `main` (no PR) |
| projects | `github.com/hypefast-arbi/projects` | `D:\Code\hypefast\projects` | `main` | Commit + push straight to `main` (no PR) |
| product-spec | `bitbucket.org/hypefast-tech/product-spec` (note: Bitbucket, not GitHub, and a different org than the two repos above) | `D:\Code\hypefast\product-spec` | `main` | **Feature branch + PR required for every change** — see its own `CONTRIBUTING.md`. Never commit to `main` directly. Consumed by `product-spec-sync`. |

Even with a path/remote resolved here, **always push-confirm with the user before
running `git push`** — this table records *where things are*, not standing
permission to push. product-spec additionally requires opening a PR, never a
direct push to `main`, regardless of this note.

If in doubt, re-verify with `git remote -v` in the local clone rather than trusting
this table blindly — it can drift out of date.

If a path or remote here turns out stale (repo moved, renamed, or a new one
started), update this table rather than letting skills re-discover it every run.
