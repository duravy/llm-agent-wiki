---
title: Claim-Source Mappings
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Claim-source mappings are structured JSON objects that pair every extracted claim with its source metadata. They are the primary mechanism for preventing [[attribution-loss]] in multi-agent synthesis pipelines. The source calls this "the single most important technique for preventing attribution loss."

## Required Fields

Every sub-agent output must include a structured mapping with:

| Field | Purpose |
|-------|---------|
| `source_name` | The publication or organization |
| `url` | Direct link for verification |
| `publication_date` | When the source was published (see [[publication-dates-provenance]]) |
| `excerpt` | Direct quote or data point from the source |
| `methodology` | How the data was obtained or calculated |

## Example

```json
{
  "claim": "AI diagnostic accuracy reached 94% in academic centers",
  "source_name": "Nature Medicine",
  "url": "https://...",
  "publication_date": "2025-03",
  "excerpt": "Across 12 academic medical centers, AI-assisted diagnosis achieved 94.1% accuracy...",
  "methodology": "Prospective cohort study, n=4,200"
}
```

## How It Works in the Pipeline

The synthesis agent receives structured claim-source mappings from sub-agents and **must preserve them** when combining findings into the final report. The claim stays attached to its source at every level of summarization — nothing gets compressed away.

> "This is a hard output contract, not a suggestion."

Setting it as a suggestion means sub-agents will skip it when under token pressure. It must be enforced as a required output schema.

## Relationship to Subagent Metadata

[[subagent-metadata]] (D5 T5.1) established that sub-agents must attach source/date/URL/methodology to research findings. Claim-source mappings are the synthesis-layer extension: the same metadata must survive not just the sub-agent output but every level of summarization up to the final report.

## Exam Shortcut

If the question asks what preserves attribution:
- Structured claim-source mappings: source URL, name, date, excerpt, methodology
- Hard contract, not a suggestion

## Anti-Pattern

Summarizing findings without claim-source mappings (Anti-pattern #1 in [[provenance-anti-patterns]]) results in unverifiable claims that look authoritative but cannot be traced to any source.

Related: [[information-provenance]], [[attribution-loss]], [[subagent-metadata]]

[Source: 31-EfQSimX5wlM.md]
