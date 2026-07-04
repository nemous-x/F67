# Global memory templates

Each global memory file starts with `<!-- f67:curated -->` and ends with `## Learned`. Sections per file:

- `architecture.md` — style, layer rules, dependency direction, module boundaries, non-negotiables.
- `coding-standards.md` — naming, error handling, typing, formatting authority (linter config path), review norms.
- `conventions.md` — file placement, folder semantics, commit/branch conventions, generated-code zones.
- `security.md` — authn/authz model, input validation norms, secret handling, sensitive domains.
- `testing.md` — framework, placement, naming, fixtures/factories, coverage expectations, how to run.
- `ui.md` — framework, routing, state management, data fetching patterns.
- `design-system.md` — component library, tokens, spacing/typography rules, a11y baseline.
- `glossary.md` — business terms → definitions as used in this project (drives domain detection).

Rule: only write sections with verified content. Cite source files for every claim where possible.
