---
title: Interview Pattern
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

A technique where you ask Claude to interview you before implementing anything. Claude asks the clarifying questions; you answer them; design decisions are made before any code is written. Prevents Claude from guessing on all design decisions in unfamiliar domains. [Source: 18-J2rT6yp-Lak.md]

## The Pattern

Instead of:
> "Implement a caching layer for our API responses."

Say:
> "Implement a caching layer for our API responses. **Before you write any code, interview me to surface the design decisions.**"

Claude responds with 5 questions (e.g., for a caching layer):
1. What is the cache invalidation strategy — time-based, event-based, or manual?
2. Should cache be per-user or shared?
3. What is the acceptable staleness window?
4. Do we need cache warming on startup?
5. On cache failure — serve stale data or hit the origin?

You answer all five. Then Claude implements.

## Why It Works

Without the interview, Claude guesses on all five questions and potentially builds the wrong thing entirely. The interview pattern:
1. **Surfaces decisions** you must make (but might not know you need to make)
2. **Forces them before implementation** (cheaper to decide upfront than to refactor)
3. **Prevents partial builds** that satisfy one implicit assumption and violate another

## When to Use

- Implementing in an **unfamiliar domain** (you don't know what you don't know)
- Tasks with **multiple valid designs** (any choice is defensible; all need explicit commitment)

## Exam Takeaway

> **Interview pattern = ask Claude to interview you before it writes a single line.** Use in unfamiliar domains. Without it, Claude guesses on all design decisions.

## Connection to Goal-Oriented Prompting

[[goal-oriented-prompting]] (D1 T1.3) says: specify goal + output format + constraints; never specify tool call sequences. The interview pattern is the clarification step that surfaces constraints you didn't know to specify. Both follow the same principle: define *what* before *how*.

## Connections

- [[iterative-refinement-techniques]] — T3.5 hub page
- [[goal-oriented-prompting]] — both prioritize surfacing goals before execution
- [[iterative-refinement-anti-patterns]] — anti-pattern 4 is skipping this pattern

[Source: 18-J2rT6yp-Lak.md]
