---
title: Incremental Exploration
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Incremental exploration is the exam-named strategy for navigating a codebase efficiently: follow imports and function calls step by step, letting each discovery narrow the next search. Never guess which files matter — let the code tell you.

## The Pattern

```
Step 1: Grep for entry point
  → "Where are API routes defined?"
  → Grep: "app.route" → routes.py

Step 2: Read routes.py
  → Discover: routes call handlers in handlers/

Step 3: Read the specific handler
  → Discover: handler imports from billing_service

Step 4: Read billing_service
  → Understand the full data flow
```

Each step narrows focus based on what the previous step revealed. You only read files that are actually relevant.

## Why It Works

- **Context efficiency**: no wasted tokens on code unrelated to the task
- **Attention quality**: Claude processes a small, focused set of relevant files rather than a bloated context
- **Correctness**: follows actual code structure rather than guessing file locations

## Connection to Two-Phase Search

[[two-phase-search-strategy]] is the entry to incremental exploration: Grep (phase 1) surfaces the first file, Read (phase 2) surfaces the next reference, which becomes the input for the next Grep or Read. Incremental exploration is the multi-step generalization of the two-phase strategy.

## Connection to Large Codebase Context

In D5 T5.4, [[subagent-delegation-exploration]] uses the same pattern at scale: sub-agents perform incremental exploration across isolated contexts, returning summaries to the coordinator. The built-in tools (Grep, Read) are the mechanics that make this possible at the single-agent level.

## Exam Signal

- "How to explore a codebase without filling the context window?" → incremental exploration: follow imports step by step
- "Do not try to read the entire codebase upfront" → explicit exam rule
- Reading all files upfront = anti-pattern 1 (see [[builtin-tools-anti-patterns]])

[Source: 13-IN2WntoHedY.md]
