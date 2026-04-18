---
title: Subagent Delegation for Exploration
created: 2026-04-15
last_updated: 2026-04-15
source_count: 2
status: draft
---

Subagent delegation for exploration is the practice of having the main agent assign file reading and codebase investigation to sub-agents, which return concise summaries rather than raw content. This keeps the main agent's context reserved for decision-making rather than consumed by verbose file reads.

## The Manager Analogy

A manager doesn't read every document themselves — they send team members to investigate different areas and receive briefings. The manager's time and attention stays on decisions, not data gathering.

## How It Works

**Without delegation (bad):**
Main agent reads 20 files directly. Context fills with verbose raw content. No space left for synthesis or decisions.

**With delegation (good):**
```
Main agent: "Understand the refund flow."

Sub-agent 1 (reads 12 test files) returns:
  "3 test suites found. Coverage: happy path only. No error path tests."

Sub-agent 2 (reads 8 source files) returns:
  "Flow: API handler → validation → service layer → payment gateway → DB.
   No transaction rollback on gateway failure."

Main agent context: 2 four-line summaries, not 20 raw files.
```

Context is preserved for actual decision-making.

## What Sub-Agents Return

Sub-agents should return **concise summaries**, not raw file contents. Key elements:
- What was found (specific facts, not descriptions of files)
- Anomalies or flags
- Architectural relationships

This pairs naturally with [[scratchpad-files]] — sub-agents can write findings to a shared scratchpad.

## Relationship to Error Propagation

When sub-agent exploration fails, apply [[error-propagation-multiagent]] patterns: return structured errors with partial results rather than silently returning nothing or terminating.

## Exam Signal

"If the exam asks how to keep context clean during large codebase exploration: **subagent delegation + scratchpad files**."

Note: this is a different use of subagents than [[subagent-metadata]] (which covers metadata requirements on research findings). Here the concern is context volume, not provenance.

Related: [[large-codebase-context]], [[scratchpad-files]], [[phase-summarization]], [[large-codebase-anti-patterns]]

## Foundation in Domain 1

Each sub-agent runs its own [[agentic-loop|agentic loop]] (D1 T1.1) — calling `messages.create`, checking `stop_reason`, and executing [[tools-as-descriptions|tool descriptions]]. The delegation pattern here is an application of [[model-driven-vs-scripted|model-driven agency]] at scale: the main agent decides what to delegate, sub-agents decide how to explore.

The formal architecture for this pattern is the [[multi-agent-coordinator|hub-and-spoke coordinator]] (D1 T1.2). Key constraint from that domain: each sub-agent starts with [[isolated-context]] — it only sees its own system prompt and the explicit prompt passed by the main agent. It never inherits the main agent's history.

## D2 T2.5 Connection: Built-In Tools as the Mechanics

D2 T2.5 ([[builtin-tools]]) establishes the single-agent tool mechanics that underpin this pattern. At the single-agent level, the exploration sequence is: Grep to find entry points → Read specific files → follow imports to the next Read. See [[two-phase-search-strategy]] and [[incremental-exploration]].

Sub-agent delegation scales this: each sub-agent performs its own incremental exploration (Grep + Read cycle) across a focused slice of the codebase, then returns a summary. The Grep/Read pairing is both the single-agent exploration tool and the building block for sub-agent delegation.

## D3 T3.4 Connection: The Explore Sub-Agent in Plan Mode

Plan mode (D3 T3.4) uses a dedicated [[explore-sub-agent]] to run the investigation phase of planning in isolation — the same isolation pattern as subagent delegation for codebase exploration, applied specifically to the planning context. The explore sub-agent reads files and maps dependencies in its own context; the main conversation receives only the clean, concise plan. This is the same "delegate investigation, surface summary" pattern as described here. See [[plan-mode-vs-direct-execution]].

[Source: 29-LRUdrMbFheA.md, 13-IN2WntoHedY.md, 17-96v5N5YGMPM.md]
