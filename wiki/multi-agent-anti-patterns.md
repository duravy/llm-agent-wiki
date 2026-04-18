---
title: Multi-Agent Coordinator — Anti-Patterns
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five exam-tested anti-patterns in [[multi-agent-coordinator|multi-agent coordinator]] design. The exam presents scenarios designed to look reasonable but each has a flaw. All five violate a core rule of the [[hub-and-spoke-architecture]] or [[isolated-context]] principle.

## Anti-Pattern 1 — Direct Sub-Agent-to-Sub-Agent Communication

**What it looks like:** Sub-agent A sends its result directly to sub-agent B, bypassing the coordinator.

**Why it fails:** Violates [[hub-and-spoke-architecture]] Rule 1. All communication must go through the coordinator. No exceptions.

**Correct approach:** Sub-agent A returns its result to the coordinator. The coordinator includes that result in the explicit prompt it passes to sub-agent B.

---

## Anti-Pattern 2 — Assuming Inherited Context

**What it looks like:** A sub-agent is delegated a task like "summarize the findings" without being told what the findings are, on the assumption it "knows" from the session.

**Why it fails:** Sub-agents start completely fresh — [[isolated-context]]. They have no access to the coordinator's history or other sub-agents' results unless explicitly passed.

**Correct approach:** Coordinator includes all required context in every delegation prompt.

---

## Anti-Pattern 3 — Always-On Full Pipeline

**What it looks like:** Every incoming request triggers all sub-agents, regardless of complexity.

**Why it fails:** Burns tokens and adds latency on simple requests that could be handled directly. Violates [[dynamic-routing]] — the coordinator should evaluate the request first and invoke only what's needed.

**Correct approach:** Dynamic routing: assess request complexity and scope, then invoke the minimum necessary sub-agents.

---

## Anti-Pattern 4 — Overly Narrow / Overlapping Decomposition

**What it looks like:** Tasks are split so finely that two sub-agents end up searching the same data sources or covering the same scope, producing duplicate work.

**Why it fails:** Violates work partitioning (see [[dynamic-routing]]). Duplicate scope wastes tokens and produces conflicting or redundant results.

**Correct approach:** Each sub-agent gets a distinct, non-overlapping scope (e.g., one for web sources, one for internal data).

---

## Anti-Pattern 5 — Raw Text Handoffs

**What it looks like:** Sub-agents return unstructured prose with no source attribution. Coordinator attempts to synthesize it.

**Why it fails:** Raw text is unverifiable. The coordinator cannot reliably cite, validate, or synthesize findings without source name, URL, and date. Violates [[iterative-refinement|structured handoffs]] requirement.

**Correct approach:** Every sub-agent finding includes source name, URL, and date in structured output.

---

## Summary Table

| Anti-pattern | Rule violated | Correct principle |
|-------------|--------------|------------------|
| Sub-agent-to-sub-agent communication | Hub-and-spoke Rule 1 | All routing through coordinator |
| Assuming inherited context | Isolated context | Explicit context in every prompt |
| Always-on full pipeline | Dynamic routing | Evaluate request first |
| Overlapping decomposition | Work partitioning | Distinct non-overlapping scopes |
| Raw text handoffs | Structured handoffs | Source + URL + date on every finding |

## Relationship to SDK Anti-Patterns

Part 4 adds six SDK implementation anti-patterns in [[sdk-anti-patterns]]. These five cover coordinator *architecture* errors; those six cover SDK *configuration and prompt design* errors. Anti-pattern 5 above (raw text handoffs) overlaps with sdk-anti-patterns Anti-pattern 5 (unstructured handoffs) — same root cause described at different abstraction levels.

[Source: 03-KQSSSP0op2M.md, 04-bFdRH69h9LU.md]
