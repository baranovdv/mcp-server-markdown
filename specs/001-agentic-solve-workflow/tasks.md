---

description: "Implementation tasks for the agentic solve workflow feature"
---

# Tasks: Agentic Solve Workflow

**Input**: Design documents from `/specs/001-agentic-solve-workflow/`
**Prerequisites**: plan.md, spec.md, research.md, data-model.md, quickstart.md, contracts/solve-skill-contract.md

**Tests**: This feature changes the operator-facing workflow contract and validation behavior, so each story includes rehearsal or validation tasks plus final repository checks.

**Organization**: Tasks are grouped by user story so each increment can be implemented and verified independently.

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Create the repository surfaces that the workflow assets will occupy.

- [ ] T001 Create workflow directory scaffolding in `.github/skills/solve/`, `.github/skills/solve-testing/`, `.github/instructions/`, `.solve/plans/`, and `.solve/reports/`
- [ ] T002 Create placeholder asset files in `.github/skills/solve/references/README.md`, `.github/skills/solve/assets/.gitkeep`, `.github/skills/solve-testing/references/README.md`, and `.github/skills/solve-testing/assets/.gitkeep`
- [ ] T003 [P] Create a runtime artifact guide in `.solve/README.md`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Establish shared guidance and reporting surfaces required by every user story.

**⚠️ CRITICAL**: No user story work should start before this phase is complete.

- [ ] T004 Create the shared final report prompt in `.github/prompts/solve-report.prompt.md`
- [ ] T005 [P] Update `.github/copilot-instructions.md` with additive `/solve` workflow constraints and references to reusable assets
- [ ] T006 [P] Add maintainer-facing workflow overview and compatibility notes to `README.md`

**Checkpoint**: Shared workflow surfaces exist and all user-story work can proceed.

---

## Phase 3: User Story 1 - One-Command Task Execution (Priority: P1) 🎯 MVP

**Goal**: Provide a slash-invocable `/solve` entrypoint that can take a task file, perform task intake, create a lightweight plan for non-trivial work, and finish with a usable report.

**Independent Test**: Run `/solve @task/task.md` or `/solve task/task.md` and confirm the workflow can proceed from task intake through reporting without asking the operator for follow-up input.

### Tests for User Story 1

- [ ] T007 [P] [US1] Add one-command rehearsal coverage for `/solve @task/task.md` to `.github/workflows/solve.rehearsal.yml`
- [ ] T008 [P] [US1] Add operator rehearsal steps and expected intake/report outcomes to `specs/001-agentic-solve-workflow/quickstart.md`

### Implementation for User Story 1

- [ ] T009 [US1] Implement the `/solve` entrypoint skill in `.github/skills/solve/SKILL.md`
- [ ] T010 [P] [US1] Add task-intake and planning reference material in `.github/skills/solve/references/README.md`
- [ ] T011 [P] [US1] Add reusable intake and plan artifact placeholders in `.github/skills/solve/assets/intake-template.md` and `.github/skills/solve/assets/plan-template.md`
- [ ] T012 [US1] Wire final reporting expectations for plan artifacts, changed files, and blockers into `.github/prompts/solve-report.prompt.md`

**Checkpoint**: `/solve` can be invoked from a task file and leaves the user with clear intake, planning, and reporting guidance.

---

## Phase 4: User Story 2 - Trustworthy Autonomous Delivery (Priority: P2)

**Goal**: Ensure the workflow validates its work, creates meaningful tests, runs lightweight security checks, and performs an isolated severity-based review before completion.

**Independent Test**: Rehearse a behavior-changing task and confirm the workflow updates tests, runs focused validation, invokes an isolated reviewer, and repairs or reports blocking findings before completion.

### Tests for User Story 2

- [ ] T013 [P] [US2] Add validation, repair-loop, and review rehearsal assertions to `.github/workflows/solve.rehearsal.yml`
- [ ] T014 [P] [US2] Add focused validation and blocker-report scenarios to `specs/001-agentic-solve-workflow/quickstart.md`

### Implementation for User Story 2

- [ ] T015 [P] [US2] Create the reusable testing guidance skill in `.github/skills/solve-testing/SKILL.md`
- [ ] T016 [P] [US2] Create the isolated reviewer in `.github/agents/solve-review.agent.md`
- [ ] T017 [US2] Extend `.github/skills/solve/SKILL.md` with changed-slice validation order, lightweight security checks, and up-to-2 repair-loop rules
- [ ] T018 [US2] Extend `.github/prompts/solve-report.prompt.md` with severity-tagged review findings and validation evidence requirements

**Checkpoint**: The workflow can validate and review its own changes and will not present high- or medium-severity issues as a success.

---

## Phase 5: User Story 3 - Reusable Workflow Guidance (Priority: P3)

**Goal**: Capture reusable instructions that explain stage flow, prompt-writing discipline, and anti-overfitting rules so the workflow remains effective on hidden tasks.

**Independent Test**: Inspect the committed workflow assets and confirm a maintainer can locate task-intake, prompt-authoring, testing, review, and hidden-task safety guidance within a few minutes.

### Tests for User Story 3

