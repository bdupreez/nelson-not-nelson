---
name: conductor
description: Coordinate an agent team from mission brief through execution and wrap-up. Use when work can be parallelized, requires tight coordination, or needs explicit risk-level controls, quality gates, and a final mission report.
---

# Conductor

Execute this workflow for the user's mission.

## 1. Define Mission Brief

- Write one sentence for `outcome`, `metric`, and `deadline`.
- Set constraints: token budget, reliability floor, compliance rules, and forbidden actions.
- Define what is out of scope.
- Define stop criteria and required handoff artifacts.

Use `references/templates.md` section "Mission Brief Template" when the user does not provide structure.

## 2. Assemble Team

- Select one mode:
- `single-session`: Use for sequential tasks, low complexity, or heavy same-file editing.
- `subagents`: Use for parallel scouting or isolated tasks that report only to coordinator.
- `agent-team`: Use when independent agents must coordinate with each other directly.
- Set team size from mission complexity:
- Default to `1 coordinator + 3-6 agents`.
- Add `1 reviewer` for medium/high threat work.
- Do not exceed 10 total agents.

Use `references/team-composition.md` for selection rules.

## 3. Draft Execution Plan

- Split mission into independent tasks with clear deliverables.
- Assign owner for each task and explicit dependencies.
- Assign file ownership when implementation touches code.
- Keep one task in progress per agent unless the mission explicitly requires multitasking.

Use `references/templates.md` section "Execution Plan Template".

## 4. Run Progress Checks

- Keep coordinator focused on coordination and unblock actions.
- Run checkpoints at fixed cadence (for example every 15-30 minutes):
- Update progress by task state: `pending`, `in_progress`, `completed`.
- Identify blockers and choose a concrete next action.
- Track burn against token/time budget.
- Re-scope early when a task drifts from mission metric.

Use `references/templates.md` section "Progress Report Template".

## 5. Apply Risk Levels

- Apply risk level from `references/risk-levels.md`.
- Require verification evidence before marking tasks complete:
- Test or validation output.
- Failure modes and rollback notes.
- Reviewer assessment for medium+ risk levels.
- Trigger quality checks on:
- Task completion.
- Agent idle with unverified outputs.
- Before final synthesis.

## 6. Wrap Up And Report

- Stop or archive agent sessions.
- Produce mission report:
- Decisions and rationale.
- Diffs or artifacts.
- Validation evidence.
- Open risks and follow-ups.
- Record reusable patterns and failure modes for future missions.

Use `references/templates.md` section "Mission Report Template".

## Operating Principles

- Optimize for mission throughput, not equal work distribution.
- Prefer replacing stalled agents over waiting on undefined blockers.
- Keep coordination messages targeted and concise.
- Escalate uncertainty early with options and one recommendation.
