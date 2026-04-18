---
title: Local Error Recovery in Sub-Agents (D2 T2.2)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Sub-agents should handle transient errors locally before escalating to the coordinator. This pattern — attempt, retry, retry, then escalate with rich context — keeps the coordinator focused on unresolvable problems while reducing unnecessary escalation traffic.

## The Pattern

```
Sub-agent hits transient error (e.g., timeout)
  → Retry #1
    → Timeout again
  → Retry #2
    → Success
  → Coordinator never knew
```

If all retries fail:
```
Sub-agent hits transient error
  → Retry #1 → fail
  → Retry #2 → fail
  → Escalate to coordinator with:
      - What was attempted
      - Any partial results found
      - Alternatives the coordinator can try
```

## What to Include in Escalation

When a sub-agent has exhausted local retries, the escalation report must include:
1. **What was attempted** — which query, which service, how many retries
2. **Partial results** — any data successfully retrieved before the failure
3. **Alternatives** — other sources or approaches the coordinator could try instead

Partial context enables intelligent coordinator recovery. Without it, the coordinator must start from scratch.

## Why This Matters

- **Reduces coordinator load** — transient errors that self-resolve stay invisible at the coordinator level
- **Prevents unnecessary pipeline stalls** — the coordinator does not halt waiting for a recoverable failure to be relayed and handled
- **Improves recovery quality** — rich escalation reports give the coordinator actual options, not just a failure signal

## Exam Question

*"What should a sub-agent do before escalating a tool failure to the coordinator?"*

**Answer**: Retry locally (for transient errors), then report with partial results and alternatives if all retries fail.

The distractor answers are:
- Immediately escalate (wrong — transient errors should be handled locally first)
- Return an empty result without escalating (wrong — silently drops the failure)
- Escalate with only the error message and no context (wrong — coordinator needs partial results and alternatives)

## Connection to D5 T5.3

D5 T5.3 ([[local-vs-propagated-errors]]) establishes the same principle from the coordinator's perspective: retry transient errors locally; only propagate unresolvable failures. D2 T2.2 establishes the same principle from the sub-agent/tool side, with the specific addition that escalation reports must include partial results and alternatives.

The frameworks are complementary. D5 T5.3 answers "what should the coordinator expect?" D2 T2.2 answers "what should the sub-agent do?"

## Connection to Validation and Retry Loops (D4 T4.4)

D4 T4.4 applies the same retry-then-escalate structure to extraction pipelines. Key parallel: D2 T2.2 and D4 T4.4 both enforce a maximum retry count before escalating to a higher authority (coordinator vs human reviewer). Both check whether the information is actually available before retrying — D4 T4.4's non-retriable failures (absent source data) map to D2 T2.2's business logic violations (not transient). Same pattern, applied at different layers.

Related: [[mcp-error-responses]], [[mcp-error-categories]], [[local-vs-propagated-errors]], [[error-propagation-multiagent]], [[graceful-degradation]], [[validation-retry-loops]], [[retriable-vs-nonretriable-failures]]

[Source: 10-cMCoA_MGDdw.md, 23-wyqmHtVGxAg.md]
