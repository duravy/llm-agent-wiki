---
title: Two-Phase Batch Strategy — Sample Then Scale
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

Before submitting thousands of documents to the batch API, validate and refine the prompt on a small sample using the synchronous API. This maximizes first-pass success rate at scale and minimizes expensive resubmissions.

## The Two Phases

### Phase 1 — Synchronous Validation (20–50 documents)

- Use the synchronous API on 20–50 **representative** documents
- Immediate feedback: check extraction quality, find prompt issues, iterate
- Repeat until quality is acceptable
- **Cost**: standard synchronous pricing on a small set — negligible compared to full volume

### Phase 2 — Batch at Scale (full volume)

- Switch to the batch API for the full dataset (e.g., 10,000 documents)
- Use the **refined prompt** from Phase 1
- Result: 50% cost savings + a higher first-pass success rate (fewer expensive resubmissions)

## Why This Matters Economically

First-pass failures require resubmission at batch prices. If 20% of 10,000 documents fail due to a prompt issue you could have caught on 50 documents:

- 2,000 resubmissions × batch rate = significant cost
- Phase 1 cost on 50 documents = tiny

The Phase 1 investment always pays for itself.

## What "Representative" Means

The 20–50 sample must cover:
- Different document types within the domain
- Edge cases and unusual formats
- The types of documents most likely to cause extraction failures

A non-representative sample (all easy documents) will produce deceptively high Phase 1 quality and a poor Phase 2 first-pass rate.

## Connection to Prompt Refinement Patterns

Phase 1 is the [[iterative-refinement-techniques]] principle (D3 T3.5) applied to batch workflows: concrete examples (test documents) + specific failures → iterate the prompt → green. The same loop that applies to individual prompts applies here before committing to scale.

## Exam Signal

> *"What is the risk of skipping prompt refinement before a large batch?"*
> → Low first-pass success rate → expensive resubmissions → erases batch cost savings.

> *"What is the two-phase approach for large-scale batch processing?"*
> → Phase 1: synchronous on 20–50 representative docs to refine the prompt. Phase 2: batch for full volume with the refined prompt.

Related: [[message-batches-api]], [[batch-vs-synchronous-api]], [[partial-failure-resubmission]], [[batch-anti-patterns]], [[iterative-refinement-techniques]]

[Source: 24-6ZGgMSQz3eg.md]
