---
title: Model-Driven Agency vs Scripted Automation
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

A consistently-tested distinction in Domain 1: two ways to build multi-step workflows. Model-driven agency is the correct Claude architecture; scripted automation is the anti-pattern. The exam tests recognition of which approach is being described and which should be recommended.

## Side-by-Side Comparison

| Dimension | Scripted Automation | Model-Driven Agency |
|-----------|-------------------|---------------------|
| Who plans? | The **developer** hardcodes the sequence | **Claude** reads context and decides |
| Who executes? | Developer code + Claude fills blanks | **Your code** executes Claude's decisions |
| Sequence | Fixed regardless of user need | Dynamic — determined by conversation context |
| Efficiency | Runs all steps even if fewer needed | Calls exactly what the task requires |
| Adaptability | None — fixed sequence | Can retry with different query if search returns nothing |
| Exam label | Automation, not agency | Model-driven agency |

## The Key Insight

In model-driven agency, Claude reads the full `messages` array at each loop iteration and decides what tool to call next **based on what it knows so far** — not based on a developer-specified sequence.

Example (Tokyo + Paris weather): Claude requested Tokyo first, then inferred Paris was still needed, then synthesized both. The developer never explicitly told Claude "check Paris next."

## Exam Signal

> "When asked how a Claude agent should decide which API to call, the correct answer is that Claude reads the tool descriptions and conversation context and decides. Not that your code checks a condition and routes."

## Connection to Anti-Patterns

Scripted automation often manifests as [[agentic-loop-anti-patterns|Anti-pattern 3]] (keyword detection for routing): checking Claude's text to decide which function to call. Model-driven agency avoids this by reading the structured `tool_use` blocks in the content array.

## Connection to Domain 5

[[subagent-delegation-exploration]] in Domain 5 extends this pattern: the main agent stays model-driven (decides what to delegate) while sub-agents each run their own model-driven loops. The boundary between "planner" and "executor" scales with the architecture.

[Source: 02-OCiLc9Frq84.md]
