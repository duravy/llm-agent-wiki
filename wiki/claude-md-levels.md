---
title: CLAUDE.md — Three Levels and Priority
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

CLAUDE.md exists at three distinct levels. Each level has a different location, sharing model, and priority. The exam tests whether you know which level controls what and what happens when levels conflict.

## The Three Levels

### User Level (Lowest Priority)
- **Location**: `~/.claude/claude.md`
- **Shared via git**: No — personal to your machine
- **Use for**: Your own preferences, personal shortcuts, individual coding style
- **Who gets it**: Only you

### Project Level (Middle Priority)
- **Location**: `.claude/claude.md` or in the project root
- **Shared via git**: Yes — committed and shared with the whole team
- **Use for**: Team coding standards, project conventions, shared tool preferences
- **Who gets it**: Everyone who clones the repo

### Subdirectory Level (Highest Priority)
- **Location**: Inside a specific subdirectory, e.g. `packages/billing/claude.md`
- **Shared via git**: Yes — committed like project level
- **Use for**: Package-specific overrides — e.g., billing has different deployment rules than the root
- **Loaded when**: Claude is working inside that subdirectory only
- **Who gets it**: Everyone, but only in context of that directory

## Priority Rule

> **Most specific level wins.**

```
Subdirectory > Project > User
```

When instructions conflict between levels, the subdirectory-level CLAUDE.md overrides the project-level, which overrides the user-level.

## Exam Scenario Pattern

> "A new team member joined but isn't following the expected coding conventions. What's wrong?"

**Answer**: The config is at user level instead of project level. User-level CLAUDE.md is personal and not shared via git — teammates never receive those instructions.

**Fix**: Move the relevant instructions to the project-level `.claude/claude.md` and commit it.

## Decision Rule

```
Is this for the whole team?
  Yes → project level (.claude/claude.md, committed)
  
Is this just for me?
  Yes → user level (~/.claude/claude.md, not committed)
  
Is this for one specific package or directory?
  Yes → subdirectory level (packages/xyz/claude.md, committed)
```

## Exam Signal

- User level = personal, not shared via git = team never gets it
- Project level = committed = shared with all team members
- Subdirectory level = highest priority = most specific wins
- "Team conventions in user level" = exam-tested anti-pattern (see [[claude-md-anti-patterns]])
- "Personal preferences in project level" = exam-tested anti-pattern (imposes on everyone)

## Connections

- [[claude-md-hierarchy]] — hub page for T3.1
- [[mcp-configuration-scopes]] — MCP config has the same user/project split; user level = personal, project level = shared
- [[custom-commands]] — D3 T3.2; /commands use the identical user/project scoping (`.claude/commands/` vs `~/.claude/commands/`)
- [[skills]] — D3 T3.2; skills use the same scoping; personal skill variants override team skills if given the same name

[Source: 14-qFYN8XYMTZM.md, 15-AaGMyP2hBRY.md]
