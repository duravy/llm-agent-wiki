---
title: Environment Variable Expansion for MCP Secrets
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

MCP server configs require API tokens to authenticate with external services. The rule is: never hardcode them. Environment variable expansion is the correct pattern for keeping secrets out of version-controlled config files.

## The Rule

> **Never hardcode API tokens in `mcp.json`. Always use `${VAR_NAME}` syntax.**

The `${VAR_NAME}` syntax tells Claude Code to read the value from the developer's environment at runtime. The config file stores only the reference — the actual secret never enters the file.

## How It Works

```json
// mcp.json — safe to commit, no secrets inside
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

- `mcp.json` contains `${GITHUB_TOKEN}` — a reference, not a value
- The actual token lives in the developer's environment (shell profile, CI secret, etc.)
- Claude Code reads the environment at runtime and substitutes the real value
- `mcp.json` can be safely committed to version control

## Why It Matters

Hardcoding a token in `mcp.json` exposes it to everyone with read access to the repo — that includes contributors, CI systems, and anyone who ever clones it. A leaked token can grant unauthorized access to GitHub, Jira, or any service the token covers.

## What the Exam Calls This

"Environment variable expansion for secrets" — know this phrase. The exam tests:
- What syntax to use: `${VAR_NAME}` (dollar sign + braces)
- Why `mcp.json` is safe to commit: it contains only variable references, not actual secrets
- What the anti-pattern is: hardcoding secrets directly in the config file (see [[mcp-integration-anti-patterns]] Anti-pattern 1)

## Exam Signal

- `${VAR}` in config = correct pattern; safe to commit
- Literal token string in config = exam anti-pattern; never commit
- "How to handle secrets in MCP config" → environment variable expansion

[Source: 12-NlTC0XY8orw.md]
