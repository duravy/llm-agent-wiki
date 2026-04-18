---
title: Communication Stack (4 Layers)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The communication stack describes the four layers between your application code and Claude's language model. Understanding this stack clarifies what you write, what the SDK handles, and what returns to you — foundational context for [[agentic-loop|agentic loop]] design.

## The Four Layers

| Layer | What it is | What it does |
|-------|-----------|--------------|
| **Your code** | Python or JavaScript | Writes logic, calls one function |
| **Anthropic SDK** | Installed library | Wraps all HTTP complexity into readable method calls |
| **Claude API** | Anthropic's service | Handles authentication (API key) + routes to correct model |
| **Claude's servers** | Anthropic infrastructure | Runs the LLM, returns a **structured response object** |

> Key: Claude's servers return a **structured object**, not plain text. This object is what your loop reasons about via `stop_reason` and the `content` array.

## Restaurant Analogy

| Restaurant | Stack layer |
|-----------|-------------|
| You (customer) | Your code |
| Waiter | Anthropic SDK |
| Kitchen ticket system | Claude API |
| Kitchen | Claude's servers |

## Exam Implication

- You never write raw HTTP — one SDK call handles all four layers
- One `Anthropic` client instantiation handles authentication, retries, and response parsing automatically
- The structured response object (not plain text) is what enables reliable [[stop-reason]] checking

[Source: 02-OCiLc9Frq84.md]
