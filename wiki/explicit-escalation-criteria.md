---
title: Explicit Escalation Criteria
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Explicit escalation criteria are precisely defined, example-backed rules that tell an agent exactly when to escalate versus resolve autonomously. Vague criteria produce inconsistent behavior; explicit criteria with few-shot examples produce consistent results every time.

## Vague vs. Explicit — The Contrast

**Vague (bad):**
> "Escalate complex cases to a human agent."

What counts as complex? Claude decides differently each time. This produces random, inconsistent escalation behavior.

**Explicit (good):**

| Condition | Action |
|-----------|--------|
| Customer requests a human | Escalate immediately |
| Refund amount exceeds $500 | Escalate (policy limit) |
| Request not covered in instructions | Escalate (policy gap) |
| Three attempts with no progress | Escalate |
| Standard order lookup | Do NOT escalate |
| Password reset | Do NOT escalate |
| Tracking inquiry | Do NOT escalate |

## Few-Shot Examples in Prompts

The source explicitly recommends including few-shot examples directly in the system prompt — not just listing triggers, but showing examples of cases that should and should not escalate. This anchors the model's judgment consistently.

This is an instance of the [[few-shot-prompting|few-shot prompting]] pattern from D4 T4.2: examples that include *reasoning* (why this case escalates vs. doesn't) enable Claude to generalize to novel cases rather than just matching the specific examples shown. See [[reasoning-in-examples]]. [Source: 21-u6oCkdxU5kw.md]

## Why This Is Exam-Tested

Vague criteria are a common design mistake. The exam distinguishes between systems that define escalation rules clearly (consistent, predictable) and those that leave it to the model's discretion (random, unreliable).

See [[escalation-anti-patterns]] Anti-Pattern #5.

Related: [[escalation-ambiguity-resolution]], [[escalation-triggers]], [[sentiment-escalation-fallacy]]

[Source: 27-FOBfTw-9Olo.md]
