---
title: Structured Output via Tool Use (D4 T4.3)
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

Task 4.3 of Domain 4. The most reliable way to get structured JSON output from Claude is to define a dummy extraction tool with a JSON schema and force Claude to call it. The API enforces valid JSON at the system level — Claude literally cannot produce malformed JSON for a tool call. [Source: 22-CxwIKbepDwQ.md]

## The Problem with Prose JSON Requests

Telling Claude to "respond in JSON" is probabilistic — Claude may produce invalid JSON (missing braces, trailing commas, unquoted keys), or valid JSON that doesn't match the expected schema. Each run can vary. This is the same inconsistency problem that [[categorical-definitions]] (T4.1) and [[few-shot-prompting]] (T4.2) address for classification and format — T4.3 solves it for structured extraction.

> **Analogy:** Asking someone to write their address on a blank sheet gives inconsistent results. A form with labeled boxes (street, city, state, zip) forces a consistent format. [Source: 22-CxwIKbepDwQ.md]

## The Extraction Tool Pattern

See [[extraction-tool-pattern]] for the full 3-step mechanism. In brief:

1. Define a JSON schema as a "tool" (no real behavior — just field definitions, types, enums, required list)
2. Set `tool_choice` to `force` (specific tool) or `any` (must call some tool)
3. Read the result from `response.content[0].input` — guaranteed valid JSON

## Key Concepts

| Concept | Page | One-Line Summary |
|---------|------|-----------------|
| Extraction Tool Pattern | [[extraction-tool-pattern]] | Define a schema as a dummy tool; API enforces valid JSON |
| Nullable Fields | [[nullable-fields]] | Required fields that may be absent → hallucination; nullable fields → null instead |
| Enum Escape Hatches | [[enum-escape-hatches]] | Add `unclear` + `other` to every enum; add detail string for `other` |
| Syntax vs Semantic Errors | [[syntax-vs-semantic-errors]] | Tool use eliminates syntax errors; semantic errors (wrong values) still possible |
| Anti-Patterns | [[structured-output-anti-patterns]] | Five: prose JSON, required nullable fields, no enum escapes, auto for extraction, skipping semantic validation |

## tool_choice for Extraction

Three modes (see [[tool-choice-modes]] for full reference):

| Mode | Use case for extraction |
|------|------------------------|
| `auto` | **Never use for extraction** — Claude may return text instead of calling the tool |
| `any` | Use when document type is unknown and you have multiple extraction schemas — Claude picks which tool |
| `force` (specific name) | Use when document type is known — Claude must call that specific extraction schema |

> **"Never use auto for extraction. Claude may return text instead of structured data. The exam tests this directly."** [Source: 22-CxwIKbepDwQ.md]

## Syntax vs Semantic Errors — Exam-Critical Distinction

Tool use eliminates syntax errors (API-enforced) but does **not** eliminate semantic errors (valid JSON with wrong values). See [[syntax-vs-semantic-errors]].

**Example:** An invoice extraction where line items sum to $92, tax at 8% = $7.36, but the `total` field says `$100`. The JSON is perfectly valid — the numbers are just wrong. Semantic validation requires a separate step (Task 4.4). [Source: 22-CxwIKbepDwQ.md]

## Six Exam Must-Knows

1. `tool_use` with JSON schemas eliminates syntax errors — most reliable structured output approach
2. `tool_choice: any` guarantees Claude calls a tool instead of returning text
3. Nullable fields prevent hallucination — make fields optional when the source may not contain them
4. Enums with `unclear` and `other` prevent forced misclassification
5. Force tool selection ensures the specific extraction schema runs when you know the document type
6. Tool use eliminates syntax errors but **not** semantic errors — values can still be wrong

## Cross-Domain Connections

- [[probabilistic-vs-deterministic]] (D1 T1.4) — tool use = API-level deterministic JSON; prose JSON request = probabilistic
- [[tool-choice-modes]] (D2 T2.3) — same modes; extraction use case is the clearest application of `force` and `any`
- [[prerequisite-gates]] (D1 T1.4) — parallel concept: API-enforced constraints vs prompt-requested behavior
- [[prompt-design-explicit-criteria]] (D4 T4.1) — T4.1 defines what to extract; T4.3 defines how to emit it reliably
- [[few-shot-prompting]] (D4 T4.2) — T4.2 teaches null handling via examples; T4.3 enforces it via nullable schema fields — complementary approaches to the same problem
- [[gray-area-examples]] (D4 T4.2) — null extraction example (`{source: null}`) previews the nullable-fields concept from T4.3

[Source: 22-CxwIKbepDwQ.md]
