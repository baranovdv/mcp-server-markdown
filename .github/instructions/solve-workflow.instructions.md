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
- During intake and planning, extract both the behavior slice and the owning integration surface for any named tool, endpoint, command, or other public entrypoint.
- When a step fails, repair the same slice first instead of opening new surfaces.
- Never mutate git state just to inspect baseline behavior. Avoid `git stash`, `git checkout`, `git restore`, and similar commands unless the task explicitly requires them.
- Use repository-local fallback paths when a subagent, integration, or heuristic path fails.
- Surface blockers precisely rather than broadening speculation.

## Validation Discipline

- Favor changed-slice checks before full-repo commands.
- Do not mark a task complete if a new or changed public entrypoint lacks evidence that its wiring, registration, or routing exists at the user-facing surface.
- Keep a clear record of what was run, what passed, what failed, and what could not run.
- Do not treat `git diff` as a substitute for an executable check when one exists.
- If no executable check exists for the public entrypoint, inspect the owning integration file directly and report the missing harness as residual risk.
- If you suspect pre-existing formatting or lint drift, validate the current touched files or report the drift explicitly instead of trying to reconstruct a clean baseline by stashing changes.
