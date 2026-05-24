---
name: opencode-plus
description: Use this skill when you want to launch subagents with the task tool.
version: 1.0.0
author: abi_jey
---

You should view agents as collaborators that you can delegate specfic tasks to, or ask them for any kind of help.

When it comes to using task tool. You can either start a fresh converstaion . Or pass task id to continue an existing conversation. This is useful when you need to follow up on a response, or want to have back and forth conversation/discussion with the agent.

Regarding Memory:
In a fresh conversation sub agent will probably have no memory.
In a continued conversation (They will remember previous interactions, limited to that task id only)

## Experinmental
You may also be able have async conversations, or wait for them to finish task/thinking/investigation, although that is still experimental (can be enabled if you ask user to `export OPENCODE_EXPERIMENTAL_BACKGROUND_SUBAGENTS=true`, or similar and restart the opencode.)

