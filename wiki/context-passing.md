---
title: Context Passing — Active Construction
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Context passing is the process of constructing the task prompt that the coordinator passes to a sub-agent. Because sub-agents start completely blank ([[isolated-context]]), the coordinator's task description is the sub-agent's entire starting reality. The exam tests a specific philosophy: context passing is an **active construction job**, not a copy-paste.

## The Core Problem

Every sub-agent starts with zero memory — no coordinator history, no prior sub-agent results, no access to the original user request unless explicitly provided. The coordinator must construct a task description that contains everything the sub-agent needs to succeed.

## What to Pass

| Include | Reason |
|---------|--------|
| The **goal** — what you want produced | Sub-agent cannot infer the goal from nothing |
| **Relevant facts and constraints** | Only the facts pertinent to this specific task |
| **Required output format** | Don't let the sub-agent guess; specify JSON, bullet list, structured report, etc. |
| **Results from prior sub-agents** | If this task depends on an upstream result, include that result explicitly |
| **Negative constraints** — what the sub-agent must NOT do | As important as positive goals |

## What NOT to Pass

> **Do not pass the entire conversation history.** That is lazy and expensive. Be selective — extract only the signal relevant to this specific task.

Dumping the full history inflates context, wastes tokens, and buries the signal in noise. See [[sdk-anti-patterns]] Anti-pattern 3.

## Structured Task Prompt Pattern

The exam tests a specific pattern for high-quality task prompts:

```
1. Clear goal statement (top)
2. Structured metadata block: { source, date, confidence_score }
3. The actual data
4. Explicit output format specification (bottom)
```

Three things that make this work:
1. **Include the source** — the sub-agent needs to know where data came from to assess trust
2. **Include a format spec** — never let the sub-agent guess the output format you need
3. **Include constraints** — tell it what it *cannot* do, not just what it should do

This pattern extends the [[subagent-metadata|subagent metadata]] requirements from D5 T5.1 and the [[claim-source-mappings|claim-source-mappings]] requirement from D5 T5.6, establishing D1 as the baseline.

## Connection to Isolated Context

Context passing is the active solution to the problem [[isolated-context]] identifies. Isolated context explains *why* sub-agents don't inherit knowledge; context passing describes *what the coordinator must do* as a result.

## Exam Signal

"If the exam asks what the coordinator's most critical job is during delegation: **active context construction**, not routing logic."

[Source: 04-bFdRH69h9LU.md]
