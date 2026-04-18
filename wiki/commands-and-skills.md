---
title: Custom /commands and Skills — T3.2 Hub
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Claude Code can be extended with custom /commands and skills. Both standardize team workflows, but they work differently. The exam tests when to use which, how to configure each, and what anti-patterns to avoid.

> **Mental model: commands = keyboard shortcuts. Skills = plugins.**

## /commands

Markdown files that define a repeatable workflow. A team member types `/review` → Claude reads `review.md` and executes the instructions inside. The file can be a numbered checklist, a rule set, or any workflow definition.

Two scopes — same pattern as [[claude-md-levels]]:
- **Project-level** `.claude/commands/` — committed to git, shared with whole team
- **User-level** `~/.claude/commands/` — personal, never shared

See [[custom-commands]] for full detail.

## Skills

More powerful than commands. Live in `.claude/skills/` and use a `skill.md` file with YAML frontmatter that configures how the skill runs.

Three exam-critical frontmatter fields:

| Field | Purpose | Most important? |
|-------|---------|-----------------|
| `context:fork` | Run in isolated sub-agent; keep main context clean | Yes — most tested |
| `allowed_tools` | Least privilege; restrict which tools the skill can use | Yes |
| `argument_hint` | Prompt developer for input when no argument is provided | Yes |

See [[skills]] for full detail. See [[context-fork]] and [[skills-allowed-tools]] for deep dives.

## skills vs claude.md Decision

| | claude.md | Skills |
|---|-----------|--------|
| When loaded | Always — every session | On demand — explicitly invoked |
| Use for | Universal standards | Task-specific workflows |
| Example | "Always use TypeScript strict mode" | `/analyze`, `/deploy-check` |

See [[skills-vs-claude-md]] for the full decision framework.

## Anti-Patterns

See [[commands-skills-anti-patterns]] for the five exam-tested wrong answers.

## Exam Mnemonic — 6 Takeaways

1. `/commands` in `.claude/commands` = shared team shortcuts (committed); `~/.claude/commands` = personal
2. Skills in `.claude/skills` — `skill.md` with YAML frontmatter: `context:fork`, `allowed_tools`, `argument_hint`
3. `context:fork` isolates skill in a sub-agent; main conversation gets final result only
4. `allowed_tools` restricts tool access during skill execution — least privilege
5. Skills = on demand; claude.md = always loaded; universal standards go in claude.md
6. Personal skill variants need different names — `my-review` not `review` — to avoid overriding team skills

## Connections

- [[claude-md-hierarchy]] — T3.1; commands/skills use the same user/project scoping as CLAUDE.md
- [[minimum-tools-principle]] — D1 T1.3; `allowed_tools` in skills extends this principle to the skill layer
- [[tool-scoping]] — D2 T2.3; least privilege per role; `allowed_tools` is the same concept for skills
- [[fork-sessions]] — D1 T1.3/T1.7; `context:fork` is the skill-layer equivalent of session forking
- [[isolated-context]] — D1 T1.2; sub-agents start fresh; `context:fork` creates the same isolation

[Source: 15-AaGMyP2hBRY.md]
