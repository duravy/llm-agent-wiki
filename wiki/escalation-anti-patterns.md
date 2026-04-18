---
title: Escalation Anti-Patterns
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Five anti-patterns in escalation design, each causing inconsistent agent behavior or wasted human agent time. These are explicitly exam-tested in the Claude Architect Certification series (D5 T5.2).

## Anti-Pattern 1 — Investigating Before Escalating on Explicit Request

**What happens:** Customer says "I want to speak to a manager." Agent tries to resolve first, thinking it can help.

**Why it's wrong:** Honoring the customer's preference is the priority. The agent's capability is irrelevant when the customer has explicitly requested a human.

**Fix:** Escalate immediately on explicit human request.

---

## Anti-Pattern 2 — Sentiment-Based Escalation

**What happens:** Agent escalates because the customer sounds angry.

**Why it's wrong:** Frustration does not correlate with case complexity. An angry customer may just need a tracking lookup. A calm customer may need a policy exception.

**Fix:** Escalate based on policy gaps and capability limits, not emotional tone. See [[sentiment-escalation-fallacy]].

---

## Anti-Pattern 3 — Guessing When Multiple Accounts Match

**What happens:** Tool returns three "John Smith" accounts. Agent picks the first one.

**Why it's wrong:** May act on entirely the wrong customer's data.

**Fix:** Ask for additional identifiers (email, last 4 digits of phone). Never select on heuristics. See [[ambiguity-resolution]].

---

## Anti-Pattern 4 — Escalating Resolvable Issues

**What happens:** Agent sends a simple password reset or tracking inquiry to a human agent.

**Why it's wrong:** Wastes human agent time on tasks the agent can handle autonomously.

**Fix:** Acknowledge frustration, but attempt resolution first. Only escalate on the three defined triggers. See [[escalation-triggers]].

---

## Anti-Pattern 5 — Vague Escalation Criteria

**What happens:** System prompt says "escalate complex cases." Claude decides differently each time what "complex" means.

**Why it's wrong:** Produces random, inconsistent escalation behavior across conversations.

**Fix:** Define explicit triggers with few-shot examples. See [[explicit-escalation-criteria]].

---

## Quick Reference

| # | Anti-Pattern | Fix |
|---|-------------|-----|
| 1 | Investigate before escalating (explicit request) | Escalate immediately |
| 2 | Sentiment-based escalation | Use policy/capability triggers |
| 3 | Guess on multiple account matches | Ask for identifiers |
| 4 | Escalate resolvable issues | Resolve autonomously first |
| 5 | Vague escalation criteria | Explicit triggers + few-shot examples |

Related: [[escalation-ambiguity-resolution]], [[context-anti-patterns]]

[Source: 27-FOBfTw-9Olo.md]
