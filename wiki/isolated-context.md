---
title: Isolated Context
created: 2026-04-16
last_updated: 2026-04-16
source_count: 2
status: draft
---

Isolated context is the most-tested detail in the [[multi-agent-coordinator|multi-agent coordinator pattern]]. The coordinator has full visibility; each sub-agent starts completely fresh. The exam rule: the coordinator must explicitly include everything a sub-agent needs — never assume inherited context.

## What Each Party Sees

| Party | What it sees |
|-------|-------------|
| **Coordinator** | User request + its own reasoning + all task calls + all sub-agent results |
| **Each sub-agent** | Its own system prompt + the prompt the coordinator explicitly passed — nothing else |

Sub-agents have no access to:
- The coordinator's conversation history
- Other sub-agents' results
- The original user request (unless the coordinator explicitly includes it in the delegation prompt)

## The Exam Rule

> **"The coordinator must explicitly include everything a sub-agent needs. Never assume inherited context."**

This is the most commonly missed detail on the exam. A coordinator that passes a vague "do this" prompt to a sub-agent — without the user's original question, relevant prior results, or necessary context — will get a useless or incorrect sub-agent response.

## Why Isolation Exists

Each sub-agent is a **fresh Claude instance** (spawned via the [[task-tool]]). It has no memory of the session and no access to the coordinator's message history. Isolation is not a design choice — it is how the architecture works.

## Correct vs Incorrect Delegation

**Incorrect (assumes inheritance):**
```
coordinator → sub-agent: "Summarize the findings."
# Sub-agent has no idea what "the findings" are
```

**Correct (explicit context):**
```
coordinator → sub-agent: "The user asked: [original question].
  Prior research found: [specific facts].
  Your task: summarize these findings into 3 bullet points."
```

## Connection to Context Passing

[[context-passing]] is the active solution to isolated context. Where isolated context explains *why* sub-agents don't inherit knowledge, context passing describes *what the coordinator must do*: actively construct a task description containing the goal, relevant facts, constraints, required output format, and any upstream results.

## Connection to Anti-Patterns

[[multi-agent-anti-patterns]] Anti-pattern 2 (assuming inherited context) is the direct violation of this principle. Any scenario where a sub-agent is expected to "know" something it was never told is an exam trap.

## D3 T3.2 Connection — context:fork in Skills

D3 T3.2's [[context-fork]] (`context:fork` skill frontmatter) creates the same kind of isolation: the skill runs in a fresh sub-agent that cannot access the main conversation's history. The difference is intent — isolated context in D1 T1.2 is a structural fact about how sub-agents are spawned; `context:fork` in D3 T3.2 is a deliberate configuration choice to keep the main conversation context clean. Same mechanism, different framing.

## D3 T3.4 Connection — Explore Sub-Agent in Plan Mode

Plan mode's [[explore-sub-agent]] is a third instance of the same isolation: the sub-agent investigating the codebase for a planning task starts fresh and returns only a concise plan summary to the main conversation. The isolation keeps 45 files + 200 grep searches out of the main context. See [[plan-mode-vs-direct-execution]].

## D3 T3.6 Connection — Session Isolation for CI Reviews

[[session-isolation-ci]] (D3 T3.6) is a fourth instance of the isolation pattern: the CI review session must be completely fresh — not a continuation of the generation session. The generator's reasoning context biases review. Fresh session = unbiased review. The mechanism is the same as D1 T1.2 (fresh sub-agent spawn), applied at the CI pipeline layer. [Source: 19-UnN8c0Sshq8.md]

[Source: 03-KQSSSP0op2M.md, 04-bFdRH69h9LU.md, 15-AaGMyP2hBRY.md, 17-96v5N5YGMPM.md, 19-UnN8c0Sshq8.md]
