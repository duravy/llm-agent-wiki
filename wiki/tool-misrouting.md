---
title: Tool Misrouting
created: 2026-04-16
last_updated: 2026-04-16
source_count: 2
status: draft
---

Tool misrouting occurs when Claude calls the wrong tool for a given user request. It is a direct consequence of vague or incomplete tool descriptions, since the description is the only signal Claude uses for tool selection.

> "Vague descriptions create ambiguity, and Claude resolves ambiguity by **guessing**."

## Canonical Example

Two tools:
- `get_customer` — "retrieves customer information"
- `lookup_order` — "retrieves order details"

User says: "Check my order number 12345."

**What happens**: Claude picks `get_customer` — wrong tool.

**Why**: Both tools "retrieve things." The number 12345 could be a customer ID or an order ID. The descriptions don't explain:
- When to use each tool
- What inputs they expect
- How they differ from each other

Claude resolves the ambiguity by guessing, and guesses wrong.

## Root Cause

Misrouting is always caused by one or more missing elements in [[five-elements-tool-description|the five description elements]]:
- No **when-to-use** → Claude can't differentiate triggering conditions
- No **boundary** → Claude uses a tool for inputs it wasn't designed for
- Overlapping **purpose** statements → Claude picks randomly between similar tools

## Diagnosis Protocol

When the exam presents a misrouting scenario:
1. Check the tool descriptions first — this is the lowest-effort, highest-impact fix
2. Identify which of the five elements is missing
3. Add the missing element (especially when-to-use and boundary)
4. If descriptions are already good, check for [[system-prompt-keyword-trap|keyword collisions]] in the system prompt

## The Fix

For the get_customer / lookup_order example:

```
get_customer:
  Purpose: Retrieve a customer's profile and contact information
  Input: Customer ID (integer)
  Output: Name, email, phone, account status
  When to use: When the user asks about who a customer is
  Boundary: Do not use for order history or order status — use lookup_order

lookup_order:
  Purpose: Retrieve details and status for a specific order
  Input: Order number (integer, format: 5-digit order ID)
  Output: Order status, items, amounts, shipping info
  When to use: When the user references an order number or asks about an order
  Boundary: Do not use to find customer contact information — use get_customer
```

Now Claude can select correctly even when both tools involve retrieval.

## Exam Signal

"When the exam presents a tool misrouting problem, the first fix to consider is improving the tool description." — The video frames this as an explicit exam rule.

Related: [[tool-description-design]], [[five-elements-tool-description]], [[generic-tool-splitting]], [[system-prompt-keyword-trap]], [[tool-description-anti-patterns]]

## D2 T2.4 Connection: MCP Tool Description Weakness

D2 T2.4 adds an MCP-specific variant of misrouting: community MCP servers often have minimal tool descriptions, causing Claude to prefer a better-described built-in tool (e.g., Grep) over a more capable MCP tool. The root cause is identical — weak descriptions lead to wrong tool selection — but the fix differs:

- **Regular tools**: rewrite the description directly
- **MCP tools from community servers**: add tool preference guidance to CLAUDE.md (you don't own the server)

See [[mcp-tool-description-fix]].

[Source: 09-TKmhUOlzM6w.md, 12-NlTC0XY8orw.md]
