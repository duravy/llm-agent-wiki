---
title: Batch Processing Anti-Patterns (D4 T4.5)
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

Five anti-patterns for the Message Batches API. The exam presents scenarios where each looks tempting but is wrong.

## Anti-Pattern 1: Batch for Blocking Workflows

**What it looks like**: switching a pre-merge code review to batch to save 50%.

**Why it's wrong**: pre-merge checks block a developer from merging. They are waiting right now. A 24-hour processing window is unacceptable for a blocking task.

**Fix**: synchronous API for any task where someone is waiting for the result.

## Anti-Pattern 2: Resubmitting the Entire Batch on Partial Failure

**What it looks like**: 500 of 10,000 documents fail → resubmit all 10,000 to be safe.

**Why it's wrong**: wastes 50% pricing on 9,500 documents that already succeeded. Eliminates the cost savings that made batch attractive.

**Fix**: use [[custom-id-tracking]] to identify only the failed documents; resubmit only those via [[partial-failure-resubmission]].

## Anti-Pattern 3: No Prompt Refinement Before a Large Batch

**What it looks like**: write a prompt, immediately submit 10,000 documents to batch.

**Why it's wrong**: low first-pass success rate leads to expensive resubmissions at scale.

**Fix**: [[two-phase-batch-strategy]] — synchronous validation on 20–50 representative documents before scaling to full volume.

## Anti-Pattern 4: Batch for Multi-Turn Tool-Calling Tasks

**What it looks like**: running an agentic workflow (multiple tool calls, each depending on the last) through the batch API to save 50%.

**Why it's wrong**: the batch API does not support mid-request tool execution. It is single-turn only.

**Fix**: synchronous API for any workflow requiring tool calls.

## Anti-Pattern 5: No Custom ID on Batch Requests

**What it looks like**: submitting batch requests with no ID, or generic IDs like `req-1`.

**Why it's wrong**: you cannot correlate responses to original documents; you cannot identify failures for targeted resubmission.

**Fix**: always include a meaningful custom ID (e.g., `doc-invoice-00001`) on every batch request.

## Summary Table

| Anti-Pattern | Root Cause | Fix |
|-------------|-----------|-----|
| Batch for blocking tasks | Ignored latency requirement | Synchronous for blocking |
| Resubmit entire batch | No per-result tracking | Custom ID + targeted resubmission |
| No prompt refinement | Scaled before validating | Two-phase: sample → scale |
| Batch for tool-calling | Misunderstood batch capability | Synchronous for agentic loops |
| No custom ID | Oversight / misunderstanding | Always include meaningful custom ID |

Related: [[message-batches-api]], [[batch-vs-synchronous-api]], [[custom-id-tracking]], [[partial-failure-resubmission]], [[two-phase-batch-strategy]]

[Source: 24-6ZGgMSQz3eg.md]
