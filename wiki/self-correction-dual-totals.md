---
title: Self-Correction via Dual Totals (D4 T4.4)
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

A semantic validation technique for numerical data. Rather than extracting only the stated value, extract both the stated value and the independently calculated value, then flag conflicts. This catches both extraction errors (Claude misread a number) and source document errors (the invoice itself has a math mistake). [Source: 23-wyqmHtVGxAg.md]

## The Pattern

Add two fields to the extraction schema instead of one:

```python
"total_section": {
    "type": "object",
    "properties": {
        "stated_total":     {"type": ["number", "null"]},  # as written in the document
        "calculated_total": {"type": ["number", "null"]},  # sum of line items + applicable tax
        "conflict_detected": {"type": "boolean"}           # true when they differ
    }
}
```

## Concrete Example

Invoice document says: total = $100
Line items: $50 + $42 = $92
Tax at 8% on $92 = $7.36
Correct total: $99.36

Extraction output:
```json
{
    "stated_total": 100,
    "calculated_total": 99.36,
    "conflict_detected": true
}
```

Validation code flags this for human review: *Is the document wrong ($100 is a round-up), or did the extraction misalign a line item?*

## Two Error Types This Pattern Catches

| Scenario | `stated_total` | `calculated_total` | `conflict_detected` | Interpretation |
|----------|--------------|-------------------|-------------------|----------------|
| Extraction misread a line item | 100 | 100 | false | Correct — trust both |
| Extraction got the total right but missed a line item | 100 | 92 | true | Extraction error |
| Source document has rounding error | 100 | 99.36 | true | Document error or rounding policy |
| Both wrong (rare) | 100 | 85 | true | Review required |

## Relationship to Semantic Validation

This pattern is the primary example of semantic validation — checking that values are internally consistent, not just syntactically valid. See [[syntax-vs-semantic-errors]] for why schema enforcement alone is insufficient.

## Exam Signal

> **"Extract both the stated value and the calculated value, then flag discrepancies."** [Source: 23-wyqmHtVGxAg.md]

Related: [[validation-retry-loops]], [[syntax-vs-semantic-errors]], [[nullable-fields]]

[Source: 23-wyqmHtVGxAg.md]
