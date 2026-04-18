---
title: Coverage Annotations
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Coverage annotations are labels added to synthesized output that tell the reader which findings are well-supported, which have partial coverage, and which are gaps requiring further research. They are the user-facing component of [[graceful-degradation]].

## The Weather Report Analogy

A good weather report tells you which forecasts are based on satellite data and which are educated guesses. The reader knows exactly how much to trust each prediction.

Coverage annotations do the same for multi-agent research outputs.

## Three Annotation Levels

| Level | Meaning | Example label |
|-------|---------|--------------|
| Well-supported | Multiple sources confirmed this | "5 sources — high confidence" |
| Partial coverage | Some sources unavailable | "PubMed unavailable — partial data" |
| Gap | No coverage, needs manual research | "No sources found — manual verification needed" |

## Example in Practice

A multi-agent research system covering three areas where PubMed was down:

> **Diagnostic applications** — well-supported (5 sources, all retrieved successfully)
>
> **Drug discovery** — partial coverage (PubMed unavailable during this run; 2 alternative sources used)
>
> **Mental health applications** — gap (no sources retrieved; recommend manual research)

The reader knows exactly which findings are reliable and which need verification. No silent misinformation.

## Why This Matters for the Exam

Without coverage annotations, the reader has no way to distinguish reliable findings from guesses. The output appears complete but is opaque about its reliability. The exam tests that annotating gaps is the coordinator's responsibility after a partial failure.

Related: [[graceful-degradation]], [[structured-error-reporting]], [[error-propagation-multiagent]], [[confidence-structured-output]]

> Note: [[confidence-structured-output]] (D5 T5.6) applies the same three-tier structure to final synthesis reports. The taxonomy maps directly: well-supported → well-established, partial → contested, gap → coverage gap. Coverage annotations (this page) label pipeline outputs after partial failures; confidence-structured output organizes the final report by evidence strength.

[Source: 28-mOUeDdo8rF0.md]
