---
title: /memory Command — Debugging Loaded Configuration
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

The `/memory` command shows exactly which CLAUDE.md configuration files are loaded in the current session. It is the first debugging step when Claude is not following expected conventions — do not guess, verify.

## What It Does

Running `/memory` in a Claude Code session prints a list of every configuration file currently loaded. If a file is missing from the list, Claude does not know about those instructions.

## When to Use It

Use `/memory` when:
- Claude is not following a coding convention you defined in CLAUDE.md
- You added a rule but Claude is ignoring it
- A team member reports that expected conventions aren't being applied
- You suspect a file isn't loading due to a path or syntax issue

**Always check `/memory` before concluding that Claude is broken.**

## Common Root Causes

| Symptom | Likely cause |
|---------|-------------|
| File missing from /memory list | File is at user level when it should be project level |
| File missing from /memory list | The @import path is wrong (paths are relative to the importing file) |
| File missing from /memory list | A rules file has a syntax error in its YAML frontmatter |

## The Debugging Protocol

```
Claude ignoring a rule?
  → Run /memory
  → Is the file in the list?
      No → Check level (user vs project), @import path, YAML syntax
      Yes → The file is loaded; the instruction may need revision
```

## Exam Signal

- "Not checking /memory" = exam-tested anti-pattern #5 (see [[claude-md-anti-patterns]])
- "When Claude ignores rules" → correct first action = run /memory, not conclude Claude is broken
- Most common misconfiguration: team config at user level instead of project level

## Connections

- [[claude-md-hierarchy]] — hub page for T3.1
- [[claude-md-levels]] — user vs project level distinction; the most common /memory diagnosis
- [[at-import-syntax]] — @import path errors are a common cause of missing files

[Source: 14-qFYN8XYMTZM.md]
