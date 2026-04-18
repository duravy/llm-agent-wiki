---
title: Hooks Decision Framework — When to Use Hooks vs. Prompts
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

One question resolves most enforcement-choice exam scenarios. [Source: 06-UTzVVg8Aepc.md]

## The 1% Test

> **"If this rule fails 1% of the time, does it matter?"**

- **Yes, it matters** → use a hook (code enforcement)
- **No, it's annoying but not dangerous** → use a prompt

This test captures the same logic as the [[code-vs-prompt-enforcement|irreversibility test]] but frames it in terms of production failure rate rather than reversibility. Both tests produce the same answer.

## Hard Rules → Hooks

| Category | Examples |
|----------|---------|
| Financial limits | Max refund amount, spending thresholds, payment caps |
| Data format requirements | Normalize timestamps, status codes, date conventions |
| Compliance policies | Regulatory checks, audit requirements, access controls |
| Access controls | Permission gates, role-based tool restrictions |

For all of these: failure is not annoying — it causes real harm, violates policy, or corrupts data. Use a hook.

## Soft Preferences → Prompts

| Category | Examples |
|----------|---------|
| Tone | Be polite, empathetic, professional |
| Formatting suggestions | Use bullet points, keep responses concise |
| Response length | Summarize in 2–3 sentences |
| Style | Avoid jargon, use plain language |

For all of these: a 1% failure means a slightly off-tone response, not a compliance breach. Prompts are the right tool — they add no latency and are trivial to update.

## Performance Implication

Hooks run on **every tool call**. A hook that does unnecessary work (checking formatting preferences, for example) adds latency to every call in the system. Reserve hooks for the checks that genuinely require deterministic enforcement.

> "Overengineering soft preferences into hooks wastes performance. Underengineering hard rules into prompts creates compliance gaps." [Source: 06-UTzVVg8Aepc.md]

## Connection to Other Pages

- [[hooks-vs-prompts]] — the full comparison table (failure rates, override behavior, examples)
- [[code-vs-prompt-enforcement]] — the irreversibility test (complementary framework)
- [[probabilistic-vs-deterministic]] — why 1% failures matter at scale
- [[hooks-anti-patterns]] — Anti-pattern 4: using hooks for soft preferences

[Source: 06-UTzVVg8Aepc.md]
