---
name: solve-testing
description: Reusable changed-slice testing guidance for /solve runs.
---

# Solve Testing

Use this skill whenever `/solve` changes behavior, public contracts, parsing rules, validation logic, or documentation-backed examples.

## Objectives

- Add or update automated tests near the changed behavior.
- Cover the primary path, edge cases, and one regression risk when the task changes behavior.
- When the task adds or changes a public entrypoint, include at least one focused check that exercises or directly verifies that entrypoint rather than only testing private helpers.
- Prefer focused test execution before broad repository validation.

## Workflow

1. Identify the changed slice and the nearest existing tests.
2. Identify whether the task also changes a public entrypoint such as a tool, endpoint, CLI command, or exported contract.
3. Extend current tests before creating new files unless a new surface has no nearby coverage.
4. Add assertions for user-visible behavior, edge cases, and regression risks implied by the task brief.
5. For public-entrypoint tasks, prefer a focused check that proves the entrypoint wiring is usable; if no executable harness exists, require a direct verification of the owning wiring change and report the missing harness as residual risk.
6. Run the narrowest useful check first.
7. If the first focused check fails because of a local defect, hand the result back to `/solve` for same-slice repair and rerun that check.
8. Report uncovered risks when the repository lacks an executable harness for them.

## Guardrails

- Do not add placeholder tests or tautological assertions.
- Do not overfit to the open task's exact sample values.
- Prefer behavior-level assertions over implementation detail checks.
- Do not treat helper-only tests as complete validation when the task's success depends on registration, routing, or other public-surface wiring.
- Keep validation evidence specific enough that the final report can name what passed and what remains risky.

## Focused Validation Order

1. The narrowest changed-slice test or rehearsal available.
2. A focused public-entrypoint check when the task adds or changes a tool, endpoint, command, or other named interface.
3. The narrowest matching typecheck, lint, or build step.
4. Broader repository validation only after the local slice is stable.

## Edge-Case Prompts

- What happens with missing input, empty collections, malformed records, or unreadable files?
- What existing behavior is closest to this change and could regress?
- Does the task add optional flags or branching behavior that needs both enabled and disabled coverage?

See `references/README.md` for shared validation heuristics and cross-links to the `/solve` workflow assets.
