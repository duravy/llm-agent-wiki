---
title: CI/CD Integration Anti-Patterns (T3.6)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Six exam-tested anti-patterns for T3.6 CI/CD integration. Each represents a failure to treat Claude as a proper pipeline component. [Source: 19-UnN8c0Sshq8.md]

## Anti-Pattern 1: Running Without `-p`

**What it is:** Invoking `claude` in CI without the `-p` / `--print` flag.

**Consequence:** Claude waits for interactive input. The pipeline job hangs and eventually times out.

**Fix:** Always use `-p`. See [[p-flag]].

## Anti-Pattern 2: Same Session Generates and Reviews

**What it is:** Using the same Claude session (or resuming the generation session) to review the code it just wrote.

**Consequence:** Self-review bias — the model remembers its own reasoning and is unlikely to question its own decisions. Reviews come back "it looks good."

**Fix:** Always use a fresh CI session for reviews. See [[session-isolation-ci]].

## Anti-Pattern 3: No Prior Findings in Re-Reviews

**What it is:** Running reviews on successive commits without including already-reported issues in the prompt.

**Consequence:** Claude reflagged the same issues on every commit. Developers see duplicate comments, stop trusting the review.

**Fix:** Include prior findings; instruct Claude to report only new issues. See [[prior-findings-injection]].

## Anti-Pattern 4: No CLAUDE.md for CI Context

**What it is:** Running CI reviews without a `CLAUDE.md` that documents review criteria.

**Consequence:** Claude applies its default judgment instead of your project's standards. Results are inconsistent between interactive and automated reviews.

**Fix:** Document criteria in `CLAUDE.md` — it loads automatically in CI sessions. See [[claude-md-hierarchy]].

## Anti-Pattern 5: Text Output in CI

**What it is:** Using default prose output from Claude in a pipeline that needs to process findings programmatically.

**Consequence:** Downstream tools have to parse free-form text — brittle, error-prone, breaks on format changes.

**Fix:** Use `--output-format json` with `--json-schema`. See [[structured-output-ci]].

## Anti-Pattern 6: No Existing Tests When Generating New Ones

**What it is:** Asking Claude to generate tests without including the existing test file in context.

**Consequence:** Claude duplicates existing coverage instead of filling gaps.

**Fix:** Include the existing test file; instruct Claude to focus on uncovered edge cases. See [[prior-findings-injection]].

## Summary Table

| Anti-pattern | Consequence | Fix |
|---|---|---|
| No `-p` | Hangs | `-p` always |
| Same session for generate + review | Self-review bias | Fresh CI session |
| No prior findings | Duplicate comments | Inject prior findings |
| No CLAUDE.md | Inconsistent criteria | Document in CLAUDE.md |
| Text output | Unparseable | `--output-format json` |
| No existing tests | Duplicate coverage | Include test file |

[Source: 19-UnN8c0Sshq8.md]
