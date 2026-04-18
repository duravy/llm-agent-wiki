---
title: Parallel vs Sequential Execution
created: 2026-04-16
last_updated: 2026-04-16
source_count: 4
status: draft
---

In a [[multi-agent-coordinator|multi-agent coordinator system]], the coordinator chooses between two execution modes when delegating via the [[task-tool]]. The test is simple: if any sub-agent's prompt needs another sub-agent's output, it must be sequential.

## Side-by-Side

| Mode | How it works | When to use |
|------|-------------|-------------|
| **Parallel** | Multiple `task` calls in a single coordinator response; SDK launches all sub-agents simultaneously | Tasks are fully independent — no output dependency between them |
| **Sequential** | One `task` call per loop iteration; coordinator waits for result before issuing the next | Step N+1 needs step N's output |

## The Decision Test

> **"If any sub-agent's prompt needs another sub-agent's output, it must be sequential."**

Anything else — tasks that don't depend on each other — can and should be parallel to maximise speed.

## Parallel Example

```
Coordinator response (iteration 1):
  tool_use → task(research-agent, "find web sources on X")
  tool_use → task(data-agent, "query internal DB for X")
# SDK launches both simultaneously
```

## Sequential Example

```
Coordinator response (iteration 1):
  tool_use → task(search-agent, "find the contract ID for customer Y")

Coordinator response (iteration 2):  ← waits for contract ID first
  tool_use → task(billing-agent, "pull invoice history for contract ID [result from step 1]")
```

## Multiconcern Decomposition

A specific application of parallel execution: splitting a complex request into parallel **concern agents**, each handling one independent question. See [[multiconcern-decomposition]]. Key rule: concerns must be truly independent — dependent concerns must remain sequential (see [[enforcement-anti-patterns]] Anti-pattern 5).

## Fork Sessions — Third Pattern

Beyond parallel and sequential, the SDK supports **fork sessions**: multiple sub-agents spawned from the *same baseline context*, each exploring a different direction. See [[fork-sessions]].

- Parallel: independent tasks, different prompts
- Sequential: dependent tasks, one at a time
- Fork: same baseline context, diverging directions (exploration / A/B testing / hypothesis investigation)

## D1 T1.6 Connection: Prompt Chaining and Parallelism

[[prompt-chaining]] demonstrates a key nuance: the *between-step* relationship is sequential (step 2 needs step 1's output) but the *within-step* relationship can be parallel. Reviewing 14 files in step 1 is 14 independent calls — they run simultaneously. Step 2 (cross-file integration) must wait for all 14, then runs sequentially as a single call. This is not a style choice — it is a correctness and efficiency decision. [Source: 07-kjzfSpgfvss.md]

## Exam Signal

- Parallel = multiple `task` calls in **one response** → SDK runs simultaneously
- Sequential = **one `task` call per loop iteration** → coordinator waits
- Fork = same baseline context, multiple diverging sub-agents → coordinator synthesizes
- Within a chaining step: independent units can be parallelized; between steps: must be sequential
- Misusing parallel when there's a dependency = exam anti-pattern (produces incorrect results)
- Treating sequential vs parallel as a **style choice** = [[sdk-anti-patterns|Anti-pattern 6]]; it is a correctness decision

## D2 T2.3 Addition: tool_choice for Ordered Pipelines

D2 T2.3 introduces `tool_choice` force selection as the API-level mechanism for enforcing step order within a single agent call — complementary to the coordinator-level sequential execution pattern above.

- **Sequential execution** (D1 T1.2): coordinator issues one task call per iteration, waiting for each result before issuing the next
- **Force tool_choice** (D2 T2.3): within a single agent call, force a specific tool to run first before switching to auto

The force pattern ensures a pipeline step (e.g., `extract_metadata`) must complete before Claude can proceed — even without a coordinator round trip. See [[tool-choice-modes]].

[Source: 03-KQSSSP0op2M.md, 04-bFdRH69h9LU.md, 07-kjzfSpgfvss.md, 11-K-HRtSQLGfU.md]
