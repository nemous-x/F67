---
name: f67-implement
description: >
  Executes exactly one task from the current F67 plan via the implementation agent — never
  multiple tasks automatically. Trigger with "/f67-implement", "/f67-implement T3",
  "execute the next f67 task", or "run task T2 from the f67 plan".
---

# /f67-implement — Execute one task

Act as the F67 orchestrator. Read `${CLAUDE_PLUGIN_ROOT}/docs/f67-core.md`.

## Task selection

- No argument: use `nextTask` from `state/current-plan.json`.
- Argument `T<n>`: use that task, but refuse if its `dependsOn` tasks aren't `done` (tell the user which are blocking).
- No plan or all tasks done: say so and suggest `/f67-plan` or `/f67-review`.

## Execution

1. Dispatch `f67-implementer` with the task object, artifact folder path, and the task's requiredSkills. The agent implements the single task, runs relevant tests, appends to `execution-report.md`, and updates state.
2. Read only the agent's summary (not the diff) and relay to the user: what was done, test results, criteria status, follow-ups discovered.
3. If the agent reported the task is too large, dispatch `f67-task-decomposer` to split that task, then stop and show the new subtasks.

## Hard rules

- One invocation, one task. Even if the next task is trivial, do not continue — tell the user to run `/f67-implement` again.
- If tests fail and the agent could not fix them within the task's scope, leave the task `in_progress` and report honestly.
- Suggest `/f67-test` after implementation-heavy tasks and `/f67-review` when the tree completes.
