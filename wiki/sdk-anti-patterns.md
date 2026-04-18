---
title: SDK Anti-Patterns (D1 T1.3)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Six exam-tested anti-patterns for Claude Agent SDK implementation. These are distinct from the [[multi-agent-anti-patterns|coordinator architecture anti-patterns]] (Part 3) — these focus on SDK-level mistakes in configuration, context passing, and prompt design.

## Anti-Pattern 1 — Forgetting `task` in Coordinator's `allowed_tools`

**What it looks like:** Coordinator logic calls the task tool, but `task` is not in the coordinator's `allowed_tools` list.

**Why it fails:** The SDK rejects the call. The coordinator can never spawn sub-agents at all — silent total failure.

**Correct approach:** Always include `"task"` in the coordinator's `allowed_tools`. This is the single most-tested gotcha in the sub-agent section.

---

## Anti-Pattern 2 — Giving All Agents All Tools

**What it looks like:** Every sub-agent is defined with the full set of available tools in `allowed_tools`.

**Why it fails:** Violates the [[minimum-tools-principle]]. Security risk (larger attack surface), reliability risk (agent may call wrong tool), and unnecessary token cost (tool schemas sent with every request).

**Correct approach:** Each agent gets only the tools it needs for its specific job.

---

## Anti-Pattern 3 — Dumping the Entire Conversation History into the Task Prompt

**What it looks like:** Coordinator copies the full message history into the sub-agent's task description.

**Why it fails:** Lazy and expensive. Inflates context with irrelevant content, buries the signal in noise, and burns tokens. See [[context-passing]].

**Correct approach:** Extract and pass only the facts, constraints, and results relevant to this specific task.

---

## Anti-Pattern 4 — Procedural Prompts That Specify Every Step

**What it looks like:** Task prompt includes a step-by-step sequence of tool calls or specific instructions on *how* to execute.

**Why it fails:** Defeats the purpose of using an agent. Claude's value is reasoning; micromanaging its execution removes that advantage. See [[goal-oriented-prompting]].

**Correct approach:** Specify goal + output format + constraints. Let Claude choose the execution path.

---

## Anti-Pattern 5 — Unstructured Data Handoffs

**What it looks like:** Sub-agents return raw text blobs with no metadata. Coordinator tries to synthesize unverifiable prose.

**Why it fails:** Downstream agents and the coordinator cannot assess trust, cite sources, or verify claims without structured metadata. Closely related to [[multi-agent-anti-patterns|Anti-pattern 5 (raw text handoffs)]] from coordinator architecture.

**Correct approach:** Every cross-agent handoff includes source, date, and confidence score — structured JSON, not prose.

---

## Anti-Pattern 6 — Treating Sequential vs Parallel as a Style Choice

**What it looks like:** Developer chooses sequential execution because it "feels cleaner" or "is easier to debug," even though the tasks are fully independent.

**Why it fails:** Sequential execution when tasks are independent is a **correctness error**, not just a performance issue — it imposes unnecessary ordering constraints. See [[parallel-vs-sequential-execution]].

**Correct approach:** Choose based on data dependencies. Independent tasks must be parallel.

---

## Summary Table

| Anti-pattern | Root cause | Correct principle |
|-------------|-----------|------------------|
| Missing `task` in `allowed_tools` | Configuration oversight | Always include `task` for coordinators |
| All tools to all agents | Over-permissioning | [[minimum-tools-principle]] |
| Full history dump | Lazy context construction | [[context-passing]] — selective extraction |
| Procedural prompts | Misunderstanding agent purpose | [[goal-oriented-prompting]] |
| Unstructured handoffs | No output contract | Source + date + confidence on all handoffs |
| Sequential as style choice | Misclassifying correctness decision | Data dependency determines mode |

## Relationship to Coordinator Architecture Anti-Patterns

The [[multi-agent-anti-patterns]] page (Part 3) covers coordinator architecture errors. This page covers SDK implementation errors. Anti-pattern 5 here (unstructured handoffs) overlaps with Anti-pattern 5 there (raw text handoffs) — same root cause, described at different levels of abstraction.

[Source: 04-bFdRH69h9LU.md]
