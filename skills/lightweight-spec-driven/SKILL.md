---
name: lightweight-spec-driven
description: Bootstrap a lightweight spec-driven workflow — tasks structure, agents guide, and architecture documentation — for a new or existing project.
disable-model-invocation: true
---

# Lightweight spec-driven

Replace placeholders in generated files with the repo's real name and stack.

This skill sets up a lightweight spec-driven workflow for coding agents. It creates a minimal `tasks/` folder structure for tracking work, an `INDEX.md` that acts as the task contract agents read before doing anything, an agents guide that encodes conduct and sources of truth, and an `ARCHITECTURE.md` that grounds agents in the real shape of the codebase. The goal is structured enough to keep agents aligned across sessions, small enough to never bloat the repo or the context window.

Execute **phase by phase**. Each phase ends by writing its artifact. **Do not ask questions from the next phase until the current phase's artifact is written and complete.**

Do not skip AskQuestion steps unless the user has already answered the same questions in this session (then reuse their answers).

When calling **`AskQuestion`**, do not add an "Other" option — the tool already provides one natively.

---

## Phase 1 — Tasks

Collect all task-related decisions, create the folder tree, and write `tasks/INDEX.md`. Do not move to Phase 2 until `tasks/INDEX.md` exists on disk.

### Step 1 — Confirm repo root

- Use the workspace / project root the user intends to set up (usually the git root).
- If ambiguous, ask once in plain text which folder is the root before continuing.

### Step 2 — AskQuestion: changelog date in filename

Use **`AskQuestion`** so the user picks how **root** changelog files (before archive) encode the calendar day in the basename:

| Option | Example basename | Meaning |
|--------|-------------------|--------|
| **DDMM** | `CHANGELOG_0305.md` | Day then month (Europe-style) |
| **MMDD** | `CHANGELOG_0503.md` | Month then day (US-style) |
| **ISO** | `CHANGELOG_2026-05-03.md` | `YYYY-MM-DD` |

**Store** the choice as `CHANGELOG_DATE_ORDER`. All later templates and `tasks/INDEX.md` must describe **only** this convention (archived files keep the same basename pattern when moved under `tasks/<changelog-folder>/{mon}/` — use **`TASK_CHANGELOGS`** from Step 3 when writing those paths).

### Step 3 — AskQuestion: task folder and filename-prefix names

**Suggested folder names** (under `tasks/`): **`backlog/`** (queued, not started), **`active/`** (in progress), **`closed/`** (finished specs), **`changelogs/`** (ship notes; root active file + `{mon}/` archives per Step 2).

**Suggested task-document prefixes** (on `.md` specs in **`TASK_BACKLOG`**, **`TASK_ACTIVE`**, and **`TASK_CLOSED`** only—not on changelog filenames): **`IDEA_`**, **`DESIGN_`**, **`PLAN_`**, **`ISSUE_`**. Meaning for the written index: **IDEA_** rough future direction; **DESIGN_** conceptual design; **PLAN_** executable implementation plan; **ISSUE_** defect or ambiguity with investigation and fix scope. Changelogs use **`CHANGELOG_…`** naming from Step 2.

Use **`AskQuestion`** so the user either **accepts these suggested folder and prefix names** or **chooses to replace** them (then wait for their next message listing the final folder names and prefix strings for each role; map roles clearly: queue, in-progress, done, changelogs root + archive).

**Store** the resolved folder names as `TASK_BACKLOG`, `TASK_ACTIVE`, `TASK_CLOSED`, `TASK_CHANGELOGS` (must be single path segment each, no slashes). **Store** prefixes as `PREFIX_IDEA`, `PREFIX_DESIGN`, `PREFIX_PLAN`, `PREFIX_ISSUE` (include trailing `_` if the team uses that style). Use these values in Steps 5 and 6 and in **`AGENTS_FILE`** (Phase 2).

### Step 4 — AskQuestion: status definitions

Use **`AskQuestion`** to confirm the **status values** used in all prefixed task documents. Present the following definitions and ask whether to **accept** or **replace** any:

