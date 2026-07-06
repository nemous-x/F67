# Domain file templates (JSON)

`related-files.json`:

```json
{
  "updated": "{{date}}",
  "files": [
    { "path": "<path from this repository>", "role": "service | controller | component | test | ...", "note": "" }
  ]
}
```

`graph.json`: follows `schemas/graph.schema.json`, scoped to this domain's nodes and their one-hop edges.

Remaining markdown files (`architecture.md`, `dependencies.md`, `api.md`, `database.md`, `ui.md`, `tests.md`, `history.md`, `patterns.md`, `known-issues.md`, `decisions.md`) follow the same shape as overview.md: curated marker on line 1, factual bullet sections, `## Learned` heading at the end. History entries are dated, newest first:

```markdown
## {{date}} — {{feature title}} (artifact {{NNN}})
- What changed in this domain, in the project's own terms.
```
