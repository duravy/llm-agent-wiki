---
title: Code vs Prompt Enforcement — Decision Framework
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The decision framework for choosing between code enforcement and prompt guidance. One question resolves most exam scenarios.

## The Irreversibility Test

> **"Can a mistake here be undone?"**

- **No** → code enforcement required
- **Yes** → prompt guidance is acceptable

## Decision Table

| Scenario | Enforcement type | Reason |
|----------|-----------------|--------|
| Sending an email | **Code** | Cannot be recalled once sent |
| Charging a card | **Code** | Financial transaction, irreversible |
| Deleting a record | **Code** | Data loss, irreversible |
| Safety-critical action | **Code** | Harm if skipped |
| Regulatory compliance check | **Code** | Non-negotiable |
| Response tone / formality | Prompt | Style preference, easily corrected |
| Non-critical workflow ordering | Prompt | A mistake can be retried |
| Output format preferences | Prompt | Correctable, no harm |

## Use Code Enforcement When

- Safety matters
- Compliance is required
- Financial transactions are involved
- The action is **irreversible**

These are non-negotiable. Code only.

## Use Prompt Guidance When

- It is a style preference
- It is a tone adjustment
- It is non-critical workflow ordering
- A mistake can be corrected without harm

Prompts are easier to update and add no code overhead — they are the right tool for flexible, low-stakes guidance.

## The Exam Framing

> **"Prompt guidance is best effort. Code enforcement is guaranteed."**

When an exam scenario describes a rule that, if violated, would cause real harm — the answer involves code enforcement. When the scenario describes something stylistic or correctable — prompt guidance is fine.

## Implementation Patterns

- [[prerequisite-gates]] — block tool B until tool A succeeds
- [[hooks]] — intercept every tool call; pre-hook blocks; post-hook validates
- Both are [[probabilistic-vs-deterministic|deterministic compliance]]; both are exam-tested

## Complementary Framework

[[hooks-decision-framework]] provides the same logic framed as a failure-rate test: "If this rule fails 1% of the time, does it matter?" Both tests produce the same answer. The irreversibility test (here) is more intuitive for action-based scenarios; the 1% test is more intuitive for data-integrity scenarios like format normalization. [Source: 06-UTzVVg8Aepc.md]

[Source: 05-Ow0fCviHxrY.md, 06-UTzVVg8Aepc.md]
