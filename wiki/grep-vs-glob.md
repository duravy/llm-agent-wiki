---
title: Grep vs Glob — Most-Tested Distinction
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The single most common exam mistake in D2 T2.5: confusing Grep and Glob. They sound similar but search completely different things.

## Side-by-Side

| | Grep | Glob |
|--|------|------|
| **Searches** | File *contents* | File *names and paths* |
| **Input** | Text or regex pattern | Path/name pattern (with wildcards) |
| **Returns** | Files that *contain* the pattern | Files whose *path matches* the pattern |
| **Example input** | `describe(` | `*.test.tsx` |
| **Example output** | Every file containing a test suite (including non-test utility files) | Every file with `.test.tsx` in its path |

## The Decision Rule

> **Know the naming pattern → Glob. Know what the code contains → Grep.**

```
Which files have "test" in the name?
  → Glob: *.test.tsx

Which files actually contain test suites?
  → Grep: describe(
```

Note: a Glob for `*.test.tsx` gives you all files named as tests. A Grep for `describe(` finds all files that *actually contain* test suites — which could include non-test utility files that happen to define describe blocks.

## Why the Exam Tests This

The names are close, the use cases overlap superficially ("I want to find files"), but the mechanics are fundamentally different. Using Grep to find files by name is a specific exam-tested anti-pattern (see [[builtin-tools-anti-patterns]] Anti-pattern 2).

## Exam Signal

- "Find all files named `*.config.js`" → Glob (path pattern)
- "Find all files that import from `billing`" → Grep (content pattern)
- "Find all test files" → depends: Glob if by name, Grep if by content
- Using Grep for file name search = wrong; using Glob for content search = wrong

[Source: 13-IN2WntoHedY.md]