| Status | Meaning |
|--------|---------|
| `draft` | Overall direction captured; nothing specified or fully defined yet. |
| `ready` | Fully specified; ready to proceed or implement. |
| `in_progress` | Currently being worked on — implementing, or investigating if it is an issue. |
| `blocked` | Progress halted by an external dependency; the blocker must be named in the document. |
| `done` | Finished — implemented, or investigated and resolved. |
| `cancelled` | Abandoned at user request. |

**Store** the confirmed set (labels and meanings) as `STATUS_DEFINITIONS`. Use them verbatim in `tasks/INDEX.md` (Step 6).

### Step 5 — Create `tasks/` tree

Under the repo root, create the four workflow folders using **Step 3** names (`tasks/{TASK_BACKLOG}/`, etc.).

```text
tasks/
├── {TASK_BACKLOG}/
├── {TASK_ACTIVE}/
├── {TASK_CLOSED}/
└── {TASK_CHANGELOGS}/
```
### Step 6 — Write `tasks/INDEX.md`

Write `tasks/INDEX.md` as the **contract for coding agents**: it must explain **what each folder is for**, **what each prefix means**, and **how work flows**, using the **exact folder and prefix names from Step 3** and the **status definitions from Step 4**.

#### Required content

1. **## Folders** — A table **Folder | Purpose** using the resolved names. Each purpose row must be explicit, for example (adapt wording, not names, to the repo):
   - **Queue folder** (`TASK_BACKLOG`): work not started; ideas, designs, issues, or plans waiting for triage or scheduling.
   - **In-progress folder** (`TASK_ACTIVE`): specs currently being executed; keep count small.
   - **Done folder** (`TASK_CLOSED`): completed task docs kept for history; reopen by moving back and updating status if needed.
   - **Changelogs folder** (`TASK_CHANGELOGS`): end-user-oriented ship notes; one **active** root `CHANGELOG_*` file; **archived** files under `{mon}/` subfolders (`jan` … `dec` unless the user chose otherwise in Step 2 text).

2. **## Filename prefixes** — State that prefixes apply only to `.md` specs under **`TASK_BACKLOG`**, **`TASK_ACTIVE`**, and **`TASK_CLOSED`** (not changelog filenames). Table **Prefix | Meaning**, using resolved prefix strings, for example:
   - **`PREFIX_IDEA`** — Rough capture of a future direction; may be incomplete; often becomes a design doc or is discarded.
   - **`PREFIX_DESIGN`** — Product or technical **design** (decisions, behavior, flows, conceptual data shape); minimal code snippets only to clarify an interface.
   - **`PREFIX_PLAN`** — **Implementation plan** from a design (paths, steps, migrations, checklists) meant to be executed.
   - **`PREFIX_ISSUE`** — Something **broken or unclear**; symptoms, investigation, root cause when known, fix scope.

3. **Relationships** — One short paragraph (e.g. idea → design → plan → implementation; issues may spawn a plan or small design).

4. **## Required header** — Table of required frontmatter on every prefixed task doc (`date`, `status`, `description`) and the **status enum** using `STATUS_DEFINITIONS` verbatim from Step 4.

5. **## Changelogs** — State the date format (`CHANGELOG_DATE_ORDER` from Step 2) **explicitly by name and example** so tools reading this file can extract it. Then cover: root vs `{mon}/`, required top-of-file heading style, content rules, and the template for a new root changelog file.

6. Do **not** invent product domains or vendors; stack references only when they already appear in repo docs or will appear in `ARCHITECTURE.md` / Phase 2 integrations.

**`tasks/INDEX.md` must be written to disk before continuing to Phase 2.**

---

## Phase 2 — Agents

Collect all agent-related decisions and write `AGENTS_FILE`. Do not move to Phase 3 until `AGENTS_FILE` exists on disk.

### Step 7 — AskQuestion: agents guide filename (basename only)

Use **`AskQuestion`** with at least two options, for example:

