---
title: Human Review Workflows (D5 T5.5)
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Human review workflows define how to route AI extraction outputs to humans selectively — auto-accepting high-confidence results, flagging uncertain ones, and feeding corrections back into the system. The goal is to treat human time as a scarce resource and deploy it only where AI judgment is genuinely uncertain.

## Core Principle

No AI extraction system is 100% accurate. Human review is the safety net, but human time is expensive. The solution is intelligent routing — not reviewing everything, not reviewing nothing, but routing only the uncertain cases.

> Airport security scanner analogy: if it flags every bag, the line is impossibly long; if it flags none, it misses threats. Good calibration flags the right bags.

## Five Key Concepts

| Concept | Summary | Wiki Page |
|---------|---------|-----------|
| Aggregate accuracy trap | 97% overall can hide 65% accuracy on rare segments | [[aggregate-accuracy-trap]] |
| Per-field confidence scores | Output confidence per field, not per document | [[confidence-calibration]] |
| Confidence calibration | Model scores are systematically overconfident; calibrate against labeled sets | [[confidence-calibration]] |
| Human review routing | ≥0.95 auto accept / 0.5–0.95 review / <0.5 flag failed | [[human-review-routing]] |
| Stratified sampling | Over-sample rare/difficult types; random sampling misses them | [[stratified-sampling]] |
| Feedback loop | Human corrections → calibration updates + prompt improvements | [[feedback-loop-improvement]] |

## Full Workflow

1. Model extracts fields with per-field confidence scores
2. **Routing:** ≥0.95 → auto accept; 0.5–0.95 → human review; <0.5 → flag failed
3. Human reviewer sees: original document + model extraction + flagged fields + confidence scores
4. Reviewer corrects or confirms each field
5. **Corrections feed back into:** final output, calibration data, error pattern analysis → prompt improvements

## Exam Framework

| If the exam asks about… | The answer is… |
|------------------------|----------------|
| Accuracy metrics | Validate by document type and field; never trust a single % |
| Confidence scores | Field-level scores + calibrate against labeled validation sets |
| Quality monitoring | Stratified sampling; over-sample difficult types |
| Review workflow | Route by confidence per field: auto / human / flag |
| System improvement | Corrections must feed back into calibration + prompt improvements |

## Anti-Patterns

See [[human-review-anti-patterns]] for the five exam-tested anti-patterns.

## Connection to Prior Topics

- [[escalation-ambiguity-resolution]] — mid-conversation escalation to humans (reactive); human review workflows = systematic post-extraction routing (proactive)
- [[multi-instance-review]] — also uses confidence calibration to route findings
- [[subagent-metadata]] — provenance metadata supports accurate human review (reviewer needs source/date to assess correctness)

[Source: 30--sHcC8BWxD8.md]
