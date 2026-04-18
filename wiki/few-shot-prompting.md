---
title: Few-Shot Prompting (D4 T4.2)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Task 4.2 of Domain 4. Few-shot prompting means including 2–4 example input/output pairs in a prompt so Claude sees the pattern — and the reasoning behind it — before processing new data. The exam frames this as the most effective technique for achieving consistent output when instructions alone produce inconsistent results. [Source: 21-u6oCkdxU5kw.md]

## The Core Idea

**Zero-shot:** "Sort these into the right bins."

**Few-shot:** "Watch me sort three letters first. This one goes to billing because it has an invoice number. This goes to personal because it's from a friend. This goes to spam because it's unsolicited. Now you sort the rest."

The examples communicate implicit rules you would struggle to describe in words. More importantly, they communicate *why* each decision was made — enabling Claude to generalize to inputs it has never seen. [Source: 21-u6oCkdxU5kw.md]

## Why Instructions Alone Fail

Instruction: *"Route customer requests to the appropriate tool."*

Input: *"Check on my order."*

Does Claude call `get_customer`, `lookup_order`, or `search_orders`? The instruction doesn't resolve this. Three examples do:

| Input | Action | Why |
|-------|--------|-----|
| "Check my order #12345" | `lookup_order(order_id=12345)` | Has order number → direct lookup |
| "Check on my order" (no number) | `get_customer` first | Need to identify caller before lookup |
| "Check orders for john@email.com" | `get_customer(email=...)` | Email present → identify by email |

The examples teach the **reasoning pattern** (why this action), not just the action. Claude generalizes to novel inputs it has never seen. [Source: 21-u6oCkdxU5kw.md]

## Key Concepts

| Concept | Page | One-Line Summary |
|---------|------|-----------------|
| Reasoning in Examples | [[reasoning-in-examples]] | Always include why — Claude copies action without reasoning, judgment with it |
| Gray Area Examples | [[gray-area-examples]] | Highest-value use case: teach Claude to distinguish acceptable patterns from genuine issues |
| Example Count Guidelines | [[example-count-guidelines]] | 2–3 sweet spot; 4–5 for ambiguous domains; 10+ is the anti-pattern |
| Anti-Patterns | [[few-shot-anti-patterns]] | Five: happy-path only, no reasoning, too many, all-same, prose format |

## Output Format Consistency

Prose instruction: *"Return findings in JSON with location, issue, severity, and suggested fix."*

Result: format drifts — fields go missing, extra fields appear, structure varies run to run.

One complete example with all fields (including a `detected_pattern` field that prose instructions wouldn't convey clearly) → Claude produces that exact structure every run.

> **"Show the format, don't describe it."** [Source: 21-u6oCkdxU5kw.md]

## Null Handling — Must Be Demonstrated

A behavior that's hard to describe in prose but obvious from an example: when no source is present, return `null` — not a fabricated value.

```
Input: "Revenue increased 15% year-over-year." (no citation)
Output: { "claim": "Revenue increased 15% YoY", "source": null }
```

Without this example, Claude may invent plausible-sounding sources. One example eliminates the hallucination. See [[gray-area-examples]]. [Source: 21-u6oCkdxU5kw.md]

## Exam Takeaways

1. Few-shot = most effective technique when instructions alone produce inconsistent results
2. Include reasoning (*why*) so Claude generalizes judgment, not just action
3. Show edge cases and ambiguous scenarios — highest value add
4. Null/missing field handling must be demonstrated via examples (return null, not fabricated)
5. 2–4 targeted examples = sweet spot
6. Few-shot enables generalization to *novel* patterns, not just matching the specific examples shown

## Cross-Domain Connections

- [[concrete-examples]] (D3 T3.5) — same "show don't describe" principle; T4.2 deepens with reasoning + gray areas + null handling
- [[prompt-design-explicit-criteria]] (D4 T4.1) — T4.1 defines *what* to flag via categorical criteria; T4.2 demonstrates those criteria in action via examples; natural pairing
- [[explicit-escalation-criteria]] (D5 T5.2) — few-shot examples in system prompts for escalation; same pattern applied to escalation judgment
- [[tool-description-design]] (D2 T2.1) — T4.2's tool routing example (which tool for "check on my order") mirrors the tool disambiguation problem
- [[probabilistic-vs-deterministic]] (D1 T1.4) — examples make output format deterministic; reasoning examples make judgment deterministic
- [[structured-output-tool-use]] (D4 T4.3) — T4.2 teaches null handling via examples (probabilistic); T4.3 enforces it via nullable schema fields (API-level); natural progression — show the pattern, then enforce the structure

[Source: 21-u6oCkdxU5kw.md]
