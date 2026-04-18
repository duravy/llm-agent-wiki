---
title: Graceful Degradation
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Graceful degradation is the correct response to partial failure in a multi-agent system. Instead of terminating the workflow or silently producing wrong output, the coordinator continues synthesis using available results and annotates the output to show what's missing.

## The Principle

**Partial output beats no output every time.**

When sub-agents fail, the coordinator's job is not to abort — it is to proceed with what succeeded and be transparent about what didn't.

## The Three-Step Pattern

1. **Sub-agent reports a structured error** — with partial results and gap description (see [[structured-error-reporting]])
2. **Coordinator extracts partial results** — uses what was collected before the failure
3. **Coordinator continues synthesis** — annotates final output with coverage notes (see [[coverage-annotations]])

## What the User Sees

A gracefully degraded output tells the user:
- Which findings are **well-supported** (multiple sources)
- Which have **partial coverage** (some sources unavailable)
- Which are **gaps** (no coverage, needs manual research)

This is transparency, not failure. The user gets useful output and knows exactly what to verify.

## Contrast with the Two Anti-Patterns

| Approach | Result |
|---|---|
| Silent suppression | Output looks complete but is silently wrong |
| Immediate termination | User gets zero output |
| **Graceful degradation** | **User gets partial output with clear gap annotations** |

See [[error-propagation-anti-patterns]].

## Exam Signal

> "The coordinator's job is to proceed with partial results and annotate coverage gaps — not abort the entire workflow."

Related: [[error-propagation-multiagent]], [[structured-error-reporting]], [[coverage-annotations]], [[local-vs-propagated-errors]]

[Source: 28-mOUeDdo8rF0.md]
