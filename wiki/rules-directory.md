---
title: .claude/rules Directory — Auto-Loading Rule Files
created: 2026-04-16
last_updated: 2026-04-16
source_count: 2
status: draft
---

The `.claude/rules` directory is an alternative to the [[at-import-syntax]] for organizing CLAUDE.md content. Files placed in this directory are automatically loaded as additional instructions — no explicit `@import` statement required.

## How It Works

Create separate markdown files inside `.claude/rules/`:

```
.claude/
  rules/
    testing.md          ← testing conventions
    api-conventions.md  ← API design rules
    deployment.md       ← deployment procedures
```

Claude automatically loads all files in this directory at session start. No root-level `@import` entries needed.

## Key Advantage Over @import: Conditional Loading

Rule files in `.claude/rules/` can have a **`paths:` YAML frontmatter field** that scopes when the rule loads. This is the core T3.3 feature.

```yaml
---
paths:
  - src/api/**/*
---
# API Conventions
Always use async/await with Zod validation...
```

This rule only loads when Claude is editing a file matching `src/api/**/*`. Editing a React component? The API rules are never loaded.

**`@import` loads unconditionally. Rule files with `paths:` load conditionally.** This is the key difference.

Without a `paths:` field, the rule loads for every file — identical to `@import`. Always add `paths:` to scope your rule files. See [[path-specific-rules]] for the full T3.3 treatment.

## Common Glob Patterns

- `**/*.test.*` — all test files, any directory, any extension
- `src/api/**/*` — all API files and subdirectories
- `**/*.tsx` — all React component files

Multiple patterns in the array = OR logic (rule loads if any pattern matches).

## Comparison: @import vs .claude/rules

| Feature | @import | .claude/rules |
|---------|---------|---------------|
| Explicit import needed? | Yes | No (auto-loaded) |
| Path-specific scoping? | No — always unconditional | Yes — via `paths:` YAML field |
| Selective per package? | Yes | Via `paths:` field |
| Best for | Simple, explicit modular org | Projects with area-specific conventions |

## Exam Signal

- `.claude/rules` + `paths:` = conditional loading (only loads when file matches)
- `@import` = always unconditional
- No `paths:` field on a rule file = loads for every file (exam-tested anti-pattern #2; see [[path-rules-anti-patterns]])
- "No modular organization" = exam-tested anti-pattern whether you use @import or rules directory (see [[claude-md-anti-patterns]])

## Connections

- [[claude-md-hierarchy]] — hub page for T3.1
- [[at-import-syntax]] — explicit @import alternative; unconditional loading
- [[path-specific-rules]] — T3.3 hub; the `paths:` field in full detail
- [[glob-pattern-routing]] — how glob patterns work in the `paths:` field
- [[monolithic-split]] — rules directory as one solution to the 800-line monolith

[Source: 14-qFYN8XYMTZM.md, 16-iOvyCv_rwac.md]
