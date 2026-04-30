# Implementation Plan: Agentic Solve Workflow

**Branch**: `[001-agentic-solve-workflow]` | **Date**: 2026-04-29 | **Spec**: [spec.md](./spec.md)
**Input**: Feature specification from `/specs/001-agentic-solve-workflow/spec.md`

**Note**: This plan defines workflow assets only. It keeps the shipped MCP server unchanged until `/solve` is invoked against a real task file.

## Summary

Add a user-invocable GitHub Copilot skill at `.github/skills/solve/` so operators can start the workflow with `/solve @task.md` or `/solve task/task.md`. The skill will orchestrate task intake, lightweight repo-local planning, implementation, a separate testing skill, isolated code review via subagent, focused validation, lightweight security checks, and final reporting while preserving the existing read-only MCP tool surface and keeping a repository-local fallback path for every core stage.

## Technical Context

**Language/Version**: TypeScript 5.7, Node.js LTS runtime, GitHub Copilot CLI slash-command workflow  
**Primary Dependencies**: `@modelcontextprotocol/sdk`, `zod`, `vitest`, `tsup`, `eslint`, `prettier`, existing Spec Kit assets  
**Storage**: Local repository files only; workflow artifacts under `.solve/` plus long-lived design docs under `specs/001-agentic-solve-workflow/`  
**Testing**: `vitest run`, `tsc --noEmit`, `eslint . && prettier --check .`, plus focused rehearsal runs of `/solve` against task files  
**Target Platform**: GitHub Copilot CLI on Linux x64 for evaluation; repository development remains portable across macOS/Linux  
**Project Type**: Single-package TypeScript MCP server with workspace-scoped Copilot skills, agents, prompts, and instructions  
**Performance Goals**: Single-command autonomous start, changed-slice validation before broad checks, repository-local fallback when subagents or external services fail  
**Constraints**: No pre-implemented open or hidden task solution, no operator interaction after startup, preserve read-only MCP behavior, keep existing tool contracts backward compatible, stop after at most 2 repair attempts on the same failing slice  
**Scale/Scope**: One source module pair (`src/index.ts`, `src/markdown.ts`), one current Vitest file, existing CI workflows, new `.github/` customization assets, and optional `.solve/` runtime evidence for each non-trivial run

## Constitution Check

_GATE: Must pass before Phase 0 research. Re-check after Phase 1 design._

- Read-only guarantee preserved: PASS. The plan adds `.github/` workflow assets, `specs/` design docs, and `.solve/` runtime evidence only; it does not pre-ship write-capable markdown behavior or mutate repository source before `/solve` runs.
- MCP compatibility defined: PASS. Existing MCP tools (`list_files`, `search_docs`, `get_section`, `list_headings`, `find_code_blocks`, `get_frontmatter`) stay unchanged; `/solve` is an additive Copilot workflow entrypoint rather than a server contract change.
- Test coverage planned: PASS. The workflow will require changed-slice Vitest coverage for future product edits, plus rehearsal coverage proving `/solve` can intake a task, leave a plan artifact, run review, and report validation evidence.
- TypeScript and dependency discipline preserved: PASS. The workflow should prefer existing Node.js, TypeScript, Vitest, ESLint, and Copilot customization primitives; any new dependency must be justified during implementation because the current stack already covers orchestration, validation, and reporting.
- Validation commands scoped: PASS. Planned validation commands are `npm test`, `npm run typecheck`, `npm run lint`, `npm run build`, lightweight changed-slice security checks, and an isolated review stage before completion.

### Post-Design Re-Check

- Read-only guarantee preserved after design: PASS. The designed surfaces are skill, instruction, agent, and documentation files plus `.solve/` output directories; none alter normal MCP execution semantics.
- MCP compatibility preserved after design: PASS. The workflow contract lives outside `src/` and does not redefine current tool inputs or outputs.
- Test and validation plan remains enforceable: PASS. The design keeps both focused checks and full-repo verification in scope and records them as runtime outputs rather than unstated expectations.
- Dependency discipline preserved after design: PASS. No new runtime libraries are required by the planned architecture.
- Complexity justified: PASS. Separate assets are used only where the spec explicitly requires isolation or reuse: entrypoint skill, dedicated testing skill, isolated reviewer agent, and maintainable workflow guidance.

## Project Structure

### Documentation (this feature)

```text
specs/001-agentic-solve-workflow/
├── plan.md
├── research.md
├── data-model.md
├── quickstart.md
├── contracts/
│   └── solve-skill-contract.md
└── tasks.md
```

### Source Code (repository root)

```text
.github/
├── agents/
│   └── solve-review.agent.md
├── copilot-instructions.md
├── instructions/
│   ├── solve-workflow.instructions.md
│   ├── solve-prompting.instructions.md
│   └── solve-overfitting.instructions.md
├── prompts/
│   └── solve-report.prompt.md
├── skills/
│   ├── solve/
│   │   ├── SKILL.md
│   │   ├── references/
│   │   └── assets/
│   └── solve-testing/
│       ├── SKILL.md
│       ├── references/
│       └── assets/
├── workflows/
│   └── solve.rehearsal.yml
└── prompts/
    ├── speckit.plan.prompt.md
    └── ...existing Spec Kit prompts

skills/
└── markdown-search/
    └── SKILL.md

src/
├── index.ts
└── markdown.ts

tests/
└── markdown.test.ts

task/
├── Rules(fixed).md
└── task.md

.solve/
├── plans/
└── reports/
```

**Structure Decision**: Keep the repository as a single TypeScript package and add workflow surfaces beside the existing code rather than inside it. The `/solve` entrypoint lives in `.github/skills/solve/`, reusable testing guidance lives in `.github/skills/solve-testing/`, independent review lives in `.github/agents/solve-review.agent.md`, durable maintainer guidance lives in `.github/instructions/`, and per-run evidence lives under `.solve/` so the workflow can leave inspectable artifacts without polluting `src/` or pre-baking a task solution.

## Complexity Tracking

No constitution violations or design exceptions are currently required.
