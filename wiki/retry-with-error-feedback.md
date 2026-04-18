---
title: Retry with Error Feedback (D4 T4.4)
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

The primary self-correction mechanism for failed extraction. When validation catches a semantic error, a second API call includes three components so Claude knows exactly what went wrong and can fix it. [Source: 23-wyqmHtVGxAg.md]

## The Three-Part Retry Prompt

A retry is a new `messages.create` call with three entries in the messages array:

1. **Original user prompt** — includes the source document (invoice, contract, etc.)
2. **Claude's failed response** — the `assistant` turn containing the incorrect extraction
3. **New user message** — lists the specific validation errors and re-includes the source document

```python
messages = [
    # Turn 1: original extraction request
    {"role": "user", "content": f"Extract invoice data:\n{invoice_text}"},
    # Turn 2: Claude's failed extraction (verbatim from first response)
    {"role": "assistant", "content": first_response_content},
    # Turn 3: specific error feedback + re-anchor to the source
    {"role": "user", "content": (
        "The extraction has these errors:\n"
        "- Line items total $92, but you reported total = $100\n"
        "- tax is null, but the document shows $7.36 on line 8\n"
        f"\nPlease re-extract from the original document:\n{invoice_text}"
    )}
]
```

Claude sees its own failed attempt and the precise errors. It can self-correct because it knows exactly what went wrong.

## Why Specific Errors Are Required

Generic retry: *"Try again."*
Result: Claude doesn't know what to fix. It may return the same extraction or guess randomly.

Specific error feedback: *"Line items total $92, but you reported $100."*
Result: Claude knows the exact discrepancy and can locate the correct value in the source.

> **"Generic retry without specific errors gives no guidance. Always include the specific validation errors."** [Source: 23-wyqmHtVGxAg.md]

This is the same principle as [[test-driven-iteration]] (D3 T3.5): share the exact failure diff, not a vague "it doesn't work."

## What Counts as a Specific Error

- Arithmetic: "Line items sum to $X, but total field says $Y"
- Format: "Date extracted as MM/DD/YYYY, expected ISO format YYYY-MM-DD"
- Missing value: "Tax line visible on page 2, line 8 — tax field returned null"
- Mismatched field: "Invoice number in header is INV-2024-001, you extracted 2024-001"

## Connection to Retry Limits

Max 1–2 retries before sending to human review. See [[retriable-vs-nonretriable-failures]] for when retrying makes sense at all, and [[validation-retry-anti-patterns]] for the unlimited-retry anti-pattern.

Related: [[validation-retry-loops]], [[retriable-vs-nonretriable-failures]], [[test-driven-iteration]]

[Source: 23-wyqmHtVGxAg.md]
