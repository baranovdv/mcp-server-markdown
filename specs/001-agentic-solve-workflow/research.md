# Research: Agentic Solve Workflow

## Decision: Use a GitHub Copilot skill as the `/solve` entrypoint

**Rationale**: The user explicitly wants `/solve "name of the file with the task"` to be the entrypoint, and a skill is the best match for a repeatable multi-step workflow with bundled references, assets, and instructions. Skills are slash-invocable, support `argument-hint`, and can keep the orchestration guidance together without forcing the entrypoint to be a one-off prompt.

**Alternatives considered**:

- Prompt file entrypoint: rejected because prompts fit a single focused task, while `/solve` has to orchestrate intake, planning, implementation, testing, review, validation, and reporting.
- Custom agent entrypoint: rejected as the primary surface because the user asked for a skill entrypoint; agents remain useful as subordinate isolated roles.

## Decision: Keep testing guidance in a separate `solve-testing` skill

**Rationale**: Testing rules are reusable procedural knowledge rather than a one-time orchestration stage. A separate skill keeps test creation guidance discoverable, reusable across hidden tasks, and isolated from the main solve flow so the workflow does not bury testing behavior in one large entrypoint file.

**Alternatives considered**:

- Inline testing instructions inside the `/solve` skill: rejected because it would make the main skill harder to maintain and easier to overfit to the open task.
- Testing custom agent: rejected because the main need is reusable guidance and templates, not a separate autonomous persona.

## Decision: Use an isolated `solve-review` custom agent for code review

**Rationale**: The spec requires a distinct review stage isolated from implementation. A custom agent with narrow tools and explicit severity rules gives the workflow an independent reviewer context while letting the `/solve` skill delegate review through a controlled subagent handoff.

**Alternatives considered**:

- Review in the same context as implementation: rejected because it merges author and reviewer roles and fails FR-007.
- Manual review checklist only: rejected because the workflow must actually perform a review during autonomous execution.

## Decision: Put prompt-authoring and anti-overfitting rules in `.github/instructions/`

**Rationale**: The workflow needs durable human-readable guidance that explains task decomposition, validation order, fallback behavior, concise reporting, and hidden-task safety. Instructions are the right place for standing guidance that multiple skills or agents can reference without copying text or hiding solutions in executable prompts.

**Alternatives considered**:

- Put all guidance inside `SKILL.md`: rejected because it would overgrow the entrypoint skill and reduce progressive loading.
- Keep guidance only in Spec Kit docs: rejected because maintainers and runtime workflow assets need local, directly discoverable instructions under `.github/`.

## Decision: Record non-trivial run evidence under `.solve/`

**Rationale**: The spec requires a lightweight repository-local plan artifact and explicit validation evidence for every non-trivial run. A dedicated `.solve/` directory keeps runtime plans, reports, and review findings separate from long-lived design docs in `specs/`, supports multiple rehearsal runs, and preserves a repo-local fallback when Spec Kit expansion is unnecessary.

**Alternatives considered**:

- Store runtime evidence only in `specs/`: rejected because every solve run is operational output, not long-lived feature design.
- Keep evidence only in terminal output: rejected because the workflow needs inspectable artifacts that survive the chat session.

## Decision: Validation order should stay narrow first, then broaden before completion

**Rationale**: The repo already exposes `npm test`, `npm run typecheck`, `npm run lint`, and `npm run build`. The workflow should first run changed-slice tests and focused checks, repair locally when possible, apply lightweight security checks to the changed slice, run isolated review, and only then rerun broad validation before declaring success.

**Alternatives considered**:

- Full repository validation first: rejected because it is slower and less diagnostic during local repair loops.
- Skip broad validation after focused checks pass: rejected because the spec requires explicit completion evidence and no-regression confidence.

## Decision: Preserve the current runtime stack and avoid new workflow dependencies unless implementation proves a gap

**Rationale**: The repo is a small strict TypeScript MCP server. Existing Copilot customization primitives, Node.js scripts, Vitest, TypeScript, ESLint, and Prettier already cover the workflow needs. Avoiding new runtime dependencies lowers the risk of hidden-task fragility and keeps the solution additive.

**Alternatives considered**:

- Add orchestration libraries or external workflow engines: rejected because they increase setup cost and create unnecessary new failure modes.
- Depend on mandatory external services: rejected because FR-016 requires a repository-local fallback for core execution.
