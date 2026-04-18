---
title: Few-Shot Prompting Anti-Patterns (D4 T4.2)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five anti-patterns that undermine few-shot prompting effectiveness. All are exam-tested in T4.2. [Source: 21-u6oCkdxU5kw.md]

## The Five Anti-Patterns

| # | Anti-Pattern | Problem | Fix |
|---|---|---|---|
| 1 | Happy path examples only | Claude can't handle edge cases, null inputs, or missing fields | Include at least one edge case and one null/missing example |
| 2 | Examples without reasoning | Claude copies the action but not the judgment; can't generalize | Always include *why* each action was chosen |
| 3 | Too many examples (10+) | Wastes context tokens with diminishing returns | 2–4 targeted, diverse examples is enough |
| 4 | All examples look the same | Claude learns one pattern, not the range of variation | Make examples structurally diverse: different formats, edge cases, null handling |
| 5 | Relying on prose for output format | Format drifts across runs: missing fields, extra fields, structural variation | Show the exact output format in at least one example |

[Source: 21-u6oCkdxU5kw.md]

## Anti-Pattern Detail

### 1. Happy Path Only
Most prompts start with the success case. That's fine. The mistake is stopping there. The real value of few-shot examples is in edge cases — where instructions would fail because the correct action is non-obvious. If your 2–3 examples are all successful, well-formed inputs, Claude will perform well on those and poorly on everything else.

Fix: always include one example with incomplete data, missing fields, or an ambiguous pattern. See [[gray-area-examples]].

### 2. Examples Without Reasoning
An example that shows action but not reasoning teaches Claude to pattern-match, not to reason. Pattern matching works for inputs that look like the examples. Reasoning generalizes to inputs that are structurally similar but not identical.

*"The examples teach Claude the reasoning pattern, not just which action to take, but why that action was chosen."* [Source: 21-u6oCkdxU5kw.md]

Fix: add a brief *why* annotation to every example. See [[reasoning-in-examples]].

### 3. Too Many Examples (10+)
The 10th example rarely teaches anything the 3rd didn't already teach. Meanwhile, it consumes the same context tokens. In agentic sessions where examples appear in every prompt, this cost multiplies.

Fix: cap at 4–5 examples even for ambiguous domains. Use structural diversity to cover the space rather than repetition. See [[example-count-guidelines]].

### 4. All Examples Look the Same
Ten examples of "well-formed invoice → parse fields" teach Claude one thing. Three examples of "well-formed invoice", "invoice with missing date", and "not an invoice → return null" teach three distinct patterns in a third of the tokens.

Fix: ensure each example demonstrates something different. At minimum: one common case, one edge case, one null/missing case.

### 5. Relying on Prose for Output Format
Instructions like "return JSON with location, issue, severity, and suggested fix" sound specific but aren't. Does `severity` take a string or an integer? Is `location` a line number or an object? These gaps become inconsistencies at runtime.

Fix: show one complete, well-formed output example. Claude will match its structure precisely. The `detected_pattern` field example from Part 21: prose might omit it; an example that includes it makes it required. [Source: 21-u6oCkdxU5kw.md]

## Exam Takeaway

> "These are the five anti-patterns to avoid" — stated directly as exam content in Part 21. The exam most likely tests anti-pattern 2 (no reasoning → can't generalize) and anti-pattern 5 (prose format → drift).

## Connections

- [[few-shot-prompting]] — T4.2 hub
- [[reasoning-in-examples]] — anti-pattern #2 fix
- [[gray-area-examples]] — anti-pattern #1 fix
- [[example-count-guidelines]] — anti-pattern #3 fix
- [[prompt-design-anti-patterns]] (D4 T4.1) — companion anti-pattern list for explicit criteria

[Source: 21-u6oCkdxU5kw.md]
