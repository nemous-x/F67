# Stack skill: Authentication

Injected for `authentication` tasks. Security-sensitive: memory/global/security.md is binding.

- Never roll custom crypto, session tokens, or password hashing — use the project's established auth library and patterns.
- Every new route/endpoint declares its authn requirement explicitly; default-deny, never default-allow.
- Authz checks live server-side at the boundary AND in domain rules for sensitive operations; UI hiding is not authorization.
- Tokens: short-lived access, rotation for refresh, httpOnly/secure cookies unless project dictates otherwise.
- No secrets, tokens, or credentials in code, logs, error messages, or test fixtures.
- Account flows (reset, verify, invite) must be enumeration-safe: identical responses for existing/non-existing accounts.
- Changes to auth flows always require a decision record and a reviewer pass focused on security.
