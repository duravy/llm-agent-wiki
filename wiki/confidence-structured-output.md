---
title: Confidence-Structured Output
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Confidence-structured output organizes the final synthesis report into three tiers based on the strength of evidence behind each finding. This tells the reader how much trust to place in each claim — something a flat list of findings cannot do.

## Three Tiers

| Tier | Definition | Example signal |
|------|-----------|----------------|
| **Well-established** | Multiple credible sources independently support the same claim | "Five peer-reviewed studies agree that..." |
| **Contested** | Credible sources disagree — both values preserved with context | "Nature Medicine found 94%; JAMA found 87%, likely due to..." |
| **Coverage gaps** | Evidence too thin to make a confident claim | "No peer-reviewed data on rural hospital populations was found." |

## Why Structure by Confidence

Without tiers, a claim backed by five peer-reviewed studies sits next to a claim from a single unreviewed preprint. The reader cannot distinguish solid evidence from speculation. This "undermines the entire purpose of multi-source research."

## Relationship to Coverage Annotations

[[coverage-annotations]] (D5 T5.3) established the well-supported / partial / gap labeling pattern for error reporting in agent pipelines. Confidence-structured output applies the same principle to the final synthesis report — the taxonomy maps directly:

| Coverage Annotations (T5.3) | Confidence-Structured Output (T5.6) |
|-----------------------------|--------------------------------------|
| Well-supported | Well-established |
| Partial | Contested |
| Gap | Coverage gap |

## Relationship to Conflicting Sources

The "contested" tier is where [[conflicting-sources]] belong. Rather than resolving the conflict by picking a winner, the contested tier preserves both values with context.

## Exam Shortcut

If the question asks about output structure:
- Distinguish well-established, contested, and coverage gap findings
- Each section tells the reader how much trust to place in the claims

## Anti-Pattern

No confidence distinction (Anti-pattern #5 in [[provenance-anti-patterns]]) — all findings presented with equal weight.

Related: [[information-provenance]], [[coverage-annotations]], [[conflicting-sources]]

[Source: 31-EfQSimX5wlM.md]
