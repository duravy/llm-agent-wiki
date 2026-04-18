---
title: MCP Integration Anti-Patterns (D2 T2.4)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five exam-tested anti-patterns for MCP server integration. These are the wrong-answer choices on the exam for Task 2.4.

## Summary Table

| # | Anti-pattern | Correct approach |
|---|-------------|-----------------|
| 1 | Hardcoding secrets in `mcp.json` | `${VAR}` environment variable expansion |
| 2 | Building custom servers for standard integrations | Use community servers (Jira, GitHub, Slack, PostgreSQL) |
| 3 | No MCP resources for content catalogs | Expose resource catalogs to eliminate exploratory calls |
| 4 | Storing shared team config at user level | Project-level `mcp.json`, committed to git |
| 5 | Minimal MCP tool descriptions, Claude skips capable tool | Add tool preference guidance to CLAUDE.md |

## Detailed Breakdown

**Anti-pattern 1 — Hardcoding secrets in `mcp.json`**
`mcp.json` is committed to version control. A literal token inside it is exposed to everyone who can read the repo — contributors, CI systems, anyone who clones it. Use `${GITHUB_TOKEN}` instead. See [[environment-variable-expansion]].

**Anti-pattern 2 — Building custom servers for standard integrations**
Community servers already handle Jira, GitHub, and Slack with battle-tested edge case handling. Reinventing them wastes time and produces worse results. Build custom only when the workflow is truly unique and no community option covers it. See [[community-vs-custom-servers]].

**Anti-pattern 3 — No MCP resources for content catalogs**
Without resources, the agent makes repeated exploratory tool calls to discover what's available — guessing category names, searching for terms it doesn't know. Resources expose catalogs upfront (issue categories, database schemas, documentation outlines) so the agent can act precisely on the first call. See [[mcp-resources]].

**Anti-pattern 4 — Storing shared team MCP config at user level**
User-level config (`~/.claude.json`) stays on your machine. Teammates never receive the tools because it is never committed to version control. Shared tooling must live in project-level `mcp.json`, committed to the repo. See [[mcp-configuration-scopes]].

**Anti-pattern 5 — Minimal MCP tool descriptions causing Claude to skip the capable MCP tool**
Claude selects tools based on descriptions. A weak MCP tool description loses to a strong built-in description (e.g., Grep) even when the MCP tool is more capable. Fix: add tool preference guidance to `CLAUDE.md`. Do not modify the community server itself — `CLAUDE.md` is the correct fix layer. See [[mcp-tool-description-fix]].

## 5-Step Exam Checklist

1. Are API tokens in `${VAR}` references? (not literal strings)
2. Is this a standard integration? → use community server, not custom
3. Do content-heavy workflows expose resource catalogs? (not rely on exploratory calls)
4. Is shared team config in project-level `mcp.json`? (not user-level `~/.claude.json`)
5. Do weak MCP tool descriptions get addressed via `CLAUDE.md`? (not server modification)

[Source: 12-NlTC0XY8orw.md]
