---
name: opencode-collab
description: Use this skill when you want to launch subagents with the task tool.
version: 1.0.0
author: abi_jey
---

You should view agents as collaborators that you can delegate specfic tasks to, or ask them for any kind of help.

When it comes to using task tool. You can either start a fresh converstaion . Or pass task id to continue an existing conversation. This is useful when you need to follow up on a response, or want to have back and forth conversation/discussion with the agent.

Regarding Memory:
In a fresh conversation sub agent will probably have no memory.
In a continued conversation (They will remember previous interactions, limited to that task id only) try to limit yourself and do not lunch same same agent multiple times.

## Experinmental
You may also be able have async conversations, or wait for them to finish task/thinking/investigation, although that is still experimental (can be enabled if you ask user to `export OPENCODE_EXPERIMENTAL_BACKGROUND_SUBAGENTS=true`, or similar and restart the opencode.) Try first and see if it's available, if yes use the background as well.

## Examples

task "Can you tell me what are bugs, reported in gh?", "Agent A"
response "Her are the bugs reported in gh: 1. Bug 1, 2. Bug 2, 3. Bug 3"
task "Which one has more severity from your point of view?", "Agent A", task_id="123"
response "From my point of view, Bug 2 has more severity"
task "Can you fix that bug please and make a pr?", "Agent A", task_id="123"
response "Sure, I have fixed the bug and created a PR: <link to pr>"
...
... # Here you can check the PR and the rest of development workflow.
...
task "Can if any package is outdated?", "Agent A", # Fresh conversation, becase nothing from pervious conversation is helpful for this task.

The above is a very generic and simple example. the idea is to have more extensive conversations, to achieve a goal.