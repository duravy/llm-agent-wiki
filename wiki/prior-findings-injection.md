---
title: Prior Findings Injection
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Including already-reported review findings in each subsequent CI prompt so Claude reports only new issues. Without this, every pipeline run reflagged the same issues, flooding developers with duplicate comments and eroding trust in automated reviews. [Source: 19-UnN8c0Sshq8.md]

## The Problem

CI reviews run on every commit. If commit 2 touches the same code as commit 1:
- Claude reviews the same functions
- Finds the same issues
- Reports them again
- Developer sees duplicate comments on every push
- Trust in the automated review collapses

## The Fix

Include prior findings in the prompt:

```
Review this PR for issues.

Already reported (do not re-report):
- auth.js:42 — missing error handling on login failure [HIGH]
- utils.js:17 — unused import `lodash` [LOW]

Report only NEW issues not in the above list.
```

Claude focuses on what's changed or uncovered. Developers see only actionable new findings.

## The Same Principle for Test Generation

When generating new tests, include the existing test file in context:

```
Generate additional tests for auth.js.

Existing test file (do not duplicate):
[contents of auth.test.js]

Focus on uncovered edge cases.
```

Claude fills gaps instead of reproducing what's already there. Duplicate coverage is the test-generation version of duplicate comments.

## Exam Takeaway

> **Include prior findings in re-review prompts to prevent duplicate comments. Include existing tests when generating new ones to prevent duplicate coverage.** Both apply the same principle: give Claude the "already done" list so it focuses on what's missing.

## Connections

- [[cicd-integration]] — T3.6 hub page
- [[cicd-anti-patterns]] — anti-patterns 3 and 6 are both failures of this pattern

[Source: 19-UnN8c0Sshq8.md]
