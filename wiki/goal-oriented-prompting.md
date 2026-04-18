---
title: Goal-Oriented Prompt Design
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Goal-oriented prompt design is the correct philosophy for writing task prompts passed to sub-agents. The exam tests this against its anti-pattern: procedural prompts that specify every step. The distinction: you tell Claude *what* you want, not *how* to get there.

## The Rule

> **Specify the goal. Specify the output format. Specify constraints. Do NOT specify tool call sequences.**

Claude decides the execution path. That is the entire point of using an agent rather than a script.

## What to Specify vs What Not to Specify

| Specify | Do NOT specify |
|---------|---------------|
| **Goal** — what the sub-agent must produce | Exact sequence of tool calls |
| **Output format** — JSON, bullet list, structured report | Which specific tools to invoke |
| **Constraints** — what it must *not* do or access | Step-by-step execution instructions |
| **Negative constraints** — boundaries the sub-agent must respect | How to reason through the problem |

## Why Negative Constraints Matter

Positive goal: "Find all open customer complaints."
Negative constraint: "Do not access the payments database."

Both are required. Specifying only what to do leaves the sub-agent free to access systems it shouldn't. Specifying what *not* to do closes the attack surface and prevents scope creep.

## The Micromanagement Trap

> **"If you need to specify every step, use a script, not an agent."**

Over-constraining a sub-agent with procedural instructions defeats the purpose of using an LLM. An LLM's value is its reasoning ability — give it room to reason. If the execution path needs to be fixed, write deterministic code.

This principle connects to [[model-driven-vs-scripted]]: Claude is the planner; your code is the executor. Procedural prompts try to make Claude the executor — wrong direction.

## Connection to Anti-Patterns

[[sdk-anti-patterns]] Anti-pattern 4 is the direct violation: procedural prompts that specify every step. The correct approach (goal-oriented prompts) is the fix for that anti-pattern.

## Exam Signal

If the exam presents a task prompt that includes specific tool names, call sequences, or step-by-step instructions — that is the wrong approach. The correct prompt specifies the goal, output format, and constraints only.

[Source: 04-bFdRH69h9LU.md]
