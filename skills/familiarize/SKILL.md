---
name: familiarize
description: Reads AGENTS.md, ARCHITECTURE.md, tasks/index.md, and tasks/changelogs (root CHANGELOG for today or newest, then latest under {mon}); creates a root changelog for today if none exists per tasks/index.md; summarizes what was read. Use when the user says familiarize, starts work needing norms and changelogs, or asks for project context before coding.
---

# Familiarize (session start)

Run this **at the beginning of a new session** when the user invokes **`familiarize`** or asks to load project context (repo layout, conduct, tasks, recent changelogs).

## Steps

1. **Read** (repo root, in order):
   - `AGENTS.md`
   - `ARCHITECTURE.md`
   - `tasks/index.md`

2. **Active changelog (root only)** — Consider only files matching **`tasks/changelogs/CHANGELOG_*.md`** that sit **directly in** `tasks/changelogs/` (**not** inside `jan`/`feb`/… subfolders).
   - If **no such file exists**, **create** `tasks/changelogs/CHANGELOG_{DDMM}.md` for **today’s calendar date** (`dd`/`mm`, two digits each, e.g. 3 May → `CHANGELOG_0305.md`) using the **root active template** in `tasks/index.md`.
   - If **one or more** exist, **read** the file for **today’s** `DDMM` if present; otherwise read the **most recently modified** root `CHANGELOG_*.md` (and note if it is not today’s date).

3. **Last archived changelog** — Under `tasks/changelogs/{mon}/` (`jan` … `dec`, per `tasks/index.md`), find all `CHANGELOG_*.md` files and pick the **most recent** by `DDMM` in the filename (tie-break with month order if needed). **Read** that file. If **none** exist, skip and say so briefly.

4. **Summarize** for the user in a short reply: what you read, whether a **new root `CHANGELOG_{DDMM}.md`** was created in step 2, and which archived file (if any) was “last”.

Do not skip step 1–3 unless a file is genuinely missing from the repo.
