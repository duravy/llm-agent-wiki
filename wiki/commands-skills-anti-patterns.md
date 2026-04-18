---
title: /commands and Skills Anti-Patterns (T3.2)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five exam-tested anti-patterns for Task 3.2. These are the wrong answer choices on the exam.

## Anti-pattern 1: Team Commands in User-Level Location

**Wrong**: Placing team workflow commands in `~/.claude/commands/`.

**Why it's wrong**: User-level commands are personal and never committed to git. Teammates never receive them.

**Correct approach**: Team workflow commands belong in `.claude/commands/`, committed to git. See [[custom-commands]].

---

## Anti-pattern 2: Skills Without context:fork for Verbose Operations

**Wrong**: A skill that performs deep codebase analysis, exploration, or brainstorming runs without `context:fork`.

**Why it's wrong**: Without isolation, the skill's 50+ tool calls and verbose intermediate output fill the main conversation context window before real work starts.

**Correct approach**: Add `context:fork` to any skill that explores, analyzes, or brainstorms. See [[context-fork]].

---

## Anti-pattern 3: Skills with Full Tool Access for Read-Only Tasks

**Wrong**: An analysis skill that only needs to read code is given unrestricted tool access (no `allowed_tools`).

**Why it's wrong**: Without restriction, the skill can call `edit`, `write`, and `bash` — risking accidental file modification or dangerous shell commands.

**Correct approach**: Always set `allowed_tools`. A read-only skill should declare `[read, grep, glob]` only. See [[skills-allowed-tools]].

---

## Anti-pattern 4: Universal Standards in Skills Instead of claude.md

**Wrong**: A rule that should apply to every interaction (e.g., "always use TypeScript strict mode") is put in a skill instead of `claude.md`.

**Why it's wrong**: Skills are invoked on demand. Developers must remember to run them. Some will forget. The standard doesn't actually apply universally.

**Correct approach**: Universal standards belong in `claude.md` (always loaded). Task-specific workflows go in skills. See [[skills-vs-claude-md]].

---

## Anti-pattern 5: Personal Skill Variants with Same Name as Team Skills

**Wrong**: Creating `~/.claude/skills/review.md` when the team already has `.claude/skills/review.md`.

**Why it's wrong**: Your personal variant overrides the team version for you specifically. You silently diverge from the team workflow without realizing it.

**Correct approach**: Use a different name for personal variants — `my-review` instead of `review`. This way your customization doesn't override or hide the team skill.

---

## Summary Table

| # | Anti-pattern | Correct approach |
|---|-------------|-----------------|
| 1 | Team commands at user level | Move to `.claude/commands/` (committed) |
| 2 | Verbose skill without `context:fork` | Add `context:fork` |
| 3 | Read-only skill with full tool access | Restrict with `allowed_tools: [read, grep, glob]` |
| 4 | Universal standards in skills | Move to claude.md |
| 5 | Personal skill with same name as team skill | Rename personal variant (`my-review`) |

[Source: 15-AaGMyP2hBRY.md]
