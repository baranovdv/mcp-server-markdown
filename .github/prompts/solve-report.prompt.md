# Solve Final Report

Use this prompt when `/solve` reaches a terminal state.

## Output Files

- Write the human-readable summary to `.solve/reports/<run-id>/report.md`.
- Write review output to `.solve/reports/<run-id>/review-findings.md` when the review stage runs.
- Store any validation logs or summaries beside the report when a command produces output worth preserving.

## Required Sections

1. `Task Brief`
   - Source path
   - Goal summary
   - Constraints that shaped the implementation
2. `Outcome`
   - Final status: `completed` or `blocked`
   - Short explanation of what happened
3. `Changed Files`
   - Repository paths changed during the run
4. `Plan Artifact`
   - Path to `.solve/plans/<run-id>-plan.md` for non-trivial runs
   - `not-needed` only when the task was trivial and that decision is explained
5. `Validation Evidence`
   - A table with `phase`, `check`, `scope`, `result`, `details`, and `artifact`
   - Include focused checks before broad checks
   - Always include lightweight security evidence
6. `Review Findings`
   - Summarize the isolated review outcome as `high`, `medium`, `low`, or `none`
   - High- and medium-severity findings must either be repaired or shown again under `Blockers`
7. `Fallbacks Used`
   - Any repository-local fallback taken because a preferred tool, subagent, or search path failed
8. `Blockers`
   - Concrete blockers only
   - If none, say `none`

## Reporting Rules

- Keep the report concise and evidence-based.
- Do not claim success without listing the checks that justify it.
- If a check could not run, record `not-run` with the reason.
- If review produced no findings, say so explicitly instead of omitting the section.
- If the run ended blocked, explain the exact point of failure and the next safe action.
