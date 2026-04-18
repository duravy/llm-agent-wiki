---
title: The /compact Command
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

The `/compact` command instructs Claude Code to summarize the current conversation, compressing verbose tool output and accumulated history to free context space for new work. It is a key tool for combating [[context-degradation]] in extended codebase exploration sessions.

## The Meeting Notes Analogy

At the end of a long meeting, you summarize the discussion into key decisions — freeing your notebook for new work. `/compact` does this for the conversation context: hours of tool output compressed into the decisions and findings that matter.

## When to Use It

**Use proactively** — when you notice:
- Responses becoming vague or citing "typical patterns" instead of specific facts
- Many tool results have accumulated in the session
- The session has been running for a long time with heavy file exploration

**Do not wait** until the model is already confused. At that point, the specific details are already lost and compaction cannot fully recover them.

## What Happens

Claude summarizes the conversation, retaining key findings and decisions while compressing or discarding verbose intermediate tool outputs. The context is now freed for continued exploration.

## Exam Signal

"If a question describes vague generic responses during extended exploration, `/compact` is one of the correct answers."

The other correct answer in that scenario is typically [[subagent-delegation-exploration]] or [[scratchpad-files]] — used proactively before degradation, not after.

## Relationship to Other Techniques

| Situation | Tool |
|-----------|------|
| Context already filling up | `/compact` |
| Starting a long session | Set up [[scratchpad-files]] early |
| Many files to read | [[subagent-delegation-exploration]] |
| Multi-hour investigation | [[state-persistence-crash-recovery]] |
| Context fully exhausted across sessions | Start fresh + inject summary — see [[stale-context]] |

## D1 T1.7 Connection: Session Resumption

When deciding whether to resume a named session, `/compact` is the first line of defence against a filling context window — use it proactively before degradation sets in. If the context is already mostly exhausted by the time you return to a session, `/compact` may not be enough: the correct action is to start a fresh session and inject a concise summary of key findings from the prior session. See [[stale-context]] Strategy 2 and [[session-anti-patterns]] Anti-pattern 5. [Source: 08-GbEpxz4wf9s.md]

Related: [[large-codebase-context]], [[context-degradation]], [[large-codebase-anti-patterns]], [[session-state]], [[stale-context]]

[Source: 29-LRUdrMbFheA.md, 08-GbEpxz4wf9s.md]
