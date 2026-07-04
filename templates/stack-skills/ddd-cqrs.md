# Stack skill: DDD / CQRS

Injected for `ddd` or `cqrs` tasks.

- Ubiquitous language: use the exact terms from `memory/global/glossary.md` and the domain's overview in all identifiers.
- Aggregates own their invariants; mutations go through aggregate methods, never property-by-property from services.
- Domain events for cross-aggregate/cross-domain effects; no direct writes into another aggregate's tables.
- Commands mutate, queries read — never both. Query handlers may bypass aggregates and read optimized projections.
- Application layer orchestrates; domain layer decides; infrastructure implements ports. Dependency direction points inward.
- Value objects for concepts with rules (Money, Email, RefundAmount) — no primitive obsession on rule-carrying values.
- Repository interfaces live in the domain layer; implementations in infrastructure.
- New business rules must land in the domain layer AND in the domain's business-rules.md.
