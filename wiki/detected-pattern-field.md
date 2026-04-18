---
title: Detected Pattern Field (D4 T4.4)
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

A schema design pattern that enables systematic analysis of false positives and drives long-term improvement of extraction and detection systems. Each finding is tagged with a pattern identifier so dismissal trends can be tracked over time. [Source: 23-wyqmHtVGxAg.md]

## The Pattern

Add a `detected_pattern` string field to the extraction schema for findings that may be dismissed:

```python
{
    "finding": "Broad exception catch at line 45",
    "severity": "medium",
    "detected_pattern": "broad_exception_catch",
    "location": {"file": "auth.py", "line": 45}
}
```

## Why It Matters

Without pattern identifiers, dismissals are opaque: a human clicks "dismiss" and the information is lost.

With pattern identifiers, dismissals are analyzable:
- "broad_exception_catch has been dismissed 47 out of 50 times → likely a false positive"
- "missing_null_check has been dismissed 2 out of 30 times → mostly valid findings"

This feedback enables two improvements:
1. **Calibration updates** — adjust confidence thresholds for known high-false-positive patterns
2. **Prompt improvements** — refine detection criteria to reduce the false positive rate for that pattern

> **"Without pattern identifiers, you can't analyze false positive trends or improve detection criteria over time."** [Source: 23-wyqmHtVGxAg.md]

## Connection to Feedback Loop Improvement

The `detected_pattern` field is the data collection mechanism that makes the [[feedback-loop-improvement]] (D5 T5.5) possible in the extraction domain. D5's feedback loop feeds human corrections back into calibration and prompts; the `detected_pattern` field is what makes systematic (rather than ad hoc) analysis of those corrections tractable.

## Connection to Categorical Definitions

[[categorical-definitions]] (D4 T4.1) defines include/exclude lists for categories. The `detected_pattern` field provides the empirical data to validate whether those definitions are accurate — high dismiss rates on a pattern signal that the definition may be too broad.

## Exam Signal

> **"Always include the detected_pattern field for systematic dismissal analysis."** [Source: 23-wyqmHtVGxAg.md]

Anti-pattern: no `detected_pattern` field → can't analyze false positive trends → system never improves. See [[validation-retry-anti-patterns]].

Related: [[validation-retry-loops]], [[feedback-loop-improvement]], [[categorical-definitions]]

[Source: 23-wyqmHtVGxAg.md]
