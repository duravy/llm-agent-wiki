---
title: Context Degradation
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Context degradation is the progressive loss of precision in Claude's responses during extended investigation sessions. As the context window fills, specific details are pushed out and the model begins substituting generic knowledge for retrieved facts.

## The Pattern

**Early in a session (precise):**
> "The AuthManager class in `src/auth/manager.ts` uses JWT tokens with RS256 signing. The `verify` method on line 45 checks expiry but not signature validation."

**Late in a session (degraded):**
> "The authentication system uses typical patterns for token verification."

The model has lost the specific details and is filling in with plausible-sounding but unverified general knowledge. This is the same as researching 50 articles without taking notes — by article 40, the details of article 5 are gone.

## The Signal

**Watch for:** responses that cite "typical patterns" or make vague general claims where earlier responses cited specific classes, methods, files, and line numbers.

Do not wait until responses are already vague before acting. That is too late.

## Root Cause

The context window has finite space. As verbose tool outputs accumulate — file reads, search results, command outputs — earlier specific findings are compressed or displaced. See [[context-management]] for the foundational concept.

## Mitigations

| Signal | Action |
|--------|--------|
| Responses becoming vague | Run `/compact` proactively — see [[compact-command]] |
| Extended session with many files | Use [[scratchpad-files]] to write findings as discovered |
| Many files to read | Delegate to sub-agents — see [[subagent-delegation-exploration]] |
| Multi-hour investigation | Set up [[state-persistence-crash-recovery]] at the start |

## Exam Signal

"Context degrades over time in extended sessions. Recognize the signal: vague responses citing typical patterns instead of specific findings."

Related: [[large-codebase-context]], [[large-codebase-anti-patterns]]

[Source: 29-LRUdrMbFheA.md]
