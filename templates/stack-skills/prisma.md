# Stack skill: Prisma

Injected for `prisma` tasks.

- Schema changes always come with a migration (`prisma migrate dev --name <slug>`); never edit applied migrations.
- Multi-write operations use `$transaction`; assume concurrent access.
- Prevent N+1: use `include`/`select` deliberately; select only needed fields on hot paths.
- Repository/data-access layer owns all Prisma calls if the project has one — check domain architecture.md; never scatter prisma client usage through services.
- Nullability and defaults live in the schema, not in application patch-up code.
- Use enums in schema for closed value sets; keep them in sync with domain types.
- Seed/test data through the project's factory patterns (see memory/global/testing.md).
- After schema changes, update the domain's database.md and the dependency graph (persists-to edges).
