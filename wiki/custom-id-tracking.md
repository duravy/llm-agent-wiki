---
title: Custom ID Tracking in Batch Requests
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

Every batch request must include a meaningful custom ID. The custom ID is the only link between a request and its response — without it, you cannot correlate results or identify failures after processing.

## The Tracking Number Analogy

Think of custom ID like a tracking number on a package. When you ship 10,000 packages, each gets a unique number. When packages arrive, you match them to orders by tracking number. Without it, you receive an undifferentiated pile of packages with no way to know which order they belong to.

## How It Works

1. **Create the batch** — assign a meaningful custom ID to each request: `doc-invoice-00001`, `doc-invoice-00002`, etc.
2. **Submit** — the batch processes over up to 24 hours
3. **Retrieve results** — results come back with the original custom ID attached
4. **Match** — correlate each result to its original document via custom ID
5. **Identify failures** — check status per custom ID; failed requests stand out immediately

## Why Meaningful IDs Matter

A custom ID like `doc-invoice-00001` encodes:
- Document type (invoice)
- Sequence number
- Enough context to retrieve the original document for resubmission

A generic ID like `request-1` provides no context when you need to retrieve the original document to resubmit.

## Connection to Partial Failure Handling

Custom IDs are the mechanism that makes [[partial-failure-resubmission]] possible. After a batch completes:

1. Iterate results and check each status
2. If failed: collect the custom ID + error message + original document
3. Analyze the error (context limit → split document; other → resubmit as-is)
4. Resubmit **only the failed custom IDs** — not the entire batch

Without custom IDs, you cannot identify which documents failed and must resubmit everything (Anti-pattern #5 in [[batch-anti-patterns]]).

## Exam Signal

> *"Why is a custom ID required on every batch request?"*
> → To correlate responses back to original documents and identify failures for targeted resubmission.

> *"What happens if you omit custom IDs?"*
> → You cannot identify which requests failed; you must resubmit the entire batch, wasting money on already-succeeded documents.

Related: [[message-batches-api]], [[batch-vs-synchronous-api]], [[partial-failure-resubmission]], [[batch-anti-patterns]]

[Source: 24-6ZGgMSQz3eg.md]
