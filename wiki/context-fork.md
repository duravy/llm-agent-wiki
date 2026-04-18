---
title: context:fork — Isolated Skill Execution
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

`context:fork` is the most important skill configuration option. It runs the skill in an isolated sub-agent so the main conversation only receives the final result — all intermediate output is discarded.

## The Problem Without context:fork

A verbose skill — like a deep codebase analysis — might make 50 tool calls and generate thousands of lines of output. Without `context:fork`, all of that runs directly in the main conversation:

```
Main conversation context:
  [50 tool calls × verbose output]   ← consumes most of the context window
  [real work you wanted to do]       ← almost no space left
```

The analysis output fills the context window before your actual work begins.

## The Solution: context:fork

```yaml
---
context: fork
allowed_tools: [read, grep, glob]
---
# Deep Analysis Skill
...
```

With `context:fork`, the skill runs in a **separate sub-agent**:

```
Sub-agent (forked):
  [50 tool calls + verbose intermediate output]  → discarded after
  [produces final summary]

Main conversation:
  [concise final summary only]  ← context stays clean
```

The main conversation stays clean. Only the final result is returned.

## When to Use context:fork

Use `context:fork` for any skill that involves:
- **Exploration** — mapping a codebase, following imports
- **Analysis** — deep code review, dependency analysis
- **Brainstorming** — generating options, evaluating alternatives

Rule of thumb: if the skill will make many tool calls or produce verbose intermediate output, use `context:fork`.

## Connection to Fork Sessions (D1 T1.3 / T1.7)

`context:fork` at the skill layer is the same mechanism as fork sessions at the agent layer (see [[fork-sessions]]). Both:
- Create a copy of the current context
- Run independently without affecting the original
- Produce output that flows back to the caller

The difference is scope: fork sessions branch an entire conversation for parallel exploration; `context:fork` isolates a single skill invocation.

## Connection to Isolated Context (D1 T1.2)

Sub-agents in the multi-agent coordinator pattern also start with isolated context (see [[isolated-context]]). `context:fork` applies the same principle at the skill layer: the forked execution starts fresh and cannot pollute the caller's context.

## Exam Signal

- `context:fork` = most tested skill frontmatter option
- Without it: verbose skill floods main conversation context
- With it: main conversation receives only the final summary
- Use for: exploration, analysis, brainstorming skills
- "Skills without `context:fork` for verbose operations" = exam-tested anti-pattern #2 (see [[commands-skills-anti-patterns]])

## Connections

- [[skills]] — parent page; full skill anatomy
- [[commands-and-skills]] — T3.2 hub page
- [[fork-sessions]] — D1 T1.3/T1.7; same forking mechanism at session level
- [[isolated-context]] — D1 T1.2; sub-agents start fresh; same isolation principle
- [[explore-sub-agent]] — D3 T3.4; plan mode's investigation sub-agent is a third instance of the same isolation pattern

[Source: 15-AaGMyP2hBRY.md, 17-96v5N5YGMPM.md]
