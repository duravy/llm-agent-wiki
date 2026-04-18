---
title: Agentic Loop — Anti-Patterns
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Three exam-tested anti-patterns in [[agentic-loop|agentic loop]] design. The exam expects candidates to recognize each pattern, name what is wrong, and explain the correct alternative. All three share a root cause: using unreliable signals (text, fixed counts, keywords) instead of the structured `stop_reason` and `tool_use` blocks.

## Anti-Pattern 1 — Text Parsing for Loop Control

**What it looks like:** Checking Claude's output text for words like "done", "finished", or "complete" to decide whether to exit the loop.

**Why it fails:** Claude uses natural language that varies. Claude might say "I'm done with step one" while still having three steps remaining. Any text-based check will produce false positives and false negatives.

**Correct approach:** Check `stop_reason`. It is structured, deterministic, and always present. See [[stop-reason]].

---

## Anti-Pattern 2 — Fixed Iteration Cap as Primary Exit

**What it looks like:** `for i in range(5): ...` — using a fixed count as the primary mechanism to end the loop.

**Why it fails:** A cap of 5 cuts off a 6-step task at the worst possible moment — mid-execution, with partial results.

**Correct approach:** Use `while True` with `stop_reason == end_turn` as the primary exit. A **safety counter** is fine and recommended as a *fallback* to prevent infinite loops — but it must not be the primary exit condition.

---

## Anti-Pattern 3 — Keyword Detection for Routing

**What it looks like:** Checking whether Claude's text contains certain words (e.g., "search", "calendar", "payment") to decide which function to call next.

**Why it fails in two ways:**
1. Claude's wording varies — the keyword may not appear even when the intent is present
2. Claude might *mention* a tool in passing without actually requesting it

**Correct approach:** Read the structured `tool_use` blocks in the `content` array. They contain exactly which tool Claude requested with which inputs — reliably, every time. This is [[model-driven-vs-scripted|model-driven agency]] in practice.

---

## Summary Table

| Anti-pattern | Unreliable signal used | Correct signal |
|-------------|----------------------|----------------|
| Text parsing | Claude's natural language | `stop_reason` |
| Fixed iteration cap | Loop counter | `stop_reason == end_turn` |
| Keyword detection | Text content | `tool_use` blocks in `content` array |

[Source: 02-OCiLc9Frq84.md]
