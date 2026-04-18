---
title: Prompt Chaining — Fixed Sequential Pipeline
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Prompt chaining is a task decomposition strategy where all steps are defined before work begins and executed in a fixed sequence. Step N's output becomes step N+1's input. The pipeline never changes based on what steps discover. [Source: 07-kjzfSpgfvss.md]

## Three Things to Know Cold (Exam)

1. **Fixed linear pipeline** — steps are designed upfront; the sequence is identical every run regardless of what the steps find
2. **Best for structured, repeatable tasks** — code review is the canonical example: per-file analysis → cross-file integration → final summary, same three steps every time
3. **Focused context windows** — each step gets the model's full attention on a small, focused input; this is the whole point; small focused prompts beat one giant prompt

## Canonical Example: Code Review Pipeline

```
Step 1 (parallelizable): Loop over each changed file, review in isolation
  → No other files in context
  → These calls CAN be parallelized

Step 2 (must follow step 1): Cross-file pass on all per-file reviews
  → Looks for data flow issues and API mismatches
  → Must run AFTER step 1 completes

Step 3: Compile final report from cross-file findings
```

Three steps. Fixed every time. That's prompt chaining.

## Parallelism Within a Step

Within a single chaining step, independent units of work can be parallelized. Reviewing 14 files is 14 independent calls — they can run simultaneously. The sequential constraint is *between* steps (step 2 needs step 1's output), not *within* a step. See [[parallel-vs-sequential-execution]].

## When to Use

- Task structure is the same on every run
- Steps are known before execution begins
- You want predictable, fixed cost
- Independent subtasks within a step can be parallelized for speed

## When NOT to Use

If the scope is open-ended and what you find at step 2 should determine step 3, use [[dynamic-decomposition]] instead. A fixed pipeline cannot adapt when unexpected discoveries occur mid-execution.

## Connection to Attention Dilution

Prompt chaining applied to multi-file code reviews *is* the solution to [[attention-dilution]]: instead of sending all 14 files in one prompt (burying files 5–10 in the middle), you send one file per prompt, then a separate integration pass. Each file gets the model's full attention. See [[attention-dilution]].

## Connection to Phase Summarization

The cross-file integration pass — step 2 in the code review example — uses the per-file review *summaries* as input, not the raw file contents. This is identical to the [[phase-summarization]] principle from D5: integrate from summaries, not raw output.

## Cost Profile

Step count is fixed upfront → cost is predictable and lower. Contrast with [[dynamic-decomposition]] where steps can multiply as discoveries surface.

## D3 T3.4 Connection: Combined Mode as a Two-Phase Chain

The combined plan-mode + direct-execution pattern (D3 T3.4) is a two-phase prompt chain with a human review gate between phases:

- **Phase 1** (plan mode): the [[explore-sub-agent]] investigates → produces a structured plan → human approves
- **Phase 2** (direct execution, per step): each step of the approved plan is a well-scoped direct execution call

This is prompt chaining where the "output" of phase 1 that feeds into phase 2 is a human-reviewed and adjusted plan — making each subsequent step well-defined. See [[plan-mode-vs-direct-execution]].

[Source: 07-kjzfSpgfvss.md, 17-96v5N5YGMPM.md]
