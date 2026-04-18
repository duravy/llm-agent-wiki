---
title: Explore Sub-Agent — Isolated Investigation in Plan Mode
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Plan mode uses an explore sub-agent to run the investigation phase in isolation. This is the mechanism that keeps verbose discovery output out of the main conversation context.

## The Problem Without Isolation

A thorough codebase investigation for a large task might involve:
- Reading 45 files
- Running 200 grep searches
- Building a full dependency graph

Without isolation, all of this output runs directly in the main conversation. The context window fills with verbose discovery output before implementation even starts — leaving no space for the actual plan or execution.

## How the Explore Sub-Agent Solves It

```
Explore sub-agent (isolated context):
  [reads 45 files]
  [200 grep searches]
  [maps all dependencies]
  → produces: concise, structured plan

Main conversation:
  [clean plan only]  ← context preserved for review + execution
```

The sub-agent operates in its own separate context. All intermediate discovery stays there. The main conversation receives only the final plan — concise and actionable.

## Connection to Context Isolation Patterns

This is the same isolation principle that appears at multiple layers:

| Layer | Mechanism | Page |
|-------|-----------|------|
| Sub-agent coordination (D1 T1.2) | Each sub-agent starts with isolated context | [[isolated-context]] |
| Session branching (D1 T1.3/T1.7) | Fork sessions copy baseline but diverge independently | [[fork-sessions]] |
| Skill execution (D3 T3.2) | `context:fork` runs skill in isolated sub-agent | [[context-fork]] |
| Plan mode investigation (D3 T3.4) | Explore sub-agent runs in its own context | This page |
| Codebase exploration (D5 T5.4) | Sub-agents explore slices, return summaries | [[subagent-delegation-exploration]] |

The principle is consistent: **isolate verbose intermediate work; surface only the result to the caller**.

## Exam Signal

- "Explore sub-agent" = plan mode's investigation mechanism
- Without it: discovery floods main context before implementation starts (anti-pattern #5; see [[plan-mode-anti-patterns]])
- With it: main conversation sees only the clean plan
- The explore sub-agent is the D3 T3.4 instance of the same isolation pattern from D1 T1.2, D3 T3.2, and D5 T5.4

## Connections

- [[plan-mode-vs-direct-execution]] — T3.4 hub page
- [[plan-mode]] — the mode that uses the explore sub-agent
- [[subagent-delegation-exploration]] — D5 T5.4; same isolation pattern for codebase exploration
- [[isolated-context]] — D1 T1.2; sub-agents start fresh; same isolation principle
- [[context-fork]] — D3 T3.2; skill-layer isolation; same mechanism at a different scope
- [[tool-output-trimming]] — D5 T5.1; another technique to keep main context focused

[Source: 17-96v5N5YGMPM.md]
