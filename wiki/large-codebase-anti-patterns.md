---
title: Large Codebase Context Anti-Patterns
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Five anti-patterns in large codebase context management. The exam presents scenarios where you must identify which anti-pattern is causing the problem. Part of D5 T5.4 of the [[series-overview|Claude Architect Certification Exam series]].

## Anti-Pattern 1 — Main Agent Reads All Files Directly

**What happens:** The main agent reads every relevant file itself. Context fills with verbose raw file content. No space remains for synthesis or decision-making.

**Fix:** Delegate file reading to sub-agents. Main agent receives concise summaries. See [[subagent-delegation-exploration]].

---

## Anti-Pattern 2 — No Scratchpad for Extended Investigation

**What happens:** The agent relies entirely on context memory. As the session grows, key findings from early in the session are lost to context compression. Responses become vague.

**Fix:** Write key findings to a scratchpad file as they are discovered. Reference the file when context fills. See [[scratchpad-files]].

---

## Anti-Pattern 3 — Ignoring Context Degradation Signals

**What happens:** The agent notices responses getting vague but continues without intervention. By the time action is taken, precision is already gone.

**Signal:** Responses citing "typical patterns" instead of specific classes, methods, and line numbers.

**Fix:** Use `/compact` proactively when signals appear. Delegate to sub-agents before degradation sets in. See [[compact-command]] and [[context-degradation]].

---

## Anti-Pattern 4 — No State Persistence

**What happens:** A crash during a multi-hour investigation loses all accumulated findings. The investigation must restart from scratch.

**Fix:** Export a structured manifest file throughout the investigation so work can be recovered. See [[state-persistence-crash-recovery]].

---

## Anti-Pattern 5 — Passing Raw Phase 1 Output to Phase 2

**What happens:** Phase 2 sub-agents receive the full raw output from phase 1 as context. Their context windows fill with irrelevant detail before they begin their own work.

**Fix:** Summarize phase 1 findings into a concise brief before spawning phase 2 agents. See [[phase-summarization]].

---

## Quick Reference

| # | Anti-Pattern | Fix |
|---|-------------|-----|
| 1 | Main agent reads all files | Delegate to sub-agents for summaries |
| 2 | No scratchpad | Write findings to scratchpad file |
| 3 | Ignoring degradation signals | Compact proactively; delegate early |
| 4 | No state persistence | Export manifest file for crash recovery |
| 5 | Raw phase 1 output to phase 2 | Summarize before injecting |

Related: [[large-codebase-context]], [[context-anti-patterns]], [[error-propagation-anti-patterns]], [[escalation-anti-patterns]]

[Source: 29-LRUdrMbFheA.md]
