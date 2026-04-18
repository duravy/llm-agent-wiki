---
title: Tool Distribution Across Agents (D2 T2.3)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Hub page for Domain 2 Task 2.3. How you distribute tools across agents determines whether each agent can do its job reliably. Too many tools create decision complexity and misuse. Too few create expensive coordinator round trips. Correct distribution gives each agent exactly the tools its role requires — and no others.

## The Core Problem

Giving an agent too many tools degrades tool selection reliability. Each additional tool is another option Claude must evaluate. More options → more decision complexity → more selection errors. This is a research finding the exam directly tests.

See [[too-many-tools-problem]] for the 4–5 sweet spot and the analogy.

## Principle of Least Privilege for Tools

Each agent gets only the tools for its specific role. See [[tool-scoping]].

| Agent | Tools |
|-------|-------|
| Web researcher | `web_search` + `fetch_url` |
| Document analyzer | `read_doc` + `extract_text` |
| Customer agent | `lookup_customer` + `process_refund` |
| Coordinator | `task_tool` only |

The coordinator rule is the most exam-tested: coordinators get the task tool to spawn sub-agents — and nothing else.

## Cross-Role Tools Exception

Sometimes a sub-agent needs limited access to a tool from another domain. This is allowed only when the operation is high-frequency and routing through the coordinator costs too much latency. See [[crossrole-tools]].

## `tool_choice` Configuration

Three modes control how Claude selects tools within a given call. See [[tool-choice-modes]].

| Mode | Claude's behavior | Use when |
|------|------------------|---------|
| `auto` | May call a tool or return text | Normal operation |
| `any` | Must call some tool | Structured output always required |
| Force (named tool) | Must call that specific tool | Ordered pipeline step must run first |

## Constrained Tool Alternatives

Replace generic tools (which sub-agents can misuse) with constrained versions scoped to the sub-agent's role. See [[constrained-tool-alternatives]].

## Anti-Patterns

Five exam-tested anti-patterns in [[tool-distribution-anti-patterns]].

## Connection to D1

D1 T1.3 established the [[minimum-tools-principle]]: each agent gets only what it needs, for security, reliability, and cost reasons. D2 T2.3 deepens this with the specific 4–5 sweet spot threshold, the coordinator-gets-task-tool-only rule, the cross-role exception pattern, and the `tool_choice` configuration mechanism.

[Source: 11-K-HRtSQLGfU.md]
