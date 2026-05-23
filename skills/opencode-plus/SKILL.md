---
name: opencode-plus
description: >
  Activate when the user needs guidance on best practices for OpenCode,
  including agent orchestration, subagent patterns, skill authoring,
  code review workflows, and CI/CD integration with OpenCode.
version: 1.0.0
author: abi_jey
---

# OpenCode Plus

Comprehensive guidance for getting the most out of OpenCode in your
development workflow.

## When to Activate

- Setting up or configuring OpenCode for a project
- Authoring agent skills, plugins, or MCP servers
- Designing multi-agent workflows with subagents
- Debugging OpenCode behavior or prompt quality
- Integrating OpenCode into CI/CD pipelines
- Reviewing PRs with OpenCode's review capabilities

## Agent Orchestration

- Use **subagent_type** `explore` for codebase search, `general` for
  multi-step autonomous tasks
- Fan out parallel agents for independent work; serialize only when
  there are data dependencies
- Give each agent a clear, bounded task description with an explicit
  deliverable format

### Task Tool Parameters

| Param | Required | Description |
|-------|----------|-------------|
| `description` | Yes | 3-5 word summary of the task |
| `prompt` | Yes | Detailed instructions for the subagent |
| `subagent_type` | Yes | Agent to use: `general`, `explore`, `scout`, or custom |
| `task_id` | No | Resume a previous subagent session |
| `background` | No | Launch asynchronously (experimental) |

### Continuing Subagent Sessions

Every task call returns a `task_id`. Pass it to `task_id` to continue the
same session with full conversation history:

```
task_id: ses_abc123 (for resuming to continue this task if needed)
```

Rules:
- Any subagent type can resume any session — hand off from `explore` to
  `general` mid-session if needed
- The subagent inherits full conversation history, tool results, and context
- `task_id` works for both foreground and background tasks
- If `task_id` is invalid or missing, a fresh session is created

## Skill Authoring

- Skills live in `.opencode/skills/` (project) or
  `~/.config/opencode/skills/` (user)
- Each skill is a directory with a `SKILL.md` file
- Use YAML frontmatter for `name` and `description` (triggers)
- Frontmatter `description` doubles as the activation condition
- Keep skills focused: one skill = one capability

## Subagent Patterns

| Task Type       | Agent      | Use When                         |
|-----------------|------------|----------------------------------|
| Codebase search | explore    | Finding files, grepping patterns |
| Complex work    | general    | Multi-step research or execution |
| Background work | general    | Async analysis, parallel fan-out |
| Review          | general    | Code review with checklists      |
| Triage          | general    | Issue classification             |

### Background Subagents

Enable with `export OPENCODE_EXPERIMENTAL_BACKGROUND_SUBAGENTS=true`.

Set `background: true` to launch asynchronously. Returns immediately with
`task_id` and `state: running`. Poll with `task_status(task_id="...", wait=true)`.

### Parallel Agents

Launch multiple background tasks to fan out independent work, then collect
results and synthesize.

## Code Review Workflow

1. Load the `apm-review-panel` skill for multi-persona reviews
2. Run lint, typecheck, and tests before requesting review
3. Use `Task` agents to fan out specialist reviews in parallel
4. Synthesize findings into a single actionable comment

## CI/CD Integration

- Use `opencode` in headless mode: `opencode run "review this PR"`
- Configure permissions via `opencode.json` for restrictive CI
- Pin model versions for reproducibility
- Cache agent results between pipeline runs
