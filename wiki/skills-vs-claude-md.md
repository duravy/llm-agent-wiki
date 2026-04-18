---
title: Skills vs claude.md — Decision Framework
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The decision of whether to put something in `claude.md` or define it as a skill comes down to one question: should this always apply, or only when explicitly invoked?

## The Rule

```
Should this apply to every interaction automatically?
  Yes → claude.md

Does a developer need to invoke this explicitly when they need it?
  Yes → skill
```

## Comparison

| | claude.md | Skills |
|---|-----------|--------|
| **When active** | Always — loaded every session automatically | On demand — invoked explicitly by name |
| **Use for** | Universal standards | Task-specific workflows |
| **Examples** | "Always use TypeScript strict mode", "Always write tests before implementation" | `/analyze` (deep code review), `/deploy-check` (pre-release verification) |
| **Forgetting to invoke** | Not possible — always active | Developer must remember to run it |

## Examples

**Belongs in claude.md:**
- "Always use TypeScript strict mode"
- "Always write tests before implementation"
- "Never commit secrets to version control"
- "Use camelCase for variable names"

These should apply to every interaction, automatically. No developer should need to remember to invoke them.

**Belongs in a skill:**
- `/analyze` — deep codebase analysis before a refactor
- `/deploy-check` — pre-release verification checklist
- `/review` — thorough code review of a PR
- `/audit-deps` — security audit of dependencies

These are triggered only in specific situations. The developer explicitly invokes them when needed.

## The Exam Anti-Pattern

> "Universal standards in skills instead of claude.md"

If a rule should apply to every interaction, putting it in a skill means developers must remember to invoke it. Some will forget. The rule doesn't actually apply universally. This is exam-tested anti-pattern #4 (see [[commands-skills-anti-patterns]]).

## Exam Signal

- "Should always apply" → claude.md
- "Invoked when needed" → skill
- "Developer must remember to invoke it" → wrong tool (anti-pattern 4)
- "Universal standard in a skill" = exam-tested anti-pattern

## Connections

- [[commands-and-skills]] — T3.2 hub page
- [[skills]] — skill configuration details
- [[claude-md-hierarchy]] — T3.1; what goes in claude.md and why

[Source: 15-AaGMyP2hBRY.md]
