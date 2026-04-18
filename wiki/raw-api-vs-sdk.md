---
title: RAW API vs Claude Agent SDK
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The RAW API and Claude Agent SDK are two levels of abstraction for building Claude-powered agents. The exam tests one specific distinction between them: an API is a library; a framework calls your code. The SDK is a framework.

## Side-by-Side

| | RAW API | Claude Agent SDK |
|---|---|---|
| **What it is** | Direct HTTP calls to `/messages` endpoint | Python framework built on top of the RAW API |
| **What you manage** | Every loop, every retry, every context window | SDK handles loop, tool handling, and sub-agent spawning |
| **Abstractions provided** | None — you work with raw messages arrays | Agents, tools, sessions as structured building blocks |
| **Who controls the loop** | Your code | The SDK |
| **Mental model** | Library: you call it when you want | Framework: it calls your code |

## The Exam Distinction

> **"An API is a library. You call it whenever you want. A framework calls your code. It's in control of the loop. The SDK is a framework."**

This is directly tested. The question may be phrased as "which of the following best describes the relationship between the RAW API and the Claude Agent SDK?" — or it may appear as a scenario where a developer is "managing every retry manually" (RAW API) vs "using agents and sessions" (SDK).

## Why This Matters

The [[task-tool|task tool]] and [[agent-definition|agent definition]] constructs only exist in the SDK — not in the RAW API. The [[agentic-loop|agentic loop]] pattern exists in both, but the SDK enforces the structure automatically.

When the exam says "the SDK manages sub-agent spawning," it means the SDK intercepts the `task` tool call and handles launching the sub-agent — your code doesn't do that manually. This is the framework calling your code, not you calling the API.

## Connection to Agentic Loop

In the RAW API, the developer writes the `while True` loop, the `stop_reason` check, and the tool executor manually (see [[agentic-loop-steps]]). In the SDK, those structural concerns are handled by the framework — you define agents and tools; the SDK orchestrates the loop.

[Source: 04-bFdRH69h9LU.md]
