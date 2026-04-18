---
title: Hub-and-Spoke Architecture
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The hub-and-spoke architecture is the standard structural pattern for [[multi-agent-coordinator|multi-agent systems]]. The coordinator is the hub; each sub-agent is a spoke. Three hard rules define the pattern — any design that violates them breaks the architecture.

## The Three Hard Rules

### Rule 1 — All Communication Goes Through the Coordinator
Sub-agents never send messages to each other. Ever. All coordination, routing, and information flow pass through the coordinator. A design that lets two spokes communicate directly violates this pattern. See [[multi-agent-anti-patterns]] Anti-pattern 1.

### Rule 2 — Coordinator Owns Error Handling
If a sub-agent fails, the coordinator decides the response: retry, use a fallback, or degrade gracefully. Sub-agents don't manage each other's failures. This connects directly to the [[error-propagation-multiagent|error propagation]] patterns in Domain 5.

### Rule 3 — Coordinator Controls Information Flow
The coordinator decides exactly what context each sub-agent receives. No sub-agent gets the full coordinator history unless the coordinator explicitly passes it. This rule gives rise to [[isolated-context]], the most commonly missed exam detail.

## Visual Structure

```
                    ┌─────────────┐
         ┌──────────│ Coordinator │──────────┐
         │          │   (hub)     │          │
         ▼          └─────────────┘          ▼
   ┌──────────┐          │             ┌──────────┐
   │Sub-agent │          ▼             │Sub-agent │
   │    A     │    ┌──────────┐        │    C     │
   └──────────┘    │Sub-agent │        └──────────┘
                   │    B     │
                   └──────────┘
         ✗ No direct A↔B, A↔C, or B↔C communication
```

## Exam Signals

- "Sub-agents never message each other" — absolute rule, no exceptions
- Coordinator owns error handling → connects to [[graceful-degradation]] and retry logic
- Information flow control → see [[isolated-context]] for the exam's most-tested consequence

[Source: 03-KQSSSP0op2M.md]