| Option | Typical use |
|--------|----------------|
| `AGENTS.md` | Cursor / generic "agents" doc at repo root |
| `CLAUDE.md` | Claude Code / Anthropic-style agent doc |
| `AGENT.md` | Short single-file variant |

**Store the chosen value** as `AGENTS_FILE` (e.g. `AGENTS.md`). If the user provides a different filename, use that as `AGENTS_FILE` before continuing.

### Step 8 — AskQuestion: how the AI should behave (conduct)

Use **`AskQuestion`** (single choice) so the user picks the **default conduct** block that will be copied into **`AGENTS_FILE`** in Step 11. **Three options:**

| ID / value | Label (short) | Intent |
|------------|----------------|--------|
| `conduct_direct` | **Direct & analytical** (default quality bar) | No flattery; direct and analytically honest; never superficial, lazy, or vague; truthful—no invented facts—ground claims in repo, docs, specs, checks; investigate before fixing and target **root cause**; state trade-offs; modular, maintainable code aligned with architecture doc. |
| `conduct_concise` | **Concise executor** | Short, action-first replies; only ask blocking questions; minimal diffs; match repo style; verify before claiming; ship the requested slice without extra scope. |
| `conduct_formal` | **Formal & traceable** | Explicit steps and checklists before/after material changes; assumptions and risks written down; prefer numbered acceptance criteria and references to specs; still truthful and evidence-based. |

**Store** the choice as `CONDUCT_PRESET` (`conduct_direct` | `conduct_concise` | `conduct_formal`).

