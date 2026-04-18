---
title: Feedback Loop Improvement
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

The feedback loop is the mechanism by which human corrections are fed back into the extraction pipeline to improve future performance. It is described as "the critical step that most systems miss." Without it, the same errors repeat indefinitely.

## Three Destinations for Human Corrections

When a human reviewer corrects or confirms an extracted field, those corrections must flow into three places:

| Destination | Effect |
|------------|--------|
| **Final output** | The corrected values appear in the delivered result |
| **Calibration data** | Confidence thresholds are updated based on actual error patterns |
| **Error pattern analysis** | Extraction prompts are improved over time to reduce recurrence |

Most systems implement destination #1 but skip #2 and #3.

## Why It Matters

> "A system without this feedback loop never gets better."

Without calibration updates, the confidence thresholds drift out of alignment as document distributions change. Without prompt improvements, systematic extraction errors (e.g., always misreading a particular date format) continue indefinitely.

## Relationship to Confidence Calibration

The feedback loop is the ongoing mechanism that keeps [[confidence-calibration]] accurate after initial deployment. Initial calibration uses a static labeled validation set; the feedback loop continuously updates calibration as real-world errors are captured.

## Exam Shortcut

If the question asks about system improvement:
- Human corrections must feed back into calibration updates AND prompt improvements
- A system that doesn't close the feedback loop never gets better

## Anti-Pattern

No feedback loop from human corrections (Anti-pattern #5 in [[human-review-anti-patterns]]) means the same errors repeat indefinitely.

## Connection to Validation and Retry Loops (D4 T4.4)

D4 T4.4 has its own feedback loop operating at a different scale. The [[detected-pattern-field]] in extraction schemas enables systematic analysis of which detection patterns produce false positives — that data then feeds prompt improvements for those patterns. D4's loop is automated (validation → retry → human review); D5's loop requires human corrections → calibration updates + prompt improvements. Same architecture, different automation levels.

Related: [[human-review-workflows]], [[confidence-calibration]], [[human-review-routing]], [[detected-pattern-field]]

[Source: 30--sHcC8BWxD8.md, 23-wyqmHtVGxAg.md]
