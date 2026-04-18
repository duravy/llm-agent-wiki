---
title: Splitting a Monolithic CLAUDE.md
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

When a CLAUDE.md file has grown to hundreds of lines covering multiple topics, it's time to split it. A monolithic file has two problems: it's hard to maintain, and irrelevant rules dilute the important ones. Claude reads the whole file every session regardless of context.

## The Pattern

**Before** (monolithic):
```
CLAUDE.md — 800 lines covering:
  - Testing conventions
  - API design rules
  - Deployment procedures
  - Code style
  - And more...
```

**After** (modular with @import):
```
CLAUDE.md — ~50 lines
  High-level overview of the project
  @import ./rules/testing.md
  @import ./rules/api-conventions.md
  @import ./rules/deployment.md
  @import ./rules/code-style.md

rules/
  testing.md          — short, focused, maintainable
  api-conventions.md  — short, focused, maintainable
  deployment.md       — short, focused, maintainable
  code-style.md       — short, focused, maintainable
```

The root file becomes a readable index — not a wall of text.

## Benefits of Splitting

1. **Maintainable** — each file covers one topic; changes are localized
2. **Selective importing** — subdirectory CLAUDE.md files can import only relevant rules
3. **Readable root** — root file is a short overview, not a specification
4. **Focused context** — irrelevant rules don't dilute attention for the current task
5. **Conditional loading** — when placed in `.claude/rules/` with a `paths:` field, rule files load only for matching files — eliminating the remaining ~80% token waste (T3.3; see [[path-specific-rules]])

## When to Split

The signal: your CLAUDE.md has grown to cover multiple unrelated concerns — testing, API design, deployment, code style all in one file. If you can identify 3+ distinct topic areas, split them.

## Alternative: .claude/rules Directory

Instead of @import references, you can place the topic files in `.claude/rules/` and have them auto-loaded. See [[rules-directory]] for details.

## Exam Signal

- Root file after split = ~50 lines with @import references
- "Massive monolithic CLAUDE.md" = exam-tested anti-pattern #1 (see [[claude-md-anti-patterns]])
- Split into focused files = the correct fix
- Each file should be short, focused on one topic, and selectively importable

## Connections

- [[claude-md-hierarchy]] — hub page for T3.1
- [[at-import-syntax]] — the mechanism used to split (unconditional)
- [[rules-directory]] — auto-loading alternative to @import; add `paths:` for conditional loading
- [[path-specific-rules]] — T3.3 next step: conditional loading eliminates remaining token waste

[Source: 14-qFYN8XYMTZM.md, 16-iOvyCv_rwac.md]
