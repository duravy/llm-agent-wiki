---
title: Nullable Fields (D4 T4.3)
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

A schema design pattern for preventing hallucination in structured extraction. When a field in the extraction schema is marked required but the source document doesn't contain that information, Claude fabricates a value. Nullable fields eliminate this: Claude returns `null` instead of inventing data. [Source: 22-CxwIKbepDwQ.md]

## The Problem

Required field + absent source data = hallucination.

> **"If that field is required, Claude fabricates a value."** [Source: 22-CxwIKbepDwQ.md]

Example: An invoice schema requires a `purchase_order_number` field. Some invoices don't have a PO number. Claude cannot leave a required field empty — it invents a plausible-sounding PO number. The JSON is valid; the data is wrong.

## The Fix

Use a union type (`["type", "null"]`) for any field that may be absent:

```python
# Wrong: required + always-present assumption
"total": {"type": "number"}  # → Claude invents a number if absent

# Right: nullable for fields that may be absent
"total": {"type": ["number", "null"]}  # → Claude returns null if absent
"tax":   {"type": ["number", "null"]}  # Some invoices have no tax line
"po_number": {"type": ["string", "null"]}  # Many invoices have no PO
```

Then move nullable fields out of the `required` list:

```python
"required": ["vendor_name", "invoice_number"]
# total, tax, po_number are optional — absent = null, not fabricated
```

## Relationship to required List

The required list should contain **only fields that truly always exist in the source document type**. Everything else is a candidate for nullable + optional. This is a design decision that must be made per document type — what's always present in an invoice is different from what's always present in a contract.

## Connection to Few-Shot Null Handling

[[gray-area-examples]] (D4 T4.2) shows null handling via few-shot examples: include an example where the source has no citation and the expected output is `{source: null}`. Nullable schema fields (T4.3) enforce this same behavior at the API level. The two approaches are complementary:

- Few-shot examples: *teach* Claude to return null (probabilistic but illustrative)
- Nullable fields: *enforce* null-or-value at the schema level (deterministic, API-guaranteed)

Together they form a defense in depth: the schema cannot be fabricated into, and the examples reinforce the intent.

## Exam Takeaway

> **"Make fields nullable so Claude returns null instead of fabricating."** [Source: 22-CxwIKbepDwQ.md]

Exam question form: "An invoice extraction schema requires a `tax` field. Some invoices don't show tax. What happens?" Answer: Claude fabricates a tax value. Fix: make `tax` type `["number", "null"]` and remove it from `required`.

Related: [[extraction-tool-pattern]], [[structured-output-tool-use]], [[enum-escape-hatches]], [[gray-area-examples]]

[Source: 22-CxwIKbepDwQ.md]
