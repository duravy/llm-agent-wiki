---
title: Dynamic Routing & Work Partitioning
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Two efficiency patterns used by the [[multi-agent-coordinator|coordinator]] during the Decompose and Delegate steps (see [[coordinator-responsibilities]]). Both prevent wasted tokens and incorrect results from poor task distribution.

## Dynamic Routing

The coordinator **evaluates the request first**, then decides which sub-agents to invoke — not always all of them.

- A simple query can be handled directly by the coordinator without spawning sub-agents
- Over-invoking sub-agents burns tokens and adds latency
- The set of sub-agents invoked varies by request

**Exam signal:** "Only invoke the sub-agents actually needed. Evaluate the request first, then decide." This is violated by [[multi-agent-anti-patterns]] Anti-pattern 3 (always-on full pipeline).

### Model-Driven Routing

Dynamic routing is an expression of [[model-driven-vs-scripted|model-driven agency]] at the coordinator level: Claude reads the request and decides which tools (sub-agents) to call, rather than the developer hardcoding a fixed pipeline for every request.

## Work Partitioning

Each sub-agent is assigned a **distinct, non-overlapping scope**. If two agents cover the same data, that is bad decomposition — it produces duplicate work and wastes tokens.

**Good partitioning example:**
```
Sub-agent A: web sources only
Sub-agent B: internal database only
→ No overlap, no duplicate results
```

**Bad partitioning example:**
```
Sub-agent A: "research X" (broad)
Sub-agent B: "find information about X" (overlapping)
→ Both search the same sources, duplicate findings
```

This is the failure mode in [[multi-agent-anti-patterns]] Anti-pattern 4 (overly narrow or overlapping decomposition).

## Connection to [[isolated-context]]

Both patterns require that each sub-agent's prompt be explicitly scoped — not just "do research" but "search web sources for X, specifically looking for Y." Vague prompts produce overlap and assume context that doesn't exist.

[Source: 03-KQSSSP0op2M.md]
