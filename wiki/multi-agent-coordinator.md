---
title: Multi-Agent Coordinator Pattern (D1 T1.2)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 2
status: draft
---

The multi-agent coordinator pattern scales the single-agent [[agentic-loop]] into a system of one coordinator and multiple specialized sub-agents. The coordinator runs its own agentic loop, decomposing work, delegating via the [[task-tool]], and synthesizing results. Each sub-agent runs an independent loop in complete isolation. This is Domain 1, Task 1.2 — a direct extension of T1.1.

## When to Use Multi-Agent (4 Constraints)

| Constraint | Why it forces multi-agent |
|-----------|--------------------------|
| Context window overflow | Long complex tasks exhaust a single context; partition across agents |
| Specialization | Sub-agents get focused system prompts tailored to a narrow domain |
| Parallelism | Independent subtasks run simultaneously via parallel `task` calls |
| **When one is enough** | **Don't overengineer — exam tests knowing when NOT to use multi-agent** |

## Architecture Overview

See [[hub-and-spoke-architecture]] for the 3 hard rules.

```
User → Coordinator (hub)
         ├── Sub-agent A (spoke)
         ├── Sub-agent B (spoke)
         └── Sub-agent C (spoke)
```

Sub-agents **never** communicate with each other. All routing goes through the coordinator.

## Core Sub-Concepts

| Concept | Page | Exam weight |
|---------|------|-------------|
| Hub-and-spoke + 3 rules | [[hub-and-spoke-architecture]] | Structural foundation |
| Isolated context | [[isolated-context]] | **Most-tested detail** |
| 5 coordinator responsibilities | [[coordinator-responsibilities]] | Exam model of coordinator |
| Task tool mechanics | [[task-tool]] | How delegation actually works |
| Parallel vs sequential execution | [[parallel-vs-sequential-execution]] | Tested distinction |
| Dynamic routing + work partitioning | [[dynamic-routing]] | Efficiency patterns |
| Iterative refinement + structured handoffs | [[iterative-refinement]] | Quality control |
| Five anti-patterns | [[multi-agent-anti-patterns]] | Exam trap scenarios |

## 9 Exam Must-Knows

1. Hub-and-spoke: coordinator at center, no direct spoke-to-spoke communication
2. Isolated context: sub-agents start fresh; coordinator must explicitly pass all needed context
3. Five responsibilities: Decompose → Delegate → Aggregate → Evaluate → Respond
4. Task tool spawns fresh Claude instances; coordinator's `allowed_tools` must include `task`
5. Parallel: multiple task calls in one response for independent tasks
6. Sequential: one task call per iteration when step N+1 depends on step N
7. Dynamic routing: evaluate request first, only invoke needed sub-agents
8. Structured handoffs: every finding carries source name, URL, date
9. Iterative refinement: evaluate and redelegate if gaps found before final response

## D2 T2.3 Addition: Coordinator Gets task_tool Only

D2 T2.3 ([[tool-scoping]]) makes the coordinator tool rule explicit and exam-testable: the coordinator gets the `task_tool` to spawn sub-agents — and nothing else. Giving the coordinator domain tools (e.g., `web_search`, `lookup_customer`) causes it to do work it should be delegating, blurring the boundary between coordinator and sub-agent roles.

This extends Must-Know #4 above: not only must `task` be in `allowed_tools`, it should be the *only* domain-level tool the coordinator has.

[Source: 03-KQSSSP0op2M.md, 11-K-HRtSQLGfU.md]
