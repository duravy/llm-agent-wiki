---
title: Glob Pattern Routing — Path-Specific Rule Matching
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The `paths:` field in `.claude/rules/` YAML frontmatter accepts glob patterns that determine when the rule file loads. A rule activates if **any** pattern in the array matches the file being edited.

## Pattern Syntax

| Symbol | Meaning | Example |
|--------|---------|---------|
| `**` | Any number of directory levels | `src/api/**` matches `src/api/users/handlers.ts` |
| `*` | Any filename within one level | `*.ts` matches `utils.ts` but not `utils/index.ts` |
| `*.ext` | Files with a specific extension | `*.tsx` matches all `.tsx` files |

## Exam-Tested Patterns

```yaml
# All test files — any directory, any extension ending in .test.*
paths:
  - "**/*.test.*"

# All files inside the API directory (any subdirectory depth)
paths:
  - "src/api/**/*"

# All React component files anywhere in the repo
paths:
  - "**/*.tsx"
```

### `**/*.test.*` — The Cross-Cutting Pattern

This is the most exam-important pattern. It matches:
- `components/Button.test.tsx`
- `api/handlers.test.ts`
- `billing/utils.test.js`
- `utils/helpers.test.tsx`

All test files, regardless of directory, with a single pattern. This is why path-specific rules beat subdirectory `claude.md` for test conventions — one file covers the entire codebase. See [[crosscutting-concerns]].

## Multiple Patterns = OR Logic

A rule loads if **any** pattern matches. Useful for files that share conventions but live in separate areas:

```yaml
paths:
  - "src/api/**/*"
  - "src/middleware/**/*"
```

This rule loads for API handlers and middleware — they share async/await patterns and error handling conventions.

## Omitting Paths

If you omit the `paths:` field entirely, the rule loads for every file — identical to putting it in `claude.md` directly. **Always add `paths:` to scope your rule files.** This is exam-tested anti-pattern #2 (see [[path-rules-anti-patterns]]).

## Overly Broad Patterns

```yaml
# Anti-pattern: defeats the purpose
paths:
  - "**/*"   # loads for every file — same as omitting paths entirely
```

Be specific. `src/api/**/*` not `**/*`. This is exam-tested anti-pattern #3.

## Exam Signal

- `**/*.test.*` = the cross-cutting test-files pattern — know this cold
- Multiple patterns in array = OR logic
- No `paths:` field = loads for every file (anti-pattern #2)
- `**/*` = too broad, defeats purpose (anti-pattern #3)

## Connections

- [[path-specific-rules]] — T3.3 hub page; glob patterns are the mechanism
- [[crosscutting-concerns]] — why `**/*.test.*` solves what subdirectory claude.md cannot
- [[path-rules-anti-patterns]] — anti-patterns #2 and #3 are about glob misuse

[Source: 16-iOvyCv_rwac.md]
