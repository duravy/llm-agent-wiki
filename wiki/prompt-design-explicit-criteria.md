---
title: Prompt Design and Explicit Criteria (D4 T4.1)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Task 4.1 of Domain 4. The core insight: general instructions sound reasonable but don't work — explicit categorical criteria transform results. Vague instructions leave Claude to interpret meaning differently each run; explicit criteria define exactly what to report, what to skip, and how to classify severity with concrete code examples.

## The Core Problem

General instruction: *"Check that comments are accurate."*

Result: Claude flags everything that could possibly be inaccurate → 80% false positives → developers ignore all findings including real bugs.

Explicit instruction: *"Flag comments only when the comment's claim contradicts the actual code. Do not flag outdated parameter names, missing comments, or style issues."*

Result: 90% of findings are real issues → developers trust and act on them.

The same volume of effort, opposite outcomes. The difference is precision. [Source: 20-EzWRMJz9lXc.md]

## Key Concepts

| Concept | Page | One-Line Summary |
|---------|------|-----------------|
| False Positive Problem | [[false-positive-problem]] | High FP rate destroys trust across all categories via the smoke detector effect |
| Categorical Definitions | [[categorical-definitions]] | List exactly what to report AND what to skip; never use subjective thresholds |
| Severity Levels with Examples | [[severity-levels-examples]] | Four levels (Critical/High/Medium/Low) each with concrete code examples for consistent classification |
| Anti-Patterns | [[prompt-design-anti-patterns]] | Five anti-patterns: subjective thresholds, all-possible-issues, no severity examples, etc. |

## Exam Takeaways

1. **Explicit beats general.** Always. "Flag only when X" outperforms "check for X."
2. **Define what to skip.** Reducing false positives is as important as catching real issues.
3. **Categorical > subjective.** "Be conservative" = inconsistent. List specific issue types = consistent.
4. **Severity needs examples.** Label + description is insufficient. Each level needs a concrete code example.
5. **Precision over recall.** Fewer accurate findings > many findings with high false positive rate. This is the exam theme for Domain 4.

## Example: Explicit Criteria in Practice

**Vague (bad):**
> "Report security issues with high confidence."

**Explicit (good):**
> Report:
> - Null pointer dereferences
> - Uncaught async errors
> - SQL injection (user input directly in query without parameterization)
>
> Do not report:
> - Code style issues
> - Minor naming conventions
> - Missing documentation

[Source: 20-EzWRMJz9lXc.md]

## Domain Context

This is task 4.1 of [[domain-4-overview|Domain 4]]. It opens the domain and establishes the precision-over-recall theme that carries through T4.2 (few-shot prompting), T4.3 (structured output), and T4.4 (validation loops).

## Cross-Domain Connections

- [[false-positive-problem]] — the problem that explicit criteria solve
- [[categorical-definitions]] — the exam decision rule: categorical vs. subjective
- [[severity-levels-examples]] — concrete code examples per severity level
- [[concrete-examples]] (D3 T3.5) — same principle: examples define transformation rules; T4.1 applies it to issue classification
- [[probabilistic-vs-deterministic]] (D1 T1.4) — categorical criteria = deterministic output
- [[explicit-escalation-criteria]] (D5 T5.2) — same precision-over-vagueness principle in escalation design

[Source: 20-EzWRMJz9lXc.md]
