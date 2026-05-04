---
name: lightweight-spec-driven
description: Set up a lightweight spec-driven workflow — tasks/ tree and naming confirmed via AskQuestion, tasks/index.md with folder and prefix semantics for agents, ARCHITECTURE.md from repo survey plus inferred patterns and AskQuestion confirmation, and a user-chosen agents guide. Uses AskQuestion for filenames, conduct, changelog order, task folder and prefix names, MCP/skills sections, and architecture read validation. Invoke when bootstrapping a new repo or adding this process to an existing project.
disable-model-invocation: true
---

# Lightweight spec-driven

Apply this skill **only when the user explicitly invokes it** (e.g. run lightweight-spec-driven). Replace placeholders in generated files with the repo’s real name and stack.

Execute **in order**. Do not skip AskQuestion steps unless the user has already answered the same questions in this session (then reuse their answers).

---

## Step 1 — Confirm repo root

- Use the workspace / project root the user intends to set up (usually the git root).
- If ambiguous, ask once in plain text which folder is the root before continuing.

**Block order:** **Tasks** (changelog rule → folder/prefix names → `tasks/` tree → `tasks/index.md`) → **Agents** (conduct → MCP/skills discovery → integrations → write **`AGENTS_FILE`**) → **Architecture** (survey → confirm → **`ARCHITECTURE.md`**) → **Report**.

**Note:** Step 2 records **`AGENTS_FILE`** basename only so Step 6 (`tasks/index.md`) can link it before Step 10 writes the agents file body.

---

## Step 2 — AskQuestion: agents guide filename (basename only)

Use **`AskQuestion`** with at least two options plus an escape hatch, for example:

| Option | Typical use |
|--------|----------------|
| `AGENTS.md` | Cursor / generic “agents” doc at repo root |
| `CLAUDE.md` | Claude Code / Anthropic-style agent doc |
| `AGENT.md` | Short single-file variant |
| `Other` | User will give the exact filename in the **next** message (no dotfiles unless they ask) |

**Store the chosen value** as `AGENTS_FILE` (e.g. `AGENTS.md`). If **Other**, stop after AskQuestion and wait for the filename, then set `AGENTS_FILE` from that message before continuing.

---

## Step 3 — AskQuestion: changelog date in filename

Use **`AskQuestion`** so the user picks how **root** changelog files (before archive) encode the calendar day in the basename:

| Option | Example basename | Meaning |
|--------|-------------------|--------|
| **DDMM** | `CHANGELOG_0305.md` | Day then month (Europe-style) |
| **MMDD** | `CHANGELOG_0503.md` | Month then day (US-style) |
| **ISO** | `CHANGELOG_2026-05-03.md` | `YYYY-MM-DD` |
| **Other** | User describes in chat | Map to a clear rule in `tasks/index.md` |

**Store** the choice as `CHANGELOG_DATE_ORDER`. All later templates and `tasks/index.md` must describe **only** this convention (archived files keep the same basename pattern when moved under `tasks/<changelog-folder>/{mon}/` — use **`TASK_CHANGELOGS`** from Step 4 when writing those paths).

---

## Step 4 — AskQuestion: task folder and filename-prefix names

**Suggested folder names** (under `tasks/`): **`backlog/`** (queued, not started), **`active/`** (in progress), **`closed/`** (finished specs), **`changelogs/`** (ship notes; root active file + `{mon}/` archives per Step 3).

**Suggested task-document prefixes** (on `.md` specs in **`TASK_BACKLOG`**, **`TASK_ACTIVE`**, and **`TASK_CLOSED`** only—not on changelog filenames): **`IDEA_`**, **`DESIGN_`**, **`PLAN_`**, **`ISSUE_`**. Meaning for the written index (same semantics as a mature `tasks/index.md`): **IDEA_** rough future direction; **DESIGN_** conceptual design; **PLAN_** executable implementation plan; **ISSUE_** defect or ambiguity with investigation and fix scope. Changelogs use **`CHANGELOG_…`** naming from Step 3.

Use **`AskQuestion`** so the user either **accepts these suggested folder and prefix names** or **chooses to replace** them (then wait for their next message listing the final folder names and prefix strings for each role; map roles clearly: queue, in-progress, done, changelogs root + archive).

**Store** the resolved folder names as `TASK_BACKLOG`, `TASK_ACTIVE`, `TASK_CLOSED`, `TASK_CHANGELOGS` (must be single path segment each, no slashes). **Store** prefixes as `PREFIX_IDEA`, `PREFIX_DESIGN`, `PREFIX_PLAN`, `PREFIX_ISSUE` (include trailing `_` if the team uses that style). Use these values in Steps 5–6 and in **`AGENTS_FILE`** (Step 10).

