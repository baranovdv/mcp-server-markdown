# Contract: `/solve` Skill

## Purpose

Define the operator-facing contract for the GitHub Copilot workflow entrypoint that will be committed to this repository.

## Invocation

- Primary form: `/solve @task/task.md`
- Equivalent path form: `/solve task/task.md`

The entrypoint must be implemented as a user-invocable GitHub Copilot skill located at `.github/skills/solve/`.

## Input Contract

| Field | Type | Required | Description |
|------|------|----------|-------------|
| `task-file` | string or attachment reference | Yes | Repository-visible path or attached file containing the task brief |

## Input Rules

- The task file is the primary source of truth for requested behavior, constraints, examples, and success expectations.
- Missing optional sections inside the task file do not justify asking the operator for clarification; the workflow must infer the next safe step from repository-local evidence.
- If the task file path cannot be resolved or read, the workflow must stop with a precise blocker report.

## Required Workflow Behavior

1. Parse the task file and extract goals, constraints, acceptance expectations, and validation needs.
2. Explore the repository before planning so the workflow does not rely on task-specific canned steps.
3. Create a lightweight repository-local plan artifact for non-trivial work.
4. Implement in small validated slices.
5. Use a dedicated testing guidance asset rather than embedding all test logic in the entrypoint.
6. Run an isolated review stage that classifies findings as `high`, `medium`, `low`, or `none`.
7. Treat `high` and `medium` review findings as blocking until repaired or explicitly reported as blockers.
8. Run lightweight changed-slice security checks on every run and broaden them when the task touches sensitive surfaces.
9. Produce a final report that summarizes changes, validation evidence, review findings, fallbacks used, and blockers.

## Output Contract

The workflow must leave behind:

| Output | Required | Description |
|-------|----------|-------------|
| Repository changes | Yes for successful implementation runs | Source, tests, docs, or workflow assets modified to satisfy the task |
| `.solve/plans/<run-id>-plan.md` | Yes for non-trivial runs | Lightweight execution plan created before or alongside implementation |
| `.solve/reports/<run-id>/report.md` | Yes | Human-readable final report |
| `.solve/reports/<run-id>/review-findings.md` | Yes when review runs | Severity-tagged review output |
| Validation evidence files | Yes when checks run | Logs or summaries of focused tests, security checks, or final validation |

## Failure Contract

- If the workflow encounters a concrete blocker it cannot safely repair, it must end in a blocked state and write the blocker to the final report.
- The workflow may retry a failing slice at most 2 times before converting the issue into a blocker.
- Low-severity review findings may remain advisory, but they must still appear in the final report.

## Compatibility Contract

- `/solve` is additive to the repository and must not change the current MCP tool contracts before runtime.
- The workflow must preserve the repo's read-only markdown server behavior unless a future task explicitly and safely changes product behavior during a solve run.
- Core execution must have a repository-local fallback path and must not depend exclusively on external services.