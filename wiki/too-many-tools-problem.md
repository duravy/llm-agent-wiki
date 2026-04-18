---
title: The Too-Many-Tools Problem (D2 T2.3)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

A research finding the exam directly tests: giving an agent too many tools degrades tool selection reliability. Each additional tool is another option Claude must evaluate. More options means more decision complexity and more likely Claude picks the wrong one.

## The Numbers

- **Sweet spot**: 4–5 tools per agent
- **Problem threshold**: 18 tools — reliability drops significantly
- The exam uses 18 tools as the canonical "too many" example

## The Analogy

You hire an assistant and give them access to 12 systems: email, calendar, CRM, accounting, HR, project management, design tools, code repo, support platform, analytics, social media, and office supplies.

When you say "schedule a meeting," they have 12 systems to consider. They might check the HR system instead of the calendar.

Now give them only email and calendar. When you say "schedule a meeting" — no confusion.

**Scoping tools eliminates decision complexity.**

## Why This Happens

Tool selection is based on descriptions. When Claude has 18 tools, it must evaluate 18 descriptions against the current task. With more options, borderline cases go to the wrong tool more often. This is not a failure of instructions — it is a structural property of how selection works under high-option conditions.

The fix is architectural: reduce options per agent, not the quality of descriptions alone. See [[tool-scoping]].

## Exam Signal

Source: *"The answer comes from a research finding the exam directly tests. Giving an agent too many tools degrades tool selection reliability."*

Exam question forms:
- "An agent with 18 tools is misrouting requests — what is the most likely cause?" → Too many tools; split responsibilities across agents with 4–5 tools each
- "What is the recommended maximum tools per agent?" → 4–5

The distractor answer is usually "improve the tool descriptions" — correct for misrouting but not the primary fix when the count itself is the problem.

## Connection to D2 T2.1

[[tool-description-design]] addresses misrouting caused by poor descriptions. The too-many-tools problem addresses misrouting caused by count. Both matter — but they are separate diagnoses. When an exam question gives you an agent with 18 tools, count is the primary issue.

Related: [[tool-distribution]], [[tool-scoping]], [[tool-description-design]], [[minimum-tools-principle]]

[Source: 11-K-HRtSQLGfU.md]
