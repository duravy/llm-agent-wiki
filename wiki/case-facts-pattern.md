---
title: Case Facts Pattern
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

The case facts pattern is a context management technique where critical transactional data is extracted into a persistent structured block that lives outside the summarized conversation history. This block is injected at the beginning of every API call, immediately after the system prompt, ensuring the data is always visible regardless of conversation length.

## What Goes in the Block

Structured identifiers and transactional facts that would be destroyed by summarization:
- Customer ID
- Order ID
- Transaction date
- Amount
- Item details
- Delivery deadline

## Why It Works

Standard summarization reduces "Customer John, ID 4821, order #ORD-9934, placed 2025-11-01, $149.99, arrives by Nov 10" to "the customer had an order issue and expects quick delivery." Everything specific is gone. The case facts block avoids this by never passing this data through the summarization step.

Positioning right after the system prompt leverages the primacy effect — the model processes the beginning of context reliably. See [[lost-in-the-middle-effect]].

## Implementation

```
[SYSTEM PROMPT]
[CASE FACTS BLOCK]
  customer_id: 4821
  order_id: ORD-9934
  date: 2025-11-01
  amount: $149.99
  deadline: 2025-11-10
[SUMMARIZED HISTORY]
[RECENT TOOL RESULTS]
[LATEST USER MESSAGE]
```

See [[strategic-input-organization]] for the full layout strategy.

## Exam Signal
If the exam asks about **losing details in long conversations**, the answer is **case facts blocks**.

Related: [[context-management]], [[context-anti-patterns]]

[Source: 26-H8Vpe9j4N5I.md]
