---
title: Tool Scoping — Least Privilege for Tools (D2 T2.3)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The principle of least privilege applied to tools: each agent gets only the tools it needs for its specific role. Off-role tools create misuse. The coordinator is the most-tested special case: it gets the task tool only, and nothing else.

## Role-Specific Tool Sets

| Agent Role | Tools | Rationale |
|-----------|-------|-----------|
| Web researcher | `web_search`, `fetch_url` | Needs to find and retrieve web content |
| Document analyzer | `read_doc`, `extract_text` | Reads and processes documents only |
| Customer agent | `lookup_customer`, `process_refund` | Customer operations only |
| Coordinator | `task_tool` | Spawns sub-agents; does no direct work |

## The Coordinator Rule

The coordinator gets the task tool to spawn sub-agents — and nothing else. This is the most exam-tested application of tool scoping.

Why: if the coordinator has domain tools (e.g., `web_search`), it is tempted to do work it should be delegating. The coordinator's job is decompose, delegate, aggregate, evaluate, respond (see [[coordinator-responsibilities]]). Giving it domain tools blurs this boundary and risks pulling context into the coordinator that belongs in isolated sub-agent contexts.

## Why Off-Role Tools Cause Misuse

> "Agents with tools outside their role tend to misuse them. A synthesis agent with web search starts searching instead of synthesizing."

This is not a prompt-following failure — it is a tool selection failure. If the tool exists and its description matches the current intent, Claude will call it. The structural solution is removal, not instruction.

## Connection to D1 T1.3

[[minimum-tools-principle]] in D1 T1.3 established this principle from three angles:
- **Security**: limits blast radius if a sub-agent misbehaves
- **Reliability**: fewer tools = fewer selection errors
- **Cost**: fewer tool descriptions in context = lower token usage

D2 T2.3 adds a fourth angle: **misuse prevention**. Off-role tools are not just a reliability risk — they actively redirect agent behavior away from the agent's intended function.

## Exception

When a sub-agent needs limited access to a tool from another domain, the cross-role exception applies — but only for high-frequency, low-complexity operations that would otherwise require expensive coordinator round trips. See [[crossrole-tools]].

Related: [[tool-distribution]], [[too-many-tools-problem]], [[crossrole-tools]], [[minimum-tools-principle]], [[coordinator-responsibilities]]

## D3 T3.2 Extension — allowed_tools in Skills

D3 T3.2 ([[skills-allowed-tools]]) applies the same least-privilege principle at the skill layer. The `allowed_tools` frontmatter field in a skill restricts which tools that skill can call. A read-only skill declares `[read, grep, glob]` only — it literally cannot call `edit`, `write`, or `bash`. This is structural prevention, not instruction-based — the same design choice made here (removal, not instruction).

[Source: 11-K-HRtSQLGfU.md, 15-AaGMyP2hBRY.md]
