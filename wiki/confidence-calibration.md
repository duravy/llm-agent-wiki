---
title: Confidence Calibration
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Confidence calibration is the process of verifying that a model's reported confidence scores actually match its real-world accuracy — and adjusting routing thresholds accordingly. Models are systematically overconfident: a model reporting 0.9 confidence may be correct only 85% of the time.

## The Problem: Models Are Overconfident

Raw confidence scores from extraction models cannot be trusted directly. A model may consistently report high certainty while making frequent errors. If you set your auto-accept threshold at 0.9 based on the model's self-report, you will auto-accept extractions that are actually wrong ~15% of the time.

## Per-Field Confidence Scores

Instead of a single confidence score per document, the model should output confidence **per field**:

```
vendor_name:   0.98
invoice_date:  0.95
total_amount:  0.92
tax_amount:    0.60
category:      0.35
```

This enables targeted routing — only uncertain fields go to human review, not the entire document.

## Four-Step Calibration Process

1. **Create a labeled validation set** — 100+ documents with known correct extractions (ground truth)
2. **Run the extraction model** on all documents, collecting per-field confidence scores
3. **Compare reported confidence to actual accuracy** — e.g., the model says 0.9 but is correct only 85% of the time → overconfident
4. **Adjust thresholds** — if 0.9 is overconfident, the real "safe to automate" threshold may need to be 0.95 or higher

> "Never trust raw confidence scores. Always calibrate against known ground truth before setting automation thresholds."

## Exam Shortcut

If the question asks about confidence scores:
- Field-level scores enable targeted routing
- Raw scores lie — always calibrate against labeled validation sets before setting thresholds

## Anti-Pattern

Trusting model confidence without calibration leads to auto-accepting extractions that are actually wrong (Anti-pattern #2 in [[human-review-anti-patterns]]).

Related: [[human-review-workflows]], [[human-review-routing]], [[aggregate-accuracy-trap]]

[Source: 30--sHcC8BWxD8.md]
