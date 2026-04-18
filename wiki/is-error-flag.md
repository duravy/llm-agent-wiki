---
title: The is_error Flag (MCP Signal Layer)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The `is_error` flag is MCP's signal layer — it tells Claude that a tool call returned an error result rather than success data. It is necessary but not sufficient: the flag tells Claude *something went wrong*; the structured error body tells Claude *what kind of wrong and what to do next*.

## Two-Layer Architecture

```
Tool response:
  is_error: true          ← Signal layer: "this is an error, not success data"
  error_category: ...     ← Decision layer: "here is what kind and what to do"
  is_retryable: ...
  message: ...
  suggestion: ...
```

The exam tests both layers separately. Knowing only the flag is insufficient.

## What the Flag Does

When `is_error: true`, Claude knows:
- Do not treat this response as valid output
- Do not pass this to the user as a result
- Consult the error body to determine recovery action

When `is_error: false`:
- The response is valid output, even if the content is empty
- An empty array with `is_error: false` means "the operation succeeded and found nothing" — a legitimate result

## The Critical Distinction

`is_error` encodes the *operation outcome*, not the *content volume*.

| Situation | `is_error` | Content | Claude action |
|-----------|-----------|---------|---------------|
| Database timed out | `true` | any | Retry (transient) |
| Search succeeded, nothing found | `false` | `[]` | Report empty result to user |
| Permission denied | `true` | any | Escalate |
| Refund limit exceeded | `true` | any | Explain to user |

Returning `is_error: false` with an empty array for a failed operation is an exam-tested anti-pattern. It silently tells Claude "the service worked and found nothing" when the service was actually down. See [[access-failure-vs-empty-result]].

## Flag Alone Is Not Enough

The source explicitly frames this as two layers:

> "There are two layers. The is_error flag is the signal layer — something went wrong. The structured error body is the decision layer — here is what kind of wrong and what to do next. The exam tests both layers."

A tool that returns only `is_error: true` with a plain message string gives Claude no structured basis for its recovery decision. Claude must infer — and inference fails on edge cases.

Related: [[mcp-error-responses]], [[mcp-error-fields]], [[mcp-error-categories]], [[access-failure-vs-empty-result]]

[Source: 10-cMCoA_MGDdw.md]
