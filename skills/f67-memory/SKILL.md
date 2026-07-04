---
name: f67-memory
description: >
  Inspects or rebuilds F67 Domain Driven Memory — shows what the system knows about a
  domain, audits memory health, or regenerates memory for specific domains. Trigger with
  "/f67-memory", "/f67-memory billing", "what does f67 know about", "rebuild f67 memory
  for", or "audit f67 memory".
---

# /f67-memory — Inspect or rebuild DDM

Act as the F67 orchestrator. Read `${CLAUDE_PLUGIN_ROOT}/docs/f67-core.md`.

## Modes (choose by argument)

**No argument — overview.** List domains, their file counts, last-updated dates (from history.md), feature count, decision count, and graph node/edge totals. Flag domains with stale or thin memory.

**`<domain>` — inspect.** Dispatch `f67-memory-loader` for that domain alone and present the digest: overview, business rules, key files, dependencies, known issues, recent history.

**`rebuild <domain>` — regenerate.** Confirm with the user first (rebuilding discards learned content, never curated content). Dispatch `f67-discovery` scoped to the domain, then `f67-memory-evolver` to regenerate the domain's memory files and graph from current code. Preserve everything above `## Learned` in curated files and all of `decisions.md` and `history.md`.

**`audit` — health check.** Verify related-files.json paths exist, graphs parse and reference real nodes, every domain has the mandatory files, and no file exceeds ~200 lines. Report issues with suggested fixes (`/f67-sync` for drift, `rebuild` for rot).

Never modify memory in inspect/overview modes.
