---
title: Categorical Definitions vs Subjective Thresholds
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The exam decision rule for T4.1. Subjective thresholds ("be conservative," "only report high confidence findings") produce inconsistent results because Claude decides what they mean differently each run. Categorical definitions — explicit lists of what to report and what to skip — produce consistent results because there is nothing to interpret. [Source: 20-EzWRMJz9lXc.md]

## The Exam Decision Rule

| | Subjective Threshold | Categorical Definition |
|---|---|---|
| **Example instruction** | "Only report high confidence findings" | List: null pointer dereference, uncaught async errors, SQL injection |
| **How Claude handles it** | Decides what counts as "high confidence" each run | Matches code against explicit list — no judgment needed |
| **Result** | Varies wildly run to run | Same result every run |
| **Trust level** | Low — output unreliable | High — output consistent |

The exam tests this directly. When a question presents a vague instruction like "be conservative" or "only flag obvious issues," the answer is always: replace it with a categorical definition. [Source: 20-EzWRMJz9lXc.md]

## What a Categorical Definition Contains

A complete categorical definition has two halves — both are required:

**What to report (include list):**
- Null pointer dereferences
- Uncaught async errors
- SQL injection (user input directly in query without parameterization)

**What to skip (exclude list):**
- Code style issues
- Minor naming conventions
- Missing documentation

The exclude list is as important as the include list. Without it, Claude defaults to reporting everything it notices. [Source: 20-EzWRMJz9lXc.md]

## Why the Exclude List Matters

Partial categorical definitions — include list only — still produce false positives. Claude will flag everything not covered by the include list if there's no explicit "do not flag X" guidance. Defining what to skip is the mechanism for suppressing the [[false-positive-problem|false positive problem]].

> "Do not flag outdated parameter names, missing comments, or style issues."

That sentence eliminates an entire class of false positives.

## Exam Takeaway

> **Categorical definitions produce consistent results. Subjective thresholds produce inconsistent results. Know the difference.** This is stated directly as an exam signal in Part 20.

When you see "be conservative," "only report high confidence," or "flag obvious issues" — the correct answer is always: define specific categories, not thresholds.

## Connection to Probabilistic vs Deterministic

Subjective thresholds = [[probabilistic-vs-deterministic|probabilistic]]: Claude interprets them, varies run to run. Categorical definitions = deterministic: the rule is enumerated, no interpretation needed. This is the D4 instance of the fundamental D1 T1.4 enforcement distinction. [Source: 20-EzWRMJz9lXc.md]

## Connections

- [[prompt-design-explicit-criteria]] — T4.1 hub
- [[false-positive-problem]] — the problem categorical definitions solve
- [[severity-levels-examples]] — the same principle applied to severity classification
- [[probabilistic-vs-deterministic]] (D1 T1.4) — categorical = deterministic; subjective = probabilistic
- [[explicit-escalation-criteria]] (D5 T5.2) — same principle in escalation design

[Source: 20-EzWRMJz9lXc.md]
