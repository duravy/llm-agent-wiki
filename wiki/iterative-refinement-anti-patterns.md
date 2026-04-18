---
title: Iterative Refinement Anti-Patterns (T3.5)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five exam-tested anti-patterns for T3.5 iterative refinement techniques. Each represents a failure to make instructions concrete, contractual, or question-driven. [Source: 18-J2rT6yp-Lak.md]

## Anti-Pattern 1: Prose-Only Descriptions

**What it is:** Describing the desired transformation in natural language without providing examples.

**Why it fails:** Prose is interpreted inconsistently across runs. Claude makes different guesses on unspecified details each time.

**Fix:** Provide 2–3 concrete input/output examples. See [[concrete-examples]].

## Anti-Pattern 2: Implementing Without Tests

**What it is:** Asking Claude to implement a feature with no test suite to verify against.

**Why it fails:** Without tests, there is no way to verify correctness or catch regressions. "Does it work?" has no mechanical answer.

**Fix:** Write the test suite first. Use test failures as the basis for each iteration. See [[test-driven-iteration]].

## Anti-Pattern 3: Fixing Interacting Issues One at a Time

**What it is:** Sending sequential fix requests for issues that depend on each other.

**Why it fails:** A partial fix creates an intermediate broken state where fixing one issue invalidates the other. Cascading failures result.

**Fix:** Address all interacting issues in a single message. See [[interacting-vs-independent-issues]].

## Anti-Pattern 4: Skipping the Interview Pattern in Unfamiliar Domains

**What it is:** Jumping straight to implementation in a domain where you don't know all the design decisions.

**Why it fails:** Claude guesses on every open design question. You get an implementation built on guessed assumptions that may not match your needs.

**Fix:** Ask Claude to interview you before writing any code. See [[interview-pattern]].

## Anti-Pattern 5: Vague Error Reports

**What it is:** Reporting failures with non-specific descriptions ("it doesn't work," "the date is wrong").

**Why it fails:** Claude has nothing concrete to act on. It must guess what "doesn't work" means, which may lead to fixing the wrong thing.

**Fix:** Share the exact test failure: expected value + received value. `"Expected 2025-03-17, received 2017-03-25"` is actionable. "The date is wrong" is not.

## Summary Table

| Anti-pattern | Root cause | Fix |
|---|---|---|
| Prose-only | No concrete specification | 2–3 input/output examples |
| No tests | No verification contract | Write tests first |
| Serial interacting fixes | Cascading failures | Fix together in one message |
| Skip interview | Guessed design decisions | Interview before code |
| Vague error reports | No actionable diff | Exact expected/received values |

[Source: 18-J2rT6yp-Lak.md]
