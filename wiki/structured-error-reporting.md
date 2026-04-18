---
title: Structured Error Reporting
created: 2026-04-15
last_updated: 2026-04-15
source_count: 2
status: draft
---

Structured error reporting is the practice of returning rich, actionable context when a sub-agent fails — rather than a generic error message. It is the foundation of recoverable multi-agent systems.

## The Nurse Analogy

A bad report says "there was a problem" — the doctor can't act.

A good report says: "Patient X, creatinine 4.5 (normal < 1.2), measured at 3pm, suggestive of acute kidney injury — recommend urgent nephrology consult." The doctor can act immediately.

The same principle applies to multi-agent error reporting.

## Required Fields in a Structured Error

| Field | Purpose |
|-------|---------|
| `failure_type` | What kind of failure occurred (timeout, auth error, service down, etc.) |
| `attempted_query` | What the sub-agent tried — enables coordinator to retry differently |
| `partial_results` | What was successfully collected before failure |
| `gap_description` | What's missing and why |
| `alternative_approaches` | What the coordinator could try next |

## Generic vs. Structured — The Contrast

**Generic (bad):**
```
Error: search unavailable
```
The coordinator assumes no relevant results were found. In reality the service was completely down. Zero recovery options.

**Structured (good):**
```
failure_type: service_unavailable
attempted_query: "AI diagnostics 2024"
partial_results: [3 results from cache]
gap_description: PubMed API unreachable — drug discovery coverage incomplete
alternative_approaches: [use cached results, try Google Scholar, flag gap]
```
The coordinator can now decide: proceed with cached results, try an alternative source, or annotate the gap. Full recovery options available.

## The Actionability Principle

The key metric for a good error report is **actionability** — does it give the coordinator enough information to make a recovery decision? Generic errors fail this test entirely.

## D2 T2.2 Connection

D2 T2.2 ([[mcp-error-fields]]) defines a parallel five-field structure for MCP tool-level errors: `error_category`, `is_retryable`, `message`, `customer_message`, and `suggestion`. The D5 T5.3 fields above (failure_type, attempted_query, partial_results, gap_description, alternative_approaches) apply to sub-agent escalation reports at the coordinator level. The two schemas serve different layers of the same system — the MCP tool reports to the sub-agent; the sub-agent reports to the coordinator.

Key addition from D2 T2.2: the `is_retryable` boolean field provides a direct retry signal. D5 T5.3 infers retry eligibility from failure type; D2 T2.2 makes it explicit.

See also: [[graceful-degradation]], [[coverage-annotations]], [[error-propagation-multiagent]], [[mcp-error-responses]], [[mcp-error-fields]]

[Source: 28-mOUeDdo8rF0.md, 10-cMCoA_MGDdw.md]
