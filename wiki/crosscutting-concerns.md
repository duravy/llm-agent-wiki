---
title: Cross-Cutting Concerns — Path Rules vs Subdirectory claude.md
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Cross-cutting concerns are conventions that apply to a *type* of file scattered across the entire codebase, rather than to all files in a specific directory. Path-specific rules solve cross-cutting concerns. Subdirectory `claude.md` cannot.

## The Test Files Problem

Test files live next to the code they test:

```
src/
  components/
    Button.tsx
    Button.test.tsx          ← test file
  api/
    handlers.ts
    handlers.test.ts         ← test file
  billing/
    invoice.ts
    invoice.test.ts          ← test file
  utils/
    helpers.ts
    helpers.test.tsx         ← test file
```

Test files are in `components/`, `api/`, `billing/`, `utils/` — everywhere. They all share the same assertion patterns and mock setup conventions.

**Without path-specific rules**: You need a `claude.md` file in every single directory containing test files. Duplicate content. A maintenance nightmare. Change one test convention → update it in dozens of files.

**With one path-specific rule**:

```yaml
---
paths:
  - "**/*.test.*"
---
# Test Conventions
Use vitest. Describe blocks for grouping. Mock external dependencies with vi.mock()...
```

One file. Zero duplication. Covers the entire codebase.

## What Counts as a Cross-Cutting Concern

| File type | Pattern | Why it's cross-cutting |
|-----------|---------|----------------------|
| Test files | `**/*.test.*` | Live next to the code they test — everywhere |
| React components | `**/*.tsx` | Spread across features, not confined to one dir |
| Terraform configs | `**/*.tf` | Live in infra dirs scattered through the repo |
| GraphQL schemas | `**/*.graphql` | Live near the features they define |

## When to Use Subdirectory claude.md Instead

Subdirectory `claude.md` is location-bound: it applies to everything inside a specific directory and its subdirectories. Use it for monorepo packages with a distinct codebase:

```
packages/
  billing/
    claude.md   ← billing-specific conventions for everything in this package
```

This makes sense because the billing package has its own distinct conventions that apply to *all* code in that package — API handlers, models, tests, utilities — not just a specific file type.

See [[path-rules-vs-subdirectory-claude-md]] for the full decision framework.

## Exam Signal

- "Conventions for files spread across many directories" → **always path-specific rules**
- "Conventions for a specific bounded package" → **subdirectory claude.md**
- "Test files need consistent conventions" → path-specific rule with `**/*.test.*`
- "Need a claude.md in every directory" → anti-pattern; use path-specific rule instead

## Connections

- [[path-specific-rules]] — T3.3 hub page
- [[glob-pattern-routing]] — the `**/*.test.*` pattern
- [[path-rules-vs-subdirectory-claude-md]] — full decision framework
- [[path-rules-anti-patterns]] — anti-pattern #1: subdirectory claude.md for cross-cutting concerns

[Source: 16-iOvyCv_rwac.md]
