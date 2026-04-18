---
title: Attention Dilution — Per-Pass Decomposition Solution
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Attention dilution is the same phenomenon as the [[lost-in-the-middle-effect]] applied specifically to multi-file code reviews and task decomposition. When many files or items are sent in a single prompt, items in the middle receive less model attention and bugs are missed. The solution is per-pass decomposition. [Source: 07-kjzfSpgfvss.md]

## The Problem

Language models process the beginning and end of long inputs more reliably than the middle. In a 14-file code review sent as one prompt:

- File 1: thorough review
- Files 5–10: shallow pass — buried in the middle
- File 14: thorough review

A critical security bug in file 7 ships to production. Not because the model can't catch it — but because file 7 is in the attention valley.

## The Solution: Per-Pass Decomposition

Split the work into focused, individual prompts:

```
14 separate passes (one file each)
  → Each file gets the model's full attention
  → File 7 gets its own dedicated pass → bug caught

+

1 cross-file integration pass
  → Input: summaries from the 14 per-file reviews (not raw file contents)
  → Catches: data flow inconsistencies, API contract mismatches between modules
```

**15 total prompts. Same task. Completely different quality.** The only difference is how the work was decomposed.

## Why the Integration Pass Is Mandatory

Per-file passes catch *within-file* bugs. They cannot catch cross-cutting concerns — data flow inconsistencies between modules, API contract mismatches, shared state assumptions. The cross-file integration pass covers what per-file passes miss. Skipping it is [[decomposition-anti-patterns|Anti-pattern 5]].

## Relationship to the Lost-in-the-Middle Effect

This is the same underlying LLM behavior — documented in [[lost-in-the-middle-effect]] from the D5 context management domain. That page covers mitigation via input layout (system prompt first, case facts second, user message last). This page covers mitigation via decomposition (one focused prompt per unit of work). Both address the same root cause.

## Relationship to Prompt Chaining

Per-pass decomposition *is* [[prompt-chaining]] applied to multi-file review:
- Step 1: 14 parallel per-file passes (each file in its own context window)
- Step 2: cross-file integration pass on the summaries

The sequential constraint (step 2 needs step 1's summaries) and the parallel opportunity (14 per-file passes are independent) both follow the prompt chaining model.

## Relationship to Phase Summarization

The cross-file integration pass uses *summaries* from per-file reviews, not raw file contents. This is the [[phase-summarization]] principle: phase 2 receives a compact summary of phase 1 findings, not the raw output. Passing raw file contents to the integration pass would itself re-introduce the attention dilution problem.

## Exam Signal

> "Send all files in one prompt" → **attention dilution trap** → wrong answer
> "Per-file passes + separate integration pass" → correct decomposition

The visual the exam uses: left side (one massive prompt, file 7 missed) vs. right side (14 passes + integration pass, file 7 caught).

## Confirmation from D4 T4.6

[[multi-instance-review]] (D4 T4.6, Part 25) independently confirms this architecture from the review angle. Part 25 explicitly describes the same 14-file scenario, the same per-file parallel pass structure, and the same cross-file integration pass — labeled as the three-pass review architecture. The convergence across D1 T1.6 and D4 T4.6 is strong exam signal that this decomposition pattern is the canonical answer to the attention dilution problem.

[Source: 07-kjzfSpgfvss.md, 25-W7c9wNfoCjU.md]
