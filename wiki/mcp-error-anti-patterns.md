---
title: MCP Error Response Anti-Patterns (D2 T2.2)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five exam-tested anti-patterns in MCP tool error response design. All appear as wrong answer choices or scenario setups. Recognizing them quickly is a core D2 T2.2 skill.

## Anti-Pattern Summary

| # | Anti-Pattern | What Goes Wrong | Fix |
|---|-------------|-----------------|-----|
| 1 | Generic errors | Claude has no decision data | Include error_category, is_retryable, and suggestion |
| 2 | Empty results for failures | Silent wrong answer to user | Set is_error: true for failed operations |
| 3 | Retrying business rule violations | Wasted tokens; rules don't change | Set is_retryable: false for business errors |
| 4 | Propagating all errors to coordinator | Unnecessary escalation traffic | Handle transient errors locally in sub-agents |
| 5 | No partial results in escalation | Coordinator starts from scratch | Always include what was attempted and alternatives |

---

## Anti-Pattern 1: Generic Errors

**What it looks like**: Tool returns `"operation failed"` or `{ "error": "something went wrong" }`

**The problem**: Claude reads this and has no basis for deciding: retry? escalate? explain? A generic string forces Claude to guess, and guessing produces inconsistent behavior.

**The fix**: Return all five structured fields — `error_category`, `is_retryable`, `message`, `customer_message` (business only), `suggestion`.

---

## Anti-Pattern 2: Returning Empty Results for Failures

**What it looks like**: Database times out → tool returns `{ "is_error": false, "results": [] }`

**The problem**: Claude interprets this as "service worked, found nothing." Claude tells the user they have no records. The user has records — the database was just down. Silent wrong answer.

**The fix**: Failed operations always set `is_error: true`. Empty arrays are only correct when the operation genuinely succeeded and found no matches. See [[access-failure-vs-empty-result]].

---

## Anti-Pattern 3: Retrying Business Rule Violations

**What it looks like**: Tool returns business error → Claude retries → business error again → loop

**The problem**: Business rules do not change between attempts. A $500 refund limit is still $500 on the second call. Retrying burns tokens and confuses the user with repeated failures.

**The fix**: Set `is_retryable: false` on all business errors. The `is_retryable` field exists precisely to prevent this loop.

---

## Anti-Pattern 4: Propagating All Errors to the Coordinator

**What it looks like**: Sub-agent encounters timeout → immediately escalates to coordinator → coordinator handles retry

**The problem**: Transient errors are recoverable locally. Escalating them adds latency (coordinator round-trip), increases coordinator load, and fragments the retry logic across two agents unnecessarily.

**The fix**: Sub-agents handle transient errors with local retries. Only escalate failures that cannot be resolved internally. See [[local-error-recovery]].

---

## Anti-Pattern 5: No Partial Results in Escalation Reports

**What it looks like**: Sub-agent exhausts retries → escalates with only `{ "error": "all retries failed" }` → coordinator has no context

**The problem**: The coordinator must start from scratch. It doesn't know what was attempted, what partial data exists, or what alternatives to try. This produces a full pipeline stall when partial recovery was possible.

**The fix**: Every escalation report includes: what was attempted (query, service, retry count), any partial results retrieved, and alternatives the coordinator can try instead.

---

## Exam Application

When a scenario presents a tool failure and asks what went wrong:
1. Did the tool return a generic error string? → AP1
2. Did the tool return empty results for a failed operation? → AP2
3. Did Claude retry a business rule violation? → AP3
4. Did a sub-agent escalate a transient error before retrying? → AP4
5. Did the escalation report omit partial results? → AP5

Related: [[mcp-error-responses]], [[mcp-error-categories]], [[mcp-error-fields]], [[is-error-flag]], [[access-failure-vs-empty-result]], [[local-error-recovery]]

[Source: 10-cMCoA_MGDdw.md]
