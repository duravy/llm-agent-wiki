---
title: MCP Configuration Scopes
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

MCP servers are configured at two scopes. The scope determines whether the whole team gets the tools or just you. Getting this wrong is a common exam question.

## The Two Scopes

| Scope | File location | Version controlled? | Who gets it? | Use for |
|-------|--------------|-------------------|--------------|---------|
| **Project level** | `mcp.json` at project root | Yes — committed to git | Everyone who clones the repo | Shared team tooling: Jira, GitHub, databases |
| **User level** | `~/.claude.json` | No — stays on your machine | You only | Personal or experimental servers |

## Exam Question Pattern

> "A new team member joined and isn't receiving the expected MCP tools. What should you check?"

**Answer**: Check whether the MCP config is at user level instead of project level. User-level config stays on the original developer's machine — it is never committed to version control, so teammates never receive those tools.

**Fix**: Move the server configuration from `~/.claude.json` to `mcp.json` at the project root and commit it.

## Project-Level Config Example

```json
// mcp.json — committed to version control
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

Note: `${GITHUB_TOKEN}` is environment variable expansion — the actual token is never stored in this file. See [[environment-variable-expansion]].

## Decision Rule

```
Is this tooling the whole team needs?
  Yes → mcp.json (project level, committed to git)
  No  → ~/.claude.json (user level, personal)
```

## Connection to D3 T3.1 CLAUDE.md Hierarchy

MCP configuration scopes use the same user/project design principle as the [[claude-md-hierarchy]]: user-level config is personal and not shared via git, while project-level config is committed and shared with the team. The exam-tested anti-pattern is the same in both: putting shared configuration at user level so teammates never receive it.

The key difference: MCP config lives in `mcp.json` / `~/.claude.json`, while CLAUDE.md instructions live in `.claude/claude.md` / `~/.claude/claude.md`. Different files, same architecture.

## Exam Signal

- Project-level = `mcp.json` + committed = shared
- User-level = `~/.claude.json` + not committed = personal
- "Team doesn't have tools" = config is at wrong scope
- Shared tooling at user level = exam-tested anti-pattern (see [[mcp-integration-anti-patterns]] Anti-pattern 4)

[Source: 12-NlTC0XY8orw.md, 14-qFYN8XYMTZM.md]
