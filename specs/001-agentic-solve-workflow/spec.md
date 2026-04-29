# Feature Specification: Agentic Solve Workflow

**Feature Branch**: `[001-agentic-solve-workflow]`  
**Created**: 2026-04-29  
**Status**: Draft  
**Input**: User description: "You need to implement agentic workflow that is described in Rules(fixed).md. You can use task.md as an example of the input for the agentic workflow. You have to use AI agentic workflow best practices like using Subagents for Code Review, using separate skill for tests creation, and explicitly highlight prompt creation best practices."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - One-Command Task Execution (Priority: P1)

As a hackathon operator, I can run `/solve @task.md` in a clean copy of the repository and the workflow carries the task from intake through repository changes and validation without requiring follow-up answers from me.

**Why this priority**: The competition entry is judged first on whether a single orchestrator command can complete real tasks autonomously under the published runtime constraints.

**Independent Test**: Can be fully tested by running `/solve @task.md` against a clean repository copy and confirming the workflow produces the required repository updates, validation evidence, and final report without operator interaction.

**Acceptance Scenarios**:

1. **Given** a clean repository copy and a task attachment in the published format, **When** the operator runs `/solve @task.md`, **Then** the workflow starts from that single command, interprets the task, and executes the required stages without asking the operator to choose options or provide missing values.
2. **Given** a task that requires code, tests, and documentation updates, **When** the workflow completes, **Then** it leaves behind concrete repository changes and a final report that states what was changed, what was validated, and whether any blockers remain.

---

### User Story 2 - Trustworthy Autonomous Delivery (Priority: P2)

As a hackathon participant, I need the workflow to validate its own work, create meaningful tests, and run an independent review so that successful runs are reliable and not just superficially complete.

**Why this priority**: Correctness, no regressions, and stability are core scoring criteria, and a workflow that cannot verify itself will fail even if it can edit files.

**Independent Test**: Can be fully tested by giving the workflow a representative feature task and confirming that it generates or updates tests for the changed slice, runs the relevant checks, performs a separate review step, and reports failures before finishing.

**Acceptance Scenarios**:

1. **Given** a task that changes behavior, **When** the workflow implements it, **Then** the workflow also creates or updates automated tests that exercise the new behavior and any identified edge cases.
2. **Given** a run with newly produced changes, **When** the workflow reaches its verification stage, **Then** it performs a review stage isolated from the main implementation stage and surfaces findings or explicitly states that no findings were detected.
3. **Given** a validation failure or review finding, **When** the issue is local and repairable, **Then** the workflow iterates on the affected slice and reruns the focused checks before presenting the run as complete.

---

### User Story 3 - Reusable Workflow Guidance (Priority: P3)

As a maintainer of the repository workflow, I need reusable prompts, skills, and instructions that explain how the solver should parse tasks, choose stages, create tests, and avoid overfitting to the open task so that the setup remains effective on hidden tasks.

**Why this priority**: The submission is evaluated on both the published task and an unseen task, so the workflow must encode general operating guidance rather than a single prepared solution.

**Independent Test**: Can be fully tested by reviewing the workflow assets and confirming they describe task intake, planning, implementation, testing, code review, and final reporting in a reusable way that is not hard-coded to one feature request.

**Acceptance Scenarios**:

1. **Given** a new task in the same general format but with a different change type, **When** the solver begins, **Then** it uses repository exploration and shared workflow guidance to determine its plan instead of relying on task-specific canned steps.
2. **Given** a maintainer reviewing the workflow assets, **When** they inspect the repository, **Then** they can identify where prompt design rules, testing guidance, and review-stage guidance live and how each part contributes to the `/solve` flow.

### Edge Cases

- The workflow receives a task with incomplete examples or missing implementation hints and must still infer a reasonable next step without asking the operator for clarification.
- A subagent or search path fails temporarily, and the main workflow must fall back to local inspection or alternate validation rather than terminating prematurely.
- The task touches an unexpected surface, and the workflow must preserve existing behavior while limiting changes to the smallest validated slice.
- The workflow finds unrelated dirty files or generated artifacts in the repository and must avoid reverting or corrupting them while continuing the requested work.
- The task cannot be completed fully because of a real repository or environment blocker, and the workflow must stop with a precise blocker report rather than pretending success.

## Compatibility & Safety Constraints *(mandatory)*

