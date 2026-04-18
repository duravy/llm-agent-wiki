---
title: Cross-Role Tools Exception (D2 T2.3)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The cross-role tools exception allows a sub-agent to receive limited access to a tool from another domain — but only when two conditions are both met: the operation is high-frequency, and routing through the coordinator is prohibitively expensive.

## The Rule

> Cross-role tools are the exception only — for high-frequency operations that would require expensive coordinator round trips.

This is a deliberate departure from strict tool scoping ([[tool-scoping]]). It is justified on latency grounds, not convenience.

## Canonical Example

**Agent**: Synthesis agent  
**Normal role**: Combine and synthesize research findings — no direct search  
**Problem**: Synthesis frequently needs to verify simple facts (dates, names, statistics)

Without a direct tool, every fact check requires:
1. Synthesis agent → coordinator (request fact check)
2. Coordinator → web researcher sub-agent (perform check)
3. Web researcher → coordinator (return result)
4. Coordinator → synthesis agent (relay result)

**Cost**: 2–3 extra steps, +40% latency per fact check. At high frequency, this compounds significantly.

**Solution**: Give the synthesis agent a scoped `verify_fact` tool with an explicit description boundary:

> "Use only for simple fact checks during synthesis — dates, names, statistics. Complex research queries must route through the coordinator."

## What Makes This Work

The cross-role tool must be:
1. **Constrained in scope** — description explicitly limits it to simple operations
2. **Different from the full version** — `verify_fact` is not `web_search`; it handles simple lookups only
3. **Justified by frequency** — the latency saving must be real and significant

If these conditions aren't met, it's not a cross-role exception — it's off-role tool misuse.

## Exam Signal

*"Three, cross-role tools are the exception — only for high-frequency operations that would require expensive coordinator round trips."*

The exam tests this as a nuanced judgment: cross-role tools are NOT always wrong, but they require a specific justification. Anti-Pattern #3 in [[tool-distribution-anti-patterns]] is "no cross-role tools at all" — the exam explicitly tests that prohibiting them entirely is also wrong.

## Connection to Constrained Alternatives

Cross-role tools should always be constrained alternatives, not the full generic tool. See [[constrained-tool-alternatives]]. Giving the synthesis agent the full `web_search` tool (instead of `verify_fact`) is both a cross-role and a generic-tool problem.

Related: [[tool-distribution]], [[tool-scoping]], [[constrained-tool-alternatives]], [[tool-distribution-anti-patterns]]

[Source: 11-K-HRtSQLGfU.md]
