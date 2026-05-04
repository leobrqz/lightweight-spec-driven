---
name: familiarize
description: Reads tasks/INDEX.md, the agents guide, and ARCHITECTURE.md; reads the active root changelog and most recent archived changelog; creates a root changelog for today if none exists, using the date format recorded in tasks/INDEX.md. Use when the user says familiarize, starts work needing norms and changelogs, or asks for project context before coding.
---

# Familiarize (session start)

Run this **at the beginning of a new session** when the user invokes **`familiarize`** or asks to load project context (repo layout, conduct, tasks, recent changelogs).

## Steps

1. **Read core files** (in order):
   - `tasks/INDEX.md` — extract `CHANGELOG_DATE_ORDER` (the date format stated in the Changelogs section) and the `TASK_CHANGELOGS` folder name.
   - **Agents guide** — scan the repo root for `AGENTS.md`, `CLAUDE.md`, and `AGENT.md`; read whichever exists. If none are found, note it and continue.
   - `ARCHITECTURE.md`

2. **Active changelog** — The active changelog is the single `CHANGELOG_*.md` file at the **root** of `TASK_CHANGELOGS/` (not inside any `{mon}/` subfolder).
   - If **no such file exists**, create one for today using the `CHANGELOG_DATE_ORDER` format from Step 1 (e.g. DDMM → `CHANGELOG_0305.md`; MMDD → `CHANGELOG_0503.md`; ISO → `CHANGELOG_2026-05-03.md`). Use the root active template from `tasks/INDEX.md`.
   - If one exists, **read** it. Note if it is not today's date.

3. **Last archived changelog** — Under `TASK_CHANGELOGS/{mon}/` (`jan` … `dec`), find all `CHANGELOG_*.md` files. Pick the most recent by the date encoded in the filename, interpreting it using the `CHANGELOG_DATE_ORDER` format from Step 1. **Read** that file. If none exist, skip and say so briefly.

4. **Summarize** for the user: which files were read, whether a new root changelog was created in Step 2, and which archived file (if any) was read in Step 3.

Do not skip Steps 1–3 unless a file is genuinely missing from the repo.
