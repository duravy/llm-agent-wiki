---
title: MCP Server Integration (D2 T2.4)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

MCP (Model Context Protocol) is a standard that lets Claude connect to external services through a uniform interface. Once connected, Claude auto-discovers all tools the server provides simultaneously — no manual declaration in code. This is Domain 2, Task 2.4.

## The USB Analogy

| Before | After |
|--------|-------|
| Every device had its own proprietary connector | Every AI tool integration required custom code |
| USB standardized the interface | MCP standardizes the interface |
| Plug in once, device works | Connect once, all tools available |

Build or configure a server once → Claude discovers and uses all its tools automatically at connection time. One standard, any service.

## Regular Tools vs MCP Tools — Exam-Tested Distinction

| Dimension | Regular Tools (D1 T1.1) | MCP Server Tools (D2 T2.4) |
|-----------|------------------------|---------------------------|
| **Defined by** | Your code (JSON schemas) | The MCP server |
| **Executed by** | Your code | The MCP server |
| **Discovery** | Listed explicitly in code | Auto-discovered at connection time |
| **Declaration** | You list every tool manually | Never declared manually |
| **Access** | One tool at a time as needed | All server tools available simultaneously |

Key exam point: when Claude Code connects to an MCP server, all tools that server provides become available simultaneously — one connection, complete tool set, no repeated declarations.

## Core Sub-Concepts

| Concept | Page | Exam weight |
|---------|------|-------------|
| Two configuration scopes | [[mcp-configuration-scopes]] | Common exam question |
| Environment variable expansion | [[environment-variable-expansion]] | Secrets rule |
| MCP resources | [[mcp-resources]] | Exploratory call reduction |
| Community vs custom servers | [[community-vs-custom-servers]] | Default decision |
| Tool description fix | [[mcp-tool-description-fix]] | CLAUDE.md as fix layer |
| Anti-patterns | [[mcp-integration-anti-patterns]] | Wrong answer choices |

## Six Exam Takeaways (Know Cold)

1. Project-level `mcp.json` = shared team tooling, committed to git
2. User-level `~/.claude.json` = personal, not shared with teammates
3. `${VAR}` syntax keeps actual secrets out of config files; `mcp.json` is safe to commit
4. Claude auto-discovers all tools from a connected MCP server simultaneously — never declare manually
5. MCP resources = read-only catalogs; reduce exploratory tool calls by giving upfront visibility
6. Enhance minimal MCP tool descriptions via `CLAUDE.md` guidance — configuration is the correct fix layer, not server modification

## Exam Signal

- "New team member doesn't have tools" → user-level config, should be project-level
- "All tools available at once" → MCP auto-discovery, not regular tools
- "Fix weak MCP description" → CLAUDE.md guidance, not server modification
- "Standard integration, no community server" → exam trap; community servers cover Jira, GitHub, Slack, PostgreSQL

[Source: 12-NlTC0XY8orw.md]
