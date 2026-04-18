---
title: Test-Driven Iteration (TDI)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

A refinement loop where tests are written *before* implementation and Claude iterates against exact failure output until all tests pass. Tests replace prose ambiguity with an executable specification: "this input should produce this exact output." [Source: 18-J2rT6yp-Lak.md]

## The Loop

```
1. Write tests
2. Share with Claude → Claude implements
3. Run tests → some fail
4. Share the EXACT failure message (expected vs. received diff)
5. Claude fixes
6. Repeat until all green
```

## The Key Move: Exact Failure Messages

The difference between a useful iteration and a wasted one is specificity.

**Vague (useless):**
> "It doesn't work for European dates."

**Exact (actionable):**
> "Expected `2025-03-17`, received `2017-03-25`"

The exact diff is what Claude can act on. Vague reports give Claude nothing to fix — it has to guess what "doesn't work" means.

## Why Tests Remove Ambiguity

A test is a machine-readable specification. Each test case is a contract:
- This input → this exact output
- Pass/fail is binary — no interpretation needed

Without tests: "verify correctness" means Claude decides what correctness looks like. With tests: correctness is defined by the test suite, not Claude's judgment.

## Exam Takeaway

> **TDI sequence (cold):** write tests → run → share exact failure message (expected/received diff) → iterate to green. Anti-pattern: sharing vague error reports instead of exact diffs.

## Connection to Concrete Examples

[[concrete-examples]] and TDI are complementary: concrete examples define what you want; tests enforce it mechanically. TDI is concrete examples made executable.

## Connections

- [[iterative-refinement-techniques]] — T3.5 hub page
- [[concrete-examples]] — same "show don't describe" principle; TDI makes it executable
- [[iterative-refinement-anti-patterns]] — anti-patterns 2 and 5 are both TDI failures

[Source: 18-J2rT6yp-Lak.md]
