---
title: Content-Type Rendering
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Content-type rendering is the principle that different types of information should be presented in their natural format, not forced into a uniform structure. Uniform bullet points is an explicitly exam-tested anti-pattern.

## Natural Formats by Content Type

| Content Type | Natural Format | Why |
|-------------|----------------|-----|
| Financial data | Tables | Comparisons require column alignment |
| News findings | Prose narrative | Flow and context matter |
| Technical findings | Structured lists | Discrete items with hierarchy |
| Statistical comparisons | Side-by-side tables with methodology notes | Numbers need labels and context |

## The Analogy

> "You would not write a recipe as a spreadsheet and you would not present quarterly revenue as a paragraph of prose."

## Why Uniform Formatting Fails

Converting everything to bullet points:
- Makes financial comparisons harder to read
- Strips narrative flow from news findings
- Loses the relational structure of statistical comparisons
- Forces the reader to mentally reconstruct the natural structure

## Exam Signal

> "The exam tests whether you know that uniform formatting is an anti-pattern."

This is a direct exam call-out. If a question asks about output rendering or formatting, the answer is content-appropriate formats — not uniform bullets.

## Exam Shortcut

If the question asks about rendering:
- Financial data → tables
- News → prose
- Technical data → structured lists
- Statistical comparisons → side-by-side tables with methodology notes
- Never force uniformity

## Anti-Pattern

Uniform format for all content types (Anti-pattern #4 in [[provenance-anti-patterns]]).

Related: [[information-provenance]], [[confidence-structured-output]]

[Source: 31-EfQSimX5wlM.md]
