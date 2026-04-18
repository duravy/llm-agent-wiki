---
title: Enum Escape Hatches (D4 T4.3)
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

A schema design pattern that prevents forced misclassification in structured extraction. When an enum doesn't include escape hatches for ambiguous cases, Claude must pick the closest-sounding option even when none fits. Adding `"unclear"` and `"other"` gives Claude safe exits that preserve ambiguity rather than manufacturing false precision. [Source: 22-CxwIKbepDwQ.md]

## The Problem

An enum without escape hatches causes forced misclassification:

```python
# Bad: closed enum
"category": {
    "type": "string",
    "enum": ["supplies", "services", "software"]
}
```

An invoice for a software-as-a-service subscription that also bundles physical hardware: is it `software` or `supplies`? Claude must pick one — the enum allows nothing else. The result is a valid JSON value that silently misrepresents the data.

## The Fix

Two escape hatches:

1. **`"unclear"`** — for cases where the category truly cannot be determined from the source document
2. **`"other"`** — for cases where the category is clear but doesn't fit any defined bucket

Plus a companion string field for the `"other"` case:

```python
"category": {
    "type": "string",
    "enum": ["supplies", "services", "software", "unclear", "other"]
},
"category_detail": {
    "type": "string"
    # Claude populates this when category = "other"
    # e.g., "hybrid hardware/software subscription"
}
```

## Unclear vs Other

| Value | When to use |
|-------|-------------|
| `"unclear"` | Source doesn't contain enough information to classify |
| `"other"` | Source is classifiable but the category doesn't fit any defined bucket |

The distinction matters for downstream handling: `unclear` cases need human review of the source; `other` cases need a new category bucket to be defined.

## Connection to Nullable Fields

Both patterns address the same root problem: forcing Claude to produce a value when no good value exists leads to fabricated or misrepresented data. [[nullable-fields]] handles absent data (return `null`); enum escape hatches handle ambiguous classification (return `unclear` or `other`). Both belong in the [[extraction-tool-pattern]].

## Exam Takeaway

> **"Add unclear and other to your enums. This prevents forced misclassification of ambiguous cases."** [Source: 22-CxwIKbepDwQ.md]

Related: [[extraction-tool-pattern]], [[nullable-fields]], [[structured-output-tool-use]]

[Source: 22-CxwIKbepDwQ.md]
