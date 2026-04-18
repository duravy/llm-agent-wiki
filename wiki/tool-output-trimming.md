---
title: Tool Output Trimming
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Tool output trimming is the practice of filtering API or tool responses down to only the fields relevant to the current task before they enter the context window. Implemented via **post-tool-use hooks**, trimming prevents irrelevant data from consuming context budget.

## The Problem

A customer order lookup might return 40+ fields:
- Internal warehouse codes
- Batch IDs
- Shipping label URLs
- Internal routing metadata
- Audit trail entries

None of these help the agent answer "where is my order?" The full 40-field response consumes context tokens that could hold more turns of conversation or additional tool results.

## The Fix

Post-tool-use hooks intercept the raw tool response and reduce it to the 5 fields that matter:

| Keep | Discard |
|------|---------|
| Order ID | Internal IDs |
| Status | Warehouse codes |
| Amount | Batch identifiers |
| Tracking number | Label URLs |
| Estimated delivery | Audit fields |

## Important Distinction

Trim **tool outputs within history**, not the conversation structure itself. Dropping conversation history entirely destroys coherence. The goal is surgical reduction of data volume, not wholesale deletion of turns.

See also: [[context-anti-patterns]] (anti-pattern #2 and #5)

## Exam Signal
If the exam asks about **wasting context tokens**, the answer is **trimming tool outputs**.

Related: [[context-management]], [[lost-in-the-middle-effect]]

[Source: 26-H8Vpe9j4N5I.md]
