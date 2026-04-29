# Quickstart: Agentic Solve Workflow

## Goal

Set up a GitHub Copilot workflow whose primary entrypoint is the slash-invocable `/solve` skill and rehearse it against task files such as `task/task.md` without pre-implementing the open task.

## Planned Asset Set

1. Create `.github/skills/solve/SKILL.md` as the user-invocable entrypoint with an argument hint for the task file path.
2. Create `.github/skills/solve-testing/SKILL.md` with reusable guidance for changed-slice tests, edge cases, and regression coverage.
3. Create `.github/agents/solve-review.agent.md` as a non-entrypoint reviewer that classifies findings by severity and blocks completion on high/medium issues.
4. Create `.github/instructions/solve-workflow.instructions.md`, `.github/instructions/solve-prompting.instructions.md`, and `.github/instructions/solve-overfitting.instructions.md` to document stage flow, prompt-design rules, and hidden-task safety rules.
5. Add `.github/prompts/solve-report.prompt.md` as the final reporting contract.
6. Add `.github/workflows/solve.rehearsal.yml` to keep the workflow assets aligned through static rehearsal assertions.
7. Add `.solve/plans/` and `.solve/reports/` as runtime artifact locations for non-trivial runs.

## Rehearsal Flow

1. Open the repository in GitHub Copilot Chat or Copilot CLI with the new workflow assets present.
2. Confirm the slash command list exposes `/solve` and the dedicated testing skill.
3. Run `/solve @task/task.md` or `/solve task/task.md`.
4. Verify that the workflow performs task intake before editing and captures goal, constraints, acceptance signals, and validation needs.
5. Verify that the workflow creates a lightweight plan artifact for non-trivial work and keeps operator interaction optional by default.
6. Confirm the workflow invokes the dedicated testing guidance and the isolated review stage before final completion.
7. Inspect `.solve/plans/` and `.solve/reports/` for the generated run artifacts.
8. Confirm the final report names changed files, focused validation results, security evidence, review severity, fallbacks used, and blockers.

## Expected Validation Sequence

1. Focused test creation or update for the changed slice.
2. `npm test` scoped to the touched area when possible.
3. `npm run typecheck`.
4. `npm run lint`.
5. `npm run build`.
6. Lightweight changed-slice security checks.
7. Isolated code review.
8. Final broad validation before the workflow reports success.

## Blocker and Repair-Loop Rehearsal

1. Trigger a representative validation failure and confirm the workflow repairs the same local slice before opening other surfaces.
2. Confirm the workflow reruns the same focused validation after each repair attempt.
3. Confirm the workflow stops after at most 2 repair attempts for the same slice and converts the issue into a blocker when it still cannot pass.
4. Confirm any blocked run still produces a final report with the failed check, the affected slice, and the next safe action.

## Hidden-Task Rehearsal

1. Create a second task file in the same general format but with a different feature shape than `find_links`.
2. Run `/solve` against that synthetic task and check that the workflow derives its plan from repository exploration rather than assumptions baked in for the open task.
3. Verify that the workflow falls back to repository-local search, file inspection, and existing validation scripts if a preferred subagent or search path fails.
4. Reject any workflow asset that contains task-specific implementation steps, hard-coded source edits, or example assertions tied only to the published task.

## Exit Criteria

- `/solve` is discoverable and starts from a task file argument.
- Non-trivial runs leave a plan artifact and final report in `.solve/`.
- The workflow uses the separate testing skill and isolated review agent.
- The final report lists changed files, validation evidence, security evidence, review findings, fallbacks used, and any blockers.
