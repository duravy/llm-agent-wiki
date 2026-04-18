---
title: Coordinator's Five Responsibilities (D-D-A-E-R)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The coordinator in a [[multi-agent-coordinator|multi-agent system]] has five distinct responsibilities that run inside its own [[agentic-loop]] in order. This is the exam's model of what a coordinator actually does.

## The Five Responsibilities

### 1. Decompose
Break the user request into **non-overlapping subtasks** with clear, distinct scopes for each sub-agent. Good decomposition means no two agents cover the same ground. See [[dynamic-routing]] for how scope boundaries are set.

### 2. Delegate
Issue [[task-tool]] calls — one per sub-agent — with a **fully specified prompt** that includes everything the agent needs. Vague prompts produce vague results. See [[isolated-context]] for why explicit context is mandatory.

### 3. Aggregate
Collect all sub-agent results and synthesize them into a **unified, coherent view**. This is where individual findings become a single answer. Requires [[iterative-refinement|structured handoffs]] to work (raw text cannot be reliably synthesized).

### 4. Evaluate
Check quality and completeness of aggregated results. **If there are gaps, the coordinator redelegates** — it does not just accept whatever came back. This step can run more than once. See [[iterative-refinement]].

### 5. Respond
Deliver the final answer to the user — **only after the quality check passes**. The user never sees incomplete work.

## Exam Mnemonic

**D-D-A-E-R**: Decompose → Delegate → Aggregate → Evaluate → Respond

## Key Exam Points

- The Evaluate step is not one-shot: coordinator can redelegate multiple times before responding
- Delegate requires fully-specified prompts — see [[isolated-context]]
- Respond is gated by Evaluate — incomplete work never reaches the user
- All five run inside the coordinator's own agentic loop, not outside it

## Connection to Domain 5

The [[error-propagation-multiagent|error propagation]] patterns (D5 T5.3) describe what happens inside the Evaluate step when sub-agents return failures — structured error reports, graceful degradation, coverage annotations. The five responsibilities here are the frame; Domain 5 fills in the failure-handling detail.

[Source: 03-KQSSSP0op2M.md]
