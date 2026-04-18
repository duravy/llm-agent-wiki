---
title: Confidence-Calibrated Routing for Code Review Findings
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

In the third pass of [[multipass-review-architecture]], each finding is rated 1–5 for confidence and routed accordingly. This prevents low-quality findings from creating noise in the pull request while ensuring high-confidence issues are automatically surfaced.

## The Three Routing Tiers

| Confidence Score | Action | Rationale |
|-----------------|--------|-----------|
| ≥ 4 (high) | Auto-post as PR comment | Finding is reliable; no human judgment needed |
| 2–3 (medium) | Route to human reviewer | Finding may be valid but needs judgment |
| ≤ 1 (low) | Discard | Not worth a reviewer's attention |

## Why Routing Matters

Without confidence routing, all findings from all passes go directly to the PR as comments. On a 14-file PR, this creates:
- Dozens of low-confidence findings cluttering the review
- Reviewers spending time on noise instead of real issues
- Trust erosion: reviewers start ignoring PR comments entirely

The routing pass **prioritizes limited human review capacity** on findings that actually need judgment.

## Relationship to D5 Confidence Calibration

This is the same architectural pattern as [[confidence-calibration]] and [[human-review-routing]] in D5 T5.5, applied to a different domain:

| Domain | What's being routed | Confidence basis |
|--------|---------------------|-----------------|
| D4 T4.6 (here) | Code review findings | 1–5 score per finding from verification pass |
| D5 T5.5 | Document extraction fields | 0–1 probability score per field |

Same principle: route high-confidence results to automation, uncertain results to human review, low-confidence results to discard. Different application layer.

## What the Verification Pass Does

The Pass 3 instance receives:
- All findings from Pass 1 (per-file local issues)
- All findings from Pass 2 (cross-file integration issues)

For each finding, it outputs:
- The finding itself
- A confidence score 1–5
- Enough context for the routing decision

The verification pass does **not** re-review the code — it evaluates the quality of the findings themselves.

## Exam Signal

> *"Auto-posting all findings as PR comments"* → creates noise, erodes trust → anti-pattern
> *"Add a verification pass with confidence routing"* → correct approach

> *"What routes high-confidence findings to automation and uncertain findings to human review?"*
> → Confidence-calibrated routing (verification pass with 1–5 scoring)

Related: [[multi-instance-review]], [[multipass-review-architecture]], [[confidence-calibration]], [[human-review-routing]], [[multi-instance-anti-patterns]]

[Source: 25-W7c9wNfoCjU.md]
