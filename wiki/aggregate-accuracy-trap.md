---
title: Aggregate Accuracy Trap
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

The aggregate accuracy trap is the mistake of trusting a single overall accuracy percentage that masks poor performance on specific document segments. A system can report 97% accuracy while silently failing on entire categories of inputs.

## The Example

| Document Type | Volume | Accuracy |
|--------------|--------|----------|
| Standard invoices | 90% | 99.5% |
| Handwritten receipts | 7% | 72% |
| Foreign language docs | 3% | 65% |
| **Aggregate** | **100%** | **~97%** |

The 97% overall accuracy is dominated by the high-volume, easy category. Handwritten receipts and foreign language documents — which fail nearly 1 in 3 times — are invisible in the headline number.

## Why It Happens

Aggregate metrics weight by volume. High-volume easy segments inflate the metric. Rare difficult segments get almost no representation — both in the accuracy number and in random quality sampling (see [[stratified-sampling]]).

## The Fix

Validate accuracy by document type AND by field before automating any high-confidence extraction path. A 99% accuracy on standard invoices does not justify automating handwritten receipt extractions.

> "Never trust a single accuracy percentage."

## Connection to Stratified Sampling

The aggregate accuracy trap explains *why* [[stratified-sampling]] is necessary: if rare segments are hidden in the aggregate, random sampling will under-review them and you'll never discover their actual failure rate.

## Exam Shortcut

If the question asks about accuracy metrics:
- Always validate by document type and field separately
- Never trust a single accuracy percentage

## Anti-Pattern

Relying only on aggregate accuracy metrics (Anti-pattern #1 in [[human-review-anti-patterns]]).

Related: [[human-review-workflows]], [[stratified-sampling]], [[confidence-calibration]]

[Source: 30--sHcC8BWxD8.md]
