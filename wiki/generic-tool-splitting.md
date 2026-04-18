---
title: Generic Tool Splitting
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

A generic tool that handles multiple behaviors forces Claude to guess which behavior applies each time. The fix is to split it into specific tools — one per distinct behavior — each with its own name, purpose, and input/output contract.

## The Problem

`analyze_document` — one tool, three possible behaviors:
1. Extract data points (names, dates, amounts)
2. Summarize the content
3. Verify a claim against the source

Claude must guess which behavior the user wants **every single time**. With no signal to distinguish the three, it will sometimes guess wrong.

## The Fix: Three Specific Tools

| Tool | Purpose | Input | Output |
|------|---------|-------|--------|
| `extract_data_points` | Pull structured facts | Document text or URL | Names, dates, amounts as structured JSON |
| `summarize_content` | Generate a concise summary | Document text or URL | 3–5 sentence summary of main arguments |
| `verify_claim_against_source` | Check if a claim is supported | Claim + document text | Supported / Contradicted / Unclear with quote |

Each tool has a **distinct name**, a **clear purpose**, and an **unambiguous input/output contract**. Claude can match user intent to the right tool with zero ambiguity.

## The Naming Signal

Splitting works because tool **names** carry selection weight alongside descriptions. Generic names like `analyze_document` provide zero differentiation. Specific names like `extract_data_points` and `summarize_content` are already half the description — Claude knows from the name alone which behavior is intended.

## When to Split vs. Keep a Single Tool

Split when a tool covers **distinct behaviors** that require different inputs or produce different outputs.

Keep a single tool when the variation is **parameter-driven** (e.g., same behavior with optional fields) — splitting for that would create unnecessary proliferation.

## Relationship to Minimum Tools Principle

Splitting does **not** violate [[minimum-tools-principle]]. That principle says each agent gets only the tools it needs — not that tools must be generic. Specific tools + selective assignment = minimum tools applied correctly.

## Anti-Pattern Connection

Anti-Pattern 3 in [[tool-description-anti-patterns]]: generic tool names (`analyze_document`) lead to ambiguous selection. Splitting is the direct fix.

Related: [[tool-description-design]], [[five-elements-tool-description]], [[tool-naming-strategies]], [[tool-description-anti-patterns]], [[minimum-tools-principle]]

[Source: 09-TKmhUOlzM6w.md]
