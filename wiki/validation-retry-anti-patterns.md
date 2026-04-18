---
title: Validation and Retry Anti-Patterns (D4 T4.4)
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

Five exam-tested anti-patterns in validation and retry loop design. Each one either encourages hallucination, prevents self-correction, or allows semantic errors to pass silently. [Source: 23-wyqmHtVGxAg.md]

## Anti-Pattern 1: Retrying When Data Is Absent from the Source

**What it looks like:** Validation flags a null field and immediately queues a retry, regardless of whether the source document contains that information.

**Why it fails:** If the document genuinely has no tax line, retrying either returns null again (correct but wasteful) or fabricates a tax amount (hallucination — actively worse than null). Retrying absent data is the primary route to confident hallucination in extraction pipelines.

**Fix:** Check if the information exists in the source before retrying. If it doesn't, accept null. See [[retriable-vs-nonretriable-failures]].

---

## Anti-Pattern 2: Generic Retry Without Specific Errors

**What it looks like:** Retry prompt says: "The extraction had errors. Please try again."

**Why it fails:** Claude doesn't know what to fix. It may return an identical extraction, make different mistakes, or guess randomly. The retry is useless.

**Fix:** Include the specific validation errors: "Line items total $92, but you reported $100. Tax is null, but the document shows $7.36 on line 8." See [[retry-with-error-feedback]].

---

## Anti-Pattern 3: No `detected_pattern` Field

**What it looks like:** Findings are emitted without a pattern identifier. Human reviewers dismiss them, but the dismissals carry no metadata.

**Why it fails:** Without pattern identifiers, false positive rates cannot be measured per pattern. The system cannot identify which detection criteria need improvement. The same false positives repeat indefinitely.

**Fix:** Add a `detected_pattern` string to every finding. See [[detected-pattern-field]].

---

## Anti-Pattern 4: Only Checking Structural Validity

**What it looks like:** Validation step verifies that the JSON is valid and all required fields are present, then accepts the result.

**Why it fails:** Structural validity ≠ semantic correctness. Valid JSON can contain wrong values: totals that don't match line items, dates that are inconsistent, fields extracted from the wrong part of the document.

**Fix:** Implement semantic validation — cross-check calculations, verify date consistency, use [[self-correction-dual-totals]] for numerical fields. See [[syntax-vs-semantic-errors]].

---

## Anti-Pattern 5: Unlimited Retries

**What it looks like:** Retry loop has no maximum — it keeps retrying until the extraction passes validation or the process times out.

**Why it fails:** If the information genuinely isn't in the source, no number of retries will find it. The loop wastes tokens on unfixable issues and may eventually hallucinate a value that passes validation checks.

**Fix:** Max 1–2 retries. If still failing, send to human review. See [[validation-retry-loops]] for the complete architecture.

---

## Summary

| Anti-Pattern | Root Cause | Fix |
|---|---|---|
| Retry absent data | Not checking source before retrying | [[retriable-vs-nonretriable-failures]] |
| Generic retry | No specific errors in prompt | [[retry-with-error-feedback]] |
| No detected_pattern | Can't analyze false positives | [[detected-pattern-field]] |
| Structural-only validation | Syntax ≠ semantics | [[self-correction-dual-totals]] |
| Unlimited retries | No escalation to human review | Max 1–2 → human review |

Related: [[validation-retry-loops]]

[Source: 23-wyqmHtVGxAg.md]
