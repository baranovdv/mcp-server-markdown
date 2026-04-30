# Hackathon Rules

## Introduction

## 1. About the Hackathon

Participants receive a repository with the MCP server code and configure an agentic workflow and its environment, not just a single AI agent. An orchestrator slash command is launched in the workflow, and tasks for implementing new features in this product are passed to it. The goal is not to implement the functionality themselves, but to ensure that the agentic workflow performs it reliably and efficiently.

As part of the hackathon, one open test task is published for preparation and trial runs. In addition, there is a hidden task, which we reveal after the hackathon. The workflow is assessed for its performance on both the open and hidden tasks: it is important that the setup helps solve more than just one predetermined problem.

## 2. Task

Participants will work with the `mcp-server-markdown` repository, an MCP server written in TypeScript.

Based on this repository, they will need to create an AI agent workflow and an environment around it so that the workflow can work with this repository: prepare instructions, prompts, automated checks, and additional tools.

Along with this file, the participant receives a task.md, an open task. It requires adding a new tool to the MCP server. It can be used for preparation and test runs.

During the test run, organizers open the GitHub Copilot CLI and submit the task using the command /solve @task.md . The entry point is running the Solve orchestrator command: after this, the workflow must autonomously execute and implement all required end-to-end functionality. In addition to the open task, there is a hidden task—in the same format, but not necessarily with the same change type. This task is used to test how the workflow handles new conditions without prior customization.

## 3. What needs to be submitted

Participants submit their GitHub repository—a fork or copy of the original. The repository can be public or private. If you're concerned about others snooping on your solution, create a private repository and add organizers as collaborators.

The repository must have an agentic workflow configured, but the source code must remain unchanged: you cannot pre-implement a public or private task, nor refactor the server source code to make the workflow more convenient.

To complete the task, use the latest LTS version of Node.js, pnpm, and standard environment conditions.

A mandatory requirement: the entry point is to run the orchestrator with the command `/solve @task.md`.

## 4. Rules

### What you can do

- Add agent instructions: `AGENTS.md`, `.github/copilot-instructions.md`, path-specific instructions in `.github/instructions/`, prompts, and rules.
- Add skills, MCP integrations, and other automation.
- Add universal scripts, specification templates, test generators, and other auxiliary automation if they don't provide a ready-made solution to the problem.
- Improve the repository structure, add documentation, tests, and CI without changing the server's source code in advance.
- Use external MCP integrations, APIs, and internet access if they are launched automatically and do not require manual operator interaction after startup.
- Use subagents, orchestration, and other features of the GitHub Copilot CLI.

This list is not exhaustive—you can add anything that helps the agent better navigate the repository and perform tasks, provided it does not violate the rules in the section below.

### What's not allowed

- Hiding a ready-made solution in prompts, tests, scripts, or documentation.
- Substituting formal stubs for real checks.
- Changing the MCP server source code before the repository is committed, including refactoring for the sake of improving the agent's future performance.
- Requiring manual actions from the operator after startup.

Manual actions after startup include any assistance to the agent after the startup command: chat replies, selecting one of the suggested options, confirming commands, entering environment variables, logging into services, and other similar actions. Ideally, the only action the operator should take is to run `/solve @task.md`.

### Runtime Environment

The official runtime environment is GitHub Copilot CLI on Linux x64. Tasks are submitted by the organizers using the command `/solve @task.md`. To run the task, use the latest LTS version of Node.js, pnpm, and standard environment conditions (including internet access).

## 5. Evaluation Criteria

### How the evaluation is conducted

The agent is run **three times on an open task** and **three times on a hidden task**—each time from a clean copy of the submitted repository. The best run for each task is scored separately.

If the agent stops during the run and asks the operator questions, we will try to answer them, but the result of such a run receives a **-30% penalty**.

A separate score for autonomy and stability takes into account how smoothly the agent runs all three runs: if one run is strong, while the others are noticeably worse or fail, this lowers the stability score.

### Main evaluation categories

| Category                             | Impact | What is assessed                                                                                                               |
| ------------------------------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------ |
| Correctness for open task            | Medium | Did the agent implement the task correctly: does the tool work, is it registered correctly in the MCP, do the test cases pass? |
| Correctness for hidden task          | High   | The same, but for a task that participants did not see in advance                                                              |
| Reliability in unexpected situations | Medium | How does the agent handle incorrect data, corrupted files, large volumes, and other edge cases?                                |
| No Regressions                       | Medium | Did the agent break anything that already worked in the original product?                                                      |
| Quality of Final Changes             | Low    | Readability, Accuracy, and Conformance to the Project Style                                                                    |
| Autonomy and Stability               | Low    | Does the agent work without operator intervention and consistently produce good results across all three runs?                 |

### Bonus Categories

Bonuses are cumulative with the base score.

Bonus tasks are not required to be completed in full, but they offer a relatively easy way to earn extra points.

Our assignment is somewhat educational, and most of the required approaches have already been covered in the sessions. Therefore, we strongly recommend attempting to complete as many of the bonus tasks as possible.

Even if the main pipeline doesn't work as planned or isn't fully implemented, bonus tasks can provide a significant boost to the final score. A main submission is worth significantly more than a bonus submission, but it's possible that no one will solve the main part completely. In such a case, bonus points can influence the distribution of prizes.

Bonus points are also awarded for a high-quality attempt.

| Category                             | Impact | What is assessed                                                                                                                                                                     |
| ------------------------------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Agent and work environment setup     | Medium | How well prepared are the instructions, automation, and environment in which the agent operates                                                                                      |
| Testing quality                      | Medium | Meaningful tests added by the agent: unit tests, integration tests, edge cases—anything that actually verifies the code's functionality                                              |
| Spec-driven workflow                 | Low    | Presence of a specification, plan, and the relationship between them. The use of ready-made frameworks is encouraged, but not limited to BMAD, SPEC Kit, OpenSpec, and similar ones. |
| Security checks and defensive checks | Low    | Explicit security checks built into the workflow                                                                                                                                     |
| Independent code review              | Low    | Code review as one of the agent's work stages—a verification process that actually took place during the task execution                                                              |
| Documentation for Humans             | Low    | Clear description of startup, architecture, and examples                                                                                                                             |
| CI and automated quality checks      | Low    | Configured checks that actually run                                                                                                                                                  |
| Model Selection and Use              | Low    | A meaningful model selection strategy that has been used in practice, and efficient use of premium models                                                                            |
