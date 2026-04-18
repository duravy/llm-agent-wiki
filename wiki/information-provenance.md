---
title: Information Provenance & Uncertainty (D5 T5.6)
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Information provenance is the discipline of preserving source attribution at every stage of a multi-agent synthesis pipeline, so that every claim in the final report is verifiable. Without explicit provenance mechanisms, summarization progressively strips away sources until the final output contains authoritative-looking but unverifiable claims.

## Core Analogy

> "Think of information provenance like a journalist's citations. The exam tests this directly because if your synthesis agent loses source attribution, the final report stops being verifiable."

## Five Key Concepts

| Concept | Summary | Wiki Page |
|---------|---------|-----------|
| Attribution loss | Each summarization level strips source details until nothing is verifiable | [[attribution-loss]] |
| Claim-source mappings | JSON object pairing every claim with source URL, name, date, excerpt, methodology | [[claim-source-mappings]] |
| Conflicting sources | Annotate both values with context; never pick a winner arbitrarily | [[conflicting-sources]] |
| Publication dates | Dates turn apparent contradictions into trends | [[publication-dates-provenance]] |
| Confidence-structured output | Well-established / contested / coverage gap tiers in final report | [[confidence-structured-output]] |
| Content-type rendering | Natural format per content type; uniform formatting is an anti-pattern | [[content-type-rendering]] |

## Core Exam Principle

> "Every claim needs a source. Every source needs a date. Every conflict needs context."

If any of these three is missing, the synthesis has failed — no matter how polished the final report looks.

## Exam Framework

| If the exam asks about… | The answer is… |
|------------------------|----------------|
| Preserving attribution | Structured claim-source mappings (URL, name, date, excerpt, methodology) |
| Conflicting sources | Annotate both values with context; never pick arbitrarily |
| Publication dates | Prevent temporal differences from being misread as contradictions |
| Output structure | Distinguish well-established / contested / coverage gaps |
| Rendering format | Each content type in natural format; tables, prose, lists — not uniform bullets |

## Anti-Patterns

See [[provenance-anti-patterns]] for the five exam-tested anti-patterns.

## Connection to Prior Topics

- [[subagent-metadata]] — established source/date/URL/methodology requirements on sub-agent outputs; claim-source mappings are the synthesis-layer extension of this
- [[coverage-annotations]] — the well-supported / partial / gap labels from D5 T5.3 map directly onto the well-established / contested / coverage-gap structure here
- [[escalation-ambiguity-resolution]] — when sources genuinely conflict and context is insufficient, escalation may be warranted rather than picking a winner

[Source: 31-EfQSimX5wlM.md]
