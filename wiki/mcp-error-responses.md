---
title: Structured Error Responses for MCP Tools (D2 T2.2)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Hub page for Domain 2 Task 2.2. When an MCP tool fails, what it returns determines whether Claude can recover intelligently. A plain string gives Claude no signal. A structured error response gives Claude exactly what it needs to make the right call.

## The Core Problem

The naive error response is a text string: `"operation failed"`. Claude reads that string and must decide: retry or give up? Escalate or explain? A plain string provides no basis for that decision.

**Analogy**: You ask a coworker to book a meeting room. They return and say "I couldn't." That is useless — you don't know why or what to try next. But if they say "Room A is booked until 3pm, Room B has a broken projector, Room C is free at 2pm" — now you can decide. Structured error responses give Claude that same context.

## Two-Layer Architecture

| Layer | Mechanism | Purpose |
|-------|-----------|---------|
| Signal | `is_error: true` flag | Tells Claude *something went wrong* |
| Decision | Structured error body | Tells Claude *what kind of wrong and what to do next* |

The `is_error` flag alone is insufficient. The exam tests both layers. See [[is-error-flag]].

## Four Error Categories

Each category requires a different Claude response. See [[mcp-error-categories]].

| Category | Example | Claude Action |
|----------|---------|---------------|
| Transient | Timeout, service down | Wait and retry |
| Validation | Bad order ID format | Fix input and retry |
| Business | Refund exceeds limit | Explain to user, do NOT retry |
| Permission | Unauthorized access | Escalate or inform user |

## Five Structured Fields

Know these cold for the exam. See [[mcp-error-fields]].

1. `error_category` — transient / validation / business / permission
2. `is_retryable` — true = try again, false = stop
3. `message` — technical detail for logging
4. `customer_message` — what to show user (business errors only)
5. `suggestion` — exact next action for the agent

## Critical Exam Distinction

[[access-failure-vs-empty-result]] — the most commonly tested scenario:
- Database timed out → `is_error: true`, category: transient → Claude retries
- Search worked, found nothing → `is_error: false`, empty array → Claude reports no results

Mix these up and Claude gives wrong answers when systems are down.

## Multi-Agent Pattern

Sub-agents handle transient errors locally before escalating. See [[local-error-recovery]].

## Anti-Patterns

Five exam-tested anti-patterns in [[mcp-error-anti-patterns]].

## Connection to Domain 5

D5 T5.3 ([[error-propagation-multiagent]]) covers the coordinator-side view of error handling across a multi-agent pipeline. D2 T2.2 covers the MCP tool interface side — how individual tools format their failures. The frameworks are complementary: D2 T2.2 defines what a good error report looks like; D5 T5.3 defines how those reports propagate and get handled at the coordinator level.

[Source: 10-cMCoA_MGDdw.md]
