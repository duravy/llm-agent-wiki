---
title: Human Review Anti-Patterns (D5 T5.5)
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Five exam-tested anti-patterns in human review workflow design. Each "looks reasonable on the surface but causes real problems in production."

## Quick Reference

| # | Anti-Pattern | Consequence | Fix |
|---|-------------|-------------|-----|
| 1 | Relying on aggregate accuracy metrics | Certain document types fail silently | Validate accuracy by document type and field separately |
| 2 | Trusting model confidence without calibration | Auto-accept extractions that are actually wrong | Calibrate thresholds with labeled validation sets |
| 3 | Random sampling without stratification | Almost never review the types that fail most | Use stratified sampling weighted toward difficult types |
| 4 | Automating all high-confidence extractions immediately | High confidence on rare types may still be wrong | Validate per-segment accuracy before automating any path |
| 5 | No feedback loop from human corrections | Same errors repeat indefinitely | Feed corrections into calibration updates and prompt improvements |

## Detail

**AP1 — Aggregate accuracy metrics**
The 97% aggregate looks great. But standard invoices at 99.5% dominate the average; handwritten receipts at 72% and foreign docs at 65% are invisible. See [[aggregate-accuracy-trap]].

**AP2 — Uncalibrated confidence**
Models are systematically overconfident. A model reporting 0.9 confidence may only be correct 85% of the time. Setting your threshold at 0.9 without calibration means auto-accepting wrong extractions. See [[confidence-calibration]].

**AP3 — Random sampling**
2% random sampling gives ~0.6 foreign doc reviews per day — almost no coverage of the hardest category. See [[stratified-sampling]].

**AP4 — Premature automation**
You've verified that high confidence is accurate on standard invoices, but not on handwritten receipts. Those may still have high-confidence wrong extractions. Validate per segment before automating any high-confidence path.

**AP5 — No feedback loop**
Human corrections fix the current output but don't update calibration thresholds or improve extraction prompts. The system never learns. See [[feedback-loop-improvement]].

Related: [[human-review-workflows]]

[Source: 30--sHcC8BWxD8.md]
