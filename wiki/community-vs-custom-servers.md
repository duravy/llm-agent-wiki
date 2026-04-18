---
title: Community vs Custom MCP Servers
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

When adding MCP capability, there are two options: use an existing community server or build a custom one. The exam has a clear default answer.

## The Decision Framework

| Option | When to use | Examples |
|--------|-------------|---------|
| **Community server** | Standard integration that a community server already covers | Jira, GitHub, Slack, PostgreSQL |
| **Custom server** | Team-specific workflow with no community option | Internal deployment pipeline, custom CRM, proprietary data source |

## The Default Answer

> **Use community servers first. Only build custom when the workflow is truly unique and no community option exists.**

Community servers are:
- **Battle-tested** — handle edge cases you haven't thought of yet
- **Maintained** — bugs and API changes handled by maintainers
- **Comprehensive** — cover the full feature surface, not just the happy path

## The Anti-Pattern

Building a custom Jira connector when a community one exists is an explicit exam-tested anti-pattern. Reinventing them wastes time and produces worse results — the community version has already solved problems you'll encounter.

## Decision Rule

```
Does a community server exist for this integration?
  Yes → use the community server
  No  → is the workflow truly unique and team-specific?
          Yes → build a custom server
          No  → reconsider whether you need MCP at all
```

## Exam Signal

- "Standard integration, no community server" → exam trap; Jira, GitHub, Slack, PostgreSQL all have community servers
- Default answer: community servers first
- Custom = truly unique workflow only; no community option is the required condition
- Building custom for standard integrations = [[mcp-integration-anti-patterns]] Anti-pattern 2

[Source: 12-NlTC0XY8orw.md]
