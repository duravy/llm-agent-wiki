---
title: Path-Specific Rules — T3.3 Hub
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Path-specific rules enable conditional convention loading: a rule file only loads into context when Claude is editing a file that matches the rule's glob patterns. This solves the one-size-fits-all problem where a monolithic `claude.md` wastes tokens loading irrelevant rules for every file.

> **Mechanism: `paths:` YAML frontmatter field in `.claude/rules/` files.**

## The Problem

Most codebases have different conventions for different areas:
- React components → functional style and hooks
- API handlers → async/await with Zod validation
- Test files → assertion patterns, mock setup

A monolithic `claude.md` loads all of these rules for every file. Three compounding problems:

1. **Wasted tokens** — API rules consume context window when editing a React component
2. **Irrelevant guidance** — Claude might apply database patterns to frontend code
3. **Non-deterministic behavior** — Claude *infers* which section applies, rather than matching it explicitly (see [[probabilistic-vs-deterministic]])

## The Solution

Create modular rule files in `.claude/rules/` and add a `paths:` YAML frontmatter field:

```yaml
---
paths:
  - src/api/**/*
---
# API Conventions
Always use async/await with Zod validation...
```

The rule loads **only** when Claude is editing a file matching `src/api/**/*`. Editing a React component? The API rules are never loaded.

**Key rule: always add a `paths:` field to scope your rule files.** Omitting `paths:` causes the file to load for every file — identical to putting it in `claude.md` directly.

## Token Savings

| | Without path rules | With path rules |
|---|---|---|
| Editing `button.tsx` | 5 rule sets loaded; React is the only relevant one | Only `**/*.tsx` rule loads |
| Wasted context | ~80% irrelevant | 0% irrelevant |

## Glob Patterns

See [[glob-pattern-routing]] for full reference. Key exam examples:
- `**/*.test.*` → all test files (any dir, any extension)
- `src/api/**/*` → all API files + subdirectories
- `**/*.tsx` → all React components

## Cross-Cutting Concerns

Path-specific rules solve cross-cutting concerns — conventions for file types spread across the entire codebase. See [[crosscutting-concerns]].

## Decision: Path-Specific Rules vs Subdirectory claude.md

See [[path-rules-vs-subdirectory-claude-md]] for the full framework. Short rule:
- Files spread across many directories → path-specific rules (pattern-bound)
- Specific bounded package → subdirectory `claude.md` (location-bound)

## @import vs Paths

`@import` always loads the imported file unconditionally. Rule files with `paths:` load conditionally only when relevant. This is the key advantage of the rules directory over @import for area-specific conventions. See [[at-import-syntax]] and [[rules-directory]].

## Anti-Patterns

See [[path-rules-anti-patterns]] for the four exam-tested wrong answers.

## Exam Mnemonic — 5 Takeaways

1. `.claude/rules/` files with `paths:` YAML frontmatter = conditional convention loading; rules activate only for matching files
2. Glob patterns: `**/*.test.*` = all test files; `src/api/**/*` = all API files
3. Path-specific rules beat subdirectory `claude.md` for cross-cutting concerns — test files spread everywhere, not confined to one directory
4. Token savings: only relevant conventions load; ~80% waste → 0% waste
5. **Exam signal**: "conventions for files spread across many directories" → path-specific rules, not subdirectory `claude.md`

## Connections

- [[rules-directory]] — the `.claude/rules/` directory mechanism; path scoping lives here
- [[at-import-syntax]] — unconditional alternative; @import vs paths: is a tested distinction
- [[glob-pattern-routing]] — deep dive on glob patterns and OR logic
- [[crosscutting-concerns]] — the primary use case: test files, React components, Terraform
- [[path-rules-vs-subdirectory-claude-md]] — decision framework
- [[path-rules-anti-patterns]] — four exam-tested wrong answers
- [[monolithic-split]] — T3.1 fix; path-specific rules take it further with conditional loading
- [[claude-md-hierarchy]] — T3.1 hub; T3.3 builds directly on modular organization
- [[probabilistic-vs-deterministic]] — D1 T1.4; inference vs explicit matching is the same distinction

[Source: 16-iOvyCv_rwac.md]
