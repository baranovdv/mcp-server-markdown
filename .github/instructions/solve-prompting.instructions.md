# Solve Prompting Instructions

## Prompt-Writing Rules

- Decompose the task into intake, exploration, planning, implementation, testing, review, validation, and reporting.
- State the current hypothesis, the next focused check, and the narrow file or behavior slice under discussion.
- Keep prompts grounded in repository evidence rather than generic solution recipes.
- Prefer clear action verbs: inspect, compare, update, validate, report.
- Ask for broader exploration only when the local evidence cannot discriminate between plausible paths.

## Final Reporting Rules

- Summaries should state what changed, how it was validated, what review found, and whether blockers remain.
- Name fallbacks explicitly when the preferred path failed.
- Avoid claiming confidence without citing the evidence produced during the run.

## Review Handoff Rules

- Give the reviewer the task summary, changed files, validation evidence, and unresolved risks.
- Keep the reviewer isolated from implementation instructions so it can judge the result independently.
