# Data Model: Agentic Solve Workflow

## Overview

The feature models workflow execution rather than a new MCP server data format. The core entities are the operator-supplied task brief, the runtime solve run, reusable workflow assets, review findings, and validation evidence.

## Entity: Task Brief

**Purpose**: Represents the operator-provided task file consumed by `/solve`.

**Fields**:

| Field               | Type                         | Required | Notes                                                              |
| ------------------- | ---------------------------- | -------- | ------------------------------------------------------------------ |
| `sourcePath`        | string                       | Yes      | Repository-visible path or attachment reference passed to `/solve` |
| `context`           | markdown text                | Yes      | Problem background and repository framing                          |
| `goal`              | markdown text                | Yes      | Requested end-state for the task                                   |
| `inputs`            | structured text              | No       | Sample input schema or arguments if provided                       |
| `outputs`           | structured text              | No       | Expected output or behavior description                            |
| `rules`             | list of strings              | Yes      | Constraints the workflow must preserve                             |
| `examples`          | list of markdown/code blocks | No       | Non-binding examples used to infer patterns                        |
| `acceptanceSignals` | list of strings              | Yes      | Extracted validation expectations used for planning                |

**Validation rules**:

- `sourcePath` must resolve to a readable task file or attachment.
- `goal` and `rules` must be present after intake, even if the original task phrases them indirectly.
- Missing optional sections do not block execution; they trigger repository-local inference and fallback planning.

## Entity: Solve Run

**Purpose**: Tracks one end-to-end `/solve` execution.

**Fields**:

| Field                   | Type            | Required | Notes                                                                                             |
| ----------------------- | --------------- | -------- | ------------------------------------------------------------------------------------------------- |
| `runId`                 | string          | Yes      | Stable identifier, typically timestamp- or hash-based                                             |
| `taskBriefPath`         | string          | Yes      | Link back to the source task brief                                                                |
| `status`                | enum            | Yes      | `intake`, `planned`, `implementing`, `testing`, `reviewing`, `validating`, `completed`, `blocked` |
| `planArtifactPath`      | string          | No       | `.solve/plans/...` artifact for non-trivial runs                                                  |
| `changedFiles`          | list of strings | No       | Files modified during execution                                                                   |
| `repairAttemptsBySlice` | map             | No       | Tracks retries for each failing slice, capped at 2                                                |
| `usedFallbacks`         | list of strings | No       | Repository-local fallbacks used when preferred paths fail                                         |
| `finalReportPath`       | string          | No       | `.solve/reports/.../report.md`                                                                    |

**State transitions**:

| From             | To             | Condition                                                                                               |
| ---------------- | -------------- | ------------------------------------------------------------------------------------------------------- |
| `intake`         | `planned`      | Task goals, constraints, and validation needs extracted                                                 |
| `planned`        | `implementing` | Lightweight plan artifact recorded for non-trivial work                                                 |
| `implementing`   | `testing`      | First implementation slice is ready for focused validation                                              |
| `testing`        | `reviewing`    | Focused checks pass or become a reported blocker                                                        |
| `reviewing`      | `validating`   | High/medium findings repaired or confirmed absent                                                       |
| `validating`     | `completed`    | Required checks and reporting complete                                                                  |
| any active state | `blocked`      | Concrete blocker remains after allowed repair attempts or environment limitation prevents safe progress |

## Entity: Workflow Asset

**Purpose**: Describes a reusable repository asset that shapes solver behavior.

**Fields**:

| Field            | Type            | Required | Notes                                                                             |
| ---------------- | --------------- | -------- | --------------------------------------------------------------------------------- |
| `name`           | string          | Yes      | Asset identifier, such as `solve` or `solve-testing`                              |
| `kind`           | enum            | Yes      | `skill`, `agent`, `prompt`, `instruction`, `workflow-doc`, `report-template`      |
| `path`           | string          | Yes      | Repository-relative location                                                      |
| `role`           | string          | Yes      | Entry point, testing guidance, review, reporting, governance, or fallback support |
| `invocationMode` | enum            | Yes      | `slash`, `auto`, `subagent`, `reference-only`                                     |
| `isTaskSpecific` | boolean         | Yes      | Must remain `false` for reusable workflow assets                                  |
| `dependsOn`      | list of strings | No       | Other assets it references or invokes                                             |

**Validation rules**:

- `isTaskSpecific` must remain `false` for committed workflow assets.
- `path` must stay outside `src/` unless the future implementation task explicitly requires product code changes.
- Entry-point workflow assets must support repository-local fallback behavior or document the fallback they rely on.

## Entity: Review Finding

**Purpose**: Records an outcome from the isolated review stage.

**Fields**:

| Field           | Type            | Required | Notes                                                      |
| --------------- | --------------- | -------- | ---------------------------------------------------------- |
| `id`            | string          | Yes      | Stable identifier within the run                           |
| `severity`      | enum            | Yes      | `high`, `medium`, `low`, `none`                            |
| `summary`       | string          | Yes      | Human-readable finding statement                           |
| `affectedPaths` | list of strings | No       | Changed files involved in the finding                      |
| `evidence`      | string          | Yes      | Reasoning or observed behavior that supports the finding   |
| `status`        | enum            | Yes      | `open`, `repaired`, `advisory`, `dismissed-with-rationale` |

**Validation rules**:

- `high` and `medium` findings are blocking until repaired or converted into an explicit blocker report.
- `low` findings may remain advisory but must still be reported.
- `none` is used only when the review stage explicitly reports no findings.

## Entity: Validation Evidence

**Purpose**: Captures the checks that justify completion.

**Fields**:

| Field            | Type   | Required | Notes                                                                   |
| ---------------- | ------ | -------- | ----------------------------------------------------------------------- |
| `phase`          | enum   | Yes      | `focused-test`, `repair-loop`, `security`, `review`, `final-validation` |
| `commandOrCheck` | string | Yes      | Command name or named non-command check                                 |
| `scope`          | string | Yes      | Changed slice or full repository                                        |
| `result`         | enum   | Yes      | `passed`, `failed`, `blocked`, `not-run`                                |
| `details`        | string | No       | Key output or blocker summary                                           |
| `artifactPath`   | string | No       | Optional log or report location                                         |

**Validation rules**:

- Every successful non-trivial run must include at least one focused validation record and one review record.
- Security evidence is always required, even if it only reports that the changed slice stayed low risk.
- `not-run` is allowed only when paired with an explicit reason in `details`.

## Relationships

- One `Solve Run` consumes one `Task Brief`.
- One `Solve Run` references many `Workflow Asset` entries while executing.
- One `Solve Run` produces zero or more `Review Finding` records.
- One `Solve Run` produces many `Validation Evidence` records.
- `Review Finding` and `Validation Evidence` records are both linked to the same `runId` and final report.
