---
title: Publication Dates in Provenance
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Publication dates are a required field in sub-agent outputs. Without them, temporal differences between sources are misread as contradictions. With them, apparent contradictions become interpretable trends.

## The Core Example

**Without dates:**
> Source A: "AI accuracy is 87%"
> Source B: "AI accuracy is 94%"
> → Looks like a contradiction. Which is correct?

**With dates:**
> Source A: "AI accuracy is 87%" (2024)
> Source B: "AI accuracy is 94%" (2025)
> → Not a contradiction — a trend showing improvement over time.

> "That is the exam anchor. Always require publication dates in subagent outputs."

## Why Dates Get Dropped

Under token pressure or when summarizing for brevity, dates are often the first metadata dropped. This is why they must be a required field in the output contract, not an optional annotation.

## Connection to Subagent Metadata

[[subagent-metadata]] (D5 T5.1) established `date` as one of the four required metadata fields on sub-agent research outputs. Publication dates in provenance extend this requirement: dates must survive all the way to the synthesis layer and appear in [[claim-source-mappings]], not just the sub-agent response.

## Connection to Conflicting Sources

Dates are often the explanation for [[conflicting-sources]]. Before annotating a conflict, check whether the values are from different time periods — what looks like disagreement may be a measurement over time.

## Exam Shortcut

If the question asks about publication dates:
- Dates prevent temporal differences from being misread as contradictions
- Always require them in subagent outputs as part of the output contract

## Anti-Pattern

Omitting publication dates (Anti-pattern #3 in [[provenance-anti-patterns]]).

Related: [[information-provenance]], [[claim-source-mappings]], [[conflicting-sources]], [[subagent-metadata]]

[Source: 31-EfQSimX5wlM.md]
