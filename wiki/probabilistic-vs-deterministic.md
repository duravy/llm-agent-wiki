---
title: Probabilistic vs Deterministic Compliance
created: 2026-04-16
last_updated: 2026-04-16
source_count: 2
status: draft
---

The most fundamental enforcement distinction in the exam. There are two ways to make Claude follow a rule — only one of them actually guarantees it.

## The Distinction

| | Probabilistic | Deterministic |
|---|---|---|
| **Mechanism** | Claude's own judgment follows your instructions | Your code enforces the rule |
| **Reliability** | "Usually works" — Claude reads the rule and generally follows it | Guaranteed — check runs in code, cannot be bypassed |
| **Can it be overridden?** | Yes — Claude can be persuaded by a convincing context | No — code doesn't reason, it enforces |
| **Implementation** | System prompt rules | [[prerequisite-gates]], [[hooks]], code checks |
| **When to use** | Style preferences, tone, non-critical ordering, correctable mistakes | Safety, compliance, financial transactions, irreversible actions |

## Why Probabilistic Isn't Enough for Safety

Claude is a language model. It can be persuaded. Given a convincing context, it might skip a step it normally follows. This is not a bug — it's how LLMs work. The implication: **any rule Claude can reason around is not a guarantee.**

**The production math:** Prompt instructions work 95–99% of the time. But 1% of 10,000 requests = **100 failures**. For data feeding downstream systems, financial transactions, or compliance checks, those 100 failures cause real harm. This is why [[hooks|hooks]] and [[prerequisite-gates|gates]] exist. [Source: 06-UTzVVg8Aepc.md]

> **"Safety-critical constraints must be code-enforced. Never rely on a prompt alone for anything that causes real harm if skipped."**

## The Production Rule

- **Prompt guidance** = best effort
- **Code enforcement** = guaranteed

This single framing answers the majority of enforcement questions on the exam. When a question asks which approach to use for a given constraint, apply the irreversibility test (see [[code-vs-prompt-enforcement]]).

## Connection to Hooks and Gates

The two primary code-enforcement mechanisms:
- [[prerequisite-gates]] — block a downstream tool until a prerequisite has succeeded
- [[hooks]] — intercept every tool call automatically; pre-tool hooks can block or modify; post-tool hooks can validate or sanitize

Both are deterministic: Claude cannot reason around a raised exception.

## Connection to Iterative Refinement Techniques (D3 T3.5)

The same probabilistic/deterministic distinction applies to prompting technique. Prose instructions = probabilistic: Claude interprets them differently each run, guessing on unspecified details. Concrete input/output examples = deterministic: the transformation rule is fixed by the examples, not left to inference. See [[concrete-examples]] and [[iterative-refinement-techniques]]. [Source: 18-J2rT6yp-Lak.md]

## Connection to Prompt Design and Explicit Criteria (D4 T4.1)

The same distinction governs issue classification in code review prompts. Subjective thresholds ("be conservative," "only report high confidence findings") = probabilistic: Claude interprets the threshold differently each run. Categorical definitions (explicit include/exclude lists per issue type; concrete code examples per severity level) = deterministic: the same code matches the same pattern every time. See [[categorical-definitions]] and [[severity-levels-examples]]. [Source: 20-EzWRMJz9lXc.md]

## Connection to Structured Output via Tool Use (D4 T4.3)

The same distinction governs JSON extraction. Prose request ("respond in JSON") = probabilistic: Claude may produce invalid JSON or wrong structure. Tool use with JSON schema = deterministic: the API enforces valid JSON matching the schema at the system level — no reasoning, no bypassing. See [[structured-output-tool-use]] and [[extraction-tool-pattern]].

Note: this is determinism at the syntax level only. Semantic errors (valid JSON with wrong values) still require separate validation (T4.4). See [[syntax-vs-semantic-errors]]. [Source: 22-CxwIKbepDwQ.md]

## Connection to Path-Specific Rules (D3 T3.3)

The same probabilistic/deterministic distinction appears in CLAUDE.md convention loading. Without explicit path scoping, Claude *infers* which section of a monolithic `claude.md` applies to the current file — probabilistic, unreliable. With `paths:` glob matching in `.claude/rules/` files, the rule either loads or it doesn't — deterministic, no inference required. See [[path-specific-rules]] and [[path-rules-anti-patterns]] (anti-pattern #4). [Source: 16-iOvyCv_rwac.md]

[Source: 05-Ow0fCviHxrY.md, 06-UTzVVg8Aepc.md, 16-iOvyCv_rwac.md, 18-J2rT6yp-Lak.md, 22-CxwIKbepDwQ.md]
