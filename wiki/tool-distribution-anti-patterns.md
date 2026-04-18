---
title: Tool Distribution Anti-Patterns (D2 T2.3)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five exam-tested anti-patterns in tool distribution and `tool_choice` configuration. All appear as wrong answer choices. Recognizing them quickly is a core D2 T2.3 skill.

## Anti-Pattern Summary

| # | Anti-Pattern | What Goes Wrong | Fix |
|---|-------------|-----------------|-----|
| 1 | Every agent gets all tools | Maximizes decision complexity; off-role tool misuse | Scope each agent to 4–5 role-appropriate tools |
| 2 | 18 tools per agent | Reliability drops significantly above 4–5 | Split responsibilities across multiple specialized agents |
| 3 | No cross-role tools at all | Forces expensive coordinator round trips for simple operations | Allow cross-role tools for high-frequency, low-complexity operations |
| 4 | Using `auto` when structured output required | Claude may return text instead of a tool call | Use `any` to guarantee a tool call every time |
| 5 | Generic tools with no constraints | Sub-agents can misuse them for out-of-scope operations | Replace with constrained alternatives that limit scope |

---

## Anti-Pattern 1: Every Agent Gets All Tools

**What it looks like**: A coordinator passes the full tool list to every sub-agent regardless of role.

**The problem**: Each agent now has 12+ tools to evaluate for every action. This maximizes decision complexity and exposes agents to tools outside their role. A customer agent with `read_doc` may start analyzing documents instead of handling the customer request.

**The fix**: Define role-specific tool sets. Each agent gets 4–5 tools maximum, all relevant to its function. See [[tool-scoping]].

---

## Anti-Pattern 2: 18 Tools Per Agent

**What it looks like**: A single agent is responsible for too many tasks and accumulates all the tools needed for each.

**The problem**: Reliability drops significantly above 4–5 tools. The agent is really several agents that haven't been separated yet.

**The fix**: Split into multiple specialized agents, each with a focused role and 4–5 tools. See [[too-many-tools-problem]].

---

## Anti-Pattern 3: No Cross-Role Tools At All

**What it looks like**: A strict interpretation of least privilege prohibits all cross-role access. Every operation routes through the coordinator.

**The problem**: For high-frequency simple operations, this adds 2–3 coordinator round trips per operation. At scale, this adds 40% latency and clogs coordinator context with low-value relay work.

**The fix**: Allow cross-role tools for operations that are both high-frequency and simple — but always as constrained alternatives, not full generic tools. See [[crossrole-tools]].

---

## Anti-Pattern 4: Using `auto` When Structured Output Is Required

**What it looks like**: A pipeline step requires a tool call every time, but `tool_choice` is set to `auto`.

**The problem**: With `auto`, Claude may return plain text instead of calling a tool. If the next pipeline step requires structured output from the tool, the pipeline breaks.

**The fix**: Use `any` when a tool call is always required. Use force (specific tool name) when a *specific* tool must run. See [[tool-choice-modes]].

---

## Anti-Pattern 5: Generic Tools With No Constraints

**What it looks like**: Giving agents `fetch_url` (access any URL), `web_search` (search anything), or other broad-scope tools.

**The problem**: The agent can use these tools for operations outside its role — and will, because the tool description matches edge-case intents. This is structural misuse, not a prompt-following failure.

**The fix**: Replace generic tools with constrained alternatives that limit domain, format, or operation type. See [[constrained-tool-alternatives]].

---

## Exam Application

When a scenario describes unreliable tool selection or agent misuse, run through this checklist:
1. Does any agent have more than 4–5 tools? → AP1 or AP2
2. Does any agent have tools outside its role? → AP1
3. Does every operation route through the coordinator including simple lookups? → AP3
4. Is a pipeline step sometimes returning text instead of a tool call? → AP4
5. Is an agent using a tool for out-of-scope operations? → AP5

Related: [[tool-distribution]], [[too-many-tools-problem]], [[tool-scoping]], [[crossrole-tools]], [[tool-choice-modes]], [[constrained-tool-alternatives]]

[Source: 11-K-HRtSQLGfU.md]
