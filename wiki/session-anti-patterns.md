---
title: Session Anti-Patterns (D1 T1.7)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five exam-tested anti-patterns for session state management. These are distinct from [[decomposition-anti-patterns|T1.6 decomposition anti-patterns]] — these focus on how sessions are started, resumed, and forked across multi-day investigations. [Source: 08-GbEpxz4wf9s.md]

## Anti-Pattern 1 — Resuming After Heavy Code Changes

**What it looks like:** Resuming a session after teammates have modified files that the prior session analyzed, without informing Claude of the changes.

**Why it fails:** [[stale-context|Stale context]] — old tool results no longer match the actual files. Claude reasons from an outdated snapshot. The incorrect mental model propagates into every analysis that follows.

**Correct approach:** Either (a) resume and immediately tell Claude exactly what changed so it re-reads the affected files, or (b) start a fresh session and inject a structured summary of prior findings.

---

## Anti-Pattern 2 — Starting Fresh When Context Is Still Valid

**What it looks like:** Opening a new session when the code hasn't changed and the prior session's context window still has room.

**Why it fails:** This throws away accurate, valuable analysis. Claude must re-read all the files it already analyzed, re-build its mental model from scratch, and re-identify all the patterns it already found. Pure wasted time.

**Correct approach:** Resume when prior context is accurate. Save "start fresh" for when code has changed significantly, context is exhausted, or you're exploring a completely different approach.

---

## Anti-Pattern 3 — Never Using Named Sessions

**What it looks like:** Running multi-day investigation work without the `--session` flag — using unnamed, ephemeral sessions.

**Why it fails:** All progress is lost when the terminal closes. The next day, the investigation starts from zero — no analysis, no mental model, no tool results. For any work that spans multiple sessions, this is pure waste.

**Correct approach:** Use named sessions (`--session <meaningful-label>`) for any multi-day investigation. The naming discipline also makes it clear which work is in which session.

---

## Anti-Pattern 4 — Forking for Simple Linear Tasks

**What it looks like:** Using `fork_session` for a task that follows a single, straightforward path — no genuine alternatives to compare.

**Why it fails:** [[fork-sessions|Forking]] adds complexity (managing multiple branches, comparing results, synthesizing outcomes) that serves no purpose when there is only one reasonable approach. The overhead is unjustified.

**Correct approach:** Fork only when you have a genuine decision point — two or more alternative strategies, architectures, or approaches that need to be evaluated independently in parallel.

---

## Anti-Pattern 5 — Resuming with an Exhausted Context Window

**What it looks like:** Resuming a session that is already mostly full of tool results from prior work.

**Why it fails:** New messages must compete with thousands of stale tool results for model attention — the [[lost-in-the-middle-effect|lost-in-the-middle effect]] at session scale. You lose the benefit of resumption anyway, because the valuable prior context is buried and degraded. The session effectively behaves like a confused new one.

**Correct approach:** Start a fresh session and inject a concise summary of the key findings from the prior session. The summary gives Claude exactly what it needs without the noise of accumulated intermediate steps.

---

## Summary Table

| Anti-pattern | Root cause | Correct principle |
|-------------|-----------|------------------|
| Resume after heavy code changes | [[stale-context\|Stale context]] propagates silently | Inform Claude of changes, or start fresh + inject summary |
| Start fresh when context is valid | Throws away accurate analysis | Resume when context is still accurate |
| Never use named sessions | Progress lost on terminal close | `--session` flag for all multi-day work |
| Fork for linear tasks | Unnecessary complexity overhead | Fork only when genuinely comparing alternatives |
| Resume with exhausted context | New messages buried by stale results | Start fresh + inject concise summary |

[Source: 08-GbEpxz4wf9s.md]
