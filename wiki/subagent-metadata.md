---
title: Subagent Metadata Requirements
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

When sub-agents return research findings to a synthesis agent, they must include structured metadata alongside their content. Without metadata, downstream agents cannot verify sources, assess recency, or make accurate citations — a 2020 statistic and a 2025 statistic become indistinguishable.

## Required Metadata Fields

| Field | Purpose |
|-------|---------|
| `source` | Where the information came from |
| `date` | When the information was published or retrieved |
| `url` | Direct link for verification |
| `methodology` | How the data was obtained or calculated |

## The Analogy

Think of metadata like a bibliography card. Without it, you have a fact with no provenance. The synthesis agent may cite it, but can't verify it, which is especially dangerous when temporal context matters — regulatory requirements, pricing, model capabilities all change over time.

## Anti-Pattern: Content Without Metadata

```
# Bad
Sub-agent returns: "The market grew 23% last year."

# Good  
Sub-agent returns:
  content: "The market grew 23% last year."
  source: "Gartner AI Market Report"
  date: 2024-11-15
  url: https://...
  methodology: "Annual survey of 500 enterprises"
```

See [[context-anti-patterns]] (anti-pattern #4).

Related: [[context-management]], [[domain-5-overview]], [[subagent-delegation-exploration]], [[claim-source-mappings]]

> Note: [[subagent-delegation-exploration]] covers a different subagent concern — keeping the main agent's context clean by having sub-agents return summaries instead of raw file content. Metadata requirements (this page) concern provenance and recency of research findings.
>
> Note: [[claim-source-mappings]] (D5 T5.6) extends this concept to the synthesis layer. The four metadata fields here (source, date, url, methodology) are a subset of the five fields in claim-source mappings, which also require a direct excerpt. The key addition in T5.6 is that these mappings must survive every level of summarization, not just the sub-agent output.

[Source: 26-H8Vpe9j4N5I.md]
