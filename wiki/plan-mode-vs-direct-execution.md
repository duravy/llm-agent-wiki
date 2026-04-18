---
title: Plan Mode vs Direct Execution — T3.4 Hub
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Claude Code can operate in two fundamentally different ways. Choosing the right one for the task is a core exam topic.

> **Analogy: "Paint that wall blue" = direct execution. "Redesign this room" = plan mode first.**

## The Two Modes

| | Plan Mode | Direct Execution |
|---|-----------|-----------------|
| **What Claude does first** | Explores, analyzes options, proposes plan | Immediately makes changes |
| **Human checkpoint** | Yes — review and approve before any file changes | No |
| **Best for** | Architectural decisions, multi-file changes, multiple valid approaches | Well-defined single-file changes, known approach |
| **Risk if misused** | Unnecessary overhead for simple tasks | Expensive rework if approach was wrong |

See [[plan-mode]] and [[direct-execution]] for full scenarios.

## Decision Framework

```
Is the change well-defined and single-file?
  Yes → Direct execution

  No → Are there multiple valid approaches?
          Yes → Plan mode
          No  → Does it touch many files?
                  Yes → Plan mode
                  No  → Direct execution
```

**Exam shortcuts:**
- Monolith-to-microservices restructuring → Plan mode (fails first two checks, hits large scope)
- Adding a validation check → Direct execution (passes the first check)

## The Combined Mode Pattern

The most effective real-world approach uses both:

1. **Plan mode** — explore the codebase, map dependencies, propose a phased plan, get human approval
2. **Direct execution** — implement each step of the approved plan

Each direct execution step is well-scoped because the plan already identified exactly what to change.

**Example: Express → Fastify migration**
- Phase 1 (plan): explore all route files and middleware, map dependencies, propose phased migration. Human reviews and approves. Zero lines of code have changed.
- Phase 2 (execute): step 1 — update `package.json`; step 2 — convert main server; step 3 — migrate route handlers.

This pattern combines the risk-reduction of planning with the efficiency of focused execution.

## The Explore Sub-Agent

Plan mode uses an [[explore-sub-agent]] for the investigation phase. The sub-agent reads files and maps dependencies in its own isolated context — keeping verbose discovery output out of the main conversation. See [[explore-sub-agent]].

## Anti-Patterns

See [[plan-mode-anti-patterns]] for the five exam-tested wrong answers.

## Exam Mnemonic — 6 Takeaways

1. Plan mode: architectural decisions, multi-file changes, multiple valid approaches, library migrations
2. Direct execution: single-file bug fixes, well-defined changes, known approaches (one-sentence description)
3. Explore sub-agent runs investigation in isolation — verbose discovery stays out of main conversation context
4. Combine both: plan mode for investigation → direct execution for each step of the approved plan
5. Monolith-to-microservices / large migration → plan mode; adding validation / fixing a stack-trace bug → direct execution
6. Plan mode's value = **the human review checkpoint**; always review and adjust before approving

## Connections

- [[plan-mode]] — 4 scenarios + explore sub-agent + human review checkpoint
- [[direct-execution]] — 4 scenarios + rule of thumb
- [[explore-sub-agent]] — isolation mechanism; keeps discovery out of main context
- [[plan-mode-anti-patterns]] — five exam-tested wrong answers
- [[subagent-delegation-exploration]] — D5 T5.4; explore sub-agent is the same isolation pattern
- [[prompt-chaining]] — D1 T1.6; combined mode is a two-phase chain with a human review gate
- [[human-review-workflows]] — D5 T5.5; human checkpoint principle

[Source: 17-96v5N5YGMPM.md]
