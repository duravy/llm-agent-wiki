---
title: Validation and Retry Loops (D4 T4.4)
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

Task 4.4 of Domain 4. Schema enforcement (T4.3) guarantees valid JSON structure. Validation and retry loops guarantee correct values. Both are required for a robust extraction pipeline. [Source: 23-wyqmHtVGxAg.md]

## The Core Distinction

> **"Schema enforcement guarantees valid JSON. Validation loops guarantee correct data. You need both."** [Source: 23-wyqmHtVGxAg.md]

> **Analogy:** A spell checker catches typos (syntax) but not factual errors (semantics). Tool use is the spell checker — it guarantees valid JSON syntax, not that the facts inside are correct. [Source: 23-wyqmHtVGxAg.md]

## The Complete Feedback Loop Architecture

```
1. Extract → structured JSON (T4.3)
2. Validate → semantic checks (do totals match? are dates consistent?)
3. Errors found?
   ├── No → Accept
   └── Yes → Retryable?
             ├── Yes → Retry with specific error feedback → Revalidate
             │         ├── Pass → Accept
             │         └── Fail (after 1–2 retries) → Human review
             └── No  → Accept null OR flag for human review
```

## Key Concepts

| Concept | Page | One-Line Summary |
|---------|------|-----------------|
| Retry with Error Feedback | [[retry-with-error-feedback]] | Send original doc + failed extraction + specific errors; Claude self-corrects |
| Retriable vs Non-Retriable Failures | [[retriable-vs-nonretriable-failures]] | Format errors: retry; absent data: accept null (retry = hallucination risk) |
| Self-Correction via Dual Totals | [[self-correction-dual-totals]] | Extract stated_total + calculated_total; conflict_detected boolean flags discrepancies |
| Detected Pattern Field | [[detected-pattern-field]] | Tag findings with pattern IDs; enables false positive trend analysis |
| Anti-Patterns | [[validation-retry-anti-patterns]] | Five: retry absent data, generic retry, no pattern field, structural-only, unlimited retries |

## Five Exam Must-Knows

1. Retry with error feedback = original document + failed extraction + specific validation errors in the retry prompt
2. Retries only fix format mismatches and structural errors — not information absent from the source
3. Self-correction = extract both `stated_total` and `calculated_total`; flag `conflict_detected: true` when they differ
4. Semantic validation (do values make sense?) is a separate step from schema validation (is the JSON valid?)
5. Max 1–2 retries, then human review — unlimited retries waste tokens on unfixable issues

## Exam Anchor

> **"Know when to accept null (data genuinely missing) versus when to retry (data exists but was missed or misformatted)."** [Source: 23-wyqmHtVGxAg.md]

## Cross-Domain Connections

- [[syntax-vs-semantic-errors]] (D4 T4.3) — T4.3 eliminates syntax errors; T4.4 addresses semantic errors; the two-stage pipeline
- [[structured-output-tool-use]] (D4 T4.3) — T4.3 provides structure; T4.4 provides correctness; natural pairing
- [[nullable-fields]] (D4 T4.3) — nullable fields prevent hallucination at schema level; non-retriable failures are the runtime manifestation of the same principle: absent data should return null, not a fabricated value
- [[feedback-loop-improvement]] (D5 T5.5) — D5 feedback loop: human corrections → calibration updates + prompt improvements; D4 feedback loop: validation errors → retry with specific feedback; same architecture pattern at different scales
- [[local-error-recovery]] (D2 T2.2) — sub-agent retry before escalation; same retry-then-escalate structure as T4.4's retry-then-human-review
- [[iterative-refinement-techniques]] (D3 T3.5) — T3.5 test-driven iteration: share exact failure diff; T4.4 retry with error feedback: include specific validation errors; same principle — precise error messages enable self-correction

[Source: 23-wyqmHtVGxAg.md]
