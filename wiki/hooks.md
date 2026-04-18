---
title: Hooks — Automatic Tool Call Interception
created: 2026-04-16
last_updated: 2026-04-16
source_count: 2
status: draft
---

Hooks are Python functions that the Claude Agent SDK calls automatically around every tool execution in the agentic loop. You register them once; they fire on every tool call without manual invocation. Hooks are one of the two primary [[probabilistic-vs-deterministic|deterministic enforcement]] mechanisms, alongside [[prerequisite-gates]].

Part 5 (T1.4) introduced hooks as an enforcement tool. Part 6 (T1.5) makes them practical with two specific jobs: **data normalization** (post-tool) and **policy enforcement with redirect** (pre-tool).

## Two Hook Types

| Hook type | When it fires | What you can do |
|-----------|--------------|-----------------|
| **Pre-tool use** | Before the tool executes | Inspect name/input, block call (raise), modify input, log |
| **Post-tool use** | After the tool executes | Validate output schema, sanitize result, record outcome |

## Three Return Behaviors (Exam-Tested)

The exam tests these three behaviors directly for pre-tool hooks:

| Return value | Effect |
|-------------|--------|
| `None` | Allow the call through unchanged |
| Raise an exception | **Block** the tool — it will not execute |
| Return a modified dict | Override the input before execution |

Post-tool hooks can return a modified result or raise to signal validation failure.

## Code Pattern

```python
def pre_tool_hook(tool_name: str, tool_input: dict):
    if tool_name == "database_write":
        validate_schema(tool_input)      # raises if invalid → blocks tool
    audit_log("attempt", tool_name, tool_input)
    return None                           # allow through

def post_tool_hook(tool_name: str, result: dict):
    audit_log("complete", tool_name, result)
    return sanitize_output(result)        # return sanitized result to Claude
```

## Common Use Cases

| Use case | Hook type | Mechanism |
|----------|-----------|-----------|
| **Data normalization** | Post | Convert heterogeneous formats (Unix timestamps, US dates, numeric status codes) to consistent ISO/string formats before Claude sees them — see [[data-normalization-hooks]] |
| **Policy enforcement** | Pre | Block tool calls that violate business rules (refunds > $500) and redirect to safe alternatives |
| **Audit logging** | Both | Log every call attempt and completion automatically |
| **Rate limiting** | Pre | Raise if tool has been called too many times |
| **Output sanitization** | Post | Strip sensitive data before result reaches Claude |
| **Schema validation** | Pre | Reject malformed inputs before downstream failures |
| **Prerequisite enforcement** | Pre | Check code state; raise if prerequisite not met (pairs with [[prerequisite-gates]]) |

## Policy Enforcement with Redirect (Pre-Tool)

The key pattern for pre-tool enforcement: **block + explain + redirect**. Claude must receive a reason and an alternative path, or it gets stuck and retries endlessly.

```python
def pre_tool_hook(tool_name: str, tool_input: dict, context: dict) -> dict:
    if tool_name == "process_refund":
        if tool_input.get("amount", 0) > 500:
            return {
                "blocked": True,
                "message": "Refund of $750 exceeds the $500 limit. Manager authorization required.",
                "redirect_to": "escalate_manager",
                "escalation_details": {
                    "customer_id": context.get("customer_id"),
                    "amount": tool_input["amount"]
                }
            }
    return {"blocked": False}  # let all other tools through
```

Critical exam insight: **Claude cannot override this.** The code decides, not the model.

## Performance Rule

Hooks run on every tool call — keep them fast. No network calls. No side effects. Pure functions only. A hook that does I/O adds latency to every single agent interaction. Reserve hooks for hard rules; see [[hooks-decision-framework]].

## Why Hooks Are Deterministic

Hooks run in your code. Claude cannot influence whether a hook fires, what it checks, or whether it raises an exception. A hook that raises blocks the tool call unconditionally — regardless of what Claude decided or what prompt it was given.

This is [[probabilistic-vs-deterministic|deterministic compliance]]: the enforcement happens in code, not in Claude's reasoning.

## Hooks vs. Prompts

See [[hooks-vs-prompts]] for the full 6-dimension comparison table. Short version:

| | Hooks | Prompts |
|--|-------|---------|
| Failure rate | 0% | 1–5% |
| Can Claude override? | No | Yes |
| Performance overhead | Yes (per call) | None |

Exam signal: "guarantee" / "ensure compliance" → hooks, not prompts.

## Anti-Patterns

- [[enforcement-anti-patterns]] (T1.4) — Anti-pattern 4: ignoring hook return values
- [[hooks-anti-patterns]] (T1.5) — Five practical anti-patterns: prompts for data consistency, blocking with no explanation, silent modification, hooks for soft preferences, no redirect on block

[Source: 05-Ow0fCviHxrY.md, 06-UTzVVg8Aepc.md]
