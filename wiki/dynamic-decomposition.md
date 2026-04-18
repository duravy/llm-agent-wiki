---
title: Dynamic Decomposition — Adaptive Execution Plan
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Dynamic decomposition is a task decomposition strategy where the plan emerges from discoveries during execution. You cannot write step 3 until you've seen what step 2 found. The scope, step count, and specific targets are all determined by what the agent discovers at runtime. [Source: 07-kjzfSpgfvss.md]

## Defining Characteristic

> **"The plan evolves based on what you discover at each step."**

You start with a rough goal. Each step reveals what the next step should be. The number of steps, their specific targets, and their ordering are not fixed — they are shaped by findings.

## Canonical Example: Adding Tests to a Legacy Codebase

**Goal known:** comprehensive test coverage. **Scope unknown:** how many modules, which are critical, which have complex branching.

```
Phase 1 — Map the structure
  → Discovery: 12 modules found, 8 without tests

Phase 2 — Prioritize (shaped by Phase 1)
  → Discovery: 3 are critical — auth, payments, user-data

Phase 3 — Deep-dive auth (shaped by Phase 2)
  → Discovery: 15 code paths, 3 with complex branching

Phase 4 — Generate tests for exactly those 3 paths (shaped by Phase 3)
```

Phase 4 targets couldn't have been written at the start. The specific paths only existed after Phase 3 ran. That's dynamic decomposition.

## Plan Insertion Mid-Execution

A key power: when a discovery demands it, the agent inserts *new* steps that weren't in the original plan.

Example: original plan is map → prioritize → generate tests. During mapping, the agent finds a shared utility module that 9 other modules import. The original plan would have treated it as one of 12 modules. The adaptive response: insert two new steps — analyze the utility module, test it first — because it is the highest-leverage target in the entire codebase.

> "The plan updates to match reality." [Source: 07-kjzfSpgfvss.md]

## When to Use

- Task is open-ended: scope cannot be specified upfront
- Discoveries at each step should shape the next step
- The goal is known but the path to it is not
- Handling novel or varied inputs where the structure changes per run

## When NOT to Use

If you know all the steps upfront and the task is the same every run, use [[prompt-chaining]] instead. Applying dynamic decomposition to a predictable task wastes tokens replanning something that could have been specified directly.

## Trade-offs vs. Prompt Chaining

| Dimension | Dynamic Decomposition | Prompt Chaining |
|-----------|----------------------|-----------------|
| Step count | Variable — grows with discoveries | Fixed upfront |
| Cost | Less predictable; can multiply | Predictable, lower |
| Parallelism | Harder — next step depends on findings | Easier — independent units can run in parallel |
| Adaptability | Full — plan can be rewritten | None — pipeline is fixed |
| When to use | Open-ended exploration | Predictable, repeatable tasks |

## Relationship to Large Codebase Context

Dynamic decomposition is the natural strategy for open-ended codebase exploration — the domain covered in [[large-codebase-context]] (D5 T5.4). The same phase-by-phase discovery pattern appears in both domains.

## Connection to Anti-Patterns

[[decomposition-anti-patterns]] Anti-pattern 2: using dynamic decomposition for a predictable task (wastes tokens). Anti-pattern 3: using prompt chaining for an exploratory task (can't adapt to discoveries).

[Source: 07-kjzfSpgfvss.md]
