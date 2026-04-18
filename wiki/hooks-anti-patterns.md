---
title: Hooks Anti-Patterns (D1 T1.5)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five exam-tested anti-patterns for hook implementation in the Claude Agent SDK. These focus on the practical application of hooks for data normalization and policy enforcement — distinct from [[enforcement-anti-patterns|T1.4 enforcement anti-patterns]] which cover broader enforcement design (prompt-only safety, prerequisite skipping, vague escalation). [Source: 06-UTzVVg8Aepc.md]

## Anti-Pattern 1 — Relying on Prompts for Data Consistency

**What it looks like:** Writing "always convert Unix timestamps to ISO format" in the system prompt.

**Why it fails:** Prompts are [[probabilistic-vs-deterministic|probabilistic]] — Claude follows them 95–99% of the time. But 1% of 10,000 requests = 100 failures. Unix timestamps in particular trip Claude up.

**Correct approach:** A [[data-normalization-hooks|post-tool-use hook]] that converts formats deterministically on every single tool result. Code never fails to apply the rule.

---

## Anti-Pattern 2 — Blocking with No Explanation

**What it looks like:** A pre-tool hook blocks a call and returns nothing (or a bare `{"blocked": true}`) to Claude.

**Why it fails:** Claude doesn't know what happened or what to do next. It retries the same blocked call endlessly, or tells the user something unhelpful.

**Correct approach:** Always return a message explaining *why* the call was blocked and *what Claude should do instead* — a redirect to an alternative tool, an escalation path, or a human handoff. Example:

```python
return {
    "blocked": True,
    "message": "Refund amount $750 exceeds $500 limit. This requires manager authorization.",
    "redirect_to": "escalate_manager",
    "escalation_details": {"customer_id": ..., "amount": 750}
}
```

---

## Anti-Pattern 3 — Hooks That Silently Modify Data

**What it looks like:** A post-tool hook changes field values (converts timestamps, maps status codes) without logging the transformations.

**Why it fails:** When Claude produces an unexpected result, you have no way to know whether the hook changed the data or the raw tool returned something wrong. Invisible changes make debugging impossible.

**Correct approach:** Log all transformations — before and after values, field name, tool name. Keep hooks auditable.

---

## Anti-Pattern 4 — Using Hooks for Soft Preferences

**What it looks like:** Registering a hook to enforce response tone, formatting preferences, or non-critical style choices.

**Why it fails:** Hooks run on every tool call and add execution latency each time. Spending that performance budget on preferences (which a prompt handles fine at 99% reliability) is wasteful and adds up at scale.

**Correct approach:** Use prompts for soft preferences. Reserve hooks exclusively for hard rules where 1% failure causes real harm. See [[hooks-decision-framework]].

---

## Anti-Pattern 5 — No Redirect When Blocking

**What it looks like:** A pre-tool hook blocks a call without providing an alternative action for Claude to take.

**Why it fails:** Claude is stuck. It blocked the intended tool and has no path forward — it cannot serve the user. This is a dead end in the agent loop.

**Correct approach:** Always provide an alternative path: escalate to a human, call a different tool, or return a structured message explaining the user's next steps. A block without a redirect is an incomplete enforcement pattern.

---

## Summary Table

| Anti-pattern | Root cause | Correct principle |
|-------------|-----------|------------------|
| Prompts for data consistency | Underestimating probabilistic failure rate | Post-tool hooks — [[data-normalization-hooks]] |
| Blocking with no explanation | Claude left without direction | Always include reason + redirect |
| Silent data modification | No audit trail | Log all hook transformations |
| Hooks for soft preferences | Misclassifying guideline as hard rule | [[hooks-decision-framework]] — 1% test |
| No redirect on block | Incomplete enforcement path | Always provide an alternative action |

[Source: 06-UTzVVg8Aepc.md]
