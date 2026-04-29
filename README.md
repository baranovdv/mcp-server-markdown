# mcp-server-markdown

[![npm version](https://img.shields.io/npm/v/mcp-server-markdown.svg)](https://www.npmjs.com/package/mcp-server-markdown)
[![npm downloads](https://img.shields.io/npm/dm/mcp-server-markdown.svg)](https://www.npmjs.com/package/mcp-server-markdown)
[![CI](https://github.com/ofershap/mcp-server-markdown/actions/workflows/ci.yml/badge.svg)](https://github.com/ofershap/mcp-server-markdown/actions/workflows/ci.yml)
[![TypeScript](https://img.shields.io/badge/TypeScript-strict-blue.svg)](https://www.typescriptlang.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Search, navigate, and extract content from local markdown files. Full-text search, section extraction, heading navigation, code block discovery, and frontmatter parsing.

```bash
npx mcp-server-markdown
```

> Works with Claude Desktop, Cursor, VS Code Copilot, and any MCP client. Reads local `.md` files, no auth needed.

![MCP server for searching and navigating markdown documentation](assets/demo.gif)

<sub>Demo built with <a href="https://github.com/ofershap/remotion-readme-kit">remotion-readme-kit</a></sub>

## Why

Tools like Context7 are great for looking up library docs from npm, but they don't help with your own documentation. Project wikis, internal knowledge bases, architecture decision records, onboarding guides: they all live as markdown files in your repo or on disk. The filesystem MCP server can read those files, but it treats them as raw text. It doesn't understand headings, sections, or code blocks. This server does. Point it at a directory and your assistant can search across all your docs, pull out a specific section by heading, list the table of contents, or find every TypeScript code example in your knowledge base.

## Tools

| Tool               | What it does                                                               |
| ------------------ | -------------------------------------------------------------------------- |
| `list_files`       | List all .md files in a directory recursively (sorted alphabetically)      |
| `search_docs`      | Full-text search across all .md files (case-insensitive, up to 50 results) |
| `get_section`      | Extract a section by heading until the next heading of same/higher level   |
| `list_headings`    | List all headings as a table of contents                                   |
| `find_code_blocks` | Find fenced code blocks, optionally filter by language (e.g. typescript)   |
| `get_frontmatter`  | Parse YAML frontmatter metadata at the start of a file                     |

## Quick Start

### Cursor

Add to `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "markdown": {
      "command": "npx",
      "args": ["-y", "mcp-server-markdown"]
    }
  }
}
```

### Claude Desktop

Add to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "markdown": {
      "command": "npx",
      "args": ["-y", "mcp-server-markdown"]
    }
  }
}
```

### VS Code

Add to user settings or `.vscode/mcp.json`:

```json
{
  "mcp": {
    "servers": {
      "markdown": {
        "command": "npx",
        "args": ["-y", "mcp-server-markdown"]
      }
    }
  }
}
```

## Examples

- "Search all docs in ./docs for mentions of 'authentication'"
- "Show me the 'API Reference' section from README.md"
- "List all headings in CONTRIBUTING.md"
- "Find all TypeScript code blocks in the docs"
- "What's the frontmatter metadata in this file?"
- "Give me the table of contents for our architecture docs"

## Copilot `/solve` Workflow

This repository also includes a GitHub Copilot workflow for end-to-end task execution. The entrypoint is `/solve @task/task.md` or `/solve task/task.md`.

### Maintainer Asset Map

- `.github/skills/solve/SKILL.md`: primary task-intake, planning, implementation, validation, and reporting workflow.
- `.github/skills/solve-testing/SKILL.md`: reusable changed-slice testing guidance.
- `.github/agents/solve-review.agent.md`: isolated reviewer that classifies findings as `high`, `medium`, `low`, or `none`.
- `.github/prompts/solve-report.prompt.md`: final reporting contract for changed files, validation evidence, review findings, and blockers.
- `.github/instructions/`: durable workflow, prompting, and hidden-task safety guidance.
- `.solve/`: runtime plan and report artifacts for non-trivial runs.
- `.github/workflows/solve.rehearsal.yml`: static rehearsal checks that keep the workflow assets aligned.

### Compatibility Notes

- The workflow is additive and does not change the published MCP tool contract by itself.
- Committed assets must stay task-agnostic. They should guide the solver to explore the repository and derive a plan instead of embedding a solution for the open task.
- Runtime artifacts under `.solve/` are for inspection and rehearsal evidence; generated run output should not be committed.

## Development

```bash
git clone https://github.com/ofershap/mcp-server-markdown.git
cd mcp-server-markdown
npm install
npm test
npm run build
```

## See also

More MCP servers and developer tools on my [portfolio](https://gitshow.dev/ofershap).

## Author

[![Made by ofershap](https://gitshow.dev/api/card/ofershap)](https://gitshow.dev/ofershap)

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://linkedin.com/in/ofershap)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat&logo=github&logoColor=white)](https://github.com/ofershap)

---

<sub>README built with [README Builder](https://ofershap.github.io/readme-builder/)</sub>

## License

MIT © 2026 Ofer Shapira