---

## Step 5 — Create `tasks/` tree

Under the repo root, create the four workflow folders using **Step 4** names (`tasks/{TASK_BACKLOG}/`, etc.). **`tasks/index.md`** is created in Step 6.

```text
tasks/
├── {TASK_BACKLOG}/
├── {TASK_ACTIVE}/
├── {TASK_CLOSED}/
└── {TASK_CHANGELOGS}/
```

(`tasks/index.md` is written in Step 6.)

Use **`.gitkeep`** in each empty leaf folder if git does not track empty dirs.

---

## Step 6 — Write `tasks/index.md`

Write `tasks/index.md` as the **contract for coding agents** (same role as a strong `tasks/index.md` in a mature repo): it must explain **what each folder is for**, **what each prefix means**, and **how work flows**, using the **exact folder and prefix names from Step 4**.

### Required content

1. **Opening** — One short paragraph: agents align with **`AGENTS_FILE`** (basename from Step 2) and code boundaries with **`ARCHITECTURE.md`** (written in Step 11 of this run).

2. **## Folders** — A table **Folder | Purpose** using the resolved names. Each purpose row must be explicit, for example (adapt wording, not names, to the repo):
   - **Queue folder** (`TASK_BACKLOG`): work not started; ideas, designs, issues, or plans waiting for triage or scheduling.
   - **In-progress folder** (`TASK_ACTIVE`): specs currently being executed; keep count small.
   - **Done folder** (`TASK_CLOSED`): completed task docs kept for history; reopen by moving back and updating status if needed.
   - **Changelogs folder** (`TASK_CHANGELOGS`): end-user-oriented ship notes; one **active** root `CHANGELOG_*` file per rules below; **archived** files under `{mon}/` subfolders (`jan` … `dec` unless the user chose otherwise in Step 3 text).

3. **Typical flow** — One sentence: queue → in-progress → done, with bullets or changelog updates per your `CHANGELOG_DATE_ORDER` rule.

4. **## Filename prefixes** — State that prefixes apply only to `.md` specs under **`TASK_BACKLOG`**, **`TASK_ACTIVE`**, and **`TASK_CLOSED`** (not changelog filenames). Table **Prefix | Meaning**, using resolved prefix strings, for example:
   - **`PREFIX_IDEA`** — Rough capture of a future direction; may be incomplete; often becomes a design doc or is discarded.
   - **`PREFIX_DESIGN`** — Product or technical **design** (decisions, behavior, flows, conceptual data shape); minimal code snippets only to clarify an interface.
   - **`PREFIX_PLAN`** — **Implementation plan** from a design (paths, steps, migrations, checklists) meant to be executed.
   - **`PREFIX_ISSUE`** — Something **broken or unclear**; symptoms, investigation, root cause when known, fix scope.

5. **Relationships** — One short paragraph (e.g. idea → design → plan → implementation; issues may spawn a plan or small design).

6. **## Required header** — Table of required frontmatter on every prefixed task doc (`date`, `status`, `description`) and a **status enum** with a plain-language line per state (`draft`, `ready`, `in_progress`, `blocked`, `done`, `cancelled`).

7. **## Changelogs** — Rules from Step 3 (`CHANGELOG_DATE_ORDER`), root vs `{mon}/`, required top-of-file heading style, content rules (no implementation dumps in changelog if that is the team norm), and the template for a new root changelog file.

8. **Pointers** — Link `ARCHITECTURE.md` and **`AGENTS_FILE`** as sources of truth.

9. Do **not** invent product domains or vendors; stack references only when they already appear in repo docs or will appear in `ARCHITECTURE.md` / Step 8 integrations.

---

## Step 7 — AskQuestion: how the AI should behave (conduct)

Use **`AskQuestion`** (single choice) so the user picks the **default conduct** block that will be copied into **`AGENTS_FILE`** in Step 10 (adapt `[ARCH]` → `ARCHITECTURE.md` in templates). **Three options:**

| ID / value | Label (short) | Intent |
|------------|----------------|--------|
| `conduct_direct` | **Direct & analytical** (default quality bar) | No flattery; direct and analytically honest; never superficial, lazy, or vague; truthful—no invented facts—ground claims in repo, docs, specs, checks; investigate before fixing and target **root cause**; state trade-offs; modular, maintainable code aligned with architecture doc. |
| `conduct_concise` | **Concise executor** | Short, action-first replies; only ask blocking questions; minimal diffs; match repo style; verify before claiming; ship the requested slice without extra scope. |
| `conduct_formal` | **Formal & traceable** | Explicit steps and checklists before/after material changes; assumptions and risks written down; prefer numbered acceptance criteria and references to specs; still truthful and evidence-based. |

