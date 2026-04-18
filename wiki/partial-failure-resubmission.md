---
title: Partial Failure Resubmission in Batch Processing
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

In batch processing, not all requests succeed. The key principle: **resubmit only the failed documents, never the entire batch**. Reprocessing already-succeeded documents wastes 50% of the cost savings that made batch attractive in the first place.

## The Pattern

After a batch completes:

```
for each result in batch_results:
    if result.status == "failed":
        collect:
          - custom_id
          - error_message
          - original_document

analyze failures before resubmitting:
    if error_type == "context_limit":
        split document into chunks → resubmit chunks
    else:
        resubmit as-is
```

## Three Steps

1. **Check each result's status** — iterate every result, not just the ones you notice
2. **Collect failure details** — custom ID + error message + original document
3. **Analyze before resubmitting** — the error type determines the resubmission strategy

## Error-Type Analysis

| Error Type | Meaning | Resubmission Strategy |
|-----------|---------|----------------------|
| Context limit | Document too long | Split into chunks; resubmit chunks separately |
| Other errors | Various | Resubmit as-is |

Context-limit errors are the only case requiring structural changes before resubmission. All other errors: resubmit the original document unchanged.

## Why Not Resubmit Everything?

The batch API costs 50% less than synchronous — but only if you don't pay to reprocess documents that already succeeded. If 9,500 of 10,000 documents succeed and 500 fail:

- Resubmit all 10,000 → pay 50% of cost again for 9,500 unnecessary calls
- Resubmit only 500 → pay 50% of cost for 500 calls only

The entire value proposition of batch depends on targeted resubmission.

## Dependency: Custom IDs

This pattern only works if every request has a [[custom-id-tracking|custom ID]]. Without custom IDs, you cannot identify which documents failed.

## Exam Signal

> *"What is the correct response to a partial batch failure?"*
> → Check each result by custom ID; resubmit only the failed documents; analyze context-limit errors and split those documents.

Anti-pattern: resubmitting the entire batch on any failure.

Related: [[message-batches-api]], [[custom-id-tracking]], [[batch-anti-patterns]], [[two-phase-batch-strategy]]

[Source: 24-6ZGgMSQz3eg.md]
