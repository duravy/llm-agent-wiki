---
title: Example Count Guidelines
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

How many few-shot examples to include is a precision-versus-cost trade-off: more examples improve coverage but consume more context tokens. The exam tests the sweet spot numbers. [Source: 21-u6oCkdxU5kw.md]

## The Count Table

| Count | When to Use | Notes |
|-------|-------------|-------|
| **0 (zero-shot)** | Task is completely unambiguous | Rare. Most tasks have at least one edge case worth showing. |
| **1** | Basic format demonstration only | Not enough to teach judgment patterns. Don't use for gray areas. |
| **2–3** | Sweet spot for most tasks | Cover common cases + at least one edge case. Default choice. |
| **4–5** | Ambiguous domains with many gray areas | When 2–3 examples leave significant judgment gaps. |
| **10+** | Never | Anti-pattern. Diminishing returns; wastes context tokens. Use 2–4 structurally diverse examples instead. |

[Source: 21-u6oCkdxU5kw.md]

## The Diversity Rule

The count number is less important than diversity. Two examples that look identical teach Claude one pattern. Two structurally diverse examples — different formats, an edge case, a null case — teach Claude three patterns.

> **Make your 2–4 examples structurally diverse: different formats, edge cases, and null handling.**

This is why 10 similar examples produce worse results than 3 diverse ones: the model sees one pattern repeated ten times, not ten patterns once each. [Source: 21-u6oCkdxU5kw.md]

## The Minimum Coverage Checklist

For a 2–3 example set:
- [ ] At least one **common/happy path** case
- [ ] At least one **edge case** (boundary condition, unusual format, partial data)
- [ ] At least one **null/missing field** case (what to return when data is absent)

For a 4–5 example set, add:
- [ ] At least one **gray area** where the decision is non-obvious
- [ ] At least one **format variation** if the input structure varies

## Context Token Cost

Every example consumes context tokens — both the input and output portions. This is the reason to stay at 2–4 rather than adding more. The cost compounds in long agentic sessions where examples are included in every prompt.

The 10+ anti-pattern is not about correctness — it's about efficiency. The marginal value of example #8 is near zero while the token cost is the same as example #1. [Source: 21-u6oCkdxU5kw.md]

## Exam Takeaway

> **2–4 targeted, structurally diverse examples is the sweet spot.** 0 for fully unambiguous tasks (rare). 1 for format-only. 2–3 standard. 4–5 for ambiguous domains. 10+ is always wrong.

## Connections

- [[few-shot-prompting]] — T4.2 hub
- [[reasoning-in-examples]] — diversity matters more than count; each diverse example teaches a different judgment
- [[gray-area-examples]] — push toward 4–5 when gray areas dominate
- [[few-shot-anti-patterns]] — anti-pattern #3: too many examples (10+)

[Source: 21-u6oCkdxU5kw.md]
