# [Project Name] — Architecture

> Primary audience: coding agents. Written so an AI can infer patterns, methodology, and boundaries before editing code.

## Purpose

[One or two sentences on what this system does and what problem it solves. State unknowns explicitly.]

## Repository layout

```
[root]/
├── [source tree — list real directories with a short comment on each]
├── ...
└── [config, CI, scripts]
```

## Patterns and implementation conventions

| Convention | Where it appears |
|---|---|
| [e.g. Feature module isolation] | `src/features/*` — each feature owns its components, hooks, services, and types |
| [e.g. Shared code rule] | `src/lib/`, `src/components/ui/` — cross-feature only; do not import between features |
| [e.g. Config loading] | `src/config.ts` — all env vars in one place; no inline `import.meta.env` elsewhere |
| [Add more as applicable] | |

## Architectural style and methodology

[Describe the overall style: vertical slices, layered, monolith, microservices, etc. State as Hypothesis if not confirmed.]

- **[Layer or module name]:** [What it owns and what it does not own.]
- **[Another layer]:** [Same.]

## Stack

| Concern | Technology |
|---|---|
| [e.g. Frontend] | [e.g. React 18 + TypeScript + Vite + Tailwind CSS] |
| [e.g. Backend] | [e.g. Supabase Edge Functions (Deno)] |
| [e.g. Database] | [e.g. PostgreSQL via Supabase Cloud] |
| [e.g. Auth] | [e.g. Supabase GoTrue] |
| [Unknown areas] | Unknown — verify in [manifest or config file] |

## Runtime and entrypoints

```bash
# Development
[command]

# Build
[command]

# Test
[command]
```

[If none found, state: No build or run commands identified in repo.]

## Data and external systems

[List databases, third-party APIs, queues, and background jobs — only what is evidenced in the repo. If none: None identified.]

| System | Role |
|---|---|
| [e.g. PostgreSQL] | [Primary datastore; all tables prefixed `proj_`] |
| [e.g. OpenAI API] | [LLM completions and embeddings] |

## Testing and CI

[Describe what test tooling exists and where CI config lives. If none found, state that.]

## Boundaries and coupling rules

[State import direction and anti-patterns. Mark as Hypothesis if inferred, not confirmed.]

- [e.g. Features must not import from other features. Cross-feature shared code lives in `src/lib/` only.]
- [e.g. Edge functions must not import from other functions' `index.ts`. Shared logic lives in `_shared/*.ts`.]

## Maintenance

Update this file when:
- New layers or modules are added.
- Stack dependencies change (version bumps that affect API shape).
- Coupling rules are established or broken.

See `tasks/INDEX.md` for task tracking conventions and `[AGENTS_FILE]` for agent conduct and workflow.
