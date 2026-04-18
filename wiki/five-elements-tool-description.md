---
title: Five Elements of a Good Tool Description
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Every effective tool description contains exactly five elements. Missing any one of them increases the likelihood of misrouting. The exam tests whether candidates can identify which element is absent when a tool underperforms.

## The Five Elements

### 1. Purpose
What does this tool actually do — specifically, not vaguely.

- Bad: "analyzes content" (circular, tells Claude nothing)
- Good: "extract structured data from web pages"

The purpose must be a **specific action**, not a generic verb.

### 2. Input Format
What should the inputs look like? Always include examples.

- Bad: "URL as a string"
- Good: "full URL including HTTPS, e.g. https://example.com/article"

Claude uses the input format description to construct valid tool call arguments. Without examples, Claude may pass malformed inputs.

### 3. Output
What does the tool return? Claude needs to know so it can plan its next step.

- Bad: (omitted entirely)
- Good: "page title, main text content, publication date, and author if available"

If Claude doesn't know what comes back, it can't know whether to call this tool or a different one to satisfy the user's need.

### 4. When To Use
In what situation should Claude choose this tool?

- Bad: (omitted entirely)
- Good: "use this when the user asks about information on a specific web page"

This narrows the selection criteria. Claude has explicit guidance about the triggering condition, rather than inferring from name and purpose alone.

### 5. Boundary (When NOT To Use)
Explicit exclusions prevent misrouting to similar-sounding tools.

- Bad: (omitted entirely)
- Good: "do not use for PDF documents — use extract_pdf instead; do not use for general web searches — use web_search instead"

**Boundaries are the most commonly omitted element** and the most directly responsible for systematic misrouting when tools serve adjacent but distinct use cases.

## Complete Example

```
Name: extract_web_content

Description:
  Purpose: Extracts the main content from a web page at a given URL.
  Input: Full URL including HTTPS scheme, e.g. https://example.com/article
  Output: Page title, main body text, publication date, and author if available
  When to use: When the user asks about information on a specific web page
  Boundaries: Do not use for PDF documents (use extract_pdf). Do not use
              for general web searches (use web_search).
```

## Exam Signal

When a scenario describes Claude calling the wrong tool, ask: which of the five elements is missing from the description?

- Missing purpose → Claude can't distinguish intent
- Missing boundary → Claude uses the tool for inputs it wasn't designed for
- Missing when-to-use → Claude can't differentiate between two tools with similar purposes

Related: [[tool-description-design]], [[tool-misrouting]], [[tool-description-anti-patterns]]

[Source: 09-TKmhUOlzM6w.md]
