---
title: Minimum Tools Principle
created: 2026-04-16
last_updated: 2026-04-16
source_count: 2
status: draft
---

The minimum tools principle states that each sub-agent should be given **only the tools it needs for its specific job** — nothing more. This is an explicit exam concept with three distinct rationales.

## The Rule

> **Each agent gets only the specific tools it needs. Nothing more.**

This is enforced via the `allowed_tools` field in the [[agent-definition|agent definition]].

## Three Rationales

| Rationale | Explanation |
|-----------|-------------|
| **Security** | Fewer tools = smaller attack surface. An agent that can't access a database tool can't be manipulated into leaking data via that tool. |
| **Reliability** | The agent can't accidentally call the wrong tool. With fewer choices, the model is less likely to invoke an irrelevant tool by mistake. |
| **Cost** | Tool descriptions consume tokens in every request. Defining 10 tools when an agent only needs 2 wastes tokens on every call. |

## How It's Enforced

In the Claude Agent SDK, the [[agent-definition]] has two fields:
- `tools` — the full set of tool schemas available
- `allowed_tools` — the subset the agent can actually call at runtime

The correct pattern: define a shared tool library in `tools`, restrict each agent to its minimum viable subset in `allowed_tools`.

## The Anti-Pattern

Giving all agents all tools (see [[sdk-anti-patterns]] Anti-pattern 2). Exam framing: "every agent gets every available tool." This fails all three rationales simultaneously — maximum attack surface, maximum confusion risk, maximum token cost.

## Connection to Coordinator

The minimum tools principle applies to sub-agents, but the coordinator itself must also include `task` in its own `allowed_tools`. If the coordinator's `allowed_tools` doesn't include `task`, it cannot spawn any sub-agents — the most-tested gotcha from [[task-tool|task tool mechanics]].

## D2 T2.3 Extension

D2 T2.3 ([[tool-distribution]]) deepens this principle with four additions:

1. **Specific threshold**: reliability drops significantly above 4–5 tools per agent. The principle is no longer just conceptual — the sweet spot is quantified. See [[too-many-tools-problem]].
2. **Coordinator rule made explicit**: the coordinator gets `task_tool` only — nothing else. Domain tools in the coordinator cause it to do work it should be delegating.
3. **Cross-role exception**: high-frequency, simple operations can bypass the coordinator via a scoped cross-role tool rather than forcing expensive round trips. See [[crossrole-tools]].
4. **Misuse angle**: off-role tools don't just reduce reliability — they actively redirect agent behavior. A synthesis agent with `web_search` starts searching instead of synthesizing. See [[tool-scoping]].

## D3 T3.2 Extension — allowed_tools in Skills

D3 T3.2 ([[skills]]) applies the minimum tools principle at the skill layer. The `allowed_tools` frontmatter field in a `skill.md` restricts which tools that skill can call during execution. A read-only analysis skill declares `[read, grep, glob]` — nothing else. Without this restriction, the skill has access to `edit`, `write`, and `bash`, creating the same security and reliability risks this principle was designed to prevent.

The principle now appears at three distinct layers: agent definition (D1 T1.3), tool distribution across agents (D2 T2.3), and skill configuration (D3 T3.2). Same concept, same rationale, different enforcement point.

[Source: 04-bFdRH69h9LU.md, 11-K-HRtSQLGfU.md, 15-AaGMyP2hBRY.md]
