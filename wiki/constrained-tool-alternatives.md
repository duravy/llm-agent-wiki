---
title: Constrained Tool Alternatives (D2 T2.3)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Instead of giving agents powerful generic tools they might misuse, replace them with constrained alternatives — tools with the same core function but a narrower, role-appropriate scope. The exam tests whether you recognize when a generic tool creates a misuse risk.

## The Pattern

| Generic Tool | Problem | Constrained Alternative | Fix |
|-------------|---------|------------------------|-----|
| `fetch_url` | Can access any URL — research agent pulls random websites | `load_document` | Validates approved domains; accepts PDF/DOCX/HTML only |
| `web_search` | Full search — synthesis agent starts searching instead of synthesizing | `verify_fact` | Simple lookups only; explicit boundary in description |

## Canonical Example

**Generic**: `fetch_url`
- Can access any URL on the internet
- A research agent might use it to pull random websites instead of research documents
- No scope enforcement

**Constrained**: `load_document`
- Validates that the URL points to an approved domain
- Only accepts document formats: PDF, DOCX, HTML
- Rejects everything else
- Same core functionality — narrower scope

The constrained version has the same underlying capability but enforces scope at the tool level, not at the prompt level.

## Why Constraints Must Be in the Tool, Not the Prompt

Telling Claude "only use fetch_url for documents" is a prompt instruction — probabilistic, not guaranteed. Claude may deviate in edge cases (see [[probabilistic-vs-deterministic]]). A constrained tool *cannot* be misused for out-of-scope requests because the implementation rejects them regardless of what Claude sends.

This is the same code-vs-prompt enforcement principle from D1 T1.4 ([[code-vs-prompt-enforcement]]) applied to tool design.

## When to Create Constrained Alternatives

Create a constrained alternative when:
1. A generic tool exists that could serve the sub-agent's need
2. The generic tool also has out-of-scope uses the sub-agent should not access
3. You cannot rely on prompt instructions alone to prevent misuse

For cross-role tools specifically, constrained alternatives are always required — giving an agent from another role the full generic tool is both a scoping violation and a misuse risk. See [[crossrole-tools]].

## Exam Signal

*"The exam tests whether you recognize when a generic tool creates a misuse risk."*

Exam question form: "A research agent with `fetch_url` is pulling non-document URLs. What is the correct fix?"  
Answer: Replace `fetch_url` with a constrained `load_document` that validates domain and format — not a prompt instruction.

Related: [[tool-distribution]], [[tool-scoping]], [[crossrole-tools]], [[code-vs-prompt-enforcement]], [[probabilistic-vs-deterministic]]

[Source: 11-K-HRtSQLGfU.md]
