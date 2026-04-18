---
title: Human Review Routing
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Human review routing is the decision logic that determines what happens to each extracted field based on its confidence score. Routing operates at the field level, not the document level — one document can have some fields auto-accepted and others flagged for review.

## Routing Thresholds

| Confidence | Action | Notes |
|-----------|--------|-------|
| ≥ 0.95 | **Auto accept** | High confidence; safe to automate |
| 0.5–0.95 | **Human review** | Uncertain; send to reviewer |
| < 0.5 | **Flag as failed** | Do not guess; mark as unable to extract |

> "This targeted approach saves human time while catching the uncertain fields that matter."

## Key Design Decisions

**Per-field, not per-document:** A document with vendor_name at 0.98 and category at 0.35 should not be rejected in full. Route field-by-field so auto-acceptable fields are not held up by one uncertain field.

**"Do not guess" at low confidence:** Fields below 0.5 should be flagged as extraction failures, not assigned a best-guess value. A wrong auto-accept is worse than an explicit gap.

**Thresholds must be calibrated:** The 0.95 threshold is not a universal constant. After [[confidence-calibration]], your actual safe threshold may be higher or lower depending on how overconfident your model is.

## Reviewer Interface

The human reviewer sees:
- The original document
- The model's extraction output
- The specific fields flagged for review
- The confidence score for each flagged field

This allows the reviewer to focus effort rather than re-reading the entire document.

## Exam Shortcut

If the question asks about the review workflow:
- Route by confidence: auto accept high / human review medium / flag low as failed
- The key is **per-field routing**, not whole-document decisions

Related: [[human-review-workflows]], [[confidence-calibration]], [[feedback-loop-improvement]]

[Source: 30--sHcC8BWxD8.md]
