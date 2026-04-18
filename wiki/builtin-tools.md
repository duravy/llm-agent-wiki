---
title: Built-In Tools (D2 T2.5)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Claude Code ships with six built-in tools for working with code. Each has a specific strength — picking the wrong one wastes time or produces poor results. This is Domain 2, Task 2.5.

## The Six Tools

| Tool | What it does | Use when |
|------|-------------|----------|
| **Grep** | Searches file *contents* for text or regex patterns | You know what the code *contains* |
| **Glob** | Finds files by *name or path* pattern | You know the file naming pattern |
| **Read** | Loads a full file into context | You know which file you need |
| **Write** | Creates a new file or completely overwrites one | Creating new files or full rewrites |
| **Edit** | Targeted text replacement (old_string → new_string) | Changing one specific part of an existing file |
| **Bash** | Runs shell commands | Build, test, install, and git operations only |

> "Think of them as specialized instruments in a workshop. You would not use a hammer to turn a screw."

## Most-Tested Distinction

**Grep vs Glob** — see [[grep-vs-glob]]. These are the single most-confused pair on the exam.

## Core Patterns

- **Discovery strategy**: [[two-phase-search-strategy]] — Grep for entry points, then Read
- **Modification decisions**: [[edit-vs-write]] — targeted Edit vs full-file Write; Edit fallback pattern
- **Codebase exploration**: [[incremental-exploration]] — follow imports step by step, never read everything upfront

## Six Exam Takeaways (Know Cold)

1. Grep = file contents; Glob = file names/paths; never confuse them
2. Search strategy: Grep first for entry points, then Read selectively; never read everything upfront
3. Edit = targeted changes with unique match; Write = new files or complete rewrites
4. Edit fallback when old_string isn't unique: Read full file → Write modified version back
5. Incremental exploration: follow imports and function calls step by step; each discovery narrows the next search
6. Bash = build, test, install, git — not file search; Grep and Glob are the discovery tools

## Anti-Patterns

See [[builtin-tools-anti-patterns]] for the five exam-tested wrong choices.

[Source: 13-IN2WntoHedY.md]
