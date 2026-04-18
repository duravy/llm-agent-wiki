---
title: Domain 4 — Prompt Engineering and Output Quality
created: 2026-04-16
last_updated: 2026-04-17
source_count: 6
status: draft
---

Domain 4 covers how to get reliable, consistent results from Claude Code every time. The domain moves from "what Claude can do" (Domains 1–3) to "how to design prompts that produce deterministic, trustworthy output." It spans Parts 20–25 (T4.1–T4.6 confirmed; **complete**).

> **Domain Note:** Part 25 ([[multi-instance-review]]) is explicitly labeled "task 4.6 in domain 4" in the source transcript. The [[series-overview]] had it listed under Domain 5 based on earlier inference — that was incorrect. The video transcript is authoritative.

## Known Tasks

| Task | Topic | Wiki Page | Source Part |
|------|-------|-----------|-------------|
| 4.1 | Prompt Design and Explicit Criteria | [[prompt-design-explicit-criteria]], [[false-positive-problem]], [[categorical-definitions]], [[severity-levels-examples]], [[prompt-design-anti-patterns]] | Part 20 |
| 4.2 | Few-Shot Prompting | [[few-shot-prompting]], [[reasoning-in-examples]], [[gray-area-examples]], [[example-count-guidelines]], [[few-shot-anti-patterns]] | Part 21 |
| 4.3 | Structured Output with JSON Schemas | [[structured-output-tool-use]], [[extraction-tool-pattern]], [[nullable-fields]], [[enum-escape-hatches]], [[syntax-vs-semantic-errors]], [[structured-output-anti-patterns]] | Part 22 |
| 4.4 | Validation and Retry Loops | [[validation-retry-loops]], [[retry-with-error-feedback]], [[retriable-vs-nonretriable-failures]], [[self-correction-dual-totals]], [[detected-pattern-field]], [[validation-retry-anti-patterns]] | Part 23 |
| 4.5 | Batch Processing Strategies | [[message-batches-api]], [[batch-vs-synchronous-api]], [[custom-id-tracking]], [[partial-failure-resubmission]], [[two-phase-batch-strategy]], [[batch-anti-patterns]] | Part 24 |
| 4.6 | Multi-Instance Review | [[multi-instance-review]], [[self-review-bias]], [[multipass-review-architecture]], [[confidence-calibrated-routing]], [[multi-instance-anti-patterns]] | Part 25 |

> Note: Task numbers and part assignments for T4.2–T4.4 are inferred from the Domain 4 introduction in Part 20. Confirm as parts are ingested.

## Domain Introduction

The core problem Domain 4 addresses: **vague instructions produce inconsistent output**. A prompt that says "be conservative" or "only report high confidence findings" leaves Claude to decide what those mean — and it decides differently each run.

The domain's solution is precision:
- **Explicit criteria** replace subjective thresholds with categorical definitions
- **Few-shot examples** teach Claude the *reasoning pattern*, not just the action
- **Structured output** with JSON schemas makes findings machine-parseable
- **Validation and retry loops** catch output that fails to meet criteria and recover automatically

## Key Exam Signal

- General instructions → inconsistent; explicit categorical criteria → consistent
- Subjective thresholds ("be conservative") → Claude decides differently each run; categorical definitions → same result every run
- Define what to *skip* as explicitly as what to *report* — reducing false positives is as important as catching real issues
- High false positive rates destroy trust across **all** categories, not just the high-FP category
- Precision over recall: fewer accurate findings > many findings with high false positive rate
- Severity levels require concrete code examples per level, not just label descriptions
- The fix for a high-FP category: disable → improve criteria → re-enable (not: live with it)

## Cross-Domain Connections

- [[probabilistic-vs-deterministic]] (D1 T1.4) — categorical definitions = deterministic classification; subjective thresholds = probabilistic
- [[concrete-examples]] (D3 T3.5) — same "show don't describe" principle; T4.1 extends it to severity level code examples
- [[structured-output-ci]] (D3 T3.6) — T4.1 explicit criteria define *what* to find; structured output defines *how* to emit it; natural pairing
- [[structured-output-tool-use]] (D4 T4.3) — tool use enforces JSON structure at API level; T4.1 criteria define what fields to populate; complementary precision mechanisms
- [[syntax-vs-semantic-errors]] (D4 T4.3) — T4.3 ensures structure; T4.4 ensures correctness; two-stage pipeline
- [[validation-retry-loops]] (D4 T4.4) — complete extract→validate→retry→human-review architecture
- [[retriable-vs-nonretriable-failures]] (D4 T4.4) — most exam-tested T4.4 distinction: retry format errors, accept null for absent data
- [[message-batches-api]] (D4 T4.5) — batch vs. synchronous decision; no mid-request tool calls; custom ID + targeted resubmission
- [[multi-instance-review]] (D4 T4.6) — separate instances eliminate self-review bias; three-pass architecture; confidence routing; closes Domain 4
- [[attention-dilution]] (D1 T1.6) — per-file passes in T4.6 implement the same decomposition solution documented in D1
- [[session-isolation-ci]] (D3 T3.6) — self-review bias origin; T4.6 extends it to multi-instance architectures
- [[confidence-calibration]] (D5 T5.5) — same confidence-routing pattern; D4 T4.6 applies to code findings, D5 applies to extraction fields
- [[explicit-escalation-criteria]] (D5 T5.2) — same precision-over-vagueness principle applied to escalation thresholds

[Source: 20-EzWRMJz9lXc.md, 22-CxwIKbepDwQ.md, 23-wyqmHtVGxAg.md, 24-6ZGgMSQz3eg.md, 25-W7c9wNfoCjU.md]
