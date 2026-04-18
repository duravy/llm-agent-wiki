---
title: Lost in the Middle Effect
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

The lost-in-the-middle effect is a known limitation of large language models where information positioned in the middle of a long context window is processed less reliably than information at the beginning or end. Models exhibit a primacy effect (strong attention to the start) and a recency effect (strong attention to the end), with a "valley" in the middle.

## Practical Impact on Agents

If critical details — order IDs, tool outputs, user requirements — are buried deep in the middle of a long context, the model may overlook them even though they are technically present. This compounds with [[context-management]] challenges in long-running conversations.

## Mitigations

1. **Place the system prompt at the start** — always processed well
2. **Place the case facts block near the top** — right after the system prompt; see [[case-facts-pattern]]
3. **Use explicit section headers** for detailed tool results — helps the model navigate
4. **Place the latest user message at the end** — always processed well
5. **Place summaries at the beginning** of long sections

See [[strategic-input-organization]] for the full input layout.

## D1 T1.6 Application: Attention Dilution in Code Reviews

The same effect appears in D1 T1.6 under the name **attention dilution** — applied specifically to multi-file code reviews. If all 14 files are sent in one prompt, files 5–10 are buried in the middle and bugs are missed. The solution is per-pass decomposition: one file per prompt, then a separate cross-file integration pass. See [[attention-dilution]] for the full treatment.

The D5 mitigations (input layout, case facts position) and the D1 mitigation (per-pass decomposition) address the same root cause from different angles.

## Exam Signal
"Lost in the middle means beginning and end are processed reliably, not the middle."

Related: [[context-management]], [[strategic-input-organization]], [[attention-dilution]]

[Source: 26-H8Vpe9j4N5I.md, 07-kjzfSpgfvss.md]
