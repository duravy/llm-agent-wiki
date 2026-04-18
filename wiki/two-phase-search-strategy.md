---
title: Two-Phase Search Strategy
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

When exploring an unfamiliar codebase, use a two-phase strategy: search first, then read. Never read blindly upfront.

## The Two Phases

**Phase 1 — Grep for entry points**
Search for the function, concept, or term you need. Grep returns every file that contains it.

```
Grep: "process_refund"
→ [billing_service.py, api_routes.py, refund_handler.py]
```

**Phase 2 — Read the specific files Grep surfaced**
You now know exactly which files are relevant. Read those — not the entire codebase.

```
Read: billing_service.py
Read: refund_handler.py
```

## Why Not Read Everything Upfront?

You don't know which files matter until you search. Reading blindly fills the context window with code that has nothing to do with your task — wasted tokens, diluted attention.

## Why Not Grep for Everything?

Grep finds matches but does not show the surrounding context needed to understand the code. Read gives you the full picture once you know where to look. The two tools are complementary: Grep locates, Read explains.

## Connection to Incremental Exploration

The two-phase strategy is the entry point for [[incremental-exploration]]: Phase 1 (Grep) → Phase 2 (Read a file) → discover the next reference → Read that file → and so on. Each read narrows the next search.

## Exam Signal

- "How to start exploring an unfamiliar codebase?" → Grep first, then Read selectively
- "Why not read all files upfront?" → fills context with irrelevant code; you don't know what matters yet
- Reading everything upfront = anti-pattern 1 (see [[builtin-tools-anti-patterns]])

[Source: 13-IN2WntoHedY.md]
