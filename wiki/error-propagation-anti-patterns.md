---
title: Error Propagation Anti-Patterns
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Five anti-patterns in multi-agent error handling, each breaking the system in a different way. Exam-tested in the Claude Architect Certification series (D5 T5.3).

## Anti-Pattern 1 — Generic Error Messages

**What happens:** Sub-agent returns "search unavailable" with no additional context.

**Why it's wrong:** The coordinator cannot determine failure type, what was partially retrieved, or what to try next. Zero recovery options.

**Fix:** Always include structured context: failure type, attempted query, partial results, gap description, and alternative approaches. See [[structured-error-reporting]].

---

## Anti-Pattern 2 — Empty Results as Success (Silent Suppression)

**What happens:** Sub-agent catches a timeout or service failure and returns an empty success with zero results.

**Why it's wrong:** The coordinator thinks the search found nothing relevant. In reality the service was down. Output looks correct but is silently wrong.

**Fix:** Distinguish access failures from valid empty results. Return a structured error so the coordinator knows the difference.

---

## Anti-Pattern 3 — Terminate on First Sub-Agent Failure

**What happens:** One sub-agent fails, so the coordinator kills the entire workflow.

**Why it's wrong:** Discards partial results from all successful sub-agents. User gets zero output instead of partial but useful output.

**Fix:** Continue with partial results from successful sub-agents and annotate coverage gaps. See [[graceful-degradation]].

---

## Anti-Pattern 4 — No Coverage Annotations

**What happens:** Final output presents all findings uniformly, with no indication of which are well-supported and which are gaps.

**Why it's wrong:** The reader cannot distinguish reliable findings from guesses. Silent misinformation.

**Fix:** Always annotate well-supported vs. partial vs. gap areas in the final output. See [[coverage-annotations]].

---

## Anti-Pattern 5 — Propagating Every Transient Error

**What happens:** Every timeout or brief failure is immediately sent up to the coordinator.

**Why it's wrong:** Clutters coordinator context with noise. Forces unnecessary decision-making on recoverable failures.

**Fix:** Handle transient errors locally with retries. Only propagate what cannot be resolved internally. See [[local-vs-propagated-errors]].

---

## Quick Reference

| # | Anti-Pattern | Fix |
|---|-------------|-----|
| 1 | Generic error messages | Structured context with all fields |
| 2 | Empty results as success | Distinguish access failure from no results |
| 3 | Terminate on first failure | Continue with partial results + annotate |
| 4 | No coverage annotations | Label well-supported / partial / gap |
| 5 | Propagate every transient error | Local retry first; propagate only unresolvable |

Related: [[error-propagation-multiagent]], [[context-anti-patterns]], [[escalation-anti-patterns]]

[Source: 28-mOUeDdo8rF0.md]
