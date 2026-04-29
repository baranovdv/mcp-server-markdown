# Solve Hidden-Task Safety Instructions

## Anti-Overfitting Rules

- Never encode the open task's exact implementation steps as reusable workflow guidance.
- Derive the plan from the current task brief and nearby repository evidence every run.
- Treat examples in `task/task.md` as format guidance, not as a fixed recipe.
- Prefer reusable heuristics such as nearest-owner search, smallest validated slice, and focused repair loops.

## Hidden-Task Checks

- Ask whether the current guidance would still make sense if the task touched a different part of the repository.
- Reject references, prompts, or tests that assume a specific future file edit.
- Keep validation, review, and reporting requirements general enough to apply to new task shapes.

## Fallback Rules

- If a preferred tool or search path fails, fall back to local file inspection, existing tests, and repository scripts.
- If the task shape is unfamiliar, reduce scope to the smallest slice that can be explored and validated safely.
- Record every fallback used so maintainers can improve the workflow later.
