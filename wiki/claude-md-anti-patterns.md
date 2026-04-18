---
title: CLAUDE.md Anti-Patterns (T3.1)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five exam-tested anti-patterns for Task 3.1. These are the wrong answer choices on the exam.

## Anti-pattern 1: Massive Monolithic CLAUDE.md

**Wrong**: One enormous CLAUDE.md file covering testing, API design, deployment, code style, and more — hundreds or thousands of lines.

**Why it's wrong**: Hard to maintain. Irrelevant rules dilute the important ones. Claude reads the whole file every session regardless of what task is being performed.

**Correct approach**: Split into focused files using the [[at-import-syntax]] or [[rules-directory]]. The root file becomes a ~50-line index with @import references. See [[monolithic-split]].

---

## Anti-pattern 2: Team Conventions in User-Level Config

**Wrong**: Placing team coding standards in `~/.claude/claude.md` (user level).

**Why it's wrong**: User-level config is personal and not shared via git. Other team members never receive those instructions. You may follow the convention but your teammates won't.

**Correct approach**: Team conventions belong in project-level `.claude/claude.md`, committed to version control. See [[claude-md-levels]].

---

## Anti-pattern 3: Personal Preferences in Project-Level Config

**Wrong**: Placing your personal coding style preferences in the committed project CLAUDE.md.

**Why it's wrong**: This imposes your personal style on everyone who works in the repo — a pattern the exam explicitly flags.

**Correct approach**: Personal preferences belong in `~/.claude/claude.md` (user level), which is not committed. See [[claude-md-levels]].

---

## Anti-pattern 4: No Modular Organization

**Wrong**: Everything crammed into one file with no @import or rules directory structure, even when the file has grown to cover multiple concerns.

**Why it's wrong**: Same as Anti-pattern 1 — maintenance and dilution problems. The exam specifically calls out the absence of modular organization.

**Correct approach**: Use [[at-import-syntax]] or [[rules-directory]] to keep each topic in its own focused file.

---

## Anti-pattern 5: Not Checking /memory

**Wrong**: When Claude ignores rules, concluding that Claude is broken without checking which files are actually loaded.

**Why it's wrong**: Guessing about what's loaded wastes time and leads to the wrong fix. The actual cause is almost always a configuration issue — wrong level, bad @import path, YAML syntax error.

**Correct approach**: Always verify with [[memory-command]] (`/memory`) before concluding something is broken.

---

## Summary Table

| # | Anti-pattern | Correct approach |
|---|-------------|-----------------|
| 1 | Monolithic CLAUDE.md | Split into focused files via @import |
| 2 | Team conventions at user level | Move to project level (committed) |
| 3 | Personal preferences at project level | Move to user level (not committed) |
| 4 | No modular organization | Use @import or .claude/rules directory |
| 5 | Not checking /memory | Run /memory first |

[Source: 14-qFYN8XYMTZM.md]
