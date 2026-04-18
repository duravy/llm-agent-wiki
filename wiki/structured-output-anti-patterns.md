---
title: Structured Output Anti-Patterns (D4 T4.3)
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

Five exam-tested anti-patterns in structured output design. Each one either reintroduces the unreliability that tool use was meant to eliminate, or allows hallucination and misclassification to persist. [Source: 22-CxwIKbepDwQ.md]

## Anti-Pattern 1: Prose JSON Request

**What it looks like:** `"Respond in JSON format with fields: vendor_name, total, category"`

**Why it fails:** Claude interprets the instruction probabilistically — missing braces, trailing commas, unquoted keys, fields appearing in different orders, extra fields appearing. Even when the JSON is valid, the structure may not match what the code expects.

**Fix:** Use the [[extraction-tool-pattern]] with a real JSON schema. The API enforces structure; no interpretation required.

---

## Anti-Pattern 2: All Fields Required

**What it looks like:** Every field in the schema has `"required": true`, even fields that may not appear in every source document.

**Why it fails:** Claude cannot leave a required field empty. For absent data, it fabricates a plausible-sounding value. The JSON is valid; the data is invented.

**Fix:** See [[nullable-fields]]. Mark optional fields with `["type", "null"]` and remove them from the `required` list.

---

## Anti-Pattern 3: Closed Enums (No Escape Hatches)

**What it looks like:** `"enum": ["supplies", "services", "software"]` with no `unclear` or `other`.

**Why it fails:** Ambiguous or novel cases force Claude to pick the nearest option. The classification is wrong; the confidence it implies is false.

**Fix:** See [[enum-escape-hatches]]. Add `"unclear"` (can't determine from source) and `"other"` (doesn't fit any bucket) to every enum. Add a `category_detail` string field for `"other"` cases.

---

## Anti-Pattern 4: Using `tool_choice: auto` for Extraction

**What it looks like:** Using the default `auto` mode when an extraction schema is defined.

**Why it fails:** In `auto` mode, Claude may decide plain text is an appropriate response. If Claude returns text instead of calling the tool, there's no structured output at all — the pipeline breaks silently or requires brittle text parsing as a fallback.

**Fix:** Use `force` (specific tool name, when document type is known) or `any` (must call some tool, when document type varies). Never use `auto` for extraction. See [[tool-choice-modes]].

---

## Anti-Pattern 5: Treating Tool Use as Full Validation

**What it looks like:** Assuming that because tool use produces valid JSON, the extracted values are correct. Skipping semantic validation.

**Why it fails:** Tool use guarantees syntax (valid JSON structure). It cannot guarantee semantics (correct values). Totals may not match line items; dates may be misread; numbers may be from the wrong field.

**Fix:** See [[syntax-vs-semantic-errors]]. Follow T4.3 (structural extraction) with T4.4 (semantic validation and retry loops).

---

## Summary

| Anti-Pattern | Root Cause | Fix |
|---|---|---|
| Prose JSON request | Probabilistic compliance | [[extraction-tool-pattern]] |
| All fields required | Absent data → fabrication | [[nullable-fields]] |
| Closed enums | Ambiguous data → misclassification | [[enum-escape-hatches]] |
| `auto` for extraction | Claude may return text | `force` or `any` via [[tool-choice-modes]] |
| No semantic validation | Syntax ≠ correctness | [[syntax-vs-semantic-errors]] + T4.4 |

Related: [[structured-output-tool-use]], [[extraction-tool-pattern]]

[Source: 22-CxwIKbepDwQ.md]
