---
title: Gray Area Examples
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The highest-value use case for few-shot prompting. Gray areas are cases where the correct action is non-obvious — where instructions alone cannot resolve the ambiguity. Examples that include reasoning teach Claude to distinguish acceptable patterns from genuine issues, enabling generalization to similar but not identical cases. [Source: 21-u6oCkdxU5kw.md]

## Code Review Gray Areas

Three examples from Part 21, each demonstrating a different judgment call:

### 1. Optional Chaining — Skip
```javascript
if (user?.name) { ... }
```
**Why:** The `?.` operator signals intentional defensive coding — the developer already handled the null case. This is correct code. **Skip it.**

### 2. Missing Null Check — Flag
```typescript
if (user.name) { ... }
// where the type definition says: user: User | null
```
**Why:** The type definition reveals that `user` can be null, but the code accesses `.name` without the optional chain. This will throw a TypeError at runtime. **Flag it.**

### 3. Empty Catch with Intent Comment — Skip
```javascript
catch(e) {
  // intentionally empty
}
```
**Why:** An empty catch block normally indicates a swallowed error. But the comment signals deliberate intent. **Skip it.**

These three examples teach Claude the judgment logic: look at the broader context (the type definition, the operator used, the comment) — don't just match surface-level patterns. [Source: 21-u6oCkdxU5kw.md]

## Document Extraction Gray Areas

Few-shot examples are equally valuable for handling structural variation in document extraction:

| Input Pattern | Output | Why |
|---|---|---|
| "Smith et al. (2024)" inline | `{type: "inline_citation", text: "Smith et al. (2024)"}` | Inline citation format |
| "[3]" in body text | Resolve to bibliography entry → `{type: "bibliography_reference", ...}` | Reference bracket → resolve |
| "Revenue increased 15% YoY." (no citation) | `{source: null}` | No citation present → return null, never fabricate |

The null case is the critical one. Without an example, Claude may invent a plausible-sounding source to fill the field. One example teaches: **return null, not fabricated values**. [Source: 21-u6oCkdxU5kw.md]

## Why Gray Areas Need Examples, Not Instructions

For gray areas, the judgment logic is context-dependent in a way that resists verbal description. You can't write an instruction that covers "look for the `?.` operator to determine if null handling is intentional" without it becoming longer and less reliable than simply showing the case.

Examples communicate the pattern directly. Instructions approximate it.

## Exam Takeaway

> **Show edge cases and ambiguous scenarios — that's where few-shot examples add the most value.** For gray areas specifically: include the *why* in each example so Claude can generalize the judgment to novel patterns, not just match the specific example shown.

Null handling is a special subtype: *"a behavior that's incredibly hard to describe in prose, but instantly obvious from a single example."* [Source: 21-u6oCkdxU5kw.md]

## Connections

- [[few-shot-prompting]] — T4.2 hub
- [[reasoning-in-examples]] — the *why* is what makes gray area examples work
- [[few-shot-anti-patterns]] — anti-pattern #1: happy path only (skipping gray areas)
- [[categorical-definitions]] (D4 T4.1) — T4.1 defines the categories; gray area examples show Claude where category boundaries lie
- [[nullable-fields]] (D4 T4.3) — the null extraction example (`{source: null}`) in T4.2 previews T4.3's nullable field design: few-shot teaches null return (probabilistic); nullable schema enforces it (API-level); both prevent fabrication, at different layers

[Source: 21-u6oCkdxU5kw.md]
