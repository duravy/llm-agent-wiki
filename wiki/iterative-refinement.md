---
title: Iterative Refinement & Structured Handoffs
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Two quality-control patterns used during the Aggregate and Evaluate steps of [[coordinator-responsibilities|coordinator operation]]. Together they ensure that incomplete or unverifiable sub-agent output never reaches the final response.

## Iterative Refinement

After receiving a sub-agent result, the coordinator **evaluates it for gaps**. If gaps are found, it redelegates — to the same agent or a different one — before proceeding to Respond.

- The Evaluate step in [[coordinator-responsibilities|D-D-A-E-R]] can run **more than once**
- The coordinator does not just accept whatever came back
- The user never sees incomplete work — Respond is gated by Evaluate

**When to redelegate:**
- Missing data the sub-agent was supposed to retrieve
- Partial results that don't cover the full scope
- Low-confidence output that needs verification

This connects to [[feedback-loop-improvement]] (Domain 5): the same principle of closing quality loops before surfacing output.

## Structured Handoffs

Every finding from a sub-agent must be tagged with:
- **Source name**
- **URL**
- **Date**

Raw text is unverifiable. Structured output is citable and supports the coordinator's Aggregate step — it cannot reliably synthesize findings it cannot attribute.

**Bad handoff:**
```
"The regulation changed last year and now requires X."
# No source, no date — unverifiable
```

**Good handoff:**
```json
{
  "claim": "The regulation now requires X.",
  "source": "EU Compliance Bulletin",
  "url": "https://...",
  "date": "2024-11-01"
}
```

## Connection to Domain 5

Structured handoffs here (D1 T1.2) are the foundational requirement that [[subagent-metadata]] (D5 T5.1) and [[claim-source-mappings]] (D5 T5.6) extend. Domain 5 adds methodology and excerpt fields on top of the source/URL/date baseline established here.

> No contradiction — Domain 5 extends D1's requirement; it does not replace it.

[Source: 03-KQSSSP0op2M.md]
