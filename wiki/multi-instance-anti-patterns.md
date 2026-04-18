---
title: Multi-Instance Review Anti-Patterns (D4 T4.6)
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

Five anti-patterns for multi-instance review architecture. The exam presents scenarios where each looks reasonable but is wrong.

## Anti-Pattern 1: Same Session Generates and Reviews

**What it looks like**: after generating an OAuth module, ask the same Claude session to review it.

**Why it's wrong**: the session retains its reasoning context from generation — it remembers every decision it made and won't question them. Review returns "looks correct."

**Fix**: use a separate independent review instance with no shared history. See [[self-review-bias]].

## Anti-Pattern 2: Single Pass for 14+ Files

**What it looks like**: send all 14 changed files in one review prompt.

**Why it's wrong**: [[attention-dilution]] — middle files get shallow treatment; bugs in files 5–10 ship to production. Review depth is inconsistent across files.

**Fix**: split into per-file local analysis passes (parallel) + a cross-file integration pass. See [[multipass-review-architecture]].

## Anti-Pattern 3: Extended Thinking as Substitute for Fresh Review

**What it looks like**: use extended thinking mode on the generation session to "think harder" about the review.

**Why it's wrong**: extended thinking still operates within the same reasoning context. It is deeper reasoning biased in the same direction. Self-review bias is not eliminated by thinking more — only by creating a clean context break.

**Fix**: a genuinely separate API call with a fresh session.

## Anti-Pattern 4: Auto-Posting All Findings as PR Comments

**What it looks like**: take every finding from every pass and post it directly to the pull request.

**Why it's wrong**: low-confidence findings create noise. Reviewers start ignoring comments. Trust in the review system erodes. Limited human review capacity gets wasted on false positives.

**Fix**: add a verification pass with [[confidence-calibrated-routing]] — ≥4 auto-post, 2–3 human review, ≤1 discard.

## Anti-Pattern 5: Skipping the Cross-File Pass

**What it looks like**: run per-file passes only; skip Pass 2 to save API calls.

**Why it's wrong**: per-file passes only catch within-file issues. They cannot detect data flow inconsistencies between modules, API contract mismatches, or missing error propagation across boundaries. These integration bugs are invisible to per-file passes.

**Fix**: always include Pass 2 (cross-file integration) after the per-file passes.

## Summary Table

| Anti-Pattern | Root Cause | Fix |
|-------------|-----------|-----|
| Same-session review | Ignored self-review bias | Separate instance, no shared history |
| Single pass, large PR | Ignored attention dilution | Per-file passes + integration pass |
| Extended thinking substitute | Confused deep thinking with fresh context | Fresh API call |
| Auto-post all findings | No confidence filtering | Verification pass with routing |
| Skip cross-file pass | Optimized for cost, not coverage | Always include integration pass |

Related: [[multi-instance-review]], [[self-review-bias]], [[multipass-review-architecture]], [[confidence-calibrated-routing]], [[attention-dilution]]

[Source: 25-W7c9wNfoCjU.md]
