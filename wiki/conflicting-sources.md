---
title: Conflicting Sources
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

When two credible sources report different values for the same claim, the correct response is to annotate the conflict — presenting both values with their source context and explaining why they might differ. Arbitrarily picking one source and silently dropping the other is the most dangerous anti-pattern in information provenance.

## The Pattern

**Scenario:** Nature Medicine reports 94% AI diagnostic accuracy; JAMA reports 87%.

**Wrong approach:**
> "AI diagnostic accuracy is 94%." *(silently discards JAMA)*

**Right approach:**
> "AI diagnostic accuracy ranges from 87% (JAMA, community hospitals, 2024) to 94% (Nature Medicine, academic centers, 2025). The difference likely reflects the study populations: community hospitals vs. academic medical centers."

## Why Conflicts Exist (Common Explanations to Surface)

- **Methodology:** Different data collection or calculation approaches
- **Time frame:** One study is older (see [[publication-dates-provenance]])
- **Sample:** Different populations, geographies, or institution types
- **Sample size:** One study is better powered

Explaining *why* they might differ lets the reader evaluate both claims with full context. This is more honest and more useful than a false consensus.

## Why Picking One Is Dangerous

> "Never resolve a conflict by arbitrarily choosing a winner — that misleads the reader and destroys trust in the report."

A report that hides disagreement appears more certain than the evidence supports, which can lead to poor downstream decisions.

## Relationship to Coverage Annotations

[[coverage-annotations]] (D5 T5.3) established the concept of labeling findings as well-supported / partial / gap. Conflicting sources map to the "contested" tier in [[confidence-structured-output]] — they must be surfaced, not hidden.

## Exam Shortcut

If the question asks how to handle conflicting sources:
- Annotate both values with source context
- Explain why they might differ
- Never pick one arbitrarily

## Anti-Pattern

Picking one source when sources conflict (Anti-pattern #2 in [[provenance-anti-patterns]]).

Related: [[information-provenance]], [[publication-dates-provenance]], [[confidence-structured-output]]

[Source: 31-EfQSimX5wlM.md]
