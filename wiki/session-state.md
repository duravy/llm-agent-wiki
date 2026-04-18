---
title: Session State, Resumption, and Forking (D1 T1.7)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

A session is a named, persistent record of a conversation that can be returned to later. Sessions solve the core problem of multi-day investigations: without them, every agent run starts from scratch and all prior analysis is lost. [Source: 08-GbEpxz4wf9s.md]

> **Analogy:** A session is like a notebook at your desk. Close it and go home — your notes are still there next morning. Start a blank notebook and everything is gone.

## CLI Flags

| Flag | Effect |
|------|--------|
| `--session <name>` | Start a new named session with a meaningful label |
| `--resume <name>` | Load that session's full conversation history back into context — all tool results, all analysis, all prior messages |

## Why Sessions Matter

Complex investigations naturally span multiple work periods:

- **Day 1:** Analyze codebase architecture — Claude reads every relevant file, builds a mental model, identifies critical paths. All of this lives in the session history.
- **Day 2:** Write tests — resume the session. Claude remembers exactly what it analyzed. No re-explanation needed.
- **Day 3:** Fix bugs — resume again. Claude's prior context is right there.

Without sessions, you re-explain everything from scratch each day. Sessions turn multi-day work into one continuous investigation.

## Resume vs. Start Fresh — Decision Framework (Exam-Tested)

```
Has the code changed significantly since the session?
├── No → RESUME (context is still valid; saves reanalysis time)
└── Yes → Is the context window mostly exhausted?
           ├── Yes → START FRESH + inject a concise summary of key findings
           └── No  → RESUME + explicitly inform Claude about specific changes
                      so it rereads the affected files before continuing
```

| Scenario | Action | Reason |
|----------|--------|--------|
| Code unchanged, continuing analysis | Resume | Context valid; saves all reanalysis time |
| Code modified, context has room | Resume + inform Claude | Old tool results may be stale — trigger re-read |
| Context window mostly exhausted | Start fresh + inject summary | New messages would compete with stale tool results |
| Completely different approach | Start fresh | Old context actively misleads the new direction |

## The Core Risk: Stale Context

See [[stale-context]] — old tool results in session history may no longer reflect the current state of files. This is the biggest risk of session resumption and the concept the exam tests most heavily.

## Forking

See [[fork-sessions]] — creates an independent copy of the current session at a branch point. Useful when you've done shared analysis and want to explore two approaches in parallel without contaminating one with the other.

## Anti-Patterns

See [[session-anti-patterns]] — five exam-tested failure modes: resuming after heavy code changes, starting fresh when context is valid, never using named sessions, forking for linear tasks, resuming with exhausted context.

## Domain Significance

T1.7 completes Domain 1 (Core API & Agentic Loops). Part 9 begins Domain 2: Tool Design and MCP Integration.

[Source: 08-GbEpxz4wf9s.md]
