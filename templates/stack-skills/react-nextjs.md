# Stack skill: React / Next.js

Injected when a task's requiredSkills includes `react` or `nextjs`. Always defer to the project's own patterns in memory over these defaults.

- Server Components by default; add `"use client"` only for interactivity, browser APIs, or hooks.
- Fetch data in server components / route handlers; never in client components when a server path exists.
- Colocate components with their route segment unless the project has a shared components convention — check `memory/global/conventions.md`.
- State: lift only as far as needed; prefer derived state over synced state; URL is state for filters/pagination.
- Follow the project's data-fetching layer (React Query/SWR/server actions) — never introduce a second one.
- Every async boundary needs loading and error UI (loading.tsx / error.tsx or suspense fallbacks).
- Memoize only proven hot paths; readable first.
- Accessibility is not optional: semantic elements, labelled inputs, keyboard-reachable interactions, focus management in dialogs.
