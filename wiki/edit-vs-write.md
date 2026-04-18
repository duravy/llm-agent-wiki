---
title: Edit vs Write — and the Edit Fallback
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Edit and Write both modify files but operate at completely different scales. Choosing wrong wastes tokens or causes failures.

## Side-by-Side

| | Edit | Write |
|--|------|-------|
| **Scope** | Targeted replacement of one text span | Entire file content |
| **What changes** | Only the matched old_string | Everything |
| **Use for** | Changing one specific part of an existing file | Creating new files or complete rewrites |
| **Requirement** | old_string must be **unique** in the file | No uniqueness constraint |
| **Failure mode** | Fails if old_string appears more than once | N/A (always overwrites) |

## Edit's Uniqueness Requirement

Edit requires the old_string to appear exactly once in the file. If the same text appears in multiple places, Edit cannot know which occurrence to change — it fails.

**Fix**: include more surrounding context in old_string to make the match unique:

```
# Fails: too short, appears 3 times
old_string: "return result"

# Works: enough context to be unique
old_string: "def calculate_refund(order_id):\n    ...\n    return result"
```

## The Edit Fallback: Read + Write

When Edit cannot find a unique anchor, fall back to a two-step pattern:

1. **Read** the entire file to get its current content
2. **Write** the full modified content back, replacing the file

This is more expensive — you load and replace the whole file — but it always works when targeted replacement fails.

> **"When edit fails, the answer is read then write."** — explicit exam takeaway

## Decision Tree

```
Modifying an existing file?
  ├── Changing one specific part?
  │     └── Is old_string unique in the file?
  │           ├── Yes → Edit
  │           └── No  → add more context, or fall back to Read + Write
  └── Rewriting the whole thing? → Write

Creating a new file? → Write
```

## Exam Signal

- "Use Write to change one line" → wrong; that overwrites the whole file; use Edit
- "Edit fails because the string appears twice" → include more surrounding context or use Read + Write fallback
- "What to do when Edit fails?" → Read the full file, then Write the modified version back
- Using Write for single-line changes = anti-pattern 3 (see [[builtin-tools-anti-patterns]])
- Using Edit with a non-unique match = anti-pattern 4

[Source: 13-IN2WntoHedY.md]
