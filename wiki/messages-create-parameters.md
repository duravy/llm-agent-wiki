---
title: messages.create — 5 Parameters
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

`messages.create` is the single function called in every [[agentic-loop|agentic loop]] iteration. It accepts five parameters — two required, three optional. The agentic power of the loop lives in the `tools` and `messages` parameters together.

## The Five Parameters

| # | Parameter | Required? | Purpose |
|---|-----------|-----------|---------|
| 1 | `model` | **Required** | Which Claude processes the request: Opus (deep reasoning), Sonnet (balance), Haiku (speed) |
| 2 | `max_tokens` | **Required** | Hard cap on response length. Too low → response truncated → `stop_reason: max_tokens` |
| 3 | `system` | Optional | Persistent instructions for every response. Think: permanent job description for Claude in this session |
| 4 | `tools` | Optional | Declares what actions Claude is allowed to *request*. Essential for agentic behavior |
| 5 | `messages` | Required* | Full conversation history — every user and assistant message so far |

> *`messages` is listed as the fifth parameter but is functionally required for the loop to work.

## Why tools + messages Together

- `tools` — tells Claude **what it can do**
- `messages` — carries the growing record of **what it has already done**

Claude reads the full `messages` array on every iteration to know what it has already fetched and what is still missing. This is how model-driven planning works without external state.

## Response Object

Every call returns a structured object with two critical fields:

- `stop_reason` — why Claude stopped (`end_turn`, `tool_use`, `max_tokens`) → see [[stop-reason]]
- `content` — array of blocks: `text` blocks and/or `tool_use` blocks

Each `tool_use` block contains:
- `id` — unique identifier for this call (must be referenced in the result)
- `name` — the tool Claude wants to use
- `input` — the input object Claude chose

## Exam Signals

- Two required parameters: `model` and `max_tokens`
- `max_tokens` too low → partial response, `stop_reason: max_tokens` → handle carefully
- Tool results return as **user role messages** with a `tool_result` block referencing the exact `tool_use` ID

[Source: 02-OCiLc9Frq84.md]
