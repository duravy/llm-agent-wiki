---
title: Provenance Anti-Patterns (D5 T5.6)
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Five exam-tested anti-patterns that destroy provenance in multi-agent systems. "The exam tests these directly. Learn each failure pattern and its fix."

## Quick Reference

| # | Anti-Pattern | Consequence | Fix |
|---|-------------|-------------|-----|
| 1 | Summarizing without claim-source mappings | Unverifiable claims that look authoritative | Require structured claim-source mappings as a hard output contract |
| 2 | Picking one source when sources conflict | Hidden contradictions; report appears more certain than evidence supports | Annotate both values with source context and explain why they differ |
| 3 | Omitting publication dates | Temporal differences misread as contradictions | Always require publication dates in subagent output contracts |
| 4 | Uniform format for all content types | Loses natural clarity of each content type | Render each type in its natural format (tables, prose, lists) |
| 5 | No confidence distinction | Reader cannot tell solid findings from speculative ones | Structure output by well-established / contested / coverage gaps |

## Detail

**AP1 — Summarization without claim-source mappings**
Sub-agents return prose like "AI is transforming healthcare" with no source attached. The synthesis agent has nothing to cite. The final report looks authoritative but is unverifiable. See [[claim-source-mappings]].

**AP2 — Picking one source when sources conflict**
Two credible studies report 87% and 94%. The synthesis agent picks 94% and drops 87% silently. The reader sees a confident claim; the disagreement is hidden. Potentially dangerous for decision-making. See [[conflicting-sources]].

**AP3 — Omitting publication dates**
A 2022 study (75%) and a 2025 study (94%) look like a contradiction without dates. With dates they become a trend. See [[publication-dates-provenance]].

**AP4 — Uniform bullet point format**
Financial tables, news narratives, and technical specs all become bullet lists. Financial comparisons lose column alignment; news loses narrative flow. See [[content-type-rendering]].

**AP5 — No confidence distinction**
A peer-reviewed meta-analysis and an unreviewed preprint sit side-by-side with equal visual weight. The reader has no way to calibrate trust. See [[confidence-structured-output]].

Related: [[information-provenance]]

[Source: 31-EfQSimX5wlM.md]
