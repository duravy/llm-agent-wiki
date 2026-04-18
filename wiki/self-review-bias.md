---
title: Self-Review Bias — Why Same-Session Review Fails
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

Self-review bias occurs when the same Claude session that generates code is asked to review it. The session retains its reasoning context from generation and is systematically less likely to find flaws in its own decisions.

## The Mechanism

Language model sessions accumulate context. When a session generates an OAuth module, it "remembers" every assumption it made:
- Why it chose that token format
- Why it structured the expiry check that way
- Why certain error messages were worded as they were

When the same session reviews the output, it evaluates the code against its own intentions rather than against objective correctness. It reads what it *meant* to write, not what it actually wrote.

## Side-by-Side Example

**Same session:**
```
User: Write an OAuth module.
Claude: [generates code]
User: Review that code.
Claude: "The code looks correct and well-structured."
```
Why: Claude remembers its own assumptions and doesn't question them.

**Separate session:**
```
Session 1: [generates OAuth module]
Session 2: [receives only the code, no generation context]
Session 2 finds: 
  - No rate limiting on login attempts
  - Token expiry check using wrong comparison operator
  - Error messages that leak internal user IDs
```
Why: Session 2 has no reasoning context from generation — it evaluates from first principles.

## The Proofreading Analogy

Reading your own essay right after writing it: you read what you *intended* to write, not what you *actually* wrote. The same pattern appears at a much larger scale in code review.

## Extended Thinking Is Not a Fix

Extended thinking still operates within the same reasoning context. It doesn't create a clean break from the generation session's assumptions. It produces deeper reasoning *biased in the same direction*.

**Fix**: a genuinely separate API call with a fresh session — zero shared history with the generation session.

## Foundation in D3 T3.6

This concept was first introduced in [[session-isolation-ci]] (D3 T3.6): CI code reviews must use fresh sessions. Part 25 (D4 T4.6) extends the concept to a complete [[multi-instance-review|multi-instance architecture]] — per-file passes, cross-file integration, and confidence routing.

## Exam Signal

- *"Same session generates and reviews"* → wrong; self-review bias
- *"Fresh session with no shared history"* → correct; evaluates from first principles
- *"Extended thinking as substitute for fresh review"* → wrong; same reasoning context

Related: [[multi-instance-review]], [[session-isolation-ci]], [[multipass-review-architecture]], [[multi-instance-anti-patterns]]

[Source: 25-W7c9wNfoCjU.md]
