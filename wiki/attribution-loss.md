---
title: Attribution Loss During Summarization
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Attribution loss is the progressive stripping of source details as findings pass through multiple levels of a multi-agent pipeline. By the time a claim reaches the final report, the original source, date, and data point may be entirely gone — leaving an authoritative-looking but unverifiable statement.

## The Three-Level Degradation Pattern

| Level | Agent | Example Output |
|-------|-------|---------------|
| 1 | Web researcher | "Nature Medicine (2025, doi:10.1038/...) found 94% accuracy in academic centers using a prospective cohort of 4,200 patients." |
| 2 | Coordinator | "Studies show high AI accuracy in medical diagnosis." |
| 3 | Synthesis agent | "AI is transforming healthcare." |

Each compression step is individually reasonable — the coordinator is summarizing, not lying. But the cumulative effect is that every verifiable detail disappears.

## Why It Matters

A final report built on Level 3 outputs:
- Cannot be fact-checked by a human reader
- Cannot distinguish a well-supported claim from an unsupported one
- Cannot be updated when a specific source is retracted or superseded

## The Fix: Claim-Source Mappings

The solution is [[claim-source-mappings]] — structured JSON objects that travel with every finding through every level of the pipeline. The synthesis agent must preserve them, not compress them away. "The claim stays attached to its source at every level. Nothing gets compressed away."

## Exam Question Framing

> "The exam question about provenance is simple. Can a human reader verify every claim in the final report? If not, provenance was lost somewhere in the pipeline."

Related: [[information-provenance]], [[claim-source-mappings]], [[provenance-anti-patterns]]

[Source: 31-EfQSimX5wlM.md]
