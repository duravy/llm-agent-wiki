---
title: The -p Flag (Non-Interactive Mode)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The `-p` / `--print` flag makes Claude Code run non-interactively. It is the foundational flag for all CI/CD use of Claude Code — required in every pipeline invocation without exception. [Source: 19-UnN8c0Sshq8.md]

## What It Does

Without `-p`: Claude Code starts, waits for user input, and hangs until the pipeline times out.

With `-p`: Claude processes the prompt, writes the result to stdout, and exits. The pipeline continues.

```bash
# Without -p: hangs forever in CI
claude "Review this PR for security issues"

# With -p: processes and exits; output piped to gh CLI
claude -p "Review this PR for security issues" | gh pr comment --body-file -
```

## Flags

| Flag | Alias | Effect |
|---|---|---|
| `-p` | `--print` | Non-interactive: process prompt, write to stdout, exit |
| `--output-format json` | — | Return structured JSON instead of prose |
| `--json-schema <schema>` | — | Return JSON matching the provided schema exactly |

`-p` is always required. `--output-format json` and `--json-schema` are used when downstream tools need to parse the output programmatically. See [[structured-output-ci]].

## Exam Takeaway

> **`-p` = non-interactive. Required for every CI invocation. Without it, the pipeline hangs.**

This is the most direct exam signal for T3.6: if a question asks how to run Claude Code in a pipeline, the answer always starts with `-p`.

## Connections

- [[cicd-integration]] — T3.6 hub page
- [[structured-output-ci]] — the companion flag for machine-parsable output

[Source: 19-UnN8c0Sshq8.md]
