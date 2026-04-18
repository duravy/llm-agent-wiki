---
title: Batch vs. Synchronous API Decision Framework
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

The fundamental distinction between Anthropic's two calling modes. The exam presents scenarios and asks which API to use. The decision rule is: **is someone waiting?**

## The One-Question Test

> *Is someone waiting for this result right now?*
> - **Yes** → Synchronous API
> - **No** → Message Batches API (save 50%)

## Side-by-Side Comparison

| Dimension | Synchronous | Message Batches |
|-----------|-------------|-----------------|
| Cost | Standard pricing | 50% cheaper |
| Latency | Immediate | Up to 24 hours |
| Tool calls | Multi-turn supported | **Not supported** |
| Best for | Blocking, real-time tasks | Non-blocking, batch workloads |
| Examples | Chatbots, pre-merge checks | Nightly reports, weekly audits |

## The Blocking/Non-Blocking Distinction

**Blocking task**: a developer is waiting for a pre-merge code review result. They cannot merge until the review completes. A 24-hour wait is not acceptable. Use synchronous.

**Non-blocking task**: a nightly technical debt report just needs to be ready by morning. Weekend security audit needs to be ready by Monday. Processing can happen in the background. Use batch.

## The Tool Call Constraint

This is the second decision gate after blocking/non-blocking:

> *Does the workflow require multi-turn tool calls?*
> - **Yes** → Synchronous (batch cannot execute tools mid-request)
> - **No** → Either API; use cost as the tiebreaker

This means **agentic workflows always require synchronous**. Any task where Claude needs to call a tool, receive results, then call another tool — that's an agentic loop and batch cannot run it.

## Exam Scenarios

| Scenario | Key signal | Answer |
|----------|-----------|--------|
| Pre-merge check | Developer is waiting | Synchronous |
| Nightly report | Needs to be ready by morning | Batch |
| Weekly audit | Needs to be ready by Monday | Batch |
| Chatbot response | User waiting in real time | Synchronous |
| Tool-use pipeline | Multi-turn tool execution required | Synchronous |
| Overnight extraction of 10,000 docs | No one waiting; self-contained | Batch |

## Manager Scenario (Exam Trap)

> *"A manager proposes switching both the pre-merge review and the nightly report to batch to save costs."*

**Correct answer**: Switch the nightly report to batch (valid). Keep the pre-merge review on synchronous (blocking — developers are waiting). Not all workflows are suitable for batch.

Related: [[message-batches-api]], [[partial-failure-resubmission]], [[custom-id-tracking]], [[two-phase-batch-strategy]], [[batch-anti-patterns]]

[Source: 24-6ZGgMSQz3eg.md]
