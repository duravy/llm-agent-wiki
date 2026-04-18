---
title: Agentic Loop — 4 Steps
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The four-step agentic loop is the repeating cycle inside every Claude-driven product. It runs until `stop_reason == end_turn`. Both the assistant response and tool results must be appended on each iteration — Claude needs to see what it said and what your code returned before deciding the next step.

## The Four Steps

### Step 1 — Send
Call `messages.create` with:
- The complete `messages` array (full conversation history)
- All tool definitions

Claude receives **everything**: every prior message, every tool call, every result.

### Step 2 — Check `stop_reason`
This is the **only decision point** in the loop.

- `end_turn` → exit loop, return the answer
- `tool_use` → continue to step 3
- `max_tokens` → response was cut off; handle carefully

> **Critical**: Do not try to infer completion from Claude's text. `stop_reason` is structured, deterministic, and always present.

### Step 3 — Execute Tools
Scan the `content` array. Find **all** `tool_use` blocks. Run each implementation. Collect results with their IDs.

> Claude may request more than one tool in a single response. Execute **all of them** before continuing.

### Step 4 — Append and Loop
1. Append Claude's **full response** as an `assistant` message
2. Append **all tool results** as a `user` message
3. Loop back to step 1

Both additions are required. Missing either breaks Claude's ability to reason about what happened.

## Worked Example: Weather for Tokyo and Paris

| Iteration | Messages array size | Claude requests | stop_reason | Your action |
|-----------|--------------------|-----------------|-----------|----|
| 1 | 1 (user question) | `get_weather(Tokyo)` | `tool_use` | Execute, append |
| 2 | 3 | `get_weather(Paris)` | `tool_use` | Execute, append |
| 3 | 5 | Summary of both cities | `end_turn` | Return answer |

Key insight: you never told Claude to check Paris next. Claude inferred it from the growing conversation history — that's [[model-driven-vs-scripted|model-driven agency]].

## Loop Structure

```
while True:
    response = client.messages.create(...)
    if response.stop_reason == "end_turn":
        break
    # execute all tool_use blocks
    # append assistant message + tool results
    # safety counter to prevent infinite loops
```

Never use `for i in range(N)` as the primary exit — see [[agentic-loop-anti-patterns]].

[Source: 02-OCiLc9Frq84.md]
