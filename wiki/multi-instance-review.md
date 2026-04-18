---
title: Multi-Instance Review Architecture (D4 T4.6)
created: 2026-04-15
last_updated: 2026-04-17
source_count: 1
status: draft
---

Multi-instance review is an architecture where code review is performed by separate Claude instances with no shared history — rather than the same instance reviewing its own output. It addresses self-review bias at scale and provides a systematic solution for large pull requests with many files.

> **Domain Note:** The source video (Part 25) explicitly labels this "task 4.6 in domain 4." The series-overview.md had it placed in Domain 5 based on earlier inference. The video transcript is authoritative — this is D4 T4.6, not D5.
> CONTRADICTION: [[series-overview]] listed Part 25 under Domain 5; the actual source states "task 4.6 in domain 4."

## Core Problem: Self-Review Bias

When the same Claude session generates code and then reviews it:
- The session retains its reasoning context from generation
- It remembers *why* every decision was made
- It is less likely to question its own decisions
- Review returns: "looks correct and well-structured"

A separate session that never saw the generation:
- Receives only the code, zero context about how it was built
- Evaluates the code from first principles
- Catches issues the generator is blind to

See [[self-review-bias]] for the full treatment.

## Three-Pass Architecture

For large pull requests (14+ files), a single-pass review causes [[attention-dilution]], inconsistent depth, and contradictory findings. The solution:

### Pass 1 — Per-File Local Analysis (Parallel)
- Each file reviewed completely alone
- All files run in **parallel** as separate instances
- Each instance gets the model's full attention
- Output: detailed local review per file

### Pass 2 — Cross-File Integration (Single Instance)
- Input: all files + Pass 1 summaries
- Focus: data flow inconsistencies, API contract mismatches, missing error propagation across boundaries
- Catches what per-file passes cannot see

### Pass 3 — Confidence Verification (Optional but Powerful)
- Rates every finding 1–5 for confidence
- Routes by score: ≥4 = auto-post PR comment; 2–3 = human reviewer; ≤1 = discard

See [[multipass-review-architecture]] for the complete pipeline.

## Confidence-Calibrated Routing

The verification pass prevents noise by routing findings to the right destination based on confidence. See [[confidence-calibrated-routing]].

## 5 Anti-Patterns

See [[multi-instance-anti-patterns]].

1. Same session generates and reviews
2. Single pass for 14+ files
3. Extended thinking as substitute for fresh review
4. Auto-posting all findings (no confidence routing)
5. Skipping the cross-file pass

## Exam Decision Framework

| Scenario | Answer |
|---------|--------|
| Review misses issues in a large PR | Multi-pass review architecture |
| Same session reviews its own output | Self-review bias; use separate instance |
| Review quality is inconsistent across files | Per-file passes (attention dilution) |
| Extended thinking as review substitute | Wrong — still same reasoning context |
| All findings posted as PR comments | Add confidence routing pass |

## Exam Tests

- *"What catches issues the code generator is blind to?"* → Separate independent review instance
- *"How to handle inconsistent review quality on large PRs?"* → Multi-pass review architecture

## Cross-Domain Connections

- [[self-review-bias]] — the foundational problem this architecture solves
- [[attention-dilution]] (D1 T1.6) — per-file passes are the solution to attention dilution; same principle, confirmed here from the review angle
- [[session-isolation-ci]] (D3 T3.6) — origin of the self-review bias concept; this extends it to multi-instance architectures
- [[confidence-calibration]] (D5 T5.5) — same confidence-routing architecture; D4 applies it to code review findings, D5 applies it to extraction fields
- [[parallel-vs-sequential-execution]] (D1 T1.2) — Pass 1's parallel per-file instances follow the same parallel execution model
- [[multipass-review-architecture]] — the complete three-pass pipeline
- [[confidence-calibrated-routing]] — the routing thresholds and discard logic

[Source: 25-W7c9wNfoCjU.md, 26-H8Vpe9j4N5I.md, 19-UnN8c0Sshq8.md]
