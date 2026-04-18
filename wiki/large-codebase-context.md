---
title: Large Codebase Context (D5 T5.4)
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Large codebase context management covers strategies for keeping Claude's responses precise and specific during extended code exploration sessions. Without deliberate techniques, context degrades over time — early precise answers give way to vague, generic claims. This is Domain 5, Task 5.4 of the [[series-overview|Claude Architect Certification Exam series]].

## The Core Problem: Context Degradation

See [[context-degradation]].

Early in a session: precise answers — specific class names, file paths, line numbers.
Late in a session: vague answers — "the authentication system uses typical patterns."

The model has lost specifics and is filling in with generic knowledge. Recognize the signal early.

## Five Techniques

| # | Technique | Wiki Page | What it solves |
|---|-----------|-----------|---------------|
| 1 | Scratchpad files | [[scratchpad-files]] | Findings lost as context degrades |
| 2 | Subagent delegation | [[subagent-delegation-exploration]] | Main agent context filled with raw file content |
| 3 | `/compact` command | [[compact-command]] | Verbose tool output accumulation |
| 4 | Structured state persistence | [[state-persistence-crash-recovery]] | Crash loses hours of investigation work |
| 5 | Summarize between phases | [[phase-summarization]] | Raw phase 1 output pollutes phase 2 context |

## Five Anti-Patterns

See [[large-codebase-anti-patterns]] for the full list.

## Exam Mnemonics

- **Keep context clean during codebase exploration** → subagent delegation + scratchpad files
- **Crash recovery** → structured state persistence
- **Vague responses / degradation signal** → use `/compact` proactively + delegate to sub-agents

## Series Context

- Part 28 covered [[error-propagation-multiagent]] (D5 T5.3)
- Part 29 (this source) covers D5 T5.4
- Part 30 covers [[human-review-workflows]]

[Source: 29-LRUdrMbFheA.md]
