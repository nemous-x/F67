# Stack skill: API design

Injected for `api` tasks.

- Match the project's existing API style (REST/GraphQL/tRPC, versioning, envelope shape) from the domain's api.md — consistency beats ideal design.
- Contract first: define/adjust the schema (OpenAPI/GraphQL/zod) before implementation when the strategy is api-first.
- Inputs validated at the boundary; outputs never leak internal fields (map entities → DTOs).
- Errors: use the project's error shape; distinguish 400 (client), 403 (authz), 404 (existence), 409 (conflict), 422 (semantics) correctly.
- Idempotency for anything money- or state-critical (keys or natural idempotence); pagination on every collection endpoint.
- Breaking changes require a decision record and a migration note in the domain's api.md.
- Every new/changed endpoint: authz check explicit, added to api.md, edge added to dependency graph, covered by at least one integration test.
