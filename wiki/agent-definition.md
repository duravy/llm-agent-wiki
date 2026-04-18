---
title: Agent Definition — 5 Fields
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

An agent definition is the configuration object that tells the Claude Agent SDK how to instantiate a sub-agent. It has **exactly five fields** and the exam tests this list. Before the [[task-tool]] can spawn a sub-agent, the coordinator must reference a defined agent.

## The Five Fields

| Field | Purpose | Exam note |
|-------|---------|-----------|
| `name` | Identifier for the agent (e.g., `"research_agent"`, `"summarizer"`) | Used by coordinator to select which agent to route a task to |
| `description` | What this agent does | Coordinator reads this during [[dynamic-routing\|dynamic routing]] to decide which agent handles a task |
| `system_prompt` | Specialization instructions — role, personality, behavioral constraints | What makes it a *specialist*; distinct from coordinator's system prompt |
| `tools` | List of tool definitions (JSON schemas) the agent *can* use | Same format as RAW API tool definitions |
| `allowed_tools` | Which tools the agent is *permitted* to call at runtime | Must be a subset of `tools`; enforces the [[minimum-tools-principle]] |

## tools vs allowed_tools

Both fields exist for security and runtime control:
- `tools` declares what tools are *available* to the agent (their schemas are sent with every request)
- `allowed_tools` controls which ones the agent can *actually invoke* at runtime

Setting `allowed_tools` ⊂ `tools` lets you define a shared tool library while restricting each agent to only the subset relevant to its role. This is how the [[minimum-tools-principle|minimum tools principle]] is enforced in code.

## The description Field and Routing

The `description` field is read by the **coordinator** when doing [[dynamic-routing]]. If the coordinator decides which sub-agent to invoke by reasoning about the task, it relies on agent descriptions to make that decision. A vague description leads to misrouting.

## Exam Signal

> **"Agent definition has exactly five fields: name, description, system_prompt, tools, and allowed_tools."**

The exam may present a definition with four or six fields and ask what's missing or wrong. Know the list cold.

## Connection to Task Tool

When the coordinator calls the [[task-tool]], it references an agent by `name`. The SDK looks up the agent definition by name and uses it to spawn the fresh [[isolated-context|isolated sub-agent]] instance.

[Source: 04-bFdRH69h9LU.md]
