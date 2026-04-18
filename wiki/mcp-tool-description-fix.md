---
title: Fixing Weak MCP Tool Descriptions via CLAUDE.md
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Community MCP servers sometimes have minimal tool descriptions. Since Claude selects tools based on descriptions, a weak MCP tool description loses to a strong built-in description — even when the MCP tool is more capable. The fix is configuration, not code modification.

## The Problem

Claude evaluates which tool to use by reading descriptions. If a community MCP server's tool has a vague description, and a built-in tool (like Grep) has a detailed description, Claude chooses the built-in — even if the MCP tool is more capable for the task.

```
MCP tool description: "Search the codebase"
Grep description:     "Search file contents using regex patterns..."

Claude picks Grep — even if the MCP tool understands semantic relationships 
that Grep doesn't.
```

## The Fix: CLAUDE.md Guidance

The correct fix is to add tool preference guidance to your `CLAUDE.md` file:

```markdown
# Tool Preferences

When searching for code patterns, prefer the codebase-search MCP tool over Grep 
because it understands semantic relationships like function calls and class hierarchies.
```

This steers Claude's tool selection without modifying the server.

## Why Not Modify the Server?

- Community servers are maintained by third parties — modifying them means maintaining a fork
- Configuration changes (CLAUDE.md) are versioned with your project
- The fix should live at the layer you own: your project configuration

## The Correct Fix Layer

```
Weak MCP tool description
  → Don't: modify the community server
  → Do: add tool preference guidance to CLAUDE.md
```

This is what the exam means by "configuration is the correct layer for this fix."

## Connection to D3 T3.1 CLAUDE.md Hierarchy

CLAUDE.md is used here as the fix layer for weak MCP tool descriptions. D3 T3.1 explains the full hierarchy behind this: CLAUDE.md exists at three levels (user, project, subdirectory), and the project level is the correct scope for team-wide tool preferences. When you add tool preference guidance to CLAUDE.md, it should be at project level so all team members benefit. See [[claude-md-hierarchy]] for the three-level structure.

## Connection to T2.1 Tool Descriptions

This is the same root cause as [[tool-misrouting]] (D2 T2.1): Claude makes tool selection decisions based on descriptions. When the description is weak or absent, the wrong tool gets selected. D2 T2.4 adds the MCP-specific variant: the fix is CLAUDE.md guidance rather than rewriting the tool description directly (since you don't own the community server).

## Exam Signal

- "Community MCP tool not being selected" → weak description; fix in CLAUDE.md
- "Grep being chosen over capable MCP search tool" → description quality mismatch
- Fix layer: CLAUDE.md, NOT server modification
- Anti-pattern 5: minimal MCP descriptions causing Claude to skip the capable tool (see [[mcp-integration-anti-patterns]])

[Source: 12-NlTC0XY8orw.md, 14-qFYN8XYMTZM.md]
