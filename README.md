<h1 align="center">lightweight-spec-driven</h1>

<p align="center">Spec-driven development for coding agents: structured enough to keep them aligned across sessions, minimal enough to never bloat your repo. Stack-agnostic.</p>

## What is spec-driven development

Spec-driven development is a workflow where work is defined in structured documents before and during implementation. Agents read specs to understand what to build, track progress, and stay consistent across sessions. It keeps context between the human and the agent grounded in written artifacts rather than conversation history.

## Why this version

Most spec-driven setups solve the alignment problem by adding more files. More designs, more plans, more architecture notes. Over time the repository accumulates tens of markdown files that either bloat the agent's context window on every session or get ignored entirely. Long-term maintenance becomes a problem in itself.

This lightweight version is designed to be applied to new or existing projects without friction. It creates the minimum structure needed: a `tasks/` folder for tracking work, one index file that agents read as the task contract, one agents guide for conduct and sources of truth, and one architecture file grounded in the actual repo. Nothing more. The suggested workflow is intentionally basic and can be customized during the bootstrap phase. It does not lock you into anything.

## Skills

| Skill | Path | Purpose |
|--------|------|---------|
| **Spec bootstrap** | [`skills/lightweight-spec-driven/`](./skills/lightweight-spec-driven/) | One-time setup. Creates the `tasks/` tree, `tasks/INDEX.md`, agents guide, and `ARCHITECTURE.md` through a guided sequence of questions. Run once per project. |
| **Familiarize** | [`skills/familiarize/`](./skills/familiarize/) | Session start. Reads the agents guide, `ARCHITECTURE.md`, `tasks/INDEX.md`, and recent changelogs to load project context. Creates today's changelog if it does not exist. Use it at the beginning of every work session to get back to where you stopped. |

## Install

Install everything under `skills/`:

```bash
npx skills add leobrqz/lightweight-spec-driven --skill familiarize --skill lightweight-spec-driven
```


Or pick skills during installation:

```bash
npx skills add leobrqz/lightweight-spec-driven
```

## Usage

Run **Spec bootstrap** once on a repo to generate the layout and docs. In every subsequent session, run **Familiarize** so the agent reads the same sources of truth and picks up from the last changelog.

## Workflow

```
Bootstrap (once)
    │
    ├── tasks/INDEX.md     ← task contract: folders, prefixes, statuses, changelog rules
    ├── AGENTS_FILE        ← conduct, sources of truth, spec-driven workflow
    └── ARCHITECTURE.md    ← repo layout, patterns, stack, boundaries

Session start
    │
    └── Familiarize        ← loads agents guide + architecture + index + recent changelogs
                              creates today's changelog if it doesn't exist

During work
    │
    ├── Write or update a spec in tasks/backlog/ or tasks/active/
    ├── Move specs forward: backlog → active → closed
    └── Log shipped changes in today's changelog
```

Specs use four prefixes — `IDEA_`, `DESIGN_`, `PLAN_`, `ISSUE_` — and carry a `status` frontmatter field that moves from `draft` → `ready` → `in_progress` → `done`. The folder layout, prefix names, date format, and conduct style are all chosen during bootstrap, not hardcoded.

## License

[MIT License](./LICENSE)
