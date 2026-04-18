---
title: Iterative Refinement Techniques (T3.5) — Progressive Improvement
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Four techniques for producing consistent, high-quality output when prose instructions alone fail. The underlying problem: prose is interpreted differently each run — each technique replaces vague description with something concrete, contractual, or question-driven. [Source: 18-J2rT6yp-Lak.md]

> **Naming note**: This page is T3.5. The existing [[iterative-refinement]] page covers the D1 T1.2 concept (coordinator redelegation on gaps) — a different concept with a similar name.

## The Core Problem

Prose instructions produce inconsistent results:
> "Convert dates to ISO format, normalize names to title case, extract the first email"

Claude may interpret those rules differently each run. The painter analogy: telling a painter "make it look like autumn" gets you a different painting every time. Showing three labeled "this is what I want" and three labeled "this is NOT what I want" gets you consistent results.

That difference — prose vs. concrete examples — is the heart of T3.5.

## The Four Techniques

### 1. Concrete Input/Output Examples

Provide 2–3 input/output pairs that show the transformation. The examples define the rule better than any paragraph can.

| Vague prompt | What Claude still has to guess |
|---|---|
| "Extract and normalize contact info" | Keep titles? null vs empty string? Local or international phone? |

With examples:
- Input 1 (full data) → locks JSON structure + international phone format
- Input 2 (name includes "Dr.") → output removes the title
- Input 3 (no name) → output sets `name: null`

Those three examples communicate what a paragraph cannot. See [[concrete-examples]].

### 2. Test-Driven Iteration

Write tests before implementing. Loop:
1. Write tests
2. Implement
3. Share the **exact failure message** (expected vs. received diff)
4. Iterate until green

The key move is specificity: "Expected `2025-03-17`, received `2017-03-25`" gives Claude a concrete contract to fix. "It doesn't work" gives Claude nothing. See [[test-driven-iteration]].

### 3. Interview Pattern

For unfamiliar domains or tasks with multiple valid designs: ask Claude to interview you before it writes any code.

> "Implement a caching layer for our API responses"

Without interview → Claude guesses on all design decisions. With interview → Claude asks 5 questions (invalidation strategy? per-user or shared? acceptable staleness? cache warming? stale-on-failure behavior?) and you answer before any code is written. See [[interview-pattern]].

### 4. Interacting vs Independent Issues

Choose the right fix strategy:
- **Interacting issues** (type signature change + 5 callers): fix **together in one message**
- **Independent issues** (error handling in auth + unused imports in billing): fix **sequentially**

Exam rule: *Does fixing A change the correct behavior of B? Yes → together. No → sequential.* See [[interacting-vs-independent-issues]].

## Five Anti-Patterns

See [[iterative-refinement-anti-patterns]] for the full list. Short form:

| Anti-pattern | Fix |
|---|---|
| Prose-only descriptions | Provide 2–3 concrete input/output examples |
| No tests | Write test suite first; iterate using failures |
| Fixing interacting issues one at a time | Address together in one pass |
| Skip interview in unfamiliar domain | Ask Claude to interview you first |
| Vague error reports | Share exact expected/received values |

## Five Exam Takeaways

1. **Concrete examples > prose** — they communicate transformation rules without ambiguity
2. **TDI sequence** — write tests → run → share exact failure → iterate to green
3. **Interview pattern** — clarifying questions before any code in unfamiliar domains
4. **Interacting = together; independent = sequential**
5. **Specific test cases with expected output** = most effective tool for edge case handling

## Cross-Domain Connections

- **[[probabilistic-vs-deterministic]]** (D1 T1.4): prose → probabilistic interpretation; concrete examples → deterministic output. Same fundamental distinction, applied to prompting technique.
- **[[goal-oriented-prompting]]** (D1 T1.3): interview pattern = surface goal and constraints before specifying solutions. Both tell you to define *what* before *how*.
- **[[structured-error-reporting]]** (D5 T5.3): exact failure message = the same specificity principle as structured error reports — Claude needs category + detail, not just "it failed."
- **[[iterative-refinement]]** (D1 T1.2): different concept — coordinator redelegation on gaps. Name similarity; no content overlap.

[Source: 18-J2rT6yp-Lak.md]
