# Domain file templates (JSON)

`related-files.json`:

```json
{
  "updated": "{{date}}",
  "files": [
    { "path": "src/billing/refund.service.ts", "role": "service", "note": "" }
  ]
}
```

`graph.json`: follows `schemas/graph.schema.json`, scoped to this domain's nodes and their one-hop edges.

Remaining markdown files (`architecture.md`, `dependencies.md`, `api.md`, `database.md`, `ui.md`, `tests.md`, `history.md`, `patterns.md`, `known-issues.md`, `decisions.md`) follow the same shape as overview.md: curated marker on line 1, factual bullet sections, `## Learned` heading at the end. History entries are dated, newest first:

```markdown
## 2026-07-04 — Partial refunds (artifact 001)
- Added partial refund path to RefundService; new `amount` column on refunds.
```
