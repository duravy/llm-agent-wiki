---
title: Interacting vs Independent Issues
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The fix strategy for iterative refinement: determine whether two issues interact before deciding whether to fix them together or sequentially. Fixing interacting issues one at a time creates cascading failures. [Source: 18-J2rT6yp-Lak.md]

## The Decision Rule

> **Does fixing issue A change the correct behavior of issue B?**
> - Yes → fix them **together in one message**
> - No → fix them **sequentially**

## Interacting Issues — Fix Together

**Example:** A function's type signature changes from `string` to `string | null`, and 5 callers depend on that function.

Fixing only the signature immediately breaks all 5 callers. Fixing only one caller while the signature is wrong produces the wrong result in the other 4.

**Correct approach:** one message — update the signature AND add null checks to all 5 callers at the same time.

**Why cascading failures occur:** A partial fix creates an intermediate state where the system is more broken than before. Each fix step invalidates the next.

## Independent Issues — Fix Sequentially

**Example:** Missing error handling in the auth module AND unused imports in the billing module.

Fixing one has no effect on the correct behavior of the other. You can:
1. Fix auth error handling → review + verify in isolation
2. Fix billing imports → review + verify in isolation

Each fix is reviewable independently. No cascading risk.

## Exam Takeaway

> **Interacting issues → fix together in one message. Independent issues → fix sequentially.** Exam rule: ask yourself whether fixing A changes the correct behavior of B.

## Anti-Pattern

Fixing interacting issues one at a time creates cascading failures — each partial fix breaks something else that was working. See [[iterative-refinement-anti-patterns]].

## Connections

- [[iterative-refinement-techniques]] — T3.5 hub page
- [[iterative-refinement-anti-patterns]] — anti-pattern 3 is this mistake

[Source: 18-J2rT6yp-Lak.md]
