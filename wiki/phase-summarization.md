---
title: Phase Summarization (Summarize-Then-Inject Pattern)
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Phase summarization is the practice of creating a concise summary of one investigation phase's findings before spawning agents for the next phase. Phase 2 agents receive the compact summary as context — not the raw tool output from phase 1.

## The Pattern

```
Phase 1:
  Sub-agents explore codebase → return findings
  Main agent creates concise summary of key findings

Phase 2:
  Sub-agents receive the SUMMARY as context
  (not the raw tool output from phase 1)
  Sub-agents conduct deeper investigation
```

## Why It Matters

Passing raw phase 1 output to phase 2 agents fills their context window with noise before they even start work. A targeted summary gives phase 2 agents exactly what they need — specific findings, flagged anomalies, architectural context — without the verbose intermediate steps that produced those findings.

## What the Summary Should Contain

- Key discoveries (specific, not vague)
- Flagged issues and anomalies
- Architectural relationships relevant to phase 2's focus
- Files already examined (so phase 2 doesn't re-read them)

## Relationship to Subagent Delegation

Phase summarization extends [[subagent-delegation-exploration]]. Sub-agents handle the reading; the main agent handles the summarizing; the next wave of sub-agents receives clean context. Each phase starts fresh with only what matters from prior phases.

## Anti-Pattern Connection

Anti-Pattern #5 in [[large-codebase-anti-patterns]]: passing raw phase 1 output to phase 2.

## D1 T1.6 Connection: Cross-File Integration Pass

The cross-file integration pass in [[prompt-chaining]] (code review pipeline) is the same principle: step 2 receives per-file review *summaries*, not raw file contents. Passing raw contents would re-introduce the [[attention-dilution]] problem that per-file decomposition was designed to solve. The summarize-then-inject pattern appears in both D1 (task decomposition) and D5 (large codebase context). [Source: 07-kjzfSpgfvss.md]

## D1 T1.7 Connection: Session Resumption with Injected Summary

[[stale-context]] Mitigation Strategy 2 is the same pattern at session scale: instead of resuming a stale session, open a fresh session and inject a structured summary of what the prior analysis found. The summary replaces the history cleanly — key insights preserved, stale tool results discarded. The principle is identical: integrate from summaries, not from raw accumulated output. [Source: 08-GbEpxz4wf9s.md]

Related: [[large-codebase-context]], [[subagent-delegation-exploration]], [[scratchpad-files]], [[prompt-chaining]], [[attention-dilution]], [[stale-context]], [[session-state]]

[Source: 29-LRUdrMbFheA.md, 07-kjzfSpgfvss.md, 08-GbEpxz4wf9s.md]
