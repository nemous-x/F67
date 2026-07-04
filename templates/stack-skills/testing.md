# Stack skill: Testing

Injected for `testing` tasks (and consumed by f67-tester).

- Follow the framework, placement, and naming in `memory/global/testing.md` exactly.
- Test behavior through public interfaces, not implementation details; a refactor that preserves behavior should not break tests.
- One logical assertion focus per test; name tests as behavior statements ("rejects refund above captured amount").
- Use the project's factories/fixtures; never hand-roll entities that a factory builds.
- Unit: pure logic and domain rules. Integration: module boundaries with real infra substitutes. E2E: critical user paths only.
- Every business rule (BRn) in scope gets at least one test citing it in name or comment.
- Edge case checklist: boundaries, empty/null, duplicates, concurrency, authorization, failure/timeout paths.
- Never assert on incidental details (exact strings, ordering) unless they are the contract.
