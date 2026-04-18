---
title: Scratchpad Files
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Scratchpad files are persistent external memory files that an agent writes to during an investigation, allowing key findings to be recovered after context degrades. They are the primary solution to [[context-degradation]] in long codebase exploration sessions.

## The Detective's Notebook Analogy

Instead of keeping every clue in your head, you write findings down so you can reference them later. Even after your memory fades, the notes remain. The scratchpad works the same way — the agent externalizes its discoveries so they survive context compression.

## How It Works

**During exploration** — as the agent discovers key facts, it writes them to the scratchpad immediately:
```
auth/manager.ts: AuthManager uses JWT with RS256 signing
auth/manager.ts:45: verify() checks expiry but NOT signature validation  
auth/refresh.ts: Token refresh logic lives here
auth/refresh.ts: Race condition in concurrent refreshes — FLAGGED
```

**When context fills** — instead of losing findings to compression:
1. Agent reads the scratchpad file
2. Recovers specific discoveries without re-reading analyzed files
3. Investigation continues with full context preserved

## What to Write

- Specific class names, file paths, line numbers
- Discovered bugs or anomalies (flag these explicitly)
- Architectural connections between files
- Findings that would be expensive to re-derive

**Do not write** raw file contents — that defeats the purpose. Write compressed, specific facts.

## Relationship to Other Techniques

Scratchpad files work alongside [[subagent-delegation-exploration]] — sub-agents can write to the same scratchpad, giving the main agent a consolidated record of all sub-agent findings without filling context with raw output.

See also: [[state-persistence-crash-recovery]] for the full manifest approach in multi-hour sessions.

## Exam Signal

"If the exam asks how to keep context clean during large codebase exploration: subagent delegation + scratchpad files."

Related: [[large-codebase-context]], [[context-degradation]], [[large-codebase-anti-patterns]]

[Source: 29-LRUdrMbFheA.md]
