---
title: skills — allowed_tools and Least Privilege
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

`allowed_tools` restricts which tools a skill can access during execution. It applies the principle of least privilege to skills: a skill gets only the tools it needs to do its job.

## The Problem Without allowed_tools

Without tool restrictions, every skill has access to the full tool set — including `edit`, `write`, and `bash`. A skill intended only to read and analyze code could accidentally:
- Modify source files (`edit`, `write`)
- Run dangerous shell commands (`bash`)
- Delete files or overwrite data

## The Solution: allowed_tools

Declare exactly which tools the skill needs:

```yaml
---
context: fork
allowed_tools: [read, grep, glob]
argument_hint: "Module name to analyze"
---
# Analysis Skill
...
```

A read-only analysis skill lists `read`, `grep`, and `glob` — nothing else. The skill literally cannot call `edit`, `write`, or `bash` because they are not in the allowed list.

## The Principle

```
Skill type           → Allowed tools
Read-only analysis   → [read, grep, glob]
Formatting skill     → [read, edit]
Build/test runner    → [bash]  (nothing else needed)
```

A read-only skill **cannot write**. A formatting skill **cannot run shell commands**. This is the [[minimum-tools-principle]] applied to skills.

## Connection to Least Privilege Across Domains

`allowed_tools` in skills is the same concept as:
- **[[minimum-tools-principle]]** (D1 T1.3) — each agent gets only what it needs
- **[[tool-scoping]]** (D2 T2.3) — least privilege per agent role; off-role tools cause misuse
- **[[constrained-tool-alternatives]]** (D2 T2.3) — replace generic tools with scoped versions

The exam tests this as a consistent principle that appears at every layer: agent definition, tool distribution, and now skill configuration.

## Exam Signal

- `allowed_tools` = least privilege at the skill layer
- Read-only skill → `[read, grep, glob]` only — exam-tested example
- "Skills with full tool access for read-only tasks" = exam-tested anti-pattern #3 (see [[commands-skills-anti-patterns]])
- Always restrict skills with `allowed_tools`; absence is the wrong answer

## Connections

- [[skills]] — parent page; full skill anatomy
- [[commands-and-skills]] — T3.2 hub page
- [[minimum-tools-principle]] — D1 T1.3 root concept
- [[tool-scoping]] — D2 T2.3 application at agent level
- [[constrained-tool-alternatives]] — D2 T2.3 application at tool design level

[Source: 15-AaGMyP2hBRY.md]
