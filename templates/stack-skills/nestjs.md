# Stack skill: NestJS

Injected for `nestjs` tasks. Project memory wins over these defaults.

- Respect module boundaries: controllers → services → repositories; no repository access from controllers.
- One module per domain; cross-domain calls go through the other module's exported service, never its internals.
- DTOs with class-validator for every input boundary; never trust raw request bodies.
- Use dependency injection everywhere; no `new` for injectables, no service locators.
- Exceptions: throw Nest HTTP exceptions (or domain exceptions mapped by filters); never leak internal errors to responses.
- Events between domains via the project's event mechanism (check dependency-graph.json for emits/listens-to edges).
- Guards for authz at the controller layer; business-rule checks stay in services.
- Config via ConfigService only; no direct process.env in application code.
