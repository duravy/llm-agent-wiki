---
title: Path-Specific Rules vs Subdirectory claude.md — Decision Framework
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Two mechanisms for scoping Claude conventions to specific parts of a codebase. Choosing the wrong one is an exam-tested anti-pattern.

## The Decision

```
Are the files spread across many directories (by type)?
  Yes → path-specific rules in .claude/rules/ with paths: field

Are the files all inside one specific bounded package or directory?
  Yes → subdirectory claude.md
```

## Comparison

| | Path-Specific Rules | Subdirectory claude.md |
|---|---|---|
| **Scope type** | Pattern-bound | Location-bound |
| **Best for** | File types spread everywhere | Bounded packages with distinct codebases |
| **Example files** | All `.test.*` files, all `.tsx` files | Everything inside `packages/billing/` |
| **Mechanism** | Glob patterns in `paths:` YAML field | File in the target subdirectory |
| **Coverage** | Entire codebase matching the pattern | All files in that directory + subdirectories |

## When to Use Path-Specific Rules

The file type is spread throughout the codebase — not confined to one directory:
- **Test files** — `Button.test.tsx` in `components/`, `handlers.test.ts` in `api/`, `invoice.test.ts` in `billing/`
- **React components** — feature components in `dashboard/`, `settings/`, `auth/`, all use the same hook patterns
- **Terraform configs** — `.tf` files in `infra/`, `modules/`, scattered through the repo

Pattern: `**/*.test.*`, `**/*.tsx`, `**/*.tf`

## When to Use Subdirectory claude.md

A specific package or subsystem has its own distinct conventions that apply to *all* code within it:
- `packages/billing/claude.md` — billing-specific API contracts, currency handling rules, audit logging requirements
- `apps/mobile/claude.md` — React Native conventions, platform-specific patterns
- `services/ml-pipeline/claude.md` — Python conventions, data validation patterns

These apply to everything inside the directory regardless of file type.

## The Anti-Pattern

> Using subdirectory `claude.md` for cross-cutting concerns.

Test files live everywhere. You can't put a `claude.md` in every directory. That's duplication, and it becomes a maintenance nightmare when conventions change.

The correct fix: one path-specific rule with `**/*.test.*`. See [[path-rules-anti-patterns]] anti-pattern #1.

## Exam Signal

- **Exam question trigger**: "conventions for files spread across many directories"
- **Answer**: path-specific rules — never subdirectory `claude.md`
- **Pattern-bound** = path-specific rules; **location-bound** = subdirectory `claude.md`
- These two are complementary, not mutually exclusive — use both in the same project

## Connections

- [[path-specific-rules]] — T3.3 hub page
- [[crosscutting-concerns]] — detailed cross-cutting use case
- [[claude-md-levels]] — T3.1; subdirectory claude.md is the third level of the hierarchy
- [[path-rules-anti-patterns]] — anti-pattern #1 is the wrong choice here

[Source: 16-iOvyCv_rwac.md]
