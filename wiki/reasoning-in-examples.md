---
title: Reasoning in Examples
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The most critical detail in few-shot prompting: always include *why* each action was chosen, not just what the action was. Without reasoning, Claude copies the action pattern but cannot generalize the judgment to novel cases. With reasoning, Claude internalizes the decision logic and applies it to inputs it has never seen. [Source: 21-u6oCkdxU5kw.md]

## The Distinction

**Without reasoning (action only):**
```
Input:  "Check my order #12345"
Output: lookup_order(order_id=12345)
```
Claude learns: *when I see an order number, call lookup_order*. It matches this pattern.

**With reasoning (action + why):**
```
Input:  "Check my order #12345"
Output: lookup_order(order_id=12345)
Why:    Order number is present → direct lookup possible
```
Claude learns: *when a specific identifier is present, use direct lookup; when it's absent, identify the caller first*. It can now apply this logic to "Check my invoice from last week" even though that exact input was never shown.

## Why This Matters for Generalization

Few-shot examples that include reasoning enable Claude to generalize to **novel patterns** — inputs it has never seen. Examples without reasoning only enable pattern matching against the specific examples shown.

This is the exam-tested distinction between few-shot as a lookup table (no reasoning) versus few-shot as a judgment teacher (with reasoning). [Source: 21-u6oCkdxU5kw.md]

## The Gray Area Connection

Reasoning is most critical for [[gray-area-examples|gray area cases]] where the correct action is non-obvious. The *why* is what distinguishes:
- `if user?.name` → Skip (the `?.` signals intentional optional chaining)
- `if user.name` where type says user can be null → Flag (the type definition reveals the real bug)

Without the *why*, Claude can't tell the difference. With it, Claude understands the decision logic and applies it to structurally similar but not identical code. [Source: 21-u6oCkdxU5kw.md]

## Exam Takeaway

> **Include reasoning in every few-shot example.** "Why this action was chosen" is the mechanism that enables generalization. Without it, Claude copies action patterns; with it, Claude generalizes judgment patterns.

This is stated as an explicit exam signal in Part 21: "include reasoning why this action was chosen so Claude generalizes the judgment pattern, not just the action."

## Connections

- [[few-shot-prompting]] — T4.2 hub
- [[gray-area-examples]] — reasoning is most critical for non-obvious judgment cases
- [[example-count-guidelines]] — reasoning adds value even at 2–3 examples; not a reason to add more examples
- [[few-shot-anti-patterns]] — anti-pattern #2: examples without reasoning

[Source: 21-u6oCkdxU5kw.md]
