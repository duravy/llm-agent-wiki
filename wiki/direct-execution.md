---
title: Direct Execution — When to Use It
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Direct execution means Claude reads your request and immediately starts making changes — no planning step, no waiting for approval. It is the right choice when the scope is clear and the approach is obvious.

## The Rule of Thumb

> **If you can describe the change in one sentence and there's only one obvious way to do it, use direct execution. If you need to discuss options first, use plan mode.**

## The Four Scenarios for Direct Execution

### 1. Well-Defined Changes
The task is specific with no architectural choices. "Add input validation to the `createUser` function" — there is a clear target, a clear action, and no decision to make.

### 2. Single File or Small Scope
A bug with a clear stack trace pointing to one location. The scope is already known. No exploration required.

### 3. No Architectural Decision
"Add a date validation conditional" has one obvious implementation path. There is nothing to choose between.

### 4. Known Approach
Converting a callback to async/await is a **mechanical transformation** — the steps are well-known and deterministic. No design judgment is needed.

## Decision Signals

Use direct execution when:
- You can scope the change in one sentence
- There is only one obvious implementation
- The task is single-file or small multi-file
- The approach is mechanical / well-established
- A bug has a clear stack trace

**Exam signals**: adding validation, fixing a bug with a clear stack trace, callback → async/await conversion.

## The Cost of Misuse

Using direct execution on a task that requires planning risks:
- Discovering dependency problems mid-execution
- Leaving the codebase in a broken intermediate state
- Hours of costly undo work

If the task passes the first check in the decision framework — "well-defined and single-file?" — direct execution is safe. If not, start with [[plan-mode]].

## Connections

- [[plan-mode-vs-direct-execution]] — hub page; decision framework
- [[plan-mode]] — the other mode; when to use each
- [[plan-mode-anti-patterns]] — anti-pattern #2 is plan mode for tasks that should be direct execution (unnecessary overhead)

[Source: 17-96v5N5YGMPM.md]
