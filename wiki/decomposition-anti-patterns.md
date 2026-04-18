---
title: Decomposition Anti-Patterns (D1 T1.6)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five exam-tested anti-patterns for task decomposition. These are distinct from [[enforcement-anti-patterns|T1.4 enforcement anti-patterns]] and [[hooks-anti-patterns|T1.5 hooks anti-patterns]] — these focus specifically on how tasks are broken apart and executed. [Source: 07-kjzfSpgfvss.md]

## Anti-Pattern 1 — Single-Prompt Multi-File Review

**What it looks like:** Sending all 14 files in one review prompt.

**Why it fails:** [[attention-dilution]] — items buried in the middle of a long context receive less model attention. Files 5–10 get a shallow pass; obvious bugs in file 7 are missed and ship to production.

**Correct approach:** Per-file passes (one file per prompt, parallelizable) followed by a separate cross-file integration pass using summaries. See [[prompt-chaining]] and [[attention-dilution]].

---

## Anti-Pattern 2 — Dynamic Decomposition for a Predictable Task

**What it looks like:** Using an adaptive, discovery-driven plan for a task whose steps are known upfront (e.g., code review, data extraction).

**Why it fails:** The agent wastes tokens replanning a structure it could have specified directly before execution. Dynamic decomposition's adaptive overhead is only justified when scope genuinely cannot be predicted.

**Correct approach:** When steps are known, use [[prompt-chaining]]. Fixed pipelines are cheaper and faster for repeatable tasks.

---

## Anti-Pattern 3 — Prompt Chaining for an Exploratory Task

**What it looks like:** Using a fixed pipeline for an open-ended investigation task where discoveries at each step should shape the next step (e.g., adding tests to an unknown legacy codebase).

**Why it fails:** A rigid pipeline cannot adapt when unexpected discoveries occur mid-execution. If step 2 finds a critical shared utility module, a fixed pipeline has no mechanism to insert the new investigation steps that finding requires.

**Correct approach:** Use [[dynamic-decomposition]] for tasks where scope is unknown upfront and the plan must evolve from findings.

---

## Anti-Pattern 4 — Overly Narrow Subtask Definitions

**What it looks like:** A coordinator asked to research "creative industries" decomposes it into only visual art subtasks (digital art, graphic design, photography) — missing music, writing, and film entirely.

**Why it fails:** The subtask scope fails to cover the full breadth of the original task. The coordinator's decomposition is the ceiling of what gets covered — if a domain isn't represented as a subtask, it won't be investigated.

**Correct approach:** Subtask scope must match the full breadth of the original task. Before finalizing decomposition, explicitly verify that all major sub-domains are represented. See [[multiconcern-decomposition]] for the parallel concern model.

---

## Anti-Pattern 5 — No Cross-File Integration Pass

**What it looks like:** Per-file reviews are conducted and results are compiled directly into a report — without a dedicated cross-file integration pass.

**Why it fails:** Per-file passes catch *within-file* bugs only. Data flow inconsistencies between modules, API contract mismatches, and shared-state assumptions are cross-cutting concerns that only appear when multiple files are analyzed together. Without the integration pass, these issues are invisible.

**Correct approach:** Always follow per-file passes with a separate integration pass that examines the per-file summaries together, looking specifically for cross-cutting concerns.

---

## Summary Table

| Anti-pattern | Root cause | Correct principle |
|-------------|-----------|------------------|
| Single-prompt multi-file review | [[attention-dilution\|Attention dilution]] trap | Per-file passes + integration pass |
| Dynamic for predictable tasks | Adaptive overhead where fixed suffices | [[prompt-chaining]] — fixed pipeline |
| Chaining for exploratory tasks | Fixed pipeline can't adapt to discoveries | [[dynamic-decomposition]] — adaptive plan |
| Overly narrow subtask definitions | Decomposition fails to cover full breadth | Match scope to original task breadth |
| No cross-file integration pass | Per-file passes miss cross-cutting concerns | Mandatory integration pass on summaries |

[Source: 07-kjzfSpgfvss.md]
