---
title: Tools Are Descriptions, Not Code
created: 2026-04-16
last_updated: 2026-04-16
source_count: 3
status: draft
---

The most-tested distinction in Domain 1: when you give Claude a tool, you give it a **description**, not executable code. Claude never sees your implementation. This separation — Claude decides what to call, your code executes it — is the foundation of [[model-driven-vs-scripted|model-driven agency]].

## What Claude Sees

Every tool definition has exactly three parts:

1. **Name** — identifies the tool
2. **Description** — explains what the tool does (Claude uses this to decide when to call it)
3. **Input schema** — the parameters Claude must supply when requesting the tool

That is all Claude ever receives. The actual implementation (API call, database query, file read, etc.) is invisible to Claude.

## The Execution Pattern

```
Claude's response:
  → tool_use block: { id, name, input }

Your code:
  → receives the tool_use block
  → runs your implementation
  → returns result

Back to Claude:
  → user role message: { tool_result: { tool_use_id, content } }
```

The `tool_use_id` in the result **must match** the `id` from Claude's `tool_use` block exactly.

## Why This Matters for the Exam

- Any exam question about "giving Claude the ability to perform an action" → answer always involves a **tool description** + a **separate function that does the work**
- Claude cannot execute code — it can only request that your code executes it
- The clean separation enables [[model-driven-vs-scripted|model-driven agency]]: Claude is the planner, your code is the executor

## Tool Results Message Structure

Tool results always go back as:
- **Role**: `user`
- **Type**: `tool_result` block
- **References**: exact `tool_use_id` from Claude's request

## Connection to Multi-Agent Systems

In multi-agent systems (Domain 5), sub-agents themselves are often invoked via tool calls. The same pattern applies: the orchestrator sees a tool description for the sub-agent; the sub-agent's actual execution happens in your code. See [[subagent-delegation-exploration]].

## D2 T2.1 Connection: How to Write Those Descriptions

Domain 2 Task 2.1 is the deep dive into what the description field must actually contain. The D1 principle is "the description is all Claude sees." The D2 implementation guide is [[tool-description-design]].

Key additions from D2 T2.1:
- Every description needs five elements: purpose, input format, output, when-to-use, and boundary (when NOT to use)
- Missing boundaries cause systematic misrouting to similar-sounding tools
- Tool names carry selection weight: generic names like `analyze_document` give zero signal
- System prompt keywords can accidentally bias selection toward a wrong tool ([[system-prompt-keyword-trap]])
- When misrouting occurs, the first fix is always the description — not the architecture

## D2 T2.4 Extension: MCP Tools vs Regular Tools

D2 T2.4 introduces a second class of tools — MCP server tools — that differ fundamentally in how they are declared and discovered:

| Dimension | Regular Tools (D1 T1.1) | MCP Server Tools (D2 T2.4) |
|-----------|------------------------|---------------------------|
| **Defined by** | Your code (JSON schemas) | The MCP server |
| **Executed by** | Your code | The MCP server |
| **Discovery** | Listed explicitly in code | Auto-discovered at connection time |
| **Declaration** | Manual, per tool | Never declared manually |

The "description is all Claude sees" principle still applies to MCP tools — but the description is provided by the server, not your code. When community servers have minimal descriptions, Claude may misroute to better-described built-in tools. The fix is CLAUDE.md guidance, not server modification. See [[mcp-server-integration]], [[mcp-tool-description-fix]].

[Source: 02-OCiLc9Frq84.md, 09-TKmhUOlzM6w.md, 12-NlTC0XY8orw.md]