When writing **`AGENTS_FILE`** in Step 11, paste the matching **Conduct template** from [Conduct templates](#conduct-templates) below (swap `[ARCH]` for the literal `ARCHITECTURE.md`).

### Step 9 — Discover MCPs and skills (read-only)

**Search both the environment and the repo** without modifying anything:

1. **Environment MCPs**: Check what MCP servers are currently active in the session — these may be configured at the IDE, workspace, or user level and will not appear inside the repo. List every MCP server available, regardless of where it is configured.
2. **Repo MCPs**: Also check inside the repo for local MCP config (e.g. `mcps/` descriptor JSON, `.cursor/mcp.json`, or any MCP config the repo documents).
3. **Skills**: Check both the environment (e.g. IDE-installed skills, user-level skill dirs) and the repo (e.g. `.agents/skills/`, `.cursor/skills/`, project rules dirs). List **directory names** or `SKILL.md` paths found.

**Summarize in one short paragraph** what exists across both sources (or "none found" for each category). You will inject this summary into Step 10.

### Step 10 — AskQuestion: include MCPs / skills in the agents guide?

Use **`AskQuestion`** with the **summary from Step 9** embedded in the `prompt` (so the user sees what was found). Example options:

| Option | Behavior when writing `AGENTS_FILE` |
|--------|-------------------------------------|
| **Neither** | No section on MCPs or skills |
| **MCPs only** | Section: which MCP servers, where descriptors live, rule to read schema before tool calls |
| **Skills only** | Section: installed skills paths, table name + when to use each |
| **Both** | Both sections |

**Store** as `INTEGRATIONS_MODE`.

### Step 11 — Write the agents guide (`AGENTS_FILE`)

Create **`AGENTS_FILE`** (from Step 7) with:

1. **Conduct** — Insert the block matching **`CONDUCT_PRESET`** from [Conduct templates](#conduct-templates) (first section of the file; title e.g. "Agent conduct" or "Engineering standards").
2. **Sources of truth (read order)** — Primary product doc if present (`README.md`, `docs/product.md`, etc.). Then `ARCHITECTURE.md` (**for coding agents: layout, patterns, methodology, boundaries** — file is written in Phase 3 of this run), `tasks/INDEX.md`, then one line per **resolved task folder** from Step 3 (`tasks/{TASK_BACKLOG}/`, `tasks/{TASK_ACTIVE}/`, …), then `tasks/{TASK_CHANGELOGS}/`, then optional skills/MCPs **only if** `INTEGRATIONS_MODE` requires it.
3. **Spec-driven workflow** — Table or bullets aligned with **`tasks/INDEX.md`** (use real folder names). Point agents at prefix meanings and required task headers in the index.
4. **Definition of done** — Short checklist grounded in this workflow.
5. **Integrations appendix** (conditional):
   - If **MCPs only** or **Both**: bullet list from Step 9 + rule "read tool schema before call".
   - If **Skills only** or **Both**: table **Skill | When to use** from discovered `SKILL.md` entries.
   - If **Neither**: omit entirely.

Use relative paths from repo root only.

**`AGENTS_FILE` must be written to disk before continuing to Phase 3.**

---

## Phase 3 — Architecture

Survey the repo, confirm findings, and write `ARCHITECTURE.md`. Do not move to the Wrap-up until `ARCHITECTURE.md` exists on disk.

Primary audience for **`ARCHITECTURE.md`** is **coding agents** (read via **`AGENTS_FILE`** and session skills). Humans benefit too, but write so an **AI** can infer **patterns, methodology signals, and boundaries** before editing code—not only a folder tree.

### Step 12a — Read-only survey

Scan roots, manifests, source trees, entrypoints, CI, data/config if present. Note **recurring patterns** (feature folders, `internal/` vs `pkg/`, shared UI vs domain modules, test layout). If the repo is nearly empty, say **greenfield** in notes—do not invent frameworks.

### Step 12b — What `ARCHITECTURE.md` must contain

Use these headings or clear equivalents when writing the file:

1. **Purpose** — Evidence-based; unknowns named explicitly.
2. **Repository layout** — Real paths from 12a; no fake subtrees.
3. **Patterns and implementation conventions** — What repeats in code (exports, error handling, config loading, naming), each tied to **file or directory paths**.
4. **Architectural style and methodology** — How the repo structures work (layers, modules, services, domain packages). Keep claims tied to evidence; keep **Hypothesis** until Step 12d removes it.
5. **Stack** — From manifests; **Unknown** + how to verify when missing.
6. **Runtime and entrypoints** — Build / run / test commands found; say if none.
7. **Data and external systems** — Only if evidenced; else **None identified**.
8. **Testing and CI** — From repo files only.
9. **Boundaries and coupling rules** — Import direction, public vs internal surfaces, anti-patterns—**only** when inferable; otherwise one line that agents must confirm in an active spec before assuming.
10. **Maintenance** — When to update this doc; link `tasks/INDEX.md` and **`AGENTS_FILE`**.

Tone: factual, concise, **evidence-first**.

### Step 12c — Present draft interpretation in chat

Send a **chat message** with the findings. This is a **standalone action** — do not combine it with any tool call, including AskQuestion. Cover: (1) layout highlights, (2) **inferred patterns** with **path evidence**, (3) **methodology / architectural style** hypotheses labeled **Hypothesis** when not certain, (4) risks or unknowns.

Only after the chat message is sent, proceed to Step 12d.

### Step 12d — AskQuestion: confirm architecture read

Call **`AskQuestion`** as a **separate action** from Step 12c. The `prompt` field must contain only a short confirmation question — exactly like: `"Does this architecture read look accurate?"` No findings, bullets, paths, or survey content belong in the prompt. Options:

| Option | What happens next |
|--------|-------------------|
| **Accurate** | Write `ARCHITECTURE.md` from 12a–12c; remove **Hypothesis** labels. |
| **I will correct** | Wait for the user's correction message; merge fixes; then write `ARCHITECTURE.md`. |
| **Greenfield / minimal** | Write a short `ARCHITECTURE.md` per Step 12e only. |

### Step 12e — Greenfield or sparse repos

If almost nothing is present after 12d **Greenfield / minimal**: short file listing what was scanned, what is unknown, and a checklist to expand after code exists—**no** fabricated stack.

**`ARCHITECTURE.md` must be written to disk before continuing to Wrap-up.**

---

## Wrap-up

### Step 13 — Verify cross-references

With all three artifacts written, check each file's references are accurate:

1. **`AGENTS_FILE`** — `ARCHITECTURE.md` is listed in Sources of truth by its exact filename.
2. **`ARCHITECTURE.md`** Maintenance section — names `AGENTS_FILE` by its exact basename (from Step 7) and links `tasks/INDEX.md`.

Update any file where a reference is missing, misspelled, or still a placeholder.

### Step 14 — Report

Reply with:

- Paths created or updated
- Values chosen: `AGENTS_FILE`, `CONDUCT_PRESET`, `CHANGELOG_DATE_ORDER`, task folder + prefix names from Step 3, `INTEGRATIONS_MODE`, and outcome of Step 12d (Accurate / corrected / greenfield)
- One line: next actions for the team (extend `ARCHITECTURE.md` after structural change, add product doc if missing, first spec under `tasks/{TASK_BACKLOG}/`)

---

## Conduct templates

Paste **one** block into **`AGENTS_FILE`** under a "read first" heading. Replace **`[ARCH]`** with **`ARCHITECTURE.md`** (literal).

### `conduct_direct` — Direct & analytical

```markdown
## Agent conduct (read first)

### Communication and honesty

- **Do not** be sycophantic or use flattery. Be **direct**, **to the point**, and **analytically honest** (including drawbacks, risks, and unknowns).
- **Never** be superficial, lazy, or vague. If something is uncertain, **say what is unknown** and what would be needed to verify it.
- Be **truthful and realistic**. **Do not** invent facts, APIs, file paths, behavior, or metrics. Ground statements in the repo, docs, specs, or reproducible checks.

### How to work on problems

- **Investigate before fixing**: reproduce or trace the failure, read the relevant code paths, and aim for the **root cause**—not a symptom patch unless scope explicitly limits the fix.
- Be **analytical**: separate assumptions from evidence; state trade-offs when recommending a change.
- Prefer **clear reasoning** over confident guessing.

### Codebase quality

- Keep the codebase **well structured**, **modular**, and **clean** so it stays **maintainable**: clear boundaries between features, shared utilities only where truly shared, consistent patterns with the rest of the repo.
- Changes should be **coherent** with existing architecture (**`[ARCH]`**); when architecture must shift, **document why** in the spec or changelog.
```

### `conduct_concise` — Concise executor

```markdown
## Agent conduct (read first)

- **Be brief and action-first.** Prefer short paragraphs and direct instructions over long preambles.
- **Ask only blocking questions** when something essential is missing; otherwise pick sensible defaults and state them in one line.
- **Verify before claiming**: read the repo; do not invent paths, APIs, or behavior.
- **Minimal diffs**: implement only what was asked; match existing style and patterns.
- **Ground technical statements** in files, docs, or reproducible runs—not speculation.
```

### `conduct_formal` — Formal & traceable

```markdown
## Agent conduct (read first)

- **Plan visible steps** for non-trivial work: state what you will do, then do it, then summarize what changed.
- **Write down assumptions and risks** when they materially affect the outcome; cite evidence (file paths, doc sections) when asserting behavior.
- **Use acceptance-style checks** where helpful: numbered criteria aligned with the active spec or task doc.
- **Do not invent facts**; when uncertain, label it explicitly and say what would confirm it.
- **Align changes with `ARCHITECTURE.md` and task specs**; if you must diverge, document why in the same change or in the task changelog.
```

---

## Notes

- **Do not** overwrite existing files without **explicit user confirmation** if `tasks/INDEX.md`, `ARCHITECTURE.md`, or `AGENTS_FILE` already exist—offer diff or append-only, or ask once.
- `ARCHITECTURE.md` must stay **grounded in the Step 12a survey** and **Step 12d**; `tasks/INDEX.md` is agent-facing process and must not invent domains beyond what `ARCHITECTURE.md` or Phase 2 integrations support.
- If **`AskQuestion`** is unavailable, ask the same choices in **numbered plain text** and wait for the user's reply before Steps 5–6 and 11–12.

## Templates

Starter templates for task documents and changelogs live in `templates/` (same directory as this skill). Refer to those files when creating new specs or changelogs.
