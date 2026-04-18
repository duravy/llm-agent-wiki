---
title: Escalation Triggers
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

There are exactly three triggers that demand escalation in an agent handling customer support or similar tasks. The exam tests these decision rules directly — getting them wrong costs points fast.

## Trigger 1 — Customer Explicitly Requests a Human

The customer says something like "I want to speak to a manager."

**Action: Escalate immediately. Do not investigate first.**

This is the most testable trigger. The wrong approach is to try to resolve the issue before escalating, even if you believe you can solve it. Honoring the customer's preference is the priority above all other steps.

## Trigger 2 — Policy Gap

The customer makes a request that the agent's policy does not cover.

**Example:** Customer asks to match a competitor's price, but policy only covers on-site adjustments. The policy is silent on this case.

**Action: Escalate.** The agent cannot make a judgment call outside its authorization.

## Trigger 3 — Three Failed Attempts

The agent has tried multiple approaches and none have resolved the issue.

**Action: Escalate.** Continuing wastes the customer's time and the system's resources.

## What Does NOT Trigger Escalation

These should be resolved autonomously, regardless of the customer's emotional state:
- Standard order lookups
- Password resets
- Tracking inquiries
- Any case within the agent's defined capability and policy coverage

## Exam Shortcut

> "If the customer asks for a human — escalate. If there's a policy gap — escalate. If you've tried three times with no progress — escalate. Everything else, resolve autonomously."

## D1 T1.4 — General Workflow Escalation Triggers

Part 5 defines a complementary set of triggers for **general agent workflows** (not customer-support specific):

1. **Confidence drops below threshold** — agent's certainty is too low to proceed
2. **Instructions are genuinely ambiguous** — cannot resolve without human clarification
3. **Irreversible action is imminent and agent isn't certain** — cost of error is too high

These do not contradict the three above — they apply at a different system level. The D5 T5.2 triggers govern *when to hand off*; the D1 T1.4 triggers govern *when an agent workflow must pause*. See [[structured-escalation-handoff]] for the 4-field handoff format that applies to both contexts.

Related: [[escalation-ambiguity-resolution]], [[escalation-anti-patterns]], [[sentiment-escalation-fallacy]], [[explicit-escalation-criteria]], [[structured-escalation-handoff]]

[Source: 27-FOBfTw-9Olo.md, 05-Ow0fCviHxrY.md]
