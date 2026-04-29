---
name: solve
description: Execute a repository task brief end to end through intake, planning, implementation, testing, review, validation, and reporting.
argument-hint: @path/to/task.md or path/to/task.md
---

# Solve

Use this skill when the operator invokes `/solve @task/task.md` or `/solve task/task.md`.

## Goal

Complete the requested task from a single command without operator follow-up while preserving the repository's existing behavior until a validated task run changes it.

## Non-Negotiable Rules

- Treat the supplied task file as the primary source of truth.
- Start with repository exploration and nearby evidence; do not rely on canned open-task steps.
- Prefer the smallest validated edit slice that can satisfy the next acceptance signal.
- Keep operator interaction optional by default. If a safe repository-local next step exists, take it.
- Use the dedicated testing skill instead of burying all testing logic in this file.
- Run lightweight changed-slice security checks on every run.
- Perform isolated review before completion and treat `high` and `medium` findings as blocking.
- Stop after at most 2 repair attempts for the same failing slice.
- Leave inspectable runtime evidence under `.solve/` for non-trivial runs.

## Required Companion Assets

- Testing guidance: `.github/skills/solve-testing/SKILL.md`
- Report contract: `.github/prompts/solve-report.prompt.md`
- Review agent: `.github/agents/solve-review.agent.md`
- Workflow instructions: `.github/instructions/solve-workflow.instructions.md`
- Prompt-writing guidance: `.github/instructions/solve-prompting.instructions.md`
- Hidden-task safety guidance: `.github/instructions/solve-overfitting.instructions.md`
- Reusable references and templates: `.github/skills/solve/references/README.md`, `.github/skills/solve/assets/intake-template.md`, `.github/skills/solve/assets/plan-template.md`

## Workflow

1. `Resolve task input`
   - Read the task file or attachment.
   - If the path cannot be resolved or read, stop and report a precise blocker.
2. `Task intake`
   - Extract goals, constraints, examples, acceptance signals, and validation needs.
   - Capture them in a lightweight structure that matches `.github/skills/solve/assets/intake-template.md`.
3. `Explore before planning`
   - Inspect the nearest owning implementation, tests, call sites, and docs before editing.
   - Form one falsifiable local hypothesis and one cheap check that could disconfirm it.
4. `Create a plan for non-trivial work`
   - For non-trivial runs, write `.solve/plans/<run-id>-plan.md` using `.github/skills/solve/assets/plan-template.md` before or alongside the first implementation slice.
   - Record intended slices, validation order, likely risks, and fallback paths.
5. `Implement in small slices`
   - Edit one local slice at a time.
   - After the first substantive edit, run a focused validation step before widening scope.
6. `Use the testing skill`
   - Apply `.github/skills/solve-testing/SKILL.md` to create or update changed-slice tests, including edge cases and regression coverage.
7. `Run validation in order`
   - Prefer this order unless the repository provides a better slice-specific check:
     1. changed-slice tests
     2. narrow typecheck, lint, or build step for the touched slice
     3. lightweight changed-slice security checks
     4. isolated review
     5. broad repository validation before completion
8. `Repair locally when possible`
   - If a focused check or review exposes a local, repairable issue, fix the same slice and rerun the same focused validation.
   - After 2 repair attempts for the same slice, convert the issue into a blocker.
9. `Review in isolation`
   - Invoke `.github/agents/solve-review.agent.md` with the task summary, changed files, validation evidence, and relevant diffs or file paths.
   - Do not finish while `high` or `medium` findings remain unresolved.
10. `Report the outcome`

- Produce `.solve/reports/<run-id>/report.md` and any supporting artifacts required by `.github/prompts/solve-report.prompt.md`.

## Fallback Behavior

- If a subagent fails, fall back to repository-local inspection and continue.
- If a preferred focused command is unavailable, use the next-cheapest executable check.
- If a file or tool is malformed but the rest of the task can continue safely, isolate the defect and report it instead of failing the whole run.
- Surface every fallback used in the final report.