- **Read-Only Impact**: Existing markdown-facing MCP capabilities remain read-only; the workflow customization itself may add or update repository automation assets, but it must not introduce pre-shipped write-capable document tools or mutate repository source before `/solve` is run for an actual task.
- **Existing Tool Contract Impact**: None for the current MCP server tools; the new capability is an additive workflow entrypoint and supporting automation around the repository.
- **Additive vs. Breaking Change**: Additive, because the feature introduces orchestration guidance, review stages, and validation assets without changing the public contract of existing markdown tools.
- **Required Compatibility Safeguards**: Preserve the current read-only MCP server behavior; do not pre-implement the published or hidden task in source files; require changed-slice automated checks before completion; keep operator interaction optional by defaulting to autonomous execution; document the workflow surfaces so future maintainers can extend them without breaking `/solve`.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The repository MUST provide a `/solve` workflow entrypoint that accepts a task attachment in the format used by the hackathon organizers and treats that attachment as the primary task brief.
- **FR-002**: The workflow MUST execute from that single entrypoint without requiring manual responses, confirmations, secret entry, or option selection after startup under the stated standard environment.
- **FR-003**: The workflow MUST derive its plan from the supplied task, repository context, and reusable workflow guidance rather than embedding a ready-made solution for the published task.
- **FR-004**: The workflow MUST include an explicit task-intake stage that extracts goals, constraints, acceptance expectations, and validation needs from the task brief before implementation begins.
- **FR-005**: The workflow MUST include a planning stage for non-trivial work that records the intended execution path in repository artifacts that can be inspected in later stages.
- **FR-006**: The workflow MUST include a dedicated testing guidance asset that tells the solver how to create and run meaningful tests for the changed slice, including edge cases and regression coverage.
- **FR-007**: The workflow MUST include a distinct code review stage performed by an isolated subagent or equivalent isolated reviewer context so implementation and review are not merged into one unchecked pass.
- **FR-008**: The workflow MUST include prompt-authoring guidance that explicitly covers clear task decomposition, requirement coverage, validation order, fallback behavior, and concise final reporting.
- **FR-009**: The workflow MUST instruct the solver to prefer repository-local evidence, narrow validation, and minimal edits, and it MUST surface when a broader search or fallback path was needed.
- **FR-010**: The workflow MUST preserve the repository rule that current MCP server functionality is not pre-modified to hide a solution ahead of task execution.
- **FR-011**: The workflow MUST run the relevant automated checks for the touched slice before presenting the run as complete and MUST report which checks passed, failed, or could not run.
- **FR-012**: The workflow MUST repair local failures when possible and rerun the focused validation before moving on, rather than accumulating unchecked edits across multiple areas.
- **FR-013**: The workflow MUST tolerate bad intermediate files, partial exploration failures, and unexpected task shapes without failing the entire run unless a concrete blocker prevents safe progress.
- **FR-014**: The workflow MUST document how its prompts, skills, agents, hooks, and validation steps fit together so a maintainer can update the workflow without reverse-engineering it.
- **FR-015**: The workflow MUST preserve backward compatibility for all existing MCP tools unless a future task explicitly requires a documented change to those tool contracts.

### Key Entities *(include if feature involves data)*

- **Task Brief**: The operator-supplied task attachment that defines the requested change, constraints, examples, and success expectations for a run.
- **Solve Workflow**: The end-to-end orchestration path triggered by `/solve`, including intake, planning, implementation, testing, review, and final reporting.
- **Workflow Asset**: Any maintained instruction, prompt, skill, hook, checklist, or supporting document that shapes solver behavior in a reusable way.
- **Review Finding**: A structured result from the independent review stage identifying a defect, risk, regression concern, or explicit no-finding outcome.
- **Validation Evidence**: The recorded outcome of tests, linting, type checks, or other focused checks run to support the solver's completion decision.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: In rehearsal runs using clean repository copies and task briefs in the published format, the workflow can be started with one `/solve` command and reaches a terminal outcome without operator follow-up in at least 95% of runs.
- **SC-002**: For representative feature tasks that require code changes, 100% of successful runs include validation evidence for the changed slice and an explicit review outcome before the final report.
- **SC-003**: For representative non-trivial tasks, 100% of successful runs leave behind inspectable planning artifacts that show the intended execution path before or alongside implementation.
- **SC-004**: Maintainers can identify where task-intake guidance, testing guidance, prompt guidance, and review guidance live in the repository within 5 minutes using only repository documentation and file layout.
- **SC-005**: The workflow completes the published open task without breaking the existing baseline quality gates that apply to the touched slice.

## Assumptions

- Organizers will execute the workflow in GitHub Copilot CLI on Linux x64 with current Node.js LTS, `pnpm`, and normal network access, matching the published rules.
- Task inputs will resemble the provided example by stating context, goals, inputs, outputs, rules, and examples, even when the requested change type differs.
- The repository may continue to use Spec Kit artifacts as part of the workflow, but the workflow remains responsible for finishing end-to-end task execution rather than stopping after planning.
- The solution may add or refine workflow assets, documentation, CI checks, and reusable automation, but it must not hide a task-specific implementation in those assets.