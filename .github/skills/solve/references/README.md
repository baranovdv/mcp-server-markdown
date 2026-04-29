# Solve References

This directory stores reusable reference material for the `/solve` workflow.

## Primary Assets

- `../SKILL.md`: the `/solve` entrypoint.
- `../assets/intake-template.md`: lightweight task-intake structure.
- `../assets/plan-template.md`: non-trivial run plan structure.

## Companion Workflow Assets

- `../../solve-testing/SKILL.md`: dedicated testing guidance.
- `../../../prompts/solve-report.prompt.md`: final reporting contract.
- `../../../agents/solve-review.agent.md`: isolated review stage.
- `../../../instructions/solve-workflow.instructions.md`: stage-by-stage operating guidance.
- `../../../instructions/solve-prompting.instructions.md`: prompt-authoring rules.
- `../../../instructions/solve-overfitting.instructions.md`: hidden-task safety and fallback rules.

## Usage Notes

- Keep durable guidance here instead of embedding task-specific steps in `SKILL.md`.
- Keep references task-agnostic so they remain valid for both open and hidden tasks.
- Use `.solve/README.md` for runtime artifact layout and retention guidance.

## Cross-Check

When `/solve` changes, verify that `README.md`, `specs/001-agentic-solve-workflow/quickstart.md`, and `specs/001-agentic-solve-workflow/contracts/solve-skill-contract.md` still describe the same flow.