**Store** the choice as `CONDUCT_PRESET` (`conduct_direct` | `conduct_concise` | `conduct_formal`).

When writing **`AGENTS_FILE`** in Step 10, paste the matching **Conduct template** from [Conduct templates](#conduct-templates) below (swap `[ARCH]` for the literal `ARCHITECTURE.md`).

---

## Step 8 — Discover MCPs and skills (read-only)

Before the next AskQuestion, **search** the repo (and common locations) without modifying anything:

1. **MCP**: e.g. workspace `mcps/` (descriptor JSON per server), `.cursor/mcp.json`, or MCP config the repo documents.
2. **Skills**: e.g. `.agents/skills/`, `.cursor/skills/`, project rules dirs—list **directory names** or `SKILL.md` paths found.

**Summarize in one short paragraph** what exists (or “none found” for each category). You will inject this summary into Step 9.

---

## Step 9 — AskQuestion: include MCPs / skills in the agents guide?

Use **`AskQuestion`** with the **summary from Step 8** embedded in the `prompt` (so the user sees what was found). Example options:

| Option | Behavior when writing `AGENTS_FILE` |
|--------|-------------------------------------|
| **Neither** | No section on MCPs or skills |
| **MCPs only** | Section: which MCP servers, where descriptors live, rule to read schema before tool calls |
| **Skills only** | Section: installed skills paths, table name + when to use each |
| **Both** | Both sections |
| **Other** | User explains in the next message; follow their instructions literally |

**Store** as `INTEGRATIONS_MODE`.

---

## Step 10 — Write the agents guide (`AGENTS_FILE`)

Create **`AGENTS_FILE`** (from Step 2) with:

1. **Conduct** — Insert the block matching **`CONDUCT_PRESET`** from [Conduct templates](#conduct-templates) (first section of the file; title e.g. “Agent conduct” or “Engineering standards”).
2. **Sources of truth (read order)** — Primary product doc if present (`README.md`, `docs/product.md`, etc.); otherwise one line telling agents which file to add. Then `ARCHITECTURE.md` (**for coding agents: layout, patterns, methodology, boundaries** — file is written in **Step 11** of this run), `tasks/index.md`, then one line per **resolved task folder** from Step 4 (`tasks/{TASK_BACKLOG}/`, `tasks/{TASK_ACTIVE}/`, …), then `tasks/{TASK_CHANGELOGS}/`, then optional skills/MCPs **only if** `INTEGRATIONS_MODE` requires it.
3. **Spec-driven workflow** — Table or bullets aligned with **`tasks/index.md`** (use real folder names). Point agents at prefix meanings and required task headers in the index.
4. **Definition of done** — Short checklist grounded in this workflow.
5. **Integrations appendix** (conditional):
   - If **MCPs only** or **Both**: bullet list from Step 8 + rule “read tool schema before call”.
   - If **Skills only** or **Both**: table **Skill | When to use** from discovered `SKILL.md` entries.
   - If **Neither**: omit entirely.
   - If **Other**: follow the user’s follow-up message.

Use relative paths from repo root only.

---

## Step 11 — Survey the repo, draft architecture read, confirm, then write `ARCHITECTURE.md`

Primary audience for **`ARCHITECTURE.md`** is **coding agents** (read via **`AGENTS_FILE`** and session skills). Humans benefit too, but write so an **AI** can infer **patterns, methodology signals, and boundaries** before editing code—not only a folder tree.

### 11a — Read-only survey

Same scope as before: roots, manifests, source trees, entrypoints, CI, data/config **if present**. Note **recurring patterns** (feature folders, `internal/` vs `pkg/`, shared UI vs domain modules, test layout). If the repo is nearly empty, say **greenfield** in notes—do not invent frameworks.

### 11b — Draft interpretation (for confirmation, not the file yet)

From 11a, prepare a **short bullet list** you will show the user: (1) layout highlights, (2) **inferred patterns** (import rules, layering, slice boundaries) with **path evidence**, (3) **methodology / architectural style** hypotheses (e.g. vertical slices, hexagonal cues, CRUD services) labeled **Hypothesis** when not certain, (4) risks or unknowns.

### 11c — AskQuestion: confirm architecture read

Use **`AskQuestion`** with those bullets in the prompt. Options, for example:

| Option | What happens next |
|--------|-------------------|
| **Accurate** | Write `ARCHITECTURE.md` from 11a–11b; remove **Hypothesis** labels where the user implicitly confirmed by choosing Accurate. |
| **I will correct** | Wait for the user’s correction message; merge fixes; then write `ARCHITECTURE.md`. |
| **Greenfield / minimal** | Write a short `ARCHITECTURE.md` per Step 11e only. |

### 11d — What `ARCHITECTURE.md` must contain (after confirmation path above)

Use these headings or clear equivalents:

1. **Purpose** — Evidence-based; unknowns named explicitly.
2. **Repository layout** — Real paths from 11a; no fake subtrees.
3. **Patterns and implementation conventions** — What repeats in code (exports, error handling, config loading, naming), each tied to **file or directory paths**.
4. **Architectural style and methodology** — How the repo structures work (layers, modules, services, domain packages). Keep claims tied to evidence; keep **Hypothesis** until Step 11c removed it.
5. **Stack** — From manifests; **Unknown** + how to verify when missing.
6. **Runtime and entrypoints** — Build / run / test commands found; say if none.
7. **Data and external systems** — Only if evidenced; else **None identified**.
8. **Testing and CI** — From repo files only.
9. **Boundaries and coupling rules** — Import direction, public vs internal surfaces, anti-patterns—**only** when inferable; otherwise one line that agents must confirm in an active spec before assuming.
10. **Maintenance** — When to update this doc; link `tasks/index.md` and **`AGENTS_FILE`**.

Tone: factual, concise, **evidence-first**.

### 11e — Greenfield or sparse repos

If almost nothing is present after 11c **Greenfield / minimal**: short file listing what was scanned, what is unknown, and a checklist to expand after code exists—**no** fabricated stack.

---

## Step 12 — Report

Reply with:

- Paths created or updated
- Values chosen: `AGENTS_FILE`, `CONDUCT_PRESET`, `CHANGELOG_DATE_ORDER`, task folder + prefix names from Step 4, `INTEGRATIONS_MODE`, and outcome of Step 11c (Accurate / corrected / greenfield)
- One line: next actions for the team (extend `ARCHITECTURE.md` after structural change, add product doc if missing, first spec under `tasks/{TASK_BACKLOG}/`)

---

## Conduct templates

Paste **one** block into **`AGENTS_FILE`** under a “read first” heading. Replace **`[ARCH]`** with **`ARCHITECTURE.md`** (literal).

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

## Templates (adapt placeholders)

### `tasks/index.md` skeleton (excerpt)

- Title: `# Tasks — layout and naming`
- Opening line for **coding agents**; pointer to `AGENTS_FILE` + `ARCHITECTURE.md`
- **Folders** table with **Purpose** per folder (queue, in-progress, done, changelogs) using Step 4 names
- **Prefixes** table with **Meaning** (IDEA / DESIGN / PLAN / ISSUE) using Step 4 strings; note prefixes only on task docs in work folders, not changelog names
- Relationships paragraph; required frontmatter + status meanings; **Changelogs** from `CHANGELOG_DATE_ORDER`; link `ARCHITECTURE.md` and `[AGENTS_FILE]`

### `ARCHITECTURE.md` skeleton (excerpt)

- Title: `# {project-name} — Architecture` (name from manifest, README, or repo folder); audience **coding agents**
- Sections align with **Step 11d** (purpose, layout, patterns & conventions, style & methodology, stack, runtime, data, testing, boundaries, maintenance)

### `AGENTS_FILE` skeleton (excerpt)

- Title matches project tone; link `tasks/index.md` + `ARCHITECTURE.md`
- Numbered “Sources of truth” including `tasks/index.md`
- § Spec-driven workflow + definition of done

Replace `[Project]` with the repo folder name or user-provided project name if they gave one.

---

## Notes

- **Do not** overwrite existing files without **explicit user confirmation** if `tasks/index.md`, `ARCHITECTURE.md`, or `AGENTS_FILE` already exist—offer diff or append-only, or ask once.
- `ARCHITECTURE.md` must stay **grounded in the Step 11a survey** and **Step 11c**; `tasks/index.md` is agent-facing process and must not invent domains beyond what `ARCHITECTURE.md` or Step 8 integrations support.
- If **`AskQuestion`** is unavailable, ask the same choices in **numbered plain text** and wait for the user’s reply before Steps 5–6 and 10–11.
