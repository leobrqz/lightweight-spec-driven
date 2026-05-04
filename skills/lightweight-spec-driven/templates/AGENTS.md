# [Project Name]

# Agent conduct

[Paste the chosen conduct block here — conduct_direct, conduct_concise, or conduct_formal.]

## Read first

- `ARCHITECTURE.md` — technical reference: layout, patterns, methodology, boundaries, stack, entrypoints
- `tasks/INDEX.md` — task tracking structure and conventions


---

## What this project is

[One paragraph describing the project: what it does, who uses it, and its primary modes or surfaces.]

## Tech stack

| Layer | Technology |
|---|---|
| [Frontend / Backend / DB / etc.] | [Name and version if relevant] |

## Sources of truth (read in order)

1. This file
2. `ARCHITECTURE.md`
3. `tasks/INDEX.md`
4. `tasks/active/`
5. `tasks/backlog/`
6. `tasks/closed/`
7. `tasks/changelogs/`

## Spec-driven workflow

Work follows a backlog → active → closed lifecycle. See `tasks/INDEX.md` for folder purposes, filename prefixes, required frontmatter, and changelog rules.

Before starting any non-trivial task:
- Check `tasks/active/` for an existing spec.
- If none exists, create one in `tasks/backlog/` with the appropriate prefix (`DESIGN_`, `PLAN_`, `ISSUE_`, `IDEA_`), move it to `tasks/active/` when work begins.
- Log shipped changes in today's changelog under `tasks/changelogs/`.

## Definition of done

- [ ] Acceptance criteria from the active spec are met.
- [ ] No regressions in adjacent areas.
- [ ] Changelog entry written for anything user-visible or system-critical.
- [ ] Spec moved to `tasks/closed/` with `status: done`.

## Conventions

- [List project-specific conventions here: naming rules, commit message format, deploy commands, etc.]

## Gotchas

[Document repeated mistakes, non-obvious constraints, and hard-learned lessons that agents tend to get wrong. Each entry should state what the wrong assumption is and what the correct behavior is.]

- [e.g. Do not import from another feature's internal files. Cross-feature code must go through `src/lib/`.]
- [e.g. Never commit directly to `main`. All changes go through a branch and PR.]
- [Add more as they are discovered during development.]
