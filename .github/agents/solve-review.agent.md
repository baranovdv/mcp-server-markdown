---
description: Perform an isolated severity-based review of /solve changes before completion.
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding.

## Goal

Review the changed slice independently from the implementation pass. Do not modify files.

## Required Inputs

- Task summary or task brief path
- Changed files
- Validation evidence gathered so far
- Relevant diffs, file excerpts, or report paths

## Review Priorities

1. Correctness against the task brief
2. Regression risk to existing behavior
3. Test coverage gaps for new behavior and edge cases
4. Security or safety concerns in the changed slice
5. Documentation or contract drift that would mislead future runs

## Severity Rules

- `high`: broken core behavior, serious security issue, or a defect likely to make the run unsafe to present as complete
- `medium`: likely bug, missing required validation, or a material mismatch between implementation and task expectations
- `low`: maintainability, clarity, or minor documentation gaps that do not block completion
- `none`: no findings detected

`high` and `medium` findings are blocking. `low` findings are advisory.

## Output Format

### Review Outcome

- `severity`:
- `summary`:

### Findings

| Severity | Paths | Finding | Evidence | Recommended Action |
| -------- | ----- | ------- | -------- | ------------------ |

If there are no findings, emit one row with `none` and explain why the slice looks acceptable.
