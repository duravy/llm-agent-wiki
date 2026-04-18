---
title: Built-In Tools Anti-Patterns (D2 T2.5)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five exam-tested anti-patterns for built-in tool usage. These are the wrong-answer choices on the exam for Task 2.5.

## Summary Table

| # | Anti-pattern | Correct approach |
|---|-------------|-----------------|
| 1 | Reading all files upfront | Grep for entry points first, then Read selectively |
| 2 | Using Grep to find files by name | Grep searches content; use Glob for file name/path patterns |
| 3 | Using Write to modify one line | Write overwrites the entire file; use Edit for targeted changes |
| 4 | Using Edit when old_string isn't unique | Include more surrounding context, or fall back to Read + Write |
| 5 | Using Bash for file search | Bash's find/grep are less optimized and less safe; use Grep/Glob built-ins |

## Detailed Breakdown

**Anti-pattern 1 — Reading all files upfront**
You don't know which files matter until you search. Loading the entire codebase fills the context window with irrelevant code before you've identified what's needed. Always start with Grep to find entry points, then Read selectively. See [[two-phase-search-strategy]].

**Anti-pattern 2 — Using Grep to find files by name**
Grep searches file *contents*, not file names. Searching Grep for `*.config.js` or a naming pattern will not find files by name. Use Glob for file name and path patterns. See [[grep-vs-glob]].

**Anti-pattern 3 — Using Write to modify one line**
Write replaces the *entire file* with new content. Using Write to change a single function overwrites everything else. Use Edit for targeted single-location changes. See [[edit-vs-write]].

**Anti-pattern 4 — Using Edit when the match isn't unique**
Edit fails when old_string appears more than once in the file — it cannot determine which occurrence to replace. Fix: add more surrounding context to make the match unique. If that's not feasible, fall back to Read + Write. See [[edit-vs-write]].

**Anti-pattern 5 — Using Bash for file search**
Shell commands like `find` and `grep` are less optimized and less safe than the built-in Grep and Glob tools. Bash is for build, test, install, and git operations. File and content discovery belongs to Grep and Glob.

## 5-Step Exam Checklist

1. Starting exploration? → Grep first, then Read selectively (not Read everything)
2. Finding files by name? → Glob (not Grep)
3. Changing one part of an existing file? → Edit (not Write)
4. Edit failing? → check for non-unique match; add context or use Read + Write
5. Searching for files? → Grep/Glob built-ins (not Bash find/grep)

[Source: 13-IN2WntoHedY.md]
