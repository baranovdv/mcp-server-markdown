<!-- SPECKIT START -->

For additional context about technologies to be used, project structure,
shell commands, and other important information, read specs/001-agentic-solve-workflow/plan.md.

<!-- SPECKIT END -->

## Solve Workflow

- The task-driven entrypoint lives at `.github/skills/solve/SKILL.md` and accepts any readable task brief file or attachment after `/solve`, for example `/solve @task/task.md`, `/solve task/task.md`, or `/solve @path/to/another-task.md`.
- Keep reusable testing guidance in `.github/skills/solve-testing/SKILL.md`, final reporting guidance in `.github/prompts/solve-report.prompt.md`, and isolated review rules in `.github/agents/solve-review.agent.md`.
- Use `.github/instructions/solve-workflow.instructions.md`, `.github/instructions/solve-prompting.instructions.md`, and `.github/instructions/solve-overfitting.instructions.md` for stage flow, prompt-writing, fallback behavior, and hidden-task safety.
- Preserve the current read-only MCP server behavior unless an actual `/solve` run validates a requested change. Do not hide a ready-made solution for `task/task.md` or any hidden task in committed assets.
- Prefer repository-local evidence, minimal validated slices, changed-slice checks before broad validation, and explicit blocker reporting when safe completion is not possible.
- Non-trivial `/solve` runs should leave inspectable runtime evidence under `.solve/plans/` and `.solve/reports/`.
