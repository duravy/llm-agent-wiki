---
title: CLAUDE.md Hierarchy — T3.1 Hub
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

`CLAUDE.md` is a configuration file that gives Claude Code persistent instructions. Every time Claude Code starts a session, it reads CLAUDE.md and follows the instructions inside. Think of it as a job description handed to a new team member: it describes coding standards, project conventions, and how things work around here.

CLAUDE.md does not live in just one place. It exists at three levels with different scopes, audiences, and priorities.

## The Three Levels

See [[claude-md-levels]] for full detail.

| Level | Location | Shared via git? | Priority |
|-------|----------|-----------------|----------|
| User | `~/.claude/claude.md` | No | Lowest |
| Project | `.claude/claude.md` or project root | Yes | Middle |
| Subdirectory | e.g. `packages/billing/claude.md` | Yes | **Highest** |

**Key rule: when levels conflict, the most specific level wins.**

## Modular Organization

Two mechanisms keep large CLAUDE.md files manageable:

1. **@import syntax** — explicit references in the root file pointing to focused rule files; always loads unconditionally; see [[at-import-syntax]]
2. **.claude/rules directory** — files placed here are auto-loaded without any explicit import; supports a `paths:` YAML frontmatter field for conditional loading; see [[rules-directory]]

Key distinction: `@import` is always unconditional. `.claude/rules/` files with a `paths:` field load **only when the active file matches the glob patterns** (T3.3 — see [[path-specific-rules]]).

See [[monolithic-split]] for the canonical before/after pattern (800-line file → 50-line root + 4 focused files).

## Debugging

When Claude ignores expected rules, use the [[memory-command]] (`/memory`) to see exactly which configuration files are loaded in the current session. The most common cause: config is at user level when it should be project level.

## Anti-Patterns

See [[claude-md-anti-patterns]] for the five exam-tested wrong answer choices.

## Exam Mnemonic — 8 Takeaways

1. Three levels: user (`~/.claude`) → project (`.claude` or root) → subdirectory (inside a specific dir); each has different scope and sharing
2. User level = personal, not shared via git; project level = shared with team; subdirectory = most specific, highest priority
3. @import references external files; each path is relative to the importing file; always loads unconditionally
4. `.claude/rules` directory holds topic-specific files loaded automatically — no explicit import needed
5. `.claude/rules/` files with a `paths:` YAML field load conditionally — only when the active file matches the glob patterns (T3.3)
6. Split a monolithic CLAUDE.md into focused files; the root becomes a 50-line index with @import references
7. `/memory` verifies which config files are actually loaded; use it before concluding Claude is ignoring your rules
8. Team member missing expected conventions? Check if config is at user level instead of project level — the most common misconfiguration

## Connections

- [[mcp-tool-description-fix]] — CLAUDE.md is already used as the fix layer for weak MCP descriptions; T3.1 gives the full hierarchy behind that pattern
- [[mcp-configuration-scopes]] — MCP config also has a user/project split; same design principle, different config files
- [[path-specific-rules]] — T3.3 builds on T3.1; the `paths:` field in `.claude/rules/` adds conditional loading on top of modular organization

[Source: 14-qFYN8XYMTZM.md, 16-iOvyCv_rwac.md]
