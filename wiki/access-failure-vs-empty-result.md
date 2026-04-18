---
title: Access Failure vs Valid Empty Result (D2 T2.2)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The most heavily exam-tested distinction in D2 T2.2. An access failure (the service was down) and a valid empty result (the operation succeeded but found nothing) require completely different handling. Mixing them up causes Claude to give wrong answers when systems are down.

## The Two Cases

### Access Failure
The underlying service failed — timeout, connection error, infrastructure outage.

**Correct response**:
```json
{
  "is_error": true,
  "error_category": "transient",
  "is_retryable": true,
  "message": "DatabaseConnectionTimeout: query timed out after 30s",
  "suggestion": "Retry after 2 seconds"
}
```

**Claude's action**: Retry. The failure is recoverable.

---

### Valid Empty Result
The operation completed successfully. The query ran. There are simply no matching records.

**Correct response**:
```json
{
  "is_error": false,
  "results": []
}
```

**Claude's action**: Tell the user they have no matching records. Do not retry — there is nothing to recover from.

---

## Why This Distinction Matters

If a database times out and the tool returns `is_error: false` with an empty array:
- Claude interprets this as "the service worked and found nothing"
- Claude tells the user: "You have no orders"
- The user has orders — the database was just down

This is a silent wrong answer. The user receives false information, and no retry was attempted to recover the actual data.

The exam question framing: *"A tool call returns an empty array. What should Claude tell the user?"* — the correct answer depends entirely on `is_error`. Without that flag, you cannot answer correctly.

## Decision Rule

> **Rule**: Use `is_error` to encode the *operation outcome*, not the *content volume*.  
> Empty content ≠ failed operation. Failed operation ≠ empty content.

| `is_error` | Content | Meaning |
|-----------|---------|---------|
| `false` | `[]` | Success — genuinely empty result |
| `true` | any | Failure — operation did not complete |
| `false` | `[...]` | Success — results found |

## Exam Signal

Source: *"The exam tests this distinction heavily, access failures versus valid empty results."*

Know which `is_error` / content combination goes with each scenario:
- **Service down** → `is_error: true` + transient category + retry
- **Nothing found** → `is_error: false` + empty array + report to user

Related: [[is-error-flag]], [[mcp-error-categories]], [[mcp-error-responses]]

[Source: 10-cMCoA_MGDdw.md]
