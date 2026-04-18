---
title: Local vs. Propagated Errors
created: 2026-04-15
last_updated: 2026-04-15
source_count: 2
status: draft
---

Sub-agents must distinguish between errors they can handle internally and errors that require coordinator attention. Propagating every error clutters the coordinator's context with noise; suppressing all errors prevents recovery. The rule: handle transient errors locally, propagate only unresolvable failures.

## Decision Rule

```
transient error (e.g. timeout)?
  → retry locally
  → if retry succeeds: return clean results (coordinator never knew)
  → if all retries fail: propagate structured error with retry count

unresolvable error (service down, auth failure, no data)?
  → propagate immediately with full structured context
```

## Why Local Retry First

If a search times out and the second attempt succeeds, the coordinator doesn't need to know. Surfacing every transient hiccup:
- Wastes coordinator context tokens on noise
- Forces the coordinator to make decisions it doesn't need to make
- Slows the overall workflow

The coordinator's context should only contain decisions it actually needs to make.

## What to Include When Propagating

When local retries are exhausted and the error must propagate, include in the structured error (see [[structured-error-reporting]]):
- `failure_type`
- `attempted_query`
- `partial_results`
- `gap_description`
- `alternative_approaches`
- `retry_count` — how many attempts were made locally

## Anti-Pattern Connection

- Propagating every transient error → Anti-Pattern #5 in [[error-propagation-anti-patterns]]
- Suppressing all errors → Anti-Pattern #2 (silent suppression)

## D2 T2.2 Connection

D2 T2.2 ([[local-error-recovery]]) establishes the same local-retry pattern from the MCP tool interface perspective, with the additional specification that escalation reports must always include: what was attempted, partial results found, and alternatives the coordinator can try. The two framings are complementary — D5 T5.3 defines what the coordinator expects; D2 T2.2 defines what the sub-agent must deliver.

D2 T2.2 also adds the `is_error` flag as the mechanism for signaling failures: transient errors with all retries exhausted set `is_error: true` + `error_category: transient`. See [[is-error-flag]].

Related: [[error-propagation-multiagent]], [[structured-error-reporting]], [[graceful-degradation]], [[local-error-recovery]], [[mcp-error-categories]]

[Source: 28-mOUeDdo8rF0.md, 10-cMCoA_MGDdw.md]
