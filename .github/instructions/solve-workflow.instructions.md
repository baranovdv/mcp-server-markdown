# Solve Workflow Instructions

## Stage Order

1. Read the task brief and extract the goal, constraints, acceptance signals, and validation needs.
2. Explore the nearest owning code, tests, call sites, and docs before editing.
3. Write a lightweight plan artifact for non-trivial work.
4. Implement the smallest useful slice.
5. Run focused validation immediately after the first substantive edit.
6. Expand into broader validation only after the local slice is stable.
7. Run lightweight security checks and isolated review.
8. Produce a concise final report with evidence, fallbacks, and blockers.

## Execution Discipline

- Prefer a falsifiable local hypothesis and a cheap disconfirming check before the first edit.
- Keep each change narrowly tied to a task requirement or acceptance signal.
- When a step fails, repair the same slice first instead of opening new surfaces.
- Use repository-local fallback paths when a subagent, integration, or heuristic path fails.
- Surface blockers precisely rather than broadening speculation.

## Validation Discipline

- Favor changed-slice checks before full-repo commands.
- Keep a clear record of what was run, what passed, what failed, and what could not run.
- Do not treat `git diff` as a substitute for an executable check when one exists.
