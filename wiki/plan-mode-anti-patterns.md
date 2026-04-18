---
title: Plan Mode Anti-Patterns (T3.4)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five exam-tested anti-patterns for Task 3.4. These are the wrong answer choices on the exam.

## Anti-pattern 1: Direct Execution for an Architectural Change

**Wrong**: Using direct execution for a task with multiple valid approaches or large multi-file scope (e.g., monolith-to-microservices restructuring).

**Why it's wrong**: You risk discovering dependency problems mid-execution and having to undo partial changes. The codebase ends up in a broken intermediate state. Costly rework.

**Correct approach**: Use plan mode first. Map the problem, get human approval, then execute.

---

## Anti-pattern 2: Plan Mode for a Single-File Bug Fix

**Wrong**: Running plan mode for a task that's already well-defined with one obvious implementation (e.g., fixing a bug with a clear stack trace pointing to one location).

**Why it's wrong**: Plan mode adds overhead that provides no value here. The scope is already clear. There are no architectural decisions to make.

**Correct approach**: Use direct execution. If you can describe the change in one sentence and there's only one obvious way to do it, there's no need to plan.

---

## Anti-pattern 3: Starting Over When Complexity Is Discovered Mid-Execution

**Wrong**: Stopping partway through a direct execution task when an unexpected dependency problem surfaces, then starting over.

**Why it's wrong**: This is avoidable. If there was any indication the task might be complex, plan mode should have been used from the start.

**Correct approach**: When in doubt about task complexity, start with plan mode. The upfront investigation cost is far cheaper than mid-execution rework.

---

## Anti-pattern 4: Running Plan Mode Without Actually Reviewing the Plan

**Wrong**: Invoking plan mode, receiving the plan, and approving it immediately without actually reading and adjusting it.

**Why it's wrong**: The human review checkpoint is the entire value of plan mode. It catches design problems before they become implementation debt. Skipping the review eliminates the only benefit.

**Correct approach**: Always read and adjust the plan before approving. The plan exists to be reviewed.

---

## Anti-pattern 5: Skipping the Explore Sub-Agent for Large Investigations

**Wrong**: Allowing discovery output (file reads, grep searches, dependency mapping) to run directly in the main conversation context for a large investigation.

**Why it's wrong**: 45 files + 200 grep searches fill the context window before implementation starts. No space remains for the actual plan or execution.

**Correct approach**: Use the [[explore-sub-agent]]. All verbose discovery stays in its isolated context; the main conversation only receives the clean, concise plan.

---

## Summary Table

| # | Anti-pattern | Correct approach |
|---|-------------|-----------------|
| 1 | Direct execution for architectural change | Plan mode first |
| 2 | Plan mode for single-file bug fix | Direct execution |
| 3 | Restart when complexity discovered mid-execution | Use plan mode from the start when task might be complex |
| 4 | Approve plan without reviewing it | Always read and adjust before approving |
| 5 | Discovery floods main context | Use the explore sub-agent |

[Source: 17-96v5N5YGMPM.md]
