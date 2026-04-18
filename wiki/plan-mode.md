---
title: Plan Mode — When and How to Use It
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Plan mode makes Claude explore, analyze, and propose a plan before touching any file. The defining feature is a **human review checkpoint** between investigation and execution. No code changes until you approve.

## The Four Scenarios That Require Plan Mode

### 1. Multiple Valid Approaches
When there's no obvious single answer — choosing between microservices vs modular monolith, selecting an integration pattern — you need Claude to map the options before committing to one. Direct execution would pick arbitrarily.

### 2. Large Scope Changes Touching Many Files
Multi-file changes carry real risk: discovering a dependency problem halfway through forces costly undo work and leaves the codebase in a broken intermediate state. Plan mode maps the problem before a single file changes.

### 3. Architectural Decisions
Integration pattern choices with different infrastructure requirements, data model restructuring, library migrations — all require upfront exploration of consequences.

### 4. Risk of Expensive Rework
If picking the wrong approach means hours of undoing, the upfront cost of planning pays for itself immediately.

**Exam signal**: monolith-to-microservices restructuring → plan mode. Large library migration → plan mode.

## How Plan Mode Works

1. **Explore** — Claude investigates the codebase using the [[explore-sub-agent]]; reads files, maps dependencies, identifies natural boundaries
2. **Analyze** — identifies options, trade-offs, potential problems
3. **Propose** — presents a phased plan to you
4. **Human review** — you review and adjust; no files have changed yet
5. **Execute** — only after your approval does implementation begin (usually with direct execution per step)

## The Human Review Checkpoint

The human review checkpoint is **the entire point of plan mode**. It catches design problems before they become implementation debt. Skipping this step — running plan mode but approving immediately without reading — eliminates the only benefit. See [[plan-mode-anti-patterns]] anti-pattern #4.

## The Explore Sub-Agent

Plan mode uses an [[explore-sub-agent]] to run the investigation in isolation. The sub-agent can read 45 files and run 200 grep searches without flooding the main conversation context. The main conversation only sees the clean, concise plan. See [[explore-sub-agent]] for full detail.

## Microservices Example

**Without plan mode**: Claude starts splitting files immediately → discovers dependency problems halfway through → has to undo partial changes and restart. Costly rework, broken intermediate state.

**With plan mode**: Claude first maps the dependencies, identifies natural service boundaries, proposes a phased restructuring plan. You review. Only after approval does a single line of code change.

## Exam Signal

- Monolith-to-microservices restructuring → always plan mode
- Large migration (Express → Fastify, REST → GraphQL) → plan mode
- "Multiple valid approaches" phrasing → plan mode
- "Risk of expensive rework" → plan mode
- "Human review before any file changes" → plan mode description

## Connections

- [[plan-mode-vs-direct-execution]] — hub page; decision framework
- [[direct-execution]] — the other mode; when to use each
- [[explore-sub-agent]] — how plan mode keeps investigation out of main context
- [[plan-mode-anti-patterns]] — five exam-tested wrong answers
- [[human-review-workflows]] — D5 T5.5; human review as a safety net (different domain context, same principle)

[Source: 17-96v5N5YGMPM.md]
