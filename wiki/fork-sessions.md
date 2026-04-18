---
title: Fork Sessions — Third Execution Pattern
created: 2026-04-16
last_updated: 2026-04-16
source_count: 2
status: draft
---

Fork sessions appear in two contexts: as a **sub-agent orchestration pattern** (T1.3 — coordinator spawns parallel branches) and as a **session-level branching tool** (T1.7 — split a named session at a decision point to explore two approaches independently). Both share the same four core properties.

## Four Properties to Know Cold (T1.7, Exam-Tested)

1. **Full independent copy** — each fork receives a complete copy of the conversation history up to the fork point; not a reference, a full copy
2. **Completely independent** — changes in fork A have no effect on fork B; they cannot see each other's work
3. **Parallel execution** — both forks run simultaneously; you are not blocked waiting for fork A before starting fork B
4. **Non-destructive** — the original session remains unchanged; if both forks lead somewhere bad, the baseline is still there untouched

## When to Fork (Session Level)

Use `fork_session` when you've completed shared analysis and face a genuine decision point between two alternative approaches:

| Use case | Why fork? |
|----------|-----------|
| Comparing two refactoring strategies | Implement each independently; compare results; choose the winner |
| Testing unit vs. integration focus | Each fork explores one approach fully without contaminating the other |
| Evaluating alternative architectures | Shared analysis preserved; diverging explorations don't pollute each other |

**Never fork for simple linear tasks** — that adds unnecessary complexity where a single thread suffices. See [[session-anti-patterns]] Anti-pattern 4.

## Fork as Sub-Agent Pattern (T1.3)

## The Pattern (Sub-Agent Orchestration)

```
Coordinator baseline context
     │
     ├── Fork A → explore direction 1 → result A
     ├── Fork B → explore direction 2 → result B
     └── Fork C → explore direction 3 → result C
                                            │
                              Coordinator synthesizes → final output
```

- **Same starting point** — all forks receive identical baseline context
- **Diverging paths** — each fork is given a different goal, angle, or strategy
- **Synthesis at the end** — coordinator aggregates all fork results

## How Fork Differs from Parallel

Both fork and parallel run sub-agents simultaneously, but:

| | Parallel | Fork |
|---|---|---|
| **Starting context** | Each sub-agent gets its own tailored prompt | All sub-agents get the *same* baseline context |
| **Task scope** | Different independent tasks | Same problem, different approaches/directions |
| **Purpose** | Efficiency (tasks that don't depend on each other) | Exploration / comparison |

## Classic Use Cases

- **Brainstorming** — multiple sub-agents each generate a different solution to the same problem
- **A/B strategy testing** — run competing approaches in parallel, coordinator picks the winner
- **Hypothesis investigation** — multiple sub-agents investigate the same question from different angles (different data sources, different reasoning strategies)

## Exam Signal

> **"Fork = multiple sub-agents from the same baseline, exploring different directions."**

If an exam question asks for a pattern that tests multiple strategies simultaneously with the same starting context, the answer is fork — not parallel, which implies independent tasks.

## Connection to Coordinator's Evaluate Step

Fork sessions produce multiple candidate outputs. The coordinator's [[coordinator-responsibilities|Evaluate step]] (D-D-A-E-R) is where fork results are compared and synthesized. The Evaluate step can loop — the coordinator may issue a second round of forks if the first round is inconclusive.

## Connection to Session Management

At the session level, forking is part of the broader [[session-state]] workflow. The decision to fork vs. resume vs. start fresh depends on whether you're genuinely comparing alternatives. See [[session-anti-patterns]] for the full decision guidance.

## D3 T3.2 Connection — context:fork in Skills

D3 T3.2 introduces [[context-fork]] — the `context:fork` frontmatter option for skills. It applies the same forking mechanism at the skill layer: the skill runs in an isolated sub-agent, the main conversation only receives the final summary, and all intermediate output is discarded. The four core fork properties (independent copy, independent execution, parallel-capable, non-destructive) apply here too. The difference in scope: fork sessions branch an entire conversation; `context:fork` isolates a single skill invocation.

[Source: 04-bFdRH69h9LU.md, 08-GbEpxz4wf9s.md, 15-AaGMyP2hBRY.md]
