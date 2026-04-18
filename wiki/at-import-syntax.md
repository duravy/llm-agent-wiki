---
title: "@import Syntax — Modular CLAUDE.md Organization"
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Large CLAUDE.md files become unwieldy. The `@import` syntax lets you split instructions into focused files while keeping the root file clean and readable.

## How It Works

In the root `CLAUDE.md`, write `@import` followed by the relative path to each rule file:

```markdown
# Project Instructions

@import ./rules/testing.md
@import ./rules/api-conventions.md
@import ./rules/deployment.md
@import ./rules/code-style.md
```

Each imported file contains instructions for one specific topic. Claude loads all imported files as part of the session configuration.

**Paths are relative to the importing file.**

## Benefits

1. **Focused files** — each file covers one topic; easy to maintain and update
2. **Readable root** — root CLAUDE.md becomes a high-level overview plus a list of imports
3. **Selective importing per package** — a subdirectory-level CLAUDE.md can import only the rules it needs:

```markdown
# packages/billing/claude.md

@import ../../rules/api-conventions.md
@import ../../rules/testing.md
# Billing has its own deployment process — skip deployment.md
```

The billing package imports the API and testing rules but skips the deployment file.

## Example: Before and After

**Before** (monolithic):
```
CLAUDE.md — 800 lines covering testing, API design, deployment, code style
```

**After** (modular):
```
CLAUDE.md — 50 lines (overview + @import references)
  @import ./rules/testing.md
  @import ./rules/api-conventions.md
  @import ./rules/deployment.md
  @import ./rules/code-style.md
```

See [[monolithic-split]] for the full before/after pattern.

## @import vs .claude/rules Directory

| Feature | @import | .claude/rules directory |
|---------|---------|------------------------|
| Explicit reference needed? | Yes | No (auto-loaded) |
| Loading behavior | **Always unconditional** | Conditional when `paths:` field is set |
| Selective per package? | Yes | Via `paths:` YAML field (T3.3) |
| Path flexibility | Any location | Must be in `.claude/rules/` |

**Critical distinction**: `@import` always loads the imported file regardless of which file is being edited. A `.claude/rules/` file with a `paths:` field loads *only* when the active file matches the glob patterns. This makes the rules directory more powerful for area-specific conventions.

See [[rules-directory]] for the auto-loading alternative. See [[path-specific-rules]] for the full T3.3 conditional loading treatment.

## Exam Signal

- @import = modular CLAUDE.md organization, always loaded unconditionally
- `.claude/rules/` + `paths:` = conditional loading — only loads for matching files
- @import vs `paths:` = tested distinction: "always" vs "only when relevant"
- Paths in @import are relative to the importing file
- Selective importing per subdirectory CLAUDE.md = exam-tested benefit of @import
- "No modular organization" (everything in one file) = exam-tested anti-pattern (see [[claude-md-anti-patterns]])

## Connections

- [[claude-md-hierarchy]] — hub page for T3.1
- [[rules-directory]] — alternative auto-loading mechanism; `paths:` field enables conditional loading
- [[path-specific-rules]] — T3.3 hub; conditional loading in full detail
- [[monolithic-split]] — canonical before/after refactoring example

[Source: 14-qFYN8XYMTZM.md, 16-iOvyCv_rwac.md]
