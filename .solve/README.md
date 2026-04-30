# Solve Runtime Artifacts

The `/solve` workflow writes runtime evidence under this directory.

## Layout

- `plans/` stores lightweight execution plans for non-trivial runs.
- `reports/` stores final reports, review findings, and validation evidence.

## Naming

- Use a stable run id in file and folder names, for example `<timestamp>-<task-slug>`.
- Preferred pattern: `.solve/plans/<run-id>-plan.md` and `.solve/reports/<run-id>/`.

## Retention

- Keep the latest local rehearsal artifacts long enough to inspect failures and review outcomes.
- Delete stale generated run output when it no longer helps debugging or comparison.
- Preserve the committed scaffold files and documentation even when generated artifacts are cleaned up.

## Commit Policy

Keep the directory structure and documentation in the repository, but do not commit generated runtime artifacts from individual runs.
