---
title: Error Propagation in Multi-Agent Systems (D5 T5.3)
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Error propagation covers how failures in sub-agents are communicated to coordinator agents in multi-agent systems. The quality of error communication determines whether a system recovers gracefully or fails silently. This is Domain 5, Task 5.3 of the [[series-overview|Claude Architect Certification Exam series]].

## The Core Problem

A sub-agent fails silently — catches a timeout, returns an empty success with zero results. The coordinator assumes the search found nothing relevant. In reality the service was completely down. The final output **looks correct but is silently wrong.**

## Two Core Anti-Patterns

| Anti-Pattern | What happens | Why it's wrong |
|---|---|---|
| Silent suppression | Sub-agent catches error, returns empty success | Coordinator can't distinguish "no results" from "service down" |
| Immediate termination | One failure kills the entire workflow | Discards partial results from successful sub-agents |

See [[error-propagation-anti-patterns]] for all five.

## Structured Error Reporting

When a sub-agent fails, it must return structured context — not a generic message. See [[structured-error-reporting]] for the required fields.

## Graceful Degradation (The Correct Approach)

See [[graceful-degradation]].

1. Sub-agent reports a structured error with partial results and gap description
2. Coordinator extracts partial results
3. Coordinator continues synthesis, annotating output with coverage notes
4. User gets useful output with transparency about what's missing

**Partial output beats no output every time.**

## Coverage Annotations

See [[coverage-annotations]].

Final output should label which findings are well-supported, which are partial, and which are gaps — like a weather report that distinguishes satellite data from educated guesses.

## Local vs. Propagated Errors

See [[local-vs-propagated-errors]].

- Transient errors → retry locally first
- If retry succeeds → return clean results, coordinator never knew
- If all retries fail → propagate structured error with retry count

## Exam Framework

> "Structured error context must include failure type, attempted query, partial results, and alternative approaches. Never silently suppress. Never terminate on first failure. Coordinator's job: proceed with partial results and annotate coverage gaps. Local recovery for transient errors; only propagate unresolvable failures."

## Foundation in Domain 1

The coordinator architecture described here builds on [[multi-agent-coordinator|D1 T1.2]]. [[hub-and-spoke-architecture]] Rule 2 establishes that the coordinator owns error handling — this page defines what good error handling looks like in practice. The [[coordinator-responsibilities|Evaluate step]] (D-D-A-E-R) is where structured errors are assessed and graceful degradation decisions are made.

## Series Context

- Part 3 covered [[multi-agent-coordinator]] (D1 T1.2) — the coordinator architecture this builds on
- Part 27 covered [[escalation-ambiguity-resolution]] (D5 T5.2)
- Part 28 (this source) covers D5 T5.3
- Part 29 covers [[large-codebase-context]]

[Source: 28-mOUeDdo8rF0.md]
