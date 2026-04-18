---
title: Agentic Loop (D1 T1.1)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The agentic loop is the fundamental repeating pattern that powers every Claude-driven product. It combines the `messages.create` API call, tool descriptions, and structured response parsing into a self-directing cycle where Claude reads the conversation history, decides what to do next, and either requests a tool or signals completion. [[domain-1-overview|Domain 1]] (27% of exam) is built around this concept.

## Core Pattern (4 Steps)

See [[agentic-loop-steps]] for full detail.

1. **Send** — call `messages.create` with full messages array + tool definitions
2. **Check** — inspect `stop_reason` (the only decision point)
3. **Execute** — run all `tool_use` blocks in the content array
4. **Append** — add Claude's response (assistant) + all results (user), loop back

Exit when `stop_reason == end_turn`. Never on text content.

## Exam Mnemonic

**S-C-E-A**: Send → Check → Execute → Append

## Key Sub-Concepts

| Concept | Page | Exam Weight |
|---------|------|-------------|
| 4-layer communication stack | [[communication-stack]] | Foundational |
| 5 parameters of messages.create | [[messages-create-parameters]] | Required |
| Tools are descriptions, not code | [[tools-as-descriptions]] | **Most tested in D1** |
| 4-step loop mechanics | [[agentic-loop-steps]] | Core pattern |
| stop_reason values | [[stop-reason]] | Critical control signal |
| Model-driven vs scripted agency | [[model-driven-vs-scripted]] | Frequently tested |
| Three anti-patterns | [[agentic-loop-anti-patterns]] | Exam expects recognition |

## Key Exam Signals

- "Tools are descriptions" — Claude sees name + description + schema only; never the implementation
- `stop_reason` is the **only** reliable loop control signal; never parse Claude's text
- Claude is the **planner**; your code is the **executor**
- Use `while True` + safety counter, never `for i in range(N)` as primary exit

## Scaling Up: Multi-Agent

T1.2 builds directly on this loop. In the [[multi-agent-coordinator]] pattern, a coordinator runs this same loop — but its tools include the [[task-tool]], which spawns sub-agents that each run their own independent agentic loops.

## Connection to Domain 5

The agentic loop is the atomic unit inside every multi-agent system covered in [[domain-5-overview|Domain 5]]. Sub-agents described in [[subagent-delegation-exploration]] each run their own agentic loop. The `messages` array that accumulates history here is the same mechanism exploited by [[context-management]] patterns.

[Source: 02-OCiLc9Frq84.md]
