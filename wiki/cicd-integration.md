---
title: CI/CD Integration (T3.6) — Running Claude Non-Interactively
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

How to integrate Claude Code into automated pipelines. The core problem: Claude Code is designed for interactive use and hangs in CI without the `-p` flag. Six techniques make it a proper pipeline component — non-interactive execution, structured output, consistent context, session isolation, duplicate prevention, and coverage-aware test generation. Closes Domain 3. [Source: 19-UnN8c0Sshq8.md]

## The Core Problem

Claude Code expects a human at the keyboard. Run it in a pipeline without modification:
- It waits for input
- The pipeline times out
- The job hangs

The fix is a single flag: `-p`.

## Six Techniques

### 1. The `-p` Flag (Non-Interactive Mode)

`-p` / `--print` makes Claude process the prompt, write output to stdout, and exit. Required for every CI invocation. See [[p-flag]].

```bash
claude -p "Review this PR for security issues" | gh pr comment --body-file -
```

### 2. Structured Output

`--output-format json` returns JSON instead of prose. Add `--json-schema` with a schema definition to get output matching exact field names (file, line_number, severity, message). See [[structured-output-ci]].

Structured output enables:
- Inline PR comments on specific lines
- Dashboard updates
- Conditional pipeline logic based on severity

### 3. CLAUDE.md Provides Consistent Context

Claude reads `CLAUDE.md` in CI exactly as it does in interactive sessions. Document review criteria once; they apply everywhere:
- Flag functions without error handling as high severity
- Do not flag code style (handled by linters)
- Use Vitest for tests

This is how interactive and automated reviews stay consistent. See [[claude-md-hierarchy]].

### 4. Session Isolation for Reviews

The session that generated code remembers its own reasoning — it's biased toward its own decisions and less likely to catch real issues. Fresh CI sessions have no memory of generation. They review with unbiased eyes. See [[session-isolation-ci]].

This is why CI reviews should always be **fresh sessions**, never continuations of the generation session.

### 5. Prior Findings Injection

On successive commits, CI may reflag already-reported issues. Include prior findings in the prompt:

> "Do not re-report these: [list of already-reported findings]"

Without this: developers see duplicate comments on every commit and stop trusting the review. See [[prior-findings-injection]].

### 6. Existing Tests Context

When generating new tests, include the existing test file in context. Claude fills uncovered edge cases instead of duplicating existing coverage.

## Six Anti-Patterns

See [[cicd-anti-patterns]]. Short form:

| Anti-pattern | Consequence | Fix |
|---|---|---|
| No `-p` flag | Pipeline hangs | Always use `-p` |
| Same session generates + reviews | Self-review bias | Fresh CI session |
| No prior findings | Duplicate comments | Inject prior findings |
| No CLAUDE.md | Inconsistent criteria | Document in CLAUDE.md |
| Text output | Hard to parse | Use `--output-format json` |
| No existing tests | Duplicate coverage | Include test file in context |

## Six Exam Takeaways

1. `-p` flag = non-interactive; required for every pipeline invocation
2. `--output-format json` + `--json-schema` = structured, machine-parsable findings
3. CLAUDE.md loads in CI the same as interactive — one source of truth for review criteria
4. Session isolation = CI reviews are always fresh; no generator bias
5. Include prior findings on re-reviews to prevent duplicate comments
6. Include existing tests when generating new ones to avoid duplicate coverage

## Cross-Domain Connections

- **[[isolated-context]]** (D1 T1.2) + **[[context-fork]]** (D3 T3.2) + **[[explore-sub-agent]]** (D3 T3.4): CI session isolation is another instance of the isolation principle — fresh context for unbiased work.
- **[[multi-instance-review]]** (D5 Part 25 stub): self-review bias here is the foundation; Part 25 extends it to multi-instance architectures.
- **[[claude-md-hierarchy]]** (D3 T3.1): CLAUDE.md consistency across interactive and CI sessions closes the loop on T3.1's "persistent session config" framing.

[Source: 19-UnN8c0Sshq8.md]
