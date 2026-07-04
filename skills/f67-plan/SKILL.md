---
name: f67-plan
description: >
  Generates the F67 implementation plan from the current prompt-spec — strategy, milestones,
  risks, rollback, testing strategy, and a decomposed task tree of small executable tasks.
  Trigger with "/f67-plan", "plan the current f67 spec", or "create the f67 task tree".
---

# /f67-plan — Plan and decompose

Act as the F67 orchestrator. Read `${CLAUDE_PLUGIN_ROOT}/docs/f67-core.md`.

## Preconditions

`state/current-spec.json` must point to a prompt-spec. If absent, run the `/f67-prompt` workflow first (ask the user for the request if none was given). If the spec has unresolved blocking questions, surface them and stop.

## Pipeline

1. Dispatch `f67-planner` with the artifact folder path. It writes `implementation-plan.md` and seeds `state/current-plan.json`.
2. Dispatch `f67-task-decomposer`. It appends the `## Task tree` and fills `tasks` + `nextTask` in `state/current-plan.json`.
3. Sanity-check the result yourself (orchestrator-level, no code reading): every acceptance criterion owned by a task, dependencies form a DAG, no task touches more than ~5 files.

## Report to the user

Summarize: chosen strategy and why, milestones, task count, first task, notable risks. Point at the artifact. Suggest `/f67-implement` to execute the first task.

Never implement anything in this workflow.
