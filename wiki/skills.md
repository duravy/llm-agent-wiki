---
title: Skills — Isolated Plugin Workflows
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Skills are more powerful than [[custom-commands]]. They are isolated plugins with their own permissions, their own execution context, and their own configuration. They live in `.claude/skills/` and use a `skill.md` file with YAML frontmatter.

> **Mental model: commands = keyboard shortcuts. Skills = plugins.**

## Anatomy of a Skill File

```yaml
---
context: fork
allowed_tools: [read, grep, glob]
argument_hint: "Enter the module name to analyze"
---

# Deep Module Analysis

Analyze the specified module:
1. Map all imports and dependencies
2. Identify public API surface
3. Flag any circular dependencies
4. Summarize key patterns used
```

## The Three Exam-Critical Frontmatter Fields

### 1. `context:fork` — Most Important

Runs the skill in an isolated sub-agent. The main conversation only receives the final result.

See [[context-fork]] for full detail.

### 2. `allowed_tools` — Least Privilege

Restricts which tools the skill can access during execution.

```yaml
allowed_tools: [read, grep, glob]   # read-only analysis skill
```

Without this, a skill intended only to read code has access to `edit`, `write`, and `bash` — and could accidentally modify files or run dangerous commands. See [[skills-allowed-tools]].

### 3. `argument_hint` — Developer UX

Prompts the developer for input when they invoke the skill without an argument.

```yaml
argument_hint: "Enter the PR number to review"
```

Without this, invoking `/analyze` with no argument produces a confusing blank prompt.

## Project vs User Scope

Skills follow the same scoping as commands and CLAUDE.md:
- **Project-level**: `.claude/skills/` — committed, shared with team
- **User-level**: `~/.claude/skills/` — personal, not shared

**Warning**: If you create a personal skill in `~/.claude/skills/` with the same name as a team skill, your version overrides the team version for you. Use a different name (`my-review` not `review`). See [[commands-skills-anti-patterns]] Anti-pattern 5.

## When to Use a Skill vs claude.md

See [[skills-vs-claude-md]] for the full decision framework. The short rule:
- Skills are **on demand** — invoked explicitly; use for task-specific workflows
- claude.md is **always loaded** — use for universal standards

## Exam Signal

- Skill file = `skill.md` with YAML frontmatter in `.claude/skills/`
- Three key frontmatter fields: `context:fork`, `allowed_tools`, `argument_hint`
- `context:fork` = most tested; prevents context pollution from verbose skills
- `allowed_tools` = least privilege; required for read-only skills
- Personal variant with same name = overrides team skill (see anti-patterns)

## Connections

- [[commands-and-skills]] — T3.2 hub page
- [[context-fork]] — deep dive on isolated sub-agent execution
- [[skills-allowed-tools]] — deep dive on least privilege tool restriction
- [[skills-vs-claude-md]] — decision framework
- [[minimum-tools-principle]] — D1 T1.3 root concept that `allowed_tools` extends
- [[isolated-context]] — D1 T1.2; sub-agents start fresh; `context:fork` creates same isolation

[Source: 15-AaGMyP2hBRY.md]
