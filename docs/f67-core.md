# F67 core conventions

This document is the single source of truth for F67 runtime layout, workflow pipeline, and artifact contracts. Every F67 skill and agent must follow it.

## Runtime layout (inside the target repository)

All F67 project knowledge lives in the repository being worked on, never in the plugin:

```
.claude/f67/
├── config.yaml                 # Project-level F67 configuration
├── memory/
│   ├── global/                 # architecture.md, coding-standards.md, security.md,
│   │                           # testing.md, ui.md, design-system.md, glossary.md, conventions.md
│   ├── domains/<domain>/       # One folder per business domain (see Domain memory files)
│   ├── features/<feature>/     # feature.md + graph.json per completed feature
│   ├── decisions/              # ADR-style decision records (NNNN-title.md)
│   └── sessions/               # Session summaries (YYYY-MM-DD-slug.md)
├── graphs/
│   ├── domain-graph.json       # Domain ↔ domain relationships
│   ├── file-graph.json         # File → domain/feature associations
│   └── dependency-graph.json   # Service/repository/component/API/test edges
├── state/
│   ├── current-session.json
│   ├── active-context.json
│   ├── current-spec.json       # Pointer to active prompt-spec artifact
│   ├── current-plan.json       # Pointer to active plan + task tree
│   ├── progress.json
│   ├── changed-files.json
│   ├── selected-domains.json
│   └── execution-history.json
├── artifacts/
│   └── <NNN>-<slug>/           # One folder per workflow run
│       ├── prompt-spec.md
│       ├── implementation-plan.md
│       ├── execution-report.md
│       ├── review-report.md
│       └── improvement-plan.md
└── logs/
```

## Domain memory files

Each `.claude/f67/memory/domains/<domain>/` contains:

`overview.md`, `business-rules.md`, `architecture.md`, `dependencies.md`, `api.md`, `database.md`, `ui.md`, `tests.md`, `related-files.json`, `graph.json`, `history.md`, `patterns.md`, `known-issues.md`, `decisions.md`

Rules:

- Files marked with `<!-- f67:curated -->` on the first line are human-curated. Agents may append below a `## Learned` heading but must never rewrite curated sections.
- Only create files that have content. An empty stub is worse than a missing file.
- Keep each file under ~200 lines; prefer dense, factual bullets over prose.

## Graph format

All graph files use one schema (`schemas/graph.schema.json` in the plugin):

```json
{
  "version": 1,
  "updated": "2026-07-04T12:00:00Z",
  "nodes": [{ "id": "domain:billing", "type": "domain", "label": "Billing" }],
  "edges": [{ "from": "domain:billing", "to": "feature:partial-refunds", "type": "contains" }]
}
```

Node types: `domain`, `feature`, `service`, `repository`, `component`, `api`, `test`, `database`, `event`, `file`.
Edge types: `contains`, `implements`, `uses`, `persists-to`, `calls`, `tested-by`, `emits`, `listens-to`, `related-to`.

Agents traverse graphs to find context instead of scanning the repository.

## The pipeline

Every substantive request flows through:

```
Detect domains → Load memory → Discover files → Build context → Build spec
→ Plan → Decompose → Implement (one task) → Test → Review → Improve → Evolve memory
```

Hard rules:

1. Understanding always precedes code. No implementation without a prompt-spec and plan.
2. The orchestrator (main session) never implements, reviews, or reads large codebases. It dispatches agents and maintains state.
3. Agents receive references (paths, node ids, artifact paths), not full file contents, wherever possible.
4. Each stage reads the artifact of the previous stage, never conversation history.
5. `/f67-implement` executes exactly one task per invocation.

## Artifact contracts

| Artifact | Producer | Consumers |
|---|---|---|
| `prompt-spec.md` | f67-prompt-builder agent | planner, decomposer, implementer, tester, reviewer |
| `implementation-plan.md` | f67-planner + f67-task-decomposer | implementer, tester |
| `execution-report.md` | f67-implementer (appended per task) | tester, reviewer, memory-evolver |
| `review-report.md` | f67-reviewer | improver, memory-evolver |
| `improvement-plan.md` | f67-improver | implementer, memory-evolver |

Artifact folders are numbered sequentially: `001-partial-refunds`, `002-fix-invoice-tax`, …

## State discipline

State files hold references only — paths, ids, task numbers, status enums. Never store code or long text in state. Example `current-plan.json`:

```json
{
  "artifact": ".claude/f67/artifacts/001-partial-refunds",
  "spec": "prompt-spec.md",
  "plan": "implementation-plan.md",
  "tasks": [
    { "id": "T1", "title": "Add refund amount to schema", "status": "done" },
    { "id": "T2", "title": "Refund service partial path", "status": "pending", "dependsOn": ["T1"] }
  ],
  "nextTask": "T2"
}
```

## config.yaml (runtime)

```yaml
version: 1
project:
  stack: []            # filled by /f67-init, e.g. [nextjs, nestjs, prisma]
  architecture: ""     # e.g. modular-monolith, hexagonal
  testFramework: ""    # e.g. vitest, jest, pytest
  uiFramework: ""      # e.g. react + tailwind + shadcn
memory:
  autoEvolve: true     # run memory evolution after workflows
  protectCurated: true
skills:
  enabled: []          # stack skills to inject, e.g. [react, prisma, testing]
```

## Skill injection

Stack knowledge lives in `${CLAUDE_PLUGIN_ROOT}/templates/stack-skills/*.md`. Agents that implement or review code load only the stack skill files matching `config.yaml → skills.enabled` and the current task's declared `requiredSkills`. Never load all of them.
