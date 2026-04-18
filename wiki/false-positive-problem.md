---
title: False Positive Problem
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

A false positive is when a system flags something as a problem that isn't actually a problem. In automated code review, this means reporting correct code as a bug. The exam-critical consequence: high false positive rates destroy developer trust across **all** categories — not just the problematic one. [Source: 20-EzWRMJz9lXc.md]

## The Smoke Detector Analogy

A smoke detector that triggers every time you make toast → you stop trusting it → when there's a real fire, you ignore it.

Same mechanism in automated review:
1. System reports many findings
2. Most findings are false positives
3. Developer reviews a few, finds them wrong
4. Developer stops reviewing any findings
5. A real bug is filed as "another false alarm" and ignored

**The cascade:** High FP rate in one category → erodes trust in all findings → real bugs get missed.

## Two Contexts from Part 20

- **Code review:** Reporting correct code as buggy (e.g., flagging a style choice as a bug)
- **Data extraction:** Extracting information that isn't present in the source

Both stem from the same root: instructions that leave too much room for Claude's interpretation. [Source: 20-EzWRMJz9lXc.md]

## The Trust Erosion Cascade

```
Vague instructions
  → Claude flags everything that could possibly be an issue
  → 80% false positives
  → Developer reviews findings, finds mostly noise
  → Developer ignores all future findings
  → Real bugs in the remaining 20% get ignored too
```

Explicit criteria break this cascade:
```
Explicit criteria: flag only when X, skip Y and Z
  → 90% of findings are real issues
  → Developer trusts and acts on findings
  → Real bugs get fixed
```

## The Across-All-Categories Effect

A critical exam point: a high-false-positive category doesn't just undermine trust in itself — it **undermines trust in accurate categories too**. If 50% of your "security" findings are false positives, developers will start ignoring "performance" findings even if those are 95% accurate.

This is why the fix is to **disable** the high-FP category, improve its criteria, then re-enable — rather than living with the noise. [Source: 20-EzWRMJz9lXc.md]

## Exam Takeaway

> **High false positive rates destroy trust across all categories.** The smoke detector analogy is the exam's frame for this problem. The fix is not to lower thresholds — it's to replace subjective thresholds with [[categorical-definitions|categorical definitions]].

"After a few false alarms, you stop trusting it. And when there's a real fire, you ignore it."

## Connections

- [[prompt-design-explicit-criteria]] — T4.1 hub page
- [[categorical-definitions]] — the fix: explicit categorical criteria
- [[prompt-design-anti-patterns]] — anti-pattern #3: reporting all possible issues

[Source: 20-EzWRMJz9lXc.md]
