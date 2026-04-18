---
title: Prerequisite Gates
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

A prerequisite gate is a code-enforced check that blocks one tool from executing until another has completed successfully and recorded its result in code state. Gates are the primary pattern for enforcing tool ordering deterministically — Claude cannot reason around them.

## The Pattern

```
Tool A (prerequisite) → sets flag in code state → Tool B (gated)
                                                        ↑
                                         wrapper checks flag; raises exception if not set
```

Gates are implemented as **wrapper functions**: before the downstream tool runs, the wrapper inspects the prerequisite state. If the prerequisite has not been satisfied, the wrapper raises an actual exception — not a soft message, not a warning, not a return value. An exception.

## Concrete Example

```python
def payment_tool(amount, card_token):
    if not state.get("card_validated"):
        raise RuntimeError("Prerequisite not met: validate_card must complete first")
    # proceed with payment...

def validate_card_tool(card_token):
    result = validate(card_token)
    if result.success:
        state["card_validated"] = True
    return result
```

Payment cannot run until `validate_card` has set the flag. Even if Claude decides to skip validation — even if it's given a convincing reason to do so — the gate says no.

## Why Exceptions, Not Soft Errors

Returning a soft message like `{"error": "prerequisite not met"}` still runs the tool setup code and leaves Claude able to work around it by rephrasing. An exception terminates the call immediately and surfaces as a hard error in the agentic loop — Claude cannot proceed without satisfying the gate.

## When to Use Gates

Apply the [[code-vs-prompt-enforcement|irreversibility test]]: if calling tool B without tool A completing could cause irreversible harm, use a gate. Common cases:
- Payment after validation
- Database write after authorization check
- Send-email after draft approval
- Delete operation after confirmation

## Gates Are Deterministic

This is the key exam point. Gates are [[probabilistic-vs-deterministic|deterministic compliance]]. Claude cannot reason its way around a gate — the code enforces the rule regardless of Claude's judgment.

> **"Even if Claude thinks skipping validation is safe, your gate says no. That's the whole point."**

[Source: 05-Ow0fCviHxrY.md]
