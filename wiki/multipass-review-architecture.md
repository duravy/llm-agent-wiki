---
title: Multi-Pass Review Architecture — Three-Pass Pipeline
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

The three-pass review architecture solves the problems that arise when a large pull request (14+ files) is reviewed in a single pass: [[attention-dilution]], inconsistent depth, and contradictory findings across files.

## Problems with Single-Pass Review of Large PRs

1. **Attention dilution** — early and late files get thorough review; middle files get shallow treatment; bugs in middle files ship
2. **Inconsistent depth** — some files get detailed feedback; others get one superficial comment
3. **Contradictory findings** — a pattern flagged as problematic in file 3 may be approved in file 9 without the model noticing the inconsistency

## The Three-Pass Solution

### Pass 1 — Per-File Local Analysis (Parallel)

**Instances**: one per file — 14 files = 14 parallel instances

**Input**: each instance receives only its assigned file

**Why it works**: each file gets the model's full context window and full attention. No other files compete for attention. No middle-of-the-list attention valley.

**Output**: detailed local review per file — bugs, code quality issues, logic errors

**Key**: all 14 can run simultaneously (independent; no shared state)

### Pass 2 — Cross-File Integration (Single Instance)

**Input**: all 14 files + the summaries from Pass 1

**Focus exclusively on**:
- Data flow inconsistencies between modules
- API contract mismatches (module A expects type X; module B provides type Y)
- Missing error propagation across module boundaries

**Why a single instance**: integration issues require seeing the whole picture simultaneously. Parallel instances can't detect cross-file inconsistencies.

**Why use summaries, not raw output**: feeding raw Pass 1 output would re-introduce attention dilution in the integration pass. Compact summaries preserve the findings without overwhelming the context.

### Pass 3 — Confidence Verification (Optional)

**Input**: all findings from Passes 1 and 2

**Process**: rates each finding 1–5 for confidence

**Routing** (see [[confidence-calibrated-routing]]):
- ≥4 → auto-post as PR comment
- 2–3 → route to human reviewer
- ≤1 → discard

## Full Pipeline Visualization

```
14 changed files
      ↓
Pass 1: 14 parallel instances (one per file)
      → 14 local review summaries
      ↓
Pass 2: 1 integration instance
      Input: all files + 14 summaries
      → integration findings (data flow, API contracts, error propagation)
      ↓
Pass 3: 1 verification instance
      Input: all findings from Passes 1 + 2
      → confidence score per finding
      ↓
≥4 → Auto-post to PR
2–3 → Human reviewer
≤1 → Discard
```

## Why the Cross-File Pass Cannot Be Skipped

Per-file passes catch *within-file* bugs. They cannot detect:
- Module A returning `null` where module B expects a non-null value
- API contracts that were renegotiated in one module but not updated in callers
- Errors that are swallowed at a module boundary and silently fail downstream

Skipping Pass 2 means these classes of bugs are never caught (Anti-pattern #5 in [[multi-instance-anti-patterns]]).

## Connection to Existing Concepts

**[[attention-dilution]]** (D1 T1.6) — Per-file passes are the canonical solution to attention dilution. This architecture implements that solution at the review layer. The D1 T1.6 page already documents the 14-file per-pass + integration pass structure; Part 25 confirms it from the review angle.

**[[phase-summarization]]** (D5 T5.4) — Pass 2 uses summaries of Pass 1 findings, not raw outputs. This is the phase-summarization principle: compact representation prevents context overflow in the next phase.

**[[parallel-vs-sequential-execution]]** (D1 T1.2) — Pass 1 parallelism follows the multi-agent parallel execution model exactly: independent tasks with no data dependencies run simultaneously.

## Exam Signal

> *"14-file PR reviewed in a single pass"* → attention dilution trap
> *"Per-file passes + cross-file integration pass"* → correct architecture

Related: [[multi-instance-review]], [[self-review-bias]], [[attention-dilution]], [[confidence-calibrated-routing]], [[multi-instance-anti-patterns]]

[Source: 25-W7c9wNfoCjU.md]
