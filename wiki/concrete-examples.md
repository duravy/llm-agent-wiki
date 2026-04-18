---
title: Concrete Input/Output Examples
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The primary technique for communicating transformation rules to Claude without ambiguity. Provide 2–3 input/output pairs that define what you want; the examples replace vague prose with a concrete contract Claude can replicate consistently across runs. [Source: 18-J2rT6yp-Lak.md]

## Why Prose Fails

Vague prompt: *"Extract and normalize contact information."*

Unanswered questions:
- Should titles like "Dr." be kept in the name field?
- Missing values: `null` or empty string?
- Phone numbers: local format or international?

Each run, Claude makes a different guess on each question. Inconsistent output is the result.

## How Examples Fix It

Three input/output pairs lock in every ambiguous rule:

```
Input 1: {name: "Dr. Alice Chen", phone: "555-1234"}
Output 1: {name: "Alice Chen", phone: "+1-555-1234", email: null}
  → Locks: international format, title removed, missing = null

Input 2: {name: "Bob", email: "b@b.com"}
Output 2: {name: "Bob", phone: null, email: "b@b.com"}
  → Locks: missing fields = null, not empty string

Input 3: {}
Output 3: {name: null, phone: null, email: null}
  → Locks: empty object → all fields null
```

Those three examples communicate what a paragraph cannot.

## Edge Case Coverage

The same technique works for edge cases. Instead of "handle null values gracefully":

```
null name   → ""         (empty string, not null)
null age    → 0          (zero, not null)
empty {}    → defaults   (both rules apply)
```

Each pair defines a precise default rule. Vague → concrete.

## Exam Takeaway

> **Concrete input/output examples > prose descriptions.** They communicate transformation rules without ambiguity. Provide 2–3 pairs; one pair locks one rule; three pairs cover normal + edge cases.

"Pair broken input with expected output every time. Don't describe correctness abstractly. Show it concretely."

## Connection to Probabilistic vs Deterministic

Prose instructions = probabilistic interpretation (Claude guesses, varies run to run). Concrete examples = deterministic output (same transformation every run). This is the [[probabilistic-vs-deterministic]] principle applied at the prompting level. [Source: 18-J2rT6yp-Lak.md]

## Connection to D4 T4.2 — Few-Shot Prompting

D4 T4.2 ([[few-shot-prompting]]) deepens the principle established here. Where T3.5 focuses on 2–3 I/O pairs to define a transformation rule, T4.2 extends this with three additions: (1) always include *why* each output was chosen so Claude generalizes judgment rather than just copying actions; (2) show edge cases and gray areas — not just the happy path; (3) null/missing field handling must be demonstrated (return `null`, not a fabricated value). See [[reasoning-in-examples]] and [[gray-area-examples]]. [Source: 21-u6oCkdxU5kw.md]

## Connection to D4 T4.1 — Prompt Design and Explicit Criteria

D4 T4.1 ([[prompt-design-explicit-criteria]]) extends this principle to issue classification. Rather than providing input/output transformation pairs, it provides concrete code examples for each severity level (Critical/High/Medium/Low) so Claude can match new code against them and classify consistently. The underlying mechanism is identical: replace interpretation with examples. See [[severity-levels-examples]]. [Source: 20-EzWRMJz9lXc.md]

## Connections

- [[iterative-refinement-techniques]] — T3.5 hub page
- [[test-driven-iteration]] — TDI uses the same "show don't describe" principle for tests
- [[probabilistic-vs-deterministic]] — examples make output deterministic
- [[severity-levels-examples]] — D4 T4.1 applies the same principle to severity classification
- [[prompt-design-explicit-criteria]] — D4 T4.1 hub

[Source: 18-J2rT6yp-Lak.md, 20-EzWRMJz9lXc.md]
