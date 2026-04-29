# Solve Testing References

This directory stores reusable testing guidance and examples for the `/solve` workflow.

## Companion Assets

- `../SKILL.md`: reusable testing workflow.
- `../../solve/SKILL.md`: `/solve` entrypoint.
- `../../../prompts/solve-report.prompt.md`: final reporting requirements.
- `../../../agents/solve-review.agent.md`: isolated review stage.

## Testing Heuristics

- Prefer the nearest existing test file before adding a new one.
- Cover the changed behavior, one realistic edge case, and one regression risk when behavior changes.
- Reuse repository scripts for broad validation only after focused checks pass.
- Report missing harnesses or untestable risks explicitly instead of pretending they are covered.

## Cross-Links

- Runtime artifact layout: `../../../../.solve/README.md`
- Workflow instructions: `../../../instructions/solve-workflow.instructions.md`
- Hidden-task safety: `../../../instructions/solve-overfitting.instructions.md`
