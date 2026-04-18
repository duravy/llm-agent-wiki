---
title: Message Batches API — Batch Processing Strategies (D4 T4.5)
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

The Message Batches API is Anthropic's asynchronous processing mode: 50% cheaper than the synchronous API but with an up to 24-hour processing window and no mid-request tool execution. The exam tests your ability to match the right API to the right workflow.

## Two APIs

| API | Cost | Latency | Tool Calls | Use Case |
|-----|------|---------|------------|----------|
| Synchronous | Standard | Immediate | Full multi-turn | Blocking tasks (someone is waiting) |
| Message Batches | 50% less | Up to 24 hours | None (single-turn) | Non-blocking tasks (overnight/weekend) |

## Critical Limitation: No Mid-Request Tool Calls

The batch API is **single-turn**. All context must be included upfront; Claude returns its complete analysis in one shot. No tool call roundtrips are possible during processing.

This means batch works only for self-contained tasks where everything the model needs is already in the request. Any workflow requiring multi-turn tool execution must use the synchronous API.

## Three Exam Scenarios

| Scenario | Why | API |
|----------|-----|-----|
| Pre-merge code review | Developer is waiting right now | Synchronous |
| Nightly technical debt report | Needs to be ready by morning | Batch |
| Weekly security audit | Needs to be ready by Monday | Batch |

If a manager proposes switching both workflows to batch for cost savings: **batch for the nightly report only, synchronous for pre-merge checks.**

## Core Techniques

- [[custom-id-tracking]] — assign a meaningful ID to each request; correlate responses back on completion
- [[partial-failure-resubmission]] — check results per custom ID; resubmit only failed documents
- [[two-phase-batch-strategy]] — synchronous for prompt refinement on a sample; batch for full volume

## 5 Anti-Patterns

See [[batch-anti-patterns]].

1. **Batch for blocking tasks** — pre-merge checks can't wait 24 hours
2. **Resubmit entire batch on partial failure** — wastes money on already-succeeded documents
3. **No prompt refinement before large batch** — low first-pass success rate → expensive resubmissions
4. **Batch for multi-turn tool-calling tasks** — batch doesn't support mid-request tool execution
5. **No custom ID** — can't correlate responses or identify failures

## Exam Decision Framework

> "Is someone waiting for the result?" → Synchronous
> "Can the work happen overnight?" → Batch (save 50%)
> Always refine prompts on a sample first.
> Always include custom IDs.
> Batch never supports multi-turn tool calling.

## Exam Test

- *"What guarantees 50% cost savings for non-blocking workflows?"* → Message Batches API
- *"What supports multi-turn tool calling?"* → Synchronous API
- *"A manager wants to switch pre-merge checks to batch. Is this valid?"* → No. Pre-merge is blocking; batch latency is unacceptable.

## Cross-Domain Connections

- [[tool-choice-modes]] (D2 T2.3) — batch's no-tool-call constraint contrasts with synchronous `any`/`force` modes; agentic tool loops require synchronous
- [[two-phase-batch-strategy]] echoes the [[two-phase-search-strategy]] principle (D2 T2.5): start narrow to validate, then scale
- [[validation-retry-loops]] (D4 T4.4) — retry logic applies within synchronous pipelines; for batch, resubmit is the equivalent pattern via [[partial-failure-resubmission]]

[Source: 24-6ZGgMSQz3eg.md]
