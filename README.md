<h1 align="center">lightweight-spec-driven</h1>

Agent skills for a lightweight spec-driven workflow: `tasks/` layout, architecture notes, and session context—stack-agnostic.

## Overview

| Skill | Path | Purpose |
|--------|------|---------|
| Familiarize | [`skills/familiarize/`](./skills/familiarize/) | Orient a session from `AGENTS.md`, `ARCHITECTURE.md`, `tasks/index.md`, and `tasks/changelogs/` per your task index. |
| Spec bootstrap | [`skills/lightweight-spec-driven/`](./skills/lightweight-spec-driven/) | AskQuestion-backed task folder/prefix names, agent-oriented `tasks/index.md`, repo-surveyed `ARCHITECTURE.md` (patterns + methodology + confirmation), and agents guide filename you choose. |

Run **Spec bootstrap** only when you explicitly ask for it (explicit-invocation metadata on that skill). Use **Familiarize** when you want the agent to reload project context from those files.

## Install

Install everything under `skills/`:

```bash
npx skills add https://github.com/leobrqz/lightweight-spec-driven
```

Or pick skills by id (folder / `name` in `SKILL.md`):

```bash
npx skills add https://github.com/leobrqz/lightweight-spec-driven --skill familiarize --skill lightweight-spec-driven
```

## Usage

Attach or invoke each skill the way your agent supports. Run **Spec bootstrap** once on a repo to generate the layout and docs, then **Familiarize** in later work so the agent reads the same sources of truth. Familiarize expects `tasks/index.md` (and the changelog rules it describes) to exist or to match what you are adopting.

## License

[MIT License](./LICENSE)
