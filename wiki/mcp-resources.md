---
title: MCP Resources — Read-Only Content Catalogs
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

MCP servers can expose **resources** in addition to tools. Resources are read-only data catalogs that give the agent upfront visibility into what's available — eliminating the need for repeated exploratory tool calls.

## The Core Problem Resources Solve

Without resources, an agent has to discover available content by guessing:

```
tool_call: search_issues(query="authentication")  → some results
tool_call: search_issues(query="auth")             → different results
tool_call: search_issues(query="login")            → more results
# Agent is guessing category names it doesn't know
```

With resources, the MCP server exposes a catalog upfront:

```
resource_read: issue_categories → ["authentication", "billing", "onboarding", "performance"]
tool_call: get_issues(category="authentication")   → precise, correct result
# One precise call — no guessing
```

## What Resources Expose

- Issue summaries and category lists
- Database schemas
- Documentation outlines
- Available report types
- Any read-only catalog of available content

## Key Distinction from Tools

| | Tools | Resources |
|--|-------|-----------|
| **Purpose** | Execute actions | Expose data catalogs |
| **Access pattern** | Called when needed | Read upfront before acting |
| **Effect** | Can modify state | Read-only only |
| **Reduces** | — | Exploratory tool calls |

## Exam Signal

- "Agent makes repeated exploratory tool calls to find what's available" → missing MCP resources
- Resources = read-only catalogs; reduce guessing before action
- "Exposes an issue categories catalog up front" → canonical resource example
- No MCP resources for content catalogs = exam anti-pattern 3 (see [[mcp-integration-anti-patterns]])
- Resources give upfront visibility; tools execute once the agent knows what to do

[Source: 12-NlTC0XY8orw.md]
