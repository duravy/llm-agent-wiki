---
title: Retriable vs Non-Retriable Failures (D4 T4.4)
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

The most exam-tested distinction in Task 4.4. Two categories of extraction failure exist and only one of them can be fixed by retrying. Retrying the wrong category encourages hallucination. [Source: 23-wyqmHtVGxAg.md]

## The Two Categories

### Retriable: Format mismatches and structural errors

These are errors where the information *exists* in the source document but was extracted incorrectly.

| Example | What went wrong | Can retry fix it? |
|---------|----------------|-------------------|
| Date extracted as `03/17/2025` instead of `2025-03-17` | Wrong format | Yes — Claude can reformat |
| Missed a line item in a table | Misread source | Yes — Claude can re-read more carefully |
| Total doesn't match sum of line items | Arithmetic error | Yes — Claude can recalculate |
| Invoice number extracted with wrong digits | Misread field | Yes — Claude can verify |

### Not Retriable: Information absent from the source

These are errors where the information simply does not exist in the source document.

| Example | What went wrong | Can retry fix it? |
|---------|----------------|-------------------|
| Tax field returns null — document has no tax line | Data absent | No — retrying returns null again or invents a value |
| PO number absent — this vendor doesn't use PO numbers | Data absent | No — same result |
| "See Appendix B" but Appendix B isn't provided | External reference | No — information is elsewhere |

> **"If the document doesn't contain the information, retrying either returns null again (correct) or fabricates a value (hallucination and actually worse)."** [Source: 23-wyqmHtVGxAg.md]

## The Decision Rule

**Before retrying, check: does the information actually exist in the source?**

- If yes → retry with specific error feedback (see [[retry-with-error-feedback]])
- If no → accept `null` (this is correct behavior); or flag for human review if the field is critical

This decision maps directly onto [[nullable-fields]] (T4.3): nullable fields at the schema level express that absence is valid data, not a failure requiring correction.

## Connection to the Feedback Loop

In the complete [[validation-retry-loops]] architecture, this decision is the branch point:

```
Errors found → Retryable?
├── Yes → Retry with specific error feedback
└── No  → Accept null OR flag for human review
```

## Exam Anchor

> **"Know when to accept null (data genuinely missing) versus when to retry (data exists but was missed or misformatted)."** [Source: 23-wyqmHtVGxAg.md]

Exam question form: "Extraction of a `tax` field returns null. The source document doesn't show any tax line. What is the correct action?" Answer: Accept null — this is not retriable. Retrying risks hallucination.

Related: [[validation-retry-loops]], [[retry-with-error-feedback]], [[nullable-fields]]

[Source: 23-wyqmHtVGxAg.md]
