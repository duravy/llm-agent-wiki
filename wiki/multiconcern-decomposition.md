---
title: Multiconcern Decomposition
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Multiconcern decomposition is the pattern of splitting a complex request with multiple independent questions into parallel concern agents, each handling one question in isolation. It is an application of [[parallel-vs-sequential-execution|parallel execution]] at the problem-decomposition level.

## The Pattern

```
Complex request: "Is this transaction safe?"
         │
         ├── Concern agent A: fraud signals      → result A
         ├── Concern agent B: regulatory compliance → result B
         └── Concern agent C: risk score calculation → result C
                                                           │
                                          Coordinator synthesizes → final answer
```

Each concern agent:
- Has **isolated context** — a failure or timeout in one does not block the others
- Has **specialized tools** — only the tools relevant to its specific concern
- Returns a focused result on its single question

## Why Not a Single Agent?

A single general-purpose agent asked to handle all three concerns at once:
- Runs them sequentially (slower)
- Has context contamination between concerns
- Creates a single point of failure

Multiconcern decomposition is faster (parallel) and more robust (isolated failures).

## The Key Design Insight

> **"Each concern gets its own agent with specialized context and tools — not a general-purpose agent asked to do everything."**

This connects directly to the [[minimum-tools-principle]]: each concern agent gets only the tools relevant to its specific question.

## When to Use vs Not Use

Use multiconcern decomposition when concerns are **truly independent** — one agent's output is not needed to start another. If concern B needs concern A's output, they must run sequentially (see [[parallel-vs-sequential-execution]] and [[enforcement-anti-patterns]] Anti-pattern 5).

## Relationship to Dynamic Routing

[[dynamic-routing]] determines *whether* to invoke sub-agents and *which* ones. Multiconcern decomposition determines *how* to split a complex question across multiple agents running in parallel. Both are coordinator-level responsibilities under the D-D-A-**E** (Decompose-Delegate-Aggregate-Evaluate) pattern — see [[coordinator-responsibilities]].

[Source: 05-Ow0fCviHxrY.md]
