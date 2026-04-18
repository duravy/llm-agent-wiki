---
title: Task Decomposition Strategies (D1 T1.6)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Task decomposition is the practice of breaking a large goal into steps an agent executes in sequence. The central question of T1.6: **how do you decide which decomposition strategy to use?** Two approaches exist — and the exam tests when to apply each. [Source: 07-kjzfSpgfvss.md]

## The Two Strategies

| Strategy | Definition | Use When |
|----------|-----------|----------|
| [[prompt-chaining]] | Fixed linear pipeline; all steps defined before work begins | Task is predictable and repeatable |
| [[dynamic-decomposition]] | Adaptive plan; steps emerge from discoveries during execution | Task is open-ended; scope is unknown upfront |

## Decision Table (Exam-Tested)

| Signal in the scenario | Strategy |
|-----------------------|----------|
| Predictable, repeatable steps | [[prompt-chaining\|Prompt chaining]] |
| Open-ended exploration | [[dynamic-decomposition\|Dynamic decomposition]] |
| Linear handoffs; step N feeds step N+1 | Prompt chaining |
| Next step depends on what you find | Dynamic decomposition |
| Easy to parallelize within a step | Prompt chaining |
| Harder to parallelize | Dynamic decomposition |
| Fixed, predictable cost required | Prompt chaining |
| Complex scenario; discoveries multiply steps | Dynamic decomposition |

## The Attention Dilution Problem

Both strategies must account for the [[attention-dilution]] problem: language models process the beginning and end of long inputs more reliably than the middle. The solution is per-pass decomposition — one focused prompt per unit of work, plus a separate integration pass using summaries.

## Anti-Patterns

See [[decomposition-anti-patterns]] — five exam-tested failure modes including the single-prompt trap, mismatching strategy to task type, narrow subtask scope, and missing the cross-file integration pass.

## Connections

- [[parallel-vs-sequential-execution]] — within prompt chaining, independent steps (e.g., per-file reviews) can be parallelized
- [[phase-summarization]] — the cross-file integration pass is the same principle: integrate from summaries, not raw output
- [[large-codebase-context]] — dynamic decomposition is the natural strategy for open-ended codebase exploration
- [[fork-sessions]] — a third execution pattern (same baseline context, diverging directions); see T1.7 for session state

[Source: 07-kjzfSpgfvss.md]
