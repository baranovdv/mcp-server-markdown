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
6. `Integration Evidence`
   - Required whenever the task adds or changes a named tool, endpoint, command, or other public entrypoint
   - Name the user-facing surface, the owning wiring or registration file, and the evidence that proves the entrypoint is usable
   - If no executable harness exists, say so explicitly and record the direct verification or residual risk
7. `Review Findings`
   - Summarize the isolated review outcome as `high`, `medium`, `low`, or `none`
   - High- and medium-severity findings must either be repaired or shown again under `Blockers`
8. `Fallbacks Used`
   - Any repository-local fallback taken because a preferred tool, subagent, or search path failed
9. `Blockers`
   - Concrete blockers only
   - If none, say `none`

## Reporting Rules

- Keep the report concise and evidence-based.
- Do not claim success without listing the checks that justify it.
- Do not claim success for a new or changed public entrypoint without naming the integration surface and the evidence that it is wired correctly.
- If a check could not run, record `not-run` with the reason.
- If review produced no findings, say so explicitly instead of omitting the section.
- If the run ended blocked, explain the exact point of failure and the next safe action.
