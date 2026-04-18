---
title: Prompt Design Anti-Patterns (D4 T4.1)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five anti-patterns that undermine prompt precision in D4 T4.1. The source notes: "Getting any of these wrong is a common exam question." All five stem from leaving too much to Claude's subjective judgment instead of providing categorical definitions. [Source: 20-EzWRMJz9lXc.md]

## The Five Anti-Patterns

| # | Anti-Pattern | Problem | Fix |
|---|---|---|---|
| 1 | "Be conservative" | Subjective — Claude decides what "conservative" means, inconsistently | Define explicit include/exclude categories |
| 2 | "Only report high confidence findings" | "High confidence" means different things each run | List specific issue types with concrete code examples |
| 3 | Reporting all possible issues | Floods developers with false positives, destroys trust | Define what to **skip** as explicitly as what to report |
| 4 | Keeping high false-positive categories active | Undermines trust in accurate categories too | Temporarily disable → improve criteria → re-enable |
| 5 | No severity examples | Inconsistent classification across reviews | Provide code examples for each severity level |

[Source: 20-EzWRMJz9lXc.md]

## Anti-Pattern Detail

### 1. "Be conservative"
The word "conservative" is not a definition. It's an instruction to apply judgment — and Claude's judgment changes based on surrounding context. One run, it flags three issues. Next run, same code, it flags twelve. The fix is a categorical definition: a list of specific issue types that are in scope, and a list that are out of scope.

### 2. "Only report high confidence findings"
What counts as high confidence? The model decides — and decides differently each time. This anti-pattern is especially deceptive because it sounds like precision. It isn't. Replace it with: "Report: null pointer dereferences, uncaught async errors, SQL injection. Do not report: style, naming, missing docs."

### 3. Reporting all possible issues
The instinct to be thorough backfires. When Claude reports everything it notices, 80%+ of findings are false positives. Developers stop trusting the tool. The 20% of real bugs get ignored. Fix: define what to skip as explicitly as what to report. Both halves of the [[categorical-definitions|categorical definition]] are required.

### 4. Keeping high-FP categories active
Once a category has a high false-positive rate, it contaminates the whole report. Developers can't trust any finding because they've been burned by this category before. Fix: **disable the category entirely, improve the criteria until they're precise, then re-enable**. Living with the noise is not a strategy. [Source: 20-EzWRMJz9lXc.md]

### 5. No severity examples
Without code examples, severity labels like "Critical" and "High" are open to interpretation. The same pattern may be classified Critical in one review and Medium in another. Fix: provide at least one concrete code example per severity level. See [[severity-levels-examples]].

## Exam Takeaway

> "Getting any of these wrong is a common exam question." — Part 20

The exam most likely tests:
- Anti-pattern 1 or 2: recognizing that subjective thresholds = inconsistent results
- Anti-pattern 3: defining the exclude list, not just the include list
- Anti-pattern 4: the disable→improve→re-enable fix (not "just add more examples to the existing prompt")
- Anti-pattern 5: code examples per severity level are required, not optional

## Connections

- [[prompt-design-explicit-criteria]] — T4.1 hub
- [[false-positive-problem]] — anti-patterns 3 and 4 feed the trust erosion cascade
- [[categorical-definitions]] — the fix for anti-patterns 1, 2, and 3
- [[severity-levels-examples]] — the fix for anti-pattern 5

[Source: 20-EzWRMJz9lXc.md]
