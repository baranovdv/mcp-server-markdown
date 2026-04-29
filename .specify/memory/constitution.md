<!--
Sync Impact Report
Version change: template -> 1.0.0
Modified principles:
- [PRINCIPLE_1_NAME] -> I. Read-Only Document Operations
- [PRINCIPLE_2_NAME] -> II. Stable MCP Tool Contracts
- [PRINCIPLE_3_NAME] -> III. Test-Gated Behavior Changes
- [PRINCIPLE_4_NAME] -> IV. Strict TypeScript and Minimal Dependencies
- [PRINCIPLE_5_NAME] -> V. Additive Functionality Over Rewrites
Added sections:
- Operating Constraints
- Delivery Workflow
Removed sections:
- None
Templates requiring updates:
- ✅ updated .specify/templates/plan-template.md
- ✅ updated .specify/templates/spec-template.md
- ✅ updated .specify/templates/tasks-template.md
Follow-up TODOs:
- None
-->
# mcp-server-markdown Constitution

## Core Principles

### I. Read-Only Document Operations
All shipped functionality MUST preserve the read-only contract of this server.
Features MAY read, search, parse, classify, or extract from markdown content, but
they MUST NOT modify user markdown files, generate in-place edits, or mutate source
code as part of normal tool execution. Any future write-capable behavior requires an
explicit constitution amendment and a separately named surface so read-only clients
do not inherit mutating behavior by surprise. Rationale: the repository promise is a
local-document navigation server, and that trust depends on non-mutating behavior.

### II. Stable MCP Tool Contracts
Existing MCP tools, their intent, and their core output shapes MUST remain backward
compatible. Additional functionality MUST be additive through new tools or clearly
optional parameters, and any incompatible change MUST be documented, justified, and
versioned before implementation. Rationale: MCP clients depend on predictable tool
names, argument contracts, and text-oriented responses.

### III. Test-Gated Behavior Changes
Every behavior change to parsing, search, heading discovery, code block extraction,
frontmatter handling, or path resolution MUST be accompanied by automated tests that
fail before the change and pass after it. Regressions in compatibility, read-only
behavior, and boundary handling MUST be covered in Vitest at the smallest practical
scope. Rationale: this project's value is precise document behavior, and silent
parsing regressions are harder to detect than compile failures.

### IV. Strict TypeScript and Minimal Dependencies
Production changes MUST preserve strict TypeScript discipline, ESM compatibility, and
the current lightweight runtime profile. New dependencies MUST have a clear need that
cannot be satisfied with the standard library or existing packages, and added types
or abstractions MUST reduce ambiguity rather than hide simple behavior. Rationale:
the server is small, portable, and distributed through npm, so unnecessary runtime
weight and loose typing directly reduce maintainability.

### V. Additive Functionality Over Rewrites
Preparation for additional functionality MUST favor extension of the existing module
and tool boundaries over broad rewrites. Plans and implementations MUST explain how a
new capability composes with current read/search/extract flows, what remains stable,
and what new tests and documentation are required. Rationale: the project should be
able to grow without destabilizing the proven read-only core.

## Operating Constraints

The implementation baseline for this repository is a TypeScript ESM MCP server built
with `@modelcontextprotocol/sdk`, validated with `zod`, bundled with `tsup`, and
verified through `vitest`, `tsc --noEmit`, and ESLint plus Prettier checks. Runtime
behavior MUST continue to work against local `.md` files without requiring auth,
remote services, or hidden state. Features that expand capability MUST declare any
path-handling assumptions, result limits, and failure modes in the relevant spec and
user-facing documentation.

## Delivery Workflow

Specifications for new functionality MUST state the read-only impact, MCP contract
impact, and compatibility plan for existing tools. Implementation plans MUST include
a Constitution Check that names affected tools, required tests, and whether the work
is additive or breaking. Task lists MUST schedule test updates, docs updates, and
validation steps for `npm test`, `npm run typecheck`, and `npm run lint` whenever the
changed slice can exercise them. Review and analysis artifacts MUST treat conflicts
with this constitution as blocking until resolved by design or by explicit
constitution amendment.

## Governance

This constitution supersedes informal workflow preferences for repository changes.
Amendments require a documented rationale, a description of affected principles or
sections, and synchronized updates to dependent Spec Kit templates before adoption.
Versioning follows semantic intent: MAJOR for incompatible governance changes or
principle removals, MINOR for new principles or materially expanded obligations, and
PATCH for wording clarifications that do not change required behavior. Every plan,
task list, review, and analysis deliverable MUST include a compliance check against
these principles. Runtime development guidance remains anchored in README.md and the
repository test/build commands.

**Version**: 1.0.0 | **Ratified**: 2026-04-28 | **Last Amended**: 2026-04-28
