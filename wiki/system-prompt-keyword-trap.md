---
title: System Prompt Keyword Trap
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Words in a system prompt can accidentally bias Claude toward a tool whose name or description contains matching keywords — even when a different tool is the correct choice. This is an exam-tested subtle failure mode distinct from simple description problems.

> "A subtle trap the exam tests."

## How It Works

Claude's tool selection is influenced by the entire context, including the system prompt. If the system prompt uses a word that also appears in a tool name, Claude may favor that tool based on the keyword association rather than actual semantic fit.

## Canonical Example

**System prompt**: "Always analyze customer requests carefully before taking action."

**Tools available**:
- `analyze_content` — extracts text from web pages
- `lookup_order` — retrieves order status

**User says**: "What's the status of order 123?"

**What happens**: Claude picks `analyze_content` — wrong tool.

**Why**: The system prompt emphasizes the word "analyze." `analyze_content` contains that exact word in its name. Even though `lookup_order` is clearly the correct tool for an order status query, the keyword match creates an unintended selection bias.

## The Fix

Rename the tool to eliminate the keyword collision:
- `analyze_content` → `extract_web_content`

Now the system prompt's "analyze" keyword has no matching tool name. Claude routes to `lookup_order` correctly.

**The fix is in the tool name, not the system prompt.** Rewriting the system prompt to remove "analyze" is fragile — it may reappear in future revisions. Renaming the tool is durable.

## Audit Process

When debugging unexpected misrouting:
1. Read the system prompt for high-frequency or emphasized words (especially verbs and action words)
2. Cross-reference against tool names — look for word-level matches
3. Rename any tool whose name contains a system-prompt keyword it doesn't need

## Exam Signal

This is framed as a "subtle trap" — meaning it appears in exam scenarios designed to mislead candidates who only check descriptions. The keyword-collision fix (rename the tool) is explicitly called out as the correct answer.

Related: [[tool-description-design]], [[tool-misrouting]], [[tool-naming-strategies]], [[tool-description-anti-patterns]]

[Source: 09-TKmhUOlzM6w.md]
