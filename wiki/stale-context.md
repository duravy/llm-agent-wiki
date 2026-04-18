---
title: Stale Context — The Core Risk of Session Resumption
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Stale context is the primary risk when resuming a named session. Old tool results stored in the conversation history may no longer reflect the current state of the codebase. Claude reasons from an outdated snapshot, and that incorrect mental model propagates into every subsequent analysis. [Source: 08-GbEpxz4wf9s.md]

## The Problem

When you resume a session, all prior tool results are still present in the conversation history — but the actual files may have changed since those tools ran.

> **Example:** Claude's session history shows `auth.py` has 45 lines. A teammate added validation logic between sessions. Now it has 60 lines with new branching. Claude doesn't know. It continues reasoning as if the file is still 45 lines — and that incorrect model shapes every analysis that follows.

This is not Claude hallucinating. It is Claude faithfully reasoning from the evidence it has — evidence that is no longer accurate.

## Why It Propagates

A single stale tool result corrupts downstream reasoning:

1. Claude's understanding of `auth.py` is wrong
2. Its analysis of code paths that call `auth.py` is wrong
3. Its test coverage recommendations are wrong
4. Its bug-fix suggestions are wrong

The error compounds silently. There is no explicit signal that the mental model is outdated.

## Two Mitigation Strategies (Exam-Tested)

### Strategy 1 — Inform the Resumed Session

Resume the session, then immediately tell Claude what changed:

> *"Since our last session, `auth.py` was updated with new validation logic. Please re-read it before continuing."*

This signals that the old mental model is outdated, prompting Claude to use tools to refresh its understanding. Best when: code changes are known and scoped; context window still has room.

### Strategy 2 — Start Fresh with an Injected Summary

Instead of resuming, open a new session with a structured briefing of what the prior analysis found:

> *"The code has changed. Prior analysis found: [key findings]. Please re-analyze and confirm or revise these findings."*

You preserve the key insights without carrying stale tool results that could corrupt the new session's reasoning. The summary replaces the history cleanly. Best when: code changes are broad or unknown; context window is mostly exhausted.

## Relationship to the Decision Framework

Stale context is the main reason the [[session-state|resume-vs-fresh decision]] branches on whether the code has changed:

- **No change** → resume freely (context is accurate)
- **Change + room in context** → resume + Strategy 1 (inform)
- **Change + exhausted context** → Strategy 2 (start fresh + inject summary)

## Connection to Phase Summarization

Strategy 2 (injected summary) is the same principle as [[phase-summarization]]: pass a concise summary of prior findings to the next phase, not raw history. The insight — integrate from summaries, not raw output — applies equally to multi-day sessions.

## Connection to Exhausted Context Windows

A related but distinct problem: even with no code changes, a context window filled with thousands of tool results degrades quality — new messages compete with stale results for attention. See [[session-anti-patterns]] Anti-pattern 5 and [[compact-command]] for the proactive mitigation.

[Source: 08-GbEpxz4wf9s.md]
