---
title: Structured State Persistence for Crash Recovery
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Structured state persistence is the practice of exporting investigation state to a manifest file at a known location during multi-hour sessions. If the session crashes, a new session loads the manifest and continues from exactly where work left off.

## The Game Checkpoint Analogy

A game saves your progress at checkpoints so a crash doesn't send you back to the start. The manifest file is the checkpoint for a long agent investigation.

## The Manifest File

The manifest tracks everything needed to resume:

```yaml
investigation: "Auth system security audit"
current_phase: 3
completed_tasks:
  - task: "Map auth flow"
    result_file: "scratch/auth-flow.md"
  - task: "Identify token validation gaps"
    result_file: "scratch/token-gaps.md"
files_in_progress:
  - "src/auth/refresh.ts"
  - "src/middleware/session.ts"
key_findings:
  - "verify() checks expiry but not signature — line 45"
  - "Race condition in concurrent token refreshes"
  - "No rollback on payment gateway failure"
```

## On Crash Recovery

1. Coordinator loads the manifest file
2. Injects manifest into the new session's context
3. Investigation continues from the current phase
4. Completed tasks are not re-analyzed — their result files are referenced directly
5. In-progress files are picked up where they were left off

No need to rediscover findings that were already documented.

## When to Use It

For any **multi-hour investigation** — set up the manifest structure at the start, not after a crash has already occurred. The cost of maintaining the manifest is trivial compared to losing hours of analysis.

## Relationship to Scratchpad Files

Both are external memory mechanisms. [[Scratchpad files|scratchpad-files]] store individual findings during exploration. The manifest tracks the investigation's overall state — which tasks completed, which are in progress, and where result files live. They work together.

## Anti-Pattern Connection

Anti-Pattern #4 in [[large-codebase-anti-patterns]]: no state persistence → crash loses all progress.

## Exam Signal

"If the exam asks about crash recovery, the answer is **structured state persistence**."

Related: [[large-codebase-context]], [[scratchpad-files]], [[context-degradation]]

[Source: 29-LRUdrMbFheA.md]
