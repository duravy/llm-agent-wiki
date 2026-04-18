---
title: Stratified Sampling
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Stratified sampling is a quality-monitoring technique that deliberately over-samples rare and difficult document categories to catch failures where they are most likely to occur. It is the correct alternative to simple random sampling for AI system auditing.

## The Problem with Random Sampling

Simple random sampling allocates review proportional to volume. For a system with:
- Standard invoices: 90% of volume
- Handwritten receipts: 7% of volume
- Foreign language docs: 3% of volume

A 2% random sample gives approximately:
- ~18 standard invoice reviews/day
- ~1.4 receipt reviews/day
- ~0.6 foreign doc reviews/day

You get almost no coverage of the types that fail most often.

## Stratified Sampling Fix

Sample at different rates by category, weighting difficult types higher:

| Document Type | Volume | Sample Rate | Reviews/Day |
|--------------|--------|-------------|-------------|
| Standard invoices | 90% | 2% | ~18 |
| Handwritten receipts | 7% | 15% | ~10 |
| Foreign language docs | 3% | 30% | ~9 |
| **Total** | | | **~37** |

Same total review budget, but the hardest types now get real coverage.

## The Principle

> Over-sample rare and difficult categories to catch problems where they are most likely to occur.

The "difficult" categories are typically identified by the [[aggregate-accuracy-trap]] analysis — the types hiding behind the good aggregate metric.

## Exam Shortcut

If the question asks about quality monitoring:
- Simple random sampling misses rare failures
- Stratified sampling over-samples difficult document types

## Anti-Pattern

Using random sampling without stratification (Anti-pattern #3 in [[human-review-anti-patterns]]) leads to almost never reviewing the document types that fail most often.

Related: [[human-review-workflows]], [[aggregate-accuracy-trap]], [[confidence-calibration]]

[Source: 30--sHcC8BWxD8.md]
