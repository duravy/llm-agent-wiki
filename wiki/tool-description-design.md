---
title: Tool Description Design (D2 T2.1)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The core insight of Task 2.1: **Claude does not read your implementation code. It reads the tool description.** That description is the sole signal Claude uses to choose which tool to call. Getting it wrong causes misrouting on every request.

> "Think of a restaurant menu. If two dishes are both described as 'a dish', you can't choose between them."

This page is the hub for D2 T2.1. For the foundational D1 concept that Claude only sees name + description + schema, see [[tools-as-descriptions]].

## The Selection Mechanism

When given multiple tools and a user request, Claude:
1. Reads every tool's name and description
2. Matches the request intent against those descriptions
3. Calls the tool whose description best fits

There is no other signal. No implementation code. No runtime type inspection. No implicit context from how tools are organized. **The description is everything.**

## Five Elements of a Good Description

Full details: [[five-elements-tool-description]]

| Element | Bad Example | Good Example |
|---------|-------------|--------------|
| Purpose | "analyzes content" | "extract structured data from web pages" |
| Input format | "URL as a string" | "full URL including HTTPS, e.g. https://example.com/article" |
| Output | (omitted) | "page title, main text content, publication date, author if available" |
| When to use | (omitted) | "use when the user asks about information on a specific web page" |
| Boundary | (omitted) | "do not use for PDF documents; use extract_pdf instead" |

## The High-Leverage Fix

When the exam presents a misrouting problem, the **first fix to consider** is improving the tool description. It is the lowest-effort, highest-impact change available.

- Renaming `analyze_content` → `extract_web_results` with a specific description eliminates ambiguity entirely
- Before considering architecture changes, check descriptions first

## Splitting Generic Tools

One generic tool that handles multiple behaviors forces Claude to guess which behavior applies. Split it: [[generic-tool-splitting]]

## System Prompt Keyword Trap

Words in your system prompt can accidentally bias tool selection toward a wrong tool. See [[system-prompt-keyword-trap]].

## Naming Strategies

Three patterns for consistent, unambiguous tool names: [[tool-naming-strategies]]

## Anti-Patterns

Five exam-tested failures in tool description design: [[tool-description-anti-patterns]]

## Connection to Domain 1

D1 T1.1 established that Claude sees only name + description + schema ([[tools-as-descriptions]]). D2 T2.1 is the implementation guide for what those descriptions must contain to avoid misrouting.

Related: [[tools-as-descriptions]], [[five-elements-tool-description]], [[tool-misrouting]], [[generic-tool-splitting]], [[system-prompt-keyword-trap]], [[tool-naming-strategies]], [[tool-description-anti-patterns]], [[minimum-tools-principle]], [[domain-2-overview]]

[Source: 09-TKmhUOlzM6w.md]
