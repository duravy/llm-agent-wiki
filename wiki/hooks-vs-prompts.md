---
title: Hooks vs. Prompts — Comparison Table
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The exam labels this a "core exam concept." Six dimensions differentiate hooks from prompt instructions. Understanding each dimension answers the majority of enforcement-choice questions on the exam. [Source: 06-UTzVVg8Aepc.md]

## The Comparison Table

| Dimension | Hooks | Prompts |
|-----------|-------|---------|
| **How it works** | Code runs on every tool call | Text Claude reads in the system prompt |
| **Failure rate** | 0% — code always runs | 1–5% — Claude may skip or override |
| **When to use** | Hard rules that *must* be followed | Soft guidelines that *should* be followed |
| **Can Claude override?** | No — code blocks unconditionally | Yes — Claude can be persuaded by context |
| **Example** | Block refunds over $500 (non-negotiable) | "Be polite and empathetic" (preference) |
| **Performance** | Adds execution time per tool call | No overhead — text already in context |

## Exam Signal

> **"Whenever a question says 'guarantee' or 'ensure compliance,' the answer is hooks, not prompts."**

The key word pairs:
- "guarantee" / "ensure" / "always" / "never" → **hooks**
- "should" / "prefer" / "typically" → **prompts**

## The Two Failure Modes

**Overengineering:** Putting soft preferences (tone, formatting, response length) into hooks.
- Result: latency on every tool call, for no safety benefit. Prompts handle these fine.

**Underengineering:** Putting hard rules (financial limits, compliance checks, data integrity) into prompts.
- Result: compliance gaps. 1% failures at production volume = real harm.

See [[hooks-decision-framework]] for the test that separates these cases.

## Connection to Other Pages

- [[hooks]] — hub page: hook types, return behaviors, registration
- [[data-normalization-hooks]] — post-tool use: normalizing heterogeneous formats
- [[hooks-decision-framework]] — the 1% test for when to use which
- [[probabilistic-vs-deterministic]] — the theoretical foundation for 0% vs 1–5%
- [[code-vs-prompt-enforcement]] — broader decision framework including prerequisite gates

[Source: 06-UTzVVg8Aepc.md]
