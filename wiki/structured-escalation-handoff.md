---
title: Structured Escalation Handoff
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

When an agent reaches a point requiring human judgment, the handoff structure determines whether the human can act effectively. Vague handoffs waste the human's time. Structured handoffs give humans exactly what they need to make a decision.

## The Rule

> **"Never escalate with a vague message. Always include actionable decision context."**

A message like "I need help" or "I'm not sure" is an exam anti-pattern. A human cannot act on vague. They need clear options.

## The Four-Field JSON Structure

```json
{
  "action_requested": "what you need the human to do",
  "context": "what happened — a focused summary, not a log dump",
  "options": ["option A", "option B", "option C"],
  "recommended_action": "what the agent thinks is best and why"
}
```

| Field | Purpose |
|-------|---------|
| `action_requested` | Specific decision or action needed from the human |
| `context` | Focused summary of what was attempted and current state — not a dump of logs |
| `options` | The available choices, enumerated clearly |
| `recommended_action` | The agent's recommendation with rationale |

## D1 T1.4 Escalation Triggers (Agent Workflow Context)

Part 5 defines three situations where an agent should escalate:

1. **Confidence drops below threshold** — agent's certainty about the right action is too low
2. **Instructions are genuinely ambiguous** — cannot resolve without human clarification
3. **Irreversible action is imminent and agent isn't certain** — cost of a wrong call is too high

These are distinct from the [[escalation-triggers|D5 T5.2 triggers]] (customer-support context: human explicitly requests, policy gap, three failed attempts). Both frameworks are valid — they apply at different system levels. See the note below.

## Relationship to D5 T5.2

The D5 T5.2 page ([[escalation-ambiguity-resolution]]) covers escalation in customer-support agent contexts. This page covers escalation handoff *format* and workflow-agent *triggers*. The structured handoff JSON pattern defined here is the implementation standard that applies to both contexts.

> No contradiction: D5 T5.2 defines *when* to escalate (in customer support); D1 T1.4 defines *how* to hand off (structured JSON) and *when* in general agent workflows.

## Connection to Enforcement

Escalation decisions are closely tied to the [[code-vs-prompt-enforcement|irreversibility test]]. When an irreversible action is imminent and the agent isn't certain, escalation (rather than proceeding) is the safe fallback. See also [[enforcement-anti-patterns]] Anti-pattern 3.

[Source: 05-Ow0fCviHxrY.md]
