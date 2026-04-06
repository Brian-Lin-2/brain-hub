# Agents

An agent is an LLM

It acts on three specific layers:

1. Codebase
2. Rules
3. Tools

## Context Stack

An agent is a "reasoning loop". It follows a cycle of: Research -> Plan -> Execute -> Verify

**After you prompt:**

1. **Repo:** The agent does an initial semantic search on your repo to determine relevant files. It accomplishes semantic search by using a vector index of your codebase.
2. **Skills/Tools:** The agent does a subsequent scan on available skills/tools that it can use. This is where it will look at MCP Servers.
3. **Loop:** This is where it generates a plan and then executes it. The first two steps were more for context gathering, this is where actual code is generated.

## Instructions/Prompt Files

**Instruction Files:** Think Cursor Rules. These act as a system prompt for the agent, allowing you to customize all agent behavior across all repos.

**Prompt Files:** These are just md files (ie. `TODO.md` or `spec.md`) that allows you to define specific prompts for the agent to use. This is best practice as it keeps the chat clean and allows the agent to refer back to the "source of truth" as it works.

## Custom Agents

This allows you to create new modes for agents besides just the default ones ("agent", "ask", etc). Common custom agents are "architect", "cleaner", and "tester".

> In Cursor, this can be done via `Settings` -> `Cursor Settings` -> `General` -> `Custom Modes`
