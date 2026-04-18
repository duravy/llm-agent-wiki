---
title: Domain 3 — Claude Code Configuration
created: 2026-04-16
last_updated: 2026-04-16
source_count: 6
status: draft
---

Domain 3 covers how Claude Code is configured — how persistent instructions are structured, how team conventions are shared, how custom commands and skills are created, how rules are scoped to specific parts of a codebase, how to refine output quality iteratively, and how to run Claude non-interactively in CI/CD pipelines. The domain spans Parts 14–19 and is complete.

## Known Tasks

| Task | Topic | Wiki Page | Source Part |
|------|-------|-----------|-------------|
| 3.1 | The CLAUDE.md Hierarchy | [[claude-md-hierarchy]], [[claude-md-levels]], [[at-import-syntax]], [[rules-directory]], [[monolithic-split]], [[memory-command]], [[claude-md-anti-patterns]] | Part 14 |
| 3.2 | Custom Commands and Skills | [[commands-and-skills]], [[custom-commands]], [[skills]], [[context-fork]], [[skills-allowed-tools]], [[skills-vs-claude-md]], [[commands-skills-anti-patterns]] | Part 15 |
| 3.3 | Path-Specific Rules | [[path-specific-rules]], [[glob-pattern-routing]], [[crosscutting-concerns]], [[path-rules-vs-subdirectory-claude-md]], [[path-rules-anti-patterns]] | Part 16 |
| 3.4 | Plan Mode vs Direct Execution | [[plan-mode-vs-direct-execution]], [[plan-mode]], [[direct-execution]], [[explore-sub-agent]], [[plan-mode-anti-patterns]] | Part 17 |
| 3.5 | Iterative Refinement Techniques | [[iterative-refinement-techniques]], [[concrete-examples]], [[test-driven-iteration]], [[interview-pattern]], [[interacting-vs-independent-issues]], [[iterative-refinement-anti-patterns]] | Part 18 |
| 3.6 | CI/CD Integration | [[cicd-integration]], [[p-flag]], [[structured-output-ci]], [[session-isolation-ci]], [[prior-findings-injection]], [[cicd-anti-patterns]] | Part 19 |

## Domain Introduction

Claude Code reads `CLAUDE.md` at the start of every session. The domain is about how to author, organize, and scope those instructions — and how to debug when they don't take effect.

The core architectural insight: there are **three levels** of CLAUDE.md (user → project → subdirectory), and the most specific level always wins. This mirrors the tool distribution and MCP configuration patterns from Domain 2.

## Key Exam Signal

- Three-level hierarchy with clear priority order
- User level = personal, not shared via git
- Project level = shared via git, team-wide
- Subdirectory level = highest priority, most specific
- @import = always unconditional; `.claude/rules/` + `paths:` = conditional (T3.3)
- /memory as the first debugging step when rules are ignored
- "Conventions for files spread across many directories" → path-specific rules, not subdirectory claude.md
- Plan mode: architectural decisions, multi-file, multiple valid approaches → explore sub-agent → human review → execute
- Direct execution: well-defined, single-file, known approach → one-sentence description test
- Prose instructions → inconsistent output; concrete input/output examples → deterministic transformation
- Interview pattern: ask Claude to interview you before implementing in unfamiliar domains
- Interacting issues: fix together; independent issues: fix sequentially
- `-p` flag: required for all CI/CD invocations; without it, pipeline hangs
- Session isolation for CI reviews: fresh session = no generator bias
- CLAUDE.md applies equally to interactive and CI sessions

[Source: 14-qFYN8XYMTZM.md, 15-AaGMyP2hBRY.md, 16-iOvyCv_rwac.md, 17-96v5N5YGMPM.md, 18-J2rT6yp-Lak.md, 19-UnN8c0Sshq8.md]
