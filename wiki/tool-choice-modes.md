---
title: tool_choice Modes (D2 T2.3)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 2
status: draft
---

Three `tool_choice` modes control how Claude selects tools within a given API call. Each solves a different problem. The exam tests whether you know when to use forced selection versus auto.

## The Three Modes

### `auto` (default)
**Behavior**: Claude may call a tool or return plain text — it decides what fits best.  
**Use when**: Normal operation. Let Claude read the situation and respond appropriately.  
**Risk**: If you always need a tool call, Claude may sometimes return text instead.

---

### `any`
**Behavior**: Claude must call some tool — any of the available tools.  
**Use when**: You always need structured output and cannot accept a plain text reply.  
**Example**: A pipeline step that requires JSON output before the next step can proceed.  
**Note**: Claude still chooses *which* tool; it just cannot return text instead of calling one.

---

### Force (specific tool name)
**Behavior**: Claude must call that specific named tool — no other tool, no text.  
**Use when**: An ordered pipeline requires a specific step to run first.  
**Example**: `extract_metadata` must run before `enrich_data` — force it in step 1, then switch to auto in step 2.

---

## Ordered Pipeline Pattern

Force tool selection is the key mechanism for building ordered pipelines. The pattern:

```
Step 1: tool_choice = { type: "tool", name: "extract_metadata" }
  → Claude must call extract_metadata. No skipping to enrichment.
  → Metadata now exists in the result.

Step 2: tool_choice = "auto"
  → Claude decides what to do next based on the metadata output.
  → Can call enrich_data, flag an issue, or return results.
```

This ensures pipeline steps run in the right order every time, regardless of what Claude might otherwise decide.

## Exam Signal

*"Force tool selection is the key to building ordered pipelines."*

*"The exam tests whether you know when to use forced versus auto."*

Exam question form: "You need metadata extraction to always run before enrichment in a pipeline. What `tool_choice` setting achieves this?"  
Answer: Force selection (specific tool name) for the extraction step.

Common distractor: `any` — this guarantees a tool call but does not guarantee *which* tool. For ordered pipelines, force (specific name) is required.

## Connection to Sequential Execution

This pattern connects to [[parallel-vs-sequential-execution]]: ordered pipelines require sequential execution where step N+1 depends on step N. `tool_choice` force is the mechanism that enforces step order at the API level.

## Connection to Structured Extraction (D4 T4.3)

Task 4.3 is the clearest use case for `any` and `force`. When using a dummy extraction tool for guaranteed JSON output (see [[extraction-tool-pattern]]):

- `auto` **must never be used** — Claude may return text, bypassing the extraction schema entirely
- `force` (specific tool name) — use when document type is known and one schema always applies
- `any` — use when document type varies and you have multiple extraction schemas; Claude picks the right one

> **"Never use auto for extraction. The exam tests this directly."** [Source: 22-CxwIKbepDwQ.md]

This extends the exam signal from D2 T2.3 (forced pipelines) to D4 T4.3 (forced extraction). Same mechanism, different application.

## Connection to Batch API (D4 T4.5)

`tool_choice` modes only apply within the synchronous API. The [[message-batches-api]] (D4 T4.5) does not support mid-request tool execution at all — it is single-turn only. Any workflow requiring tool calls (at any `tool_choice` mode) must use the synchronous API.

> Exam trap: "Switch an agentic tool-calling workflow to batch to save 50%." → Wrong. Batch does not support tool calls regardless of `tool_choice` mode.

Related: [[tool-distribution]], [[tool-scoping]], [[parallel-vs-sequential-execution]], [[prerequisite-gates]], [[structured-output-tool-use]], [[extraction-tool-pattern]], [[message-batches-api]]

[Source: 11-K-HRtSQLGfU.md, 22-CxwIKbepDwQ.md, 24-6ZGgMSQz3eg.md]
