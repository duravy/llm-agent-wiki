---
title: Strategic Input Organization
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Strategic input organization is the deliberate ordering of content in an agent's context window to counteract the [[lost-in-the-middle-effect]]. By placing critical information at positions the model processes reliably (beginning and end), agents reduce the risk of overlooking important details.

## Recommended Layout

```
1. SYSTEM PROMPT
   └── Processed reliably (primacy effect)

2. CASE FACTS BLOCK
   └── Critical transactional data — always visible
   └── See [[case-facts-pattern]]

3. DETAILED TOOL RESULTS
   └── Organized with explicit section headers
   └── Pre-trimmed via post-tool-use hooks
   └── See [[tool-output-trimming]]

4. LATEST USER MESSAGE
   └── Processed reliably (recency effect)
```

## Key Techniques

- **Summaries at the beginning** of sections, not the end
- **Section headers** for detailed data so the model can navigate
- **Most important details at beginning and end** of long sections
- **Never put critical data in the middle** of a long, unstructured block

## Why This Matters

Without intentional organization, context becomes a flat wall of text. The model's attention is finite — strategic layout directs that attention to what matters. This is especially critical when tool outputs are large, even after trimming.

Related: [[context-management]], [[lost-in-the-middle-effect]], [[case-facts-pattern]]

[Source: 26-H8Vpe9j4N5I.md]
