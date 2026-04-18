---
title: Context Management Anti-Patterns
created: 2026-04-15
last_updated: 2026-04-15
source_count: 1
status: draft
---

Five common mistakes in agent context management, each wasting context budget or losing critical information. These are explicitly exam-tested in the Claude Architect Certification series.

## Anti-Pattern 1: Progressive Summarization of Numbers and Dates

**Problem:** Condensing conversation history into summaries destroys order IDs, amounts, deadlines, and identifiers.

**Correct approach:** Extract a structured [[case-facts-pattern|case facts block]] and maintain it separately outside the summarized history.

---

## Anti-Pattern 2: Including Full Tool Output (40+ Fields)

**Problem:** Raw API responses contain dozens of irrelevant fields, wasting context tokens on data the agent will never use.

**Correct approach:** Use post-tool-use hooks to trim to the relevant fields only. See [[tool-output-trimming]].

---

## Anti-Pattern 3: Putting Critical Details in the Middle of Context

**Problem:** The [[lost-in-the-middle-effect]] means the model may overlook details buried in the center of a long context.

**Correct approach:** Place summaries at the beginning, use section headers, and position the most important data at the beginning and end.

---

## Anti-Pattern 4: Subagent Output Without Metadata

**Problem:** Downstream agents can't cite sources or assess recency when content arrives without provenance.

**Correct approach:** Require date, source, URL, and methodology in structured output. See [[subagent-metadata]].

---

## Anti-Pattern 5: Dropping Conversation History Entirely

**Problem:** Removing all conversation history to free context space loses conversational coherence — the agent loses track of what was agreed, asked, or established.

**Correct approach:** Pass complete conversation history but trim tool output *within* that history. Don't cut conversation turns; cut data volume inside turns.

---

## Quick Reference Table

| # | Anti-Pattern | Fix |
|---|-------------|-----|
| 1 | Summarizing numbers/dates | Case facts block |
| 2 | Full 40+ field tool output | Trim in post-tool hooks |
| 3 | Critical details in middle | Section headers, primacy/recency layout |
| 4 | Subagent output without metadata | Require source/date/URL/methodology |
| 5 | Dropping history entirely | Trim tool outputs within history, keep turns |

Related: [[context-management]]

[Source: 26-H8Vpe9j4N5I.md]
