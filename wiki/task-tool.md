---
title: Task Tool — Delegation Mechanics
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The task tool is how the [[multi-agent-coordinator|coordinator]] delegates work to sub-agents. It follows the same [[tools-as-descriptions|tool-as-description]] pattern from D1 T1.1, but instead of calling an external API, it spawns a fresh Claude instance. Four steps from coordinator call to result returned.

## Four Steps

### Step 1 — Coordinator Issues Task Call
The coordinator's response includes a `tool_use` block:
```json
{
  "type": "tool_use",
  "name": "task",
  "input": {
    "agent": "<target-agent-name>",
    "prompt": "<fully-specified prompt with all needed context>"
  }
}
```

### Step 2 — SDK Launches Fresh Instance
The SDK sees the `task` call and launches a **fresh Claude instance** using the target agent's [[agent-definition|agent definition]] (5 fields: name, description, system_prompt, tools, allowed_tools):
- System prompt
- Tools
- Allowed tools (minimum tools principle applies)

The sub-agent has no memory of prior sessions and no access to the coordinator's history.

### Step 3 — Sub-Agent Runs Its Own Agentic Loop
The sub-agent runs a complete, independent [[agentic-loop]] — including its own tool calls — until it reaches `end_turn`.

### Step 4 — Result Returns to Coordinator
The sub-agent's final response comes back into the coordinator's message history as a `tool_result` block, referenced by the original `tool_use` ID.

## Critical Requirement

> **The coordinator's `allowed_tools` must include `task`** or the SDK will reject the call.

This is an exam trap: a coordinator that has the `task` call in its logic but not in its `allowed_tools` list will fail at runtime.

## Parallel vs Sequential

Multiple `task` calls can appear in a single coordinator response (parallel) or one per iteration (sequential). See [[parallel-vs-sequential-execution]].

## Connection to T1.1

The task tool is an application of [[tools-as-descriptions]]: the coordinator sees the task tool's name + description + schema; the SDK runs the actual sub-agent execution. Claude (the coordinator) decides what to delegate; the SDK decides how to execute it.

[Source: 03-KQSSSP0op2M.md]
