---
title: stop_reason — The Loop Control Signal
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

`stop_reason` is the most important structured field in every [[messages-create-parameters|messages.create]] response. It is the **only** reliable signal for controlling the [[agentic-loop|agentic loop]]. The exam actively tests whether candidates use `stop_reason` rather than text parsing.

## Three Values

| Value | Meaning | Your action |
|-------|---------|------------|
| `end_turn` | Claude is done; final answer produced | Break out of loop, return text to user |
| `tool_use` | Claude needs your code to act first | Find all `tool_use` blocks, execute all, append results, loop back |
| `max_tokens` | Response cut off before Claude finished | Handle carefully — answer or tool call may be incomplete |

## Why `stop_reason`, Not Text

Claude uses natural language that changes. Claude might say "I'm done with step one" while still having three steps left. `stop_reason` is:

- **Structured** — always a known enum value
- **Deterministic** — never ambiguous
- **Always present** — in every response object

Parsing Claude's text for "done" or "finished" is fragile and exam-penalized. See [[agentic-loop-anti-patterns]] Anti-pattern 1.

## `max_tokens` — The Edge Case

If `max_tokens` is set too low, `stop_reason` returns `max_tokens` before Claude finishes generating. The content may contain a partial text block or an incomplete tool call. Handle this case explicitly — do not treat it like `end_turn` or `tool_use`.

## Exam Signal

> "Never parse Claude's text output to decide whether to continue. Stop_reason is structured, deterministic, and always present. Use it."

[Source: 02-OCiLc9Frq84.md]