- [ ] T019 [P] [US3] Add hidden-task rehearsal and fallback checks to `specs/001-agentic-solve-workflow/quickstart.md`
- [ ] T020 [P] [US3] Add maintainer discovery guidance for workflow assets to `README.md`

### Implementation for User Story 3

- [ ] T021 [P] [US3] Create stage-by-stage workflow guidance in `.github/instructions/solve-workflow.instructions.md`
- [ ] T022 [P] [US3] Create prompt-authoring best practices in `.github/instructions/solve-prompting.instructions.md`
- [ ] T023 [P] [US3] Create hidden-task and anti-overfitting guidance in `.github/instructions/solve-overfitting.instructions.md`
- [ ] T024 [US3] Update `.github/copilot-instructions.md` so the new instructions, testing skill, and review agent are discoverable from the main repository guidance

**Checkpoint**: Maintainers can understand and extend the `/solve` workflow without reverse-engineering the entrypoint skill.

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Finish cross-story documentation, rehearsal wiring, and required validation.

- [ ] T025 [P] Cross-link workflow assets in `.github/skills/solve/references/README.md` and `.github/skills/solve-testing/references/README.md`
- [ ] T026 [P] Document runtime artifact naming and retention expectations in `.solve/README.md`
- [ ] T027 Reconcile `specs/001-agentic-solve-workflow/contracts/solve-skill-contract.md` with the implemented `/solve` asset surfaces and report outputs
- [ ] T028 Run `npm test`
- [ ] T029 Run `npm run typecheck`
- [ ] T030 Run `npm run lint`
- [ ] T031 Run `npm run build`

---

## Dependencies & Execution Order

### Phase Dependencies

- Setup (Phase 1) starts immediately.
- Foundational (Phase 2) depends on Setup and blocks all user stories.
- User Story 1 (Phase 3) depends on Foundational and is the MVP slice.
- User Story 2 (Phase 4) depends on the `/solve` entrypoint from User Story 1.
- User Story 3 (Phase 5) depends on the shared workflow surfaces from Foundational and should follow the stable story flows established in User Stories 1 and 2.
- Polish (Phase 6) depends on the user stories selected for delivery.

### User Story Dependencies

- User Story 1 (P1) has no dependency on other stories once Foundational is complete.
- User Story 2 (P2) depends on User Story 1 because validation, repair, and review rules extend the `/solve` flow.
- User Story 3 (P3) depends on User Stories 1 and 2 so the reusable instructions describe the final workflow rather than a partial draft.

### Within Each User Story

- Rehearsal or validation tasks should be written before the corresponding implementation tasks.
- Shared prompts and instructions should be updated before the entrypoint skill consumes them.
- Reporting updates should land before final rehearsal or repository-wide validation.

## Parallel Opportunities

- T003 can run alongside T001-T002 once the directory layout exists.
- T005 and T006 can proceed in parallel after T004 defines the shared reporting surface.
- In User Story 1, T007 and T008 can run in parallel, and T010-T011 can run in parallel after T009 defines the entrypoint shape.
- In User Story 2, T013-T014 can run in parallel, and T015-T016 can run in parallel before T017-T018 integrate them into the main flow.
- In User Story 3, T019-T020 can run in parallel, and T021-T023 can run in parallel before T024 refreshes the top-level instructions.
- In Polish, T025-T026 can run in parallel before the final contract reconciliation and validation commands.

## Parallel Example: User Story 2

```bash
# Prepare validation rehearsal coverage in parallel:
Task: "Add validation, repair-loop, and review rehearsal assertions to .github/workflows/solve.rehearsal.yml"
Task: "Add focused validation and blocker-report scenarios to specs/001-agentic-solve-workflow/quickstart.md"

# Build the isolated quality assets in parallel:
Task: "Create the reusable testing guidance skill in .github/skills/solve-testing/SKILL.md"
Task: "Create the isolated reviewer in .github/agents/solve-review.agent.md"
```

## Implementation Strategy

### MVP First

1. Complete Phase 1: Setup.
2. Complete Phase 2: Foundational.
3. Complete Phase 3: User Story 1.
4. Rehearse `/solve` against `task/task.md` before expanding scope.

### Incremental Delivery

1. Deliver User Story 1 to establish the one-command entrypoint.
2. Add User Story 2 to make the workflow trustworthy through tests, repair loops, security checks, and isolated review.
3. Add User Story 3 to make the workflow reusable and safe for hidden tasks.
4. Finish with Phase 6 validation and contract reconciliation.

### Parallel Team Strategy

1. One contributor handles the shared prompt and top-level docs in Phases 1-2.
2. A second contributor can build the testing skill and review agent while the `/solve` entrypoint is being stabilized.
3. A third contributor can draft the instruction files once the story flows are clear, then reconcile them through T024.

## Notes

- All tasks follow the required checklist format with explicit file paths.
- Tests are represented as rehearsal, validation, and workflow-coverage tasks because this feature primarily adds Copilot workflow assets rather than runtime product code.
- The final validation phase includes the constitution-required repository commands: `npm test`, `npm run typecheck`, `npm run lint`, and `npm run build`.