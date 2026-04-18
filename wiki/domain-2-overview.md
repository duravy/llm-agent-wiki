---
title: Domain 2 — Tool Design and MCP Integration
created: 2026-04-16
last_updated: 2026-04-16
source_count: 5
status: draft
---

Domain 2 covers how agents are equipped with tools — how those tools are described, how errors are communicated, how tools are distributed across agents, how external servers are integrated, and which built-in tools to use in which situation. It makes up **20% of the exam** — one in five questions.

> "Domain one was about how agents think and orchestrate. Domain two is about the tools agents use."

## Known Tasks

| Task | Topic | Wiki Page | Source Part |
|------|-------|-----------|-------------|
| 2.1 | Designing Effective Tool Interfaces | [[tool-description-design]], [[five-elements-tool-description]], [[tool-misrouting]], [[generic-tool-splitting]], [[system-prompt-keyword-trap]], [[tool-naming-strategies]], [[tool-description-anti-patterns]] | Part 9 |
| 2.2 | Structured Error Responses | [[mcp-error-responses]], [[mcp-error-categories]], [[mcp-error-fields]], [[is-error-flag]], [[access-failure-vs-empty-result]], [[local-error-recovery]], [[mcp-error-anti-patterns]] | Part 10 |
| 2.3 | Distributing Tools Across Agents | [[tool-distribution]], [[too-many-tools-problem]], [[tool-scoping]], [[crossrole-tools]], [[tool-choice-modes]], [[constrained-tool-alternatives]], [[tool-distribution-anti-patterns]] | Part 11 |
| 2.4 | Integrating MCP Servers | [[mcp-server-integration]], [[mcp-configuration-scopes]], [[environment-variable-expansion]], [[mcp-resources]], [[community-vs-custom-servers]], [[mcp-tool-description-fix]], [[mcp-integration-anti-patterns]] | Part 12 |
| 2.5 | Selecting Built-In Tools Effectively | [[builtin-tools]], [[grep-vs-glob]], [[two-phase-search-strategy]], [[edit-vs-write]], [[incremental-exploration]], [[builtin-tools-anti-patterns]] | Part 13 |

> Domain 2 is now complete. All 5 tasks (T2.1–T2.5) confirmed across Parts 9–13.

## Domain Introduction (from Part 9)

"How you design, describe, distribute, and integrate tools determines whether an agent picks the right action or fumbles."

The domain builds task by task:
- T2.1 establishes the foundational principle: the description is the only signal Claude uses
- T2.2 handles what happens when tools break
- T2.3 addresses tool scoping across a multi-agent system
- T2.4 connects the system to external tool servers via MCP
- T2.5 covers Claude Code's built-in tool selection judgments

## Key Exam Signal

- 20% of exam = every fifth question is from this domain
- First fix for any misrouting scenario: improve the tool description (T2.1)
- Least privilege for tool distribution is an explicit exam concept (T2.3)

[Source: 09-TKmhUOlzM6w.md, 10-cMCoA_MGDdw.md, 11-K-HRtSQLGfU.md, 12-NlTC0XY8orw.md, 13-IN2WntoHedY.md]
