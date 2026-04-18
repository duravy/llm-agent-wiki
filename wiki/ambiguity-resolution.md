---
title: Ambiguity Resolution
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Ambiguity resolution covers how an agent should behave when inputs are underspecified — particularly when tool results return multiple possible matches. The rule is simple: never guess. Always ask for clarifying information.

## Multiple Match Problem

**Scenario:** The agent searches for "John Smith" and the tool returns three customer accounts.

**Wrong approach:** Pick the first result and proceed. This risks acting on the wrong customer's data entirely.

**Right approach:** Ask for additional identifiers.

> "I found three accounts under John Smith. Could you provide your email address or the last four digits of your phone number to identify the correct account?"

**Rule: Never select based on heuristics. Always ask for clarification.**

## Why This Matters

Selecting the wrong account could expose another customer's data, process the wrong refund, or update the wrong order. The cost of asking one clarifying question is always lower than the cost of acting on the wrong match.

## Connection to Escalation

Ambiguity is distinct from escalation. A multiple-match situation is not an escalation trigger — it is resolved by asking the customer, not by routing to a human. Only escalate if the customer cannot or will not provide clarifying information after asking, and the case cannot proceed.

## Anti-Pattern Connection

This is Anti-Pattern #3 in [[escalation-anti-patterns]] — guessing when multiple customers match.

Related: [[escalation-ambiguity-resolution]], [[escalation-triggers]], [[escalation-anti-patterns]]

[Source: 27-FOBfTw-9Olo.md]
