---
title: Context Management (D5 T5.1)
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Context management is the practice of deliberately controlling what information occupies an agent's context window during long-running conversations. Without it, agents forget critical details mid-conversation — losing order IDs, amounts, and deadlines — causing trust to drop and users to repeat themselves. The core insight is that good memory is a design choice, not a model property.

## The Problem

By turn 15 of a customer support conversation, an agent may have forgotten the order ID from turn 2. Three root causes:

1. **Context windows fill up** — when full, something must be removed, and the wrong thing often goes
2. **Summarization destroys structured data** — numbers, dates, and identifiers are lost when history is condensed
3. **Tool outputs bury critical details** — a 40-field API response contains mostly irrelevant data that consumes context budget

See also: [[lost-in-the-middle-effect]]

## Key Patterns

| Pattern | What it solves | Wiki Page |
|---------|---------------|-----------|
| Case Facts Block | Summarization loss of transactional data | [[case-facts-pattern]] |
| Tool Output Trimming | Bloated tool responses wasting context | [[tool-output-trimming]] |
| Strategic Input Organization | Lost-in-the-middle attention gaps | [[strategic-input-organization]] |
| Subagent Metadata | Unverifiable subagent outputs | [[subagent-metadata]] |

## Five Anti-Patterns

See [[context-anti-patterns]] for the full list.

## Exam Mnemonics

- **Losing details in long conversations** → answer is **case facts blocks**
- **Wasting context tokens** → answer is **trimming tool outputs**
- **Lost in the middle** → beginning and end are processed reliably, not the middle
- **Progressive summarization** → loses numbers, dates, and identifiers

## Extended Context Management

For context management challenges specific to **large codebase exploration**, see [[large-codebase-context]] (D5 T5.4) — which adds scratchpad files, subagent delegation, `/compact`, state persistence, and phase summarization to the toolkit.

[[context-degradation]] describes the specific failure mode of precision loss over extended sessions.

## Foundation in Domain 1

The `messages` array in [[messages-create-parameters|messages.create]] is the mechanism underlying context management. Every pattern here — case facts blocks, tool output trimming, strategic input organization — is a technique for controlling what occupies that array. The [[agentic-loop|agentic loop]] appends to it on every iteration; these patterns prevent it from filling with low-value content.

## Series Context
- Part 2 covers [[agentic-loop]] (D1 T1.1) — the foundational loop mechanism
- Part 25 covered [[multi-instance-review]]
- Part 26 (this source) covers D5 T5.1
- Part 27 covers [[escalation-ambiguity-resolution]]

[Source: 26-H8Vpe9j4N5I.md]
