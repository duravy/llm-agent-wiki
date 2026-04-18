---
title: Domain 1 — Core API & Agentic Loops
created: 2026-04-16
last_updated: 2026-04-16
source_count: 7
status: draft
---

Domain 1 is the most heavily weighted domain in the [[series-overview|Claude Architect Certification Exam]], worth **27% of the exam**. It covers the foundational mechanics of Claude's API and the agentic loop pattern that powers every Claude-driven product. Everything in other domains builds on these concepts.

## Known Tasks

| Task | Topic | Wiki Page | Source Part |
|------|-------|-----------|-------------|
| 1.1 | The Agentic Loop | [[agentic-loop]] | Part 2 |
| 1.2 | Multi-Agent Coordinator Pattern | [[multi-agent-coordinator]] | Part 3 |
| 1.3 | SDK Implementation — Task Tool, Agent Definition, Execution Patterns | [[raw-api-vs-sdk]], [[agent-definition]], [[fork-sessions]], [[goal-oriented-prompting]] | Part 4 |
| 1.4 | Enforcement and Handoff Patterns | [[probabilistic-vs-deterministic]], [[prerequisite-gates]], [[hooks]], [[structured-escalation-handoff]], [[multiconcern-decomposition]] | Part 5 |
| 1.5 | Hooks for Tool Call Interception & Data Normalization | [[data-normalization-hooks]], [[hooks-vs-prompts]], [[hooks-decision-framework]], [[hooks-anti-patterns]] | Part 6 |
| 1.6 | Task Decomposition Strategies | [[task-decomposition]], [[prompt-chaining]], [[dynamic-decomposition]], [[attention-dilution]], [[decomposition-anti-patterns]] | Part 7 |
| 1.7 | Session State, Resumption, and Forking | [[session-state]], [[stale-context]], [[fork-sessions]], [[session-anti-patterns]] | Part 8 |

> Domain 1 is now fully complete (T1.1–T1.7). Part 9 begins Domain 2: Tool Design and MCP Integration.

## Domain Introduction (from Part 2)

"Domain one is worth 27% of the exam. That's why we start here... Study order matters. Domain one first. Everything else builds on the agentic loop concepts we cover today."

"By the end of this video, you'll be able to design, explain, and reason about agentic loops at exam depth."

## Key Exam Signal

- The exam tests **applied judgment**, not memorized definitions
- Questions are scenario-based

[Source: 02-OCiLc9Frq84.md, 03-KQSSSP0op2M.md]
