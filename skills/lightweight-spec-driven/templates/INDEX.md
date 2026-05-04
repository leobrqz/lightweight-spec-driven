# Tasks

[One-sentence description of what this project is and what is tracked here.]

## Folders

| Folder | Purpose |
|---|---|
| `active/` | Work in progress. Keep this small — one or two items at a time. |
| `backlog/` | Ideas, designs, specs not yet started. Moves to `active/` when work begins. |
| `closed/` | Completed work. Designs, plans, issues — anything finished moves here. |
| `changelogs/` | Daily changelogs. Active file lives at the root. At month end, past files move into `{mon}/` subfolders (e.g. `changelogs/may/`). |

## Filename prefixes

Prefixes apply to `.md` specs in `active/`, `backlog/`, and `closed/` only. Changelog files use `CHANGELOG_` naming (see below).

| Prefix | Use for |
|---|---|
| `IDEA_` | Rough capture of a future direction; may be incomplete. Often becomes a design or gets discarded. |
| `DESIGN_` | Feature or technical design — decisions, behavior, flows, data shape. Minimal code snippets only to clarify an interface. |
| `PLAN_` | Step-by-step implementation plan derived from a design. Meant to be executed as-is. |
| `ISSUE_` | Bug investigation, incident, or known problem. Symptoms, root cause when known, fix scope. |

## Relationships

Ideas become designs; designs become plans; plans get implemented. Issues may spawn a plan or a small design. Nothing moves backward — if something in `active/` stalls indefinitely, move it back to `backlog/`.

## Required frontmatter

Every prefixed spec must open with:

```
---
date: YYYY-MM-DD
status: [see below]
description: One-line summary of the task.
---
```

| Status | Meaning |
|---|---|
| `draft` | Overall direction captured; nothing specified or fully defined yet. |
| `ready` | Fully specified; ready to proceed or implement. |
| `in_progress` | Currently being worked on — implementing, or investigating if it is an issue. |
| `blocked` | Progress halted by an external dependency; the blocker must be named in the document. |
| `done` | Finished — implemented, or investigated and resolved. |
| `cancelled` | Abandoned at user request. |

## Changelogs

**Date format: DDMM** (e.g. `CHANGELOG_0305.md` = 3 May). The active changelog lives directly in `changelogs/` root. When a month ends, its files move into a `{mon}/` subfolder (e.g. `changelogs/may/`).

Each file opens with a heading:

```markdown
# Changelog — YYYY-MM-DD
```

Entries are grouped by feature area using `##` headings. Each bullet describes a user-visible or system-level change. No implementation dumps — keep it readable by a non-engineer.

**New root changelog template:**

```markdown
# Changelog — YYYY-MM-DD

## [Feature or area]

- [What changed and why it matters]
```
