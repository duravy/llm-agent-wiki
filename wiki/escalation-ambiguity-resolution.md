---
title: Escalation and Ambiguity Resolution (D5 T5.2)
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Escalation and ambiguity resolution defines when an agent should handle a task autonomously versus handing it to a human, and how to behave when inputs are ambiguous. The core insight is that escalation decisions must be based on case complexity and policy coverage — not on emotional tone or heuristics. This is Domain 5, Task 5.2 of the [[series-overview|Claude Architect Certification Exam series]].

## The Three Escalation Triggers

There are **exactly three** triggers that demand escalation. See [[escalation-triggers]] for full detail.

| # | Trigger | Rule |
|---|---------|------|
| 1 | Customer explicitly requests a human | Escalate **immediately** — do not investigate first |
| 2 | Policy gap — request not covered by policy | Escalate |
| 3 | Three failed attempts with no progress | Escalate |

**Everything else → resolve autonomously.**

## The Critical Exam Distinction

When a customer explicitly asks for a human, escalate immediately — **do not try to resolve the issue first**, even if you think you can. Honoring the customer's preference is the priority. This is the most directly tested point in T5.2.

## Sentiment Is Not a Reliable Escalation Proxy

See [[sentiment-escalation-fallacy]].

- A furious customer may just need a tracking lookup → resolve autonomously
- A calm customer may need a policy exception → escalate

Rule: escalate based on **case complexity and policy gaps**, not emotional tone.

## Ambiguity Resolution

See [[ambiguity-resolution]].

When a tool returns multiple matches (e.g., 3 "John Smith" accounts), never pick the first result. Always ask for additional identifiers.

## Correct Resolution Pattern

1. Acknowledge the customer's frustration
2. Attempt resolution
3. Only escalate when the customer **reiterates** their preference for a human after your attempt

## Explicit vs. Vague Criteria

See [[explicit-escalation-criteria]].

Vague: *"escalate complex cases"* → Claude decides inconsistently.
Explicit: define triggers with few-shot examples of what to escalate and what not to.

## Five Anti-Patterns

See [[escalation-anti-patterns]] for the full list.

## Exam Framework (Lock This In)

- Customer asks for human → **escalate**
- Policy gap → **escalate**
- Three failed attempts → **escalate**
- Everything else → **resolve autonomously**

## Escalation Handoff Format

When escalating, structure the handoff as JSON with four fields: `action_requested`, `context`, `options`, `recommended_action`. See [[structured-escalation-handoff]] for the full pattern and the D1 T1.4 trigger framework (confidence threshold, genuine ambiguity, irreversible action imminent).

> Note: The three triggers above (human request, policy gap, three failed attempts) apply specifically to customer-support agent contexts. D1 T1.4 defines a complementary set of triggers for general agent workflows. No contradiction — different system levels.

## Series Context

- Part 26 covered [[context-management]] (D5 T5.1)
- Part 27 (this source) covers D5 T5.2
- Part 28 covers [[error-propagation-multiagent]]

[Source: 27-FOBfTw-9Olo.md, 05-Ow0fCviHxrY.md]
