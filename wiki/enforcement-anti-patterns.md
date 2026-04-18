---
title: Enforcement Anti-Patterns (D1 T1.4)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five exam-tested anti-patterns for enforcement and handoff design in the Claude Agent SDK. These are distinct from [[sdk-anti-patterns|SDK implementation anti-patterns]] (T1.3) and [[multi-agent-anti-patterns|coordinator architecture anti-patterns]] (T1.2) — these focus on safety enforcement and handoff quality.

## Anti-Pattern 1 — Prompt-Only Safety

**What it looks like:** Writing "don't run payment without validation" in a system prompt and trusting Claude to comply.

**Why it fails:** [[probabilistic-vs-deterministic|Probabilistic compliance]]. Claude usually follows the rule, but can be persuaded by a convincing context. Prompts are best effort.

**Correct approach:** A [[prerequisite-gates|code gate]] that physically blocks the payment tool until validation has set the success flag. Gates are guaranteed.

---

## Anti-Pattern 2 — Skipping Prerequisites

**What it looks like:** Claude calls tools out of order because it reasons that skipping a step is efficient or safe in this specific case.

**Why it fails:** Claude's in-context judgment overrides the intended ordering. This is [[probabilistic-vs-deterministic|probabilistic]] — Claude can rationalize skipping a step.

**Correct approach:** Enforce ordering with [[prerequisite-gates|prerequisite gate checks]] in code. Never assume Claude will respect ordering from a prompt alone.

---

## Anti-Pattern 3 — Vague Escalation Passing

**What it looks like:** Agent passes "I got confused" or "I'm not sure what to do" to the human reviewer.

**Why it fails:** A human cannot act on vague. They receive no options, no context, no recommended path forward — the escalation is useless.

**Correct approach:** [[structured-escalation-handoff|Structured handoff]] with all four fields: `action_requested`, `context`, `options`, `recommended_action`.

---

## Anti-Pattern 4 — Ignoring Hook Return Values

**What it looks like:** [[hooks|Hooks]] are registered, but the code doesn't check whether a pre-tool hook signaled a block. The tool executes anyway.

**Why it fails:** Hook decisions are not automatically propagated — if your framework integration doesn't check the hook's return/raise, the hook has no effect. You lose the deterministic enforcement hooks provide.

**Correct approach:** Always propagate hook decisions. If a pre-tool hook raises or signals block, the tool must not run.

---

## Anti-Pattern 5 — Parallelizing Dependent Concerns

**What it looks like:** Launching parallel concern agents when their work depends on shared state or one needs the other's output.

**Why it fails:** Race conditions or incorrect results when agent B starts before agent A has produced the input it needs. [[parallel-vs-sequential-execution|Parallel execution is only correct for independent tasks]].

**Correct approach:** Parallelize only truly independent investigations. If concern B needs concern A's output, they must run sequentially.

---

## Summary Table

| Anti-pattern | Root cause | Correct principle |
|-------------|-----------|------------------|
| Prompt-only safety | Relying on probabilistic compliance | Code gates — [[prerequisite-gates]] |
| Skipping prerequisites | Claude re-orders for efficiency | Code-enforced ordering |
| Vague escalation | No actionable context | [[structured-escalation-handoff]] |
| Ignoring hook values | Missing hook propagation | Always check + propagate hook decisions |
| Parallelizing dependent concerns | Misclassifying dependency | Independent only — [[parallel-vs-sequential-execution]] |

> **See also:** [[hooks-anti-patterns]] (T1.5) — five practical anti-patterns for hook implementation specifically: prompts for data consistency, blocking with no explanation, silent data modification, hooks for soft preferences, no redirect on block.

[Source: 05-Ow0fCviHxrY.md]
