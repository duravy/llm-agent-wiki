---
title: Session Isolation for CI Reviews
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

CI code reviews must run in fresh sessions — never as continuations of the session that generated the code. The session that wrote the code remembers its own reasoning and is biased against finding flaws in its own decisions. [Source: 19-UnN8c0Sshq8.md]

## The Self-Review Bias

When the same Claude session generates code and then reviews it:
- The model retains its reasoning context
- It remembers *why* it made every decision
- It is less likely to question those decisions
- Review tends to return: "it looks good"

This is not a bug — it's how language models work. Generation and review share the same context window; the generator's intent colors the reviewer's judgment.

## The Fix: Fresh CI Sessions

A CI pipeline session:
- Starts fresh with no memory of the generation
- Reviews the code as submitted, not as intended
- Is far more likely to catch real issues

CI reviews are structurally isolated by default. Each pipeline run is a new process with a new context.

## Connection to the Isolation Pattern

Session isolation for CI reviews is the same principle applied in:

| Domain | Pattern | Isolation purpose |
|---|---|---|
| D1 T1.2 | [[isolated-context]] | Sub-agents start fresh; no inherited coordinator context |
| D3 T3.2 | [[context-fork]] | Skills run in isolated sub-agent; main context stays clean |
| D3 T3.4 | [[explore-sub-agent]] | Investigation isolated from main context |
| D3 T3.6 | Session isolation (CI) | Review isolated from generation session |
| D5 T5.4 | [[subagent-delegation-exploration]] | File exploration isolated from decision context |

**The shared principle:** isolation prevents context contamination. In CI, the contaminant is the generator's reasoning.

## Connection to Multi-Instance Review (D4 T4.6)

[[multi-instance-review]] (Part 25, D4 T4.6) extends this concept to multi-instance review architectures — multiple independent Claude instances reviewing the same artifact to counteract self-review bias at scale. This page establishes the bias; Part 25 builds the complete architectural solution: per-file parallel passes, cross-file integration, and [[confidence-calibrated-routing]].

## Exam Takeaway

> **CI reviews must always be fresh sessions. The same session that generated code is biased — it remembers its own decisions.** Anti-pattern: asking the generation session to review its own output.

[Source: 19-UnN8c0Sshq8.md]
