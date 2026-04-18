---
title: Syntax Errors vs Semantic Errors (D4 T4.3)
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

The most exam-critical distinction in Task 4.3. Tool use with JSON schemas eliminates one type of error entirely but cannot prevent the other. Conflating the two leads to over-confidence in extraction pipelines. [Source: 22-CxwIKbepDwQ.md]

## The Distinction

| Error Type | Definition | Example | Tool Use Prevents? |
|------------|-----------|---------|-------------------|
| **Syntax error** | Malformed JSON — missing braces, trailing commas, unquoted keys, invalid structure | `{"total": 100,}` | **Yes** — API enforces valid JSON |
| **Semantic error** | Valid JSON with wrong values | `{"total": 100}` when line items sum to $92 + $7.36 tax = $99.36 | **No** — valid structure, incorrect content |

> **"Tool use gives you structure, not correctness."** [Source: 22-CxwIKbepDwQ.md]

## Why Semantic Errors Still Occur

The API guarantees valid JSON matching the schema's *structure*. It cannot guarantee that the extracted *values* accurately reflect the source document. Claude may:
- Misread a number from ambiguous formatting
- Sum line items incorrectly
- Extract a field from the wrong part of the document
- Return a plausible-sounding value for a field it couldn't locate (if the field is required — which is why [[nullable-fields]] matters)

## Concrete Example

Invoice says: total = $100. Line items: $50 + $42 = $92. Tax at 8% = $7.36. Correct total = $99.36.

```json
{
  "vendor_name": "Acme Corp",
  "invoice_number": "INV-2024-001",
  "total": 100,
  "tax": 8
}
```

This JSON is **valid** — it matches the schema, passes JSON parsing, and contains no malformed syntax. But the numbers don't add up. Only semantic validation (cross-checking totals against line items) will catch this. That's Task 4.4.

## Implication for Pipeline Design

A two-stage pipeline is required for high-confidence extraction:

1. **T4.3 (Structural):** Tool use → guaranteed valid JSON matching the schema
2. **T4.4 (Semantic):** Validation logic → check values for internal consistency and business rules

Relying on tool use alone gives you structure, not correctness.

## Connection to Probabilistic vs Deterministic

[[probabilistic-vs-deterministic]] (D1 T1.4) frames enforcement as code-level (deterministic) vs prompt-level (probabilistic). The syntax/semantic distinction maps onto this:

- Syntax enforcement = deterministic (API-level code check)
- Semantic validation = still requires logic (either code checks or another Claude call with validation instructions)

## Exam Signal

> **"Know this for the exam: tool use eliminates syntax errors, not semantic errors. Values can still be wrong even when the JSON is valid."** [Source: 22-CxwIKbepDwQ.md]

Exam question form: "After implementing tool use for invoice extraction, an auditor finds that extracted totals sometimes don't match line items. What does this indicate?" Answer: Semantic errors — tool use only prevents syntax errors. Semantic validation (T4.4) is a separate step.

## Connection to Validation Loops (D4 T4.4)

T4.4 is the practical answer to the semantic error problem. The two-stage pipeline:
- **Stage 1 (T4.3):** Tool use → syntax guaranteed
- **Stage 2 (T4.4):** Semantic validation → [[self-correction-dual-totals]], [[retry-with-error-feedback]], [[retriable-vs-nonretriable-failures]]

See [[validation-retry-loops]] for the complete architecture.

Related: [[structured-output-tool-use]], [[extraction-tool-pattern]], [[probabilistic-vs-deterministic]], [[validation-retry-loops]]

[Source: 22-CxwIKbepDwQ.md, 23-wyqmHtVGxAg.md]
