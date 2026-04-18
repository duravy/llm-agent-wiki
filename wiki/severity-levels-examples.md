---
title: Severity Levels with Code Examples
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

When defining severity levels for code review or issue classification, labels and descriptions alone are insufficient. Each level requires a concrete code example so Claude can match new code against the example and classify consistently across runs. Without examples, severity classification is subjective and varies. [Source: 20-EzWRMJz9lXc.md]

## The Four Levels

| Level | Definition | Code Example | Action |
|-------|------------|--------------|--------|
| **Critical** | Data loss or security vulnerability | User input directly in SQL query without parameterization | Report immediately |
| **High** | Functional bug that affects users | Function returning `undefined` instead of expected error object | Report |
| **Medium** | Code quality issue that may cause future bugs | Catching all exceptions broadly instead of specific error types | Report |
| **Low** | Style or convention issues | Variable naming, formatting, indentation | **Do not report** — handled by linters |

[Source: 20-EzWRMJz9lXc.md]

## Why Labels Alone Fail

"Critical" to one person means "system down." To another it means "will definitely be a problem eventually." Without a code example to anchor it, Claude picks a meaning each time — and classification drifts across runs.

**Without examples:**
> Critical: data loss or security vulnerability

Claude must reason about what "data loss" and "security vulnerability" mean in context. Two identical code patterns may be classified differently across reviews.

**With examples:**
> Critical: data loss or security vulnerability — e.g., `db.query("SELECT * FROM users WHERE id = " + userId)` (user input directly in SQL without parameterization)

Now Claude has a concrete pattern to match against. Classification anchors to the example.

## The "Low = Do Not Report" Rule

The Low severity level is a trap for completeness-oriented prompts. Style and convention issues are real — but they're handled by linters. Including them in Claude's output:
1. Floods developers with noise
2. Contributes to the [[false-positive-problem|false positive problem]]
3. Erodes trust in Critical and High findings

The explicit rule: **Do not report Low severity. Define it so Claude doesn't accidentally include it, then exclude it by name.** [Source: 20-EzWRMJz9lXc.md]

## Exam Takeaway

> **Severity levels without code examples lead to inconsistent classification across reviews.** Provide concrete code examples for each level. This is the mechanism that makes severity classification deterministic rather than probabilistic.

"Each severity level has concrete code examples and Claude can match new code against these examples to classify consistently." [Source: 20-EzWRMJz9lXc.md]

## Connection to Categorical Definitions

Severity levels are a specialization of [[categorical-definitions|categorical definitions]]: instead of listing issue types, you're listing severity bands. The same principle applies — examples anchor the category to a specific pattern, eliminating subjective interpretation.

## Connections

- [[prompt-design-explicit-criteria]] — T4.1 hub
- [[categorical-definitions]] — parent concept; severity levels = categorical definitions for classification
- [[false-positive-problem]] — Low severity → do not report; prevents false-positive flood from style issues
- [[probabilistic-vs-deterministic]] (D1 T1.4) — examples make classification deterministic

[Source: 20-EzWRMJz9lXc.md]
