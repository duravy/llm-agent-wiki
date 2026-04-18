---
title: Structured Output in CI (--output-format json)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Using `--output-format json` and `--json-schema` to get machine-parsable output from Claude Code in CI/CD pipelines. Turns Claude into a proper pipeline component whose output can be processed by other tools. [Source: 19-UnN8c0Sshq8.md]

## The Problem with Prose in CI

Prose output is for humans. In a pipeline, downstream tools need to:
- Extract specific fields (file, line number, severity, message)
- Post inline comments on specific PR lines
- Update dashboards
- Trigger conditional logic based on severity

Parsing prose programmatically is brittle and error-prone.

## The Flags

```bash
claude -p "Review for security issues" \
  --output-format json \
  --json-schema '{"type":"array","items":{"properties":{"file":{"type":"string"},"line":{"type":"integer"},"severity":{"type":"string"},"message":{"type":"string"}}}}'
```

| Flag | Effect |
|---|---|
| `--output-format json` | Return JSON instead of prose |
| `--json-schema <schema>` | Return JSON matching the schema exactly |

With a schema, you define the exact fields you need. Claude returns output that conforms — no parsing guesswork.

## What It Enables

```
Claude output (JSON) → extract fields → post inline PR comment per finding
                                      → update severity dashboard
                                      → fail pipeline if any severity == "critical"
```

Structured output turns Claude into a composable CI component.

## Exam Takeaway

> **`--output-format json` + `--json-schema` = structured, machine-parsable CI output.** Anti-pattern: text output in CI — hard to parse programmatically.

## Connections

- [[cicd-integration]] — T3.6 hub page
- [[p-flag]] — always used together; `-p` is required, structured output is the companion

[Source: 19-UnN8c0Sshq8.md]
