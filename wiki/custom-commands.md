---
title: Custom /commands
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Custom /commands are markdown files that define repeatable team workflows. When a team member types `/review`, Claude reads `review.md` and follows the instructions inside. The file is plain markdown — a numbered checklist, a set of rules, whatever the workflow requires.

## How Commands Work

1. Create a markdown file with the workflow instructions
2. Place it in the correct commands directory
3. Team members invoke it by typing `/` + the filename (without `.md`)

The file content is completely flexible — a code review checklist, a deployment verification procedure, a test-run sequence.

## Scoping: Project vs User

| Scope | Location | Version controlled? | Who gets it? |
|-------|----------|-------------------|--------------|
| **Project-level** | `.claude/commands/` | Yes — committed to git | Everyone who clones the repo |
| **User-level** | `~/.claude/commands/` | No — personal only | You only |

This is the **same scoping pattern as [[claude-md-levels]]**. The exam tests this explicitly.

**Use project-level for**: standard team workflows — code review, deploy checks, test runs  
**Use user-level for**: your own personal shortcuts

## Exam Question Pattern

> "A team member typed `/deploy-check` but the command wasn't available. What should you check?"

**Answer**: The command file is in `~/.claude/commands/` (user-level) instead of `.claude/commands/` (project-level). User-level commands are personal and never committed to git — teammates never receive them.

**Fix**: Move the command file to `.claude/commands/` and commit it.

## Example

```markdown
<!-- .claude/commands/review.md -->
# Code Review Checklist

1. Check that all public functions have docstrings
2. Verify no hardcoded credentials or API keys
3. Confirm test coverage for any new logic
4. Check error handling at system boundaries
5. Verify no unused imports or dead code
```

Team members invoke this with `/review`.

## Commands vs Skills

Commands are simpler and have no configuration options — they're pure markdown instructions. For more powerful workflows that need isolated execution or tool restrictions, use [[skills]] instead.

| Feature | /commands | Skills |
|---------|-----------|--------|
| Format | Plain markdown | Markdown + YAML frontmatter |
| Isolated execution | No | Yes (`context:fork`) |
| Tool restrictions | No | Yes (`allowed_tools`) |
| Best for | Simple repeatable workflows | Verbose or risky operations |

## Exam Signal

- Project-level = `.claude/commands/` + committed = shared
- User-level = `~/.claude/commands/` + not committed = personal
- "Command not available to teammates" = wrong scope (see [[commands-skills-anti-patterns]] Anti-pattern 1)

## Connections

- [[commands-and-skills]] — T3.2 hub page
- [[claude-md-levels]] — same user/project scoping pattern
- [[skills]] — more powerful alternative for complex workflows

[Source: 15-AaGMyP2hBRY.md]
