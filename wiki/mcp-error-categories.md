---
title: MCP Error Categories (D2 T2.2)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Four error categories for MCP tool failures. Each category maps to a distinct Claude response. The exam tests whether you know which action each category requires — including why retrying is correct for some but wasteful or wrong for others.

## The Four Categories

### Transient
**What it is**: Temporary infrastructure failure — timeout, service temporarily down, network blip.

**Why it's transient**: The service is healthy; the issue is momentary. A second attempt may succeed.

**Claude's action**: Wait and retry. These are recoverable.

**`is_retryable`**: `true`

**Example**: Database timed out on a query.

---

### Validation
**What it is**: The input Claude provided was malformed or invalid — bad order ID format, missing required field, out-of-range value.

**Why it's different from transient**: The service is fine. The problem is Claude's input. Retrying the same input fails again.

**Claude's action**: Fix the input and retry. Claude must correct the parameter, not simply re-call with identical arguments.

**`is_retryable`**: `true` (but only after fixing the input)

**Example**: Order ID "ORDER-XYZ" sent to a tool expecting numeric IDs only.

---

### Business
**What it is**: A policy or rule violation — refund exceeds the allowed limit, action not permitted for this account tier, operation outside business hours.

**Why retrying is wrong**: Business rules do not change between attempts. The second call with the same input produces the same rejection. Retrying wastes tokens and confuses the user.

**Claude's action**: Explain the constraint to the user. Do NOT retry.

**`is_retryable`**: `false`

**`customer_message` field**: Included — this is the one category where a user-facing explanation is always warranted.

**Example**: `"Refund of $850 exceeds the $500 limit for this account tier."`

---

### Permission
**What it is**: Unauthorized access — Claude (or the user) lacks the credentials or role to perform the action.

**Why retrying is wrong**: Retrying with the same credentials will never succeed. The permission has not changed.

**Claude's action**: Escalate or inform the user. No retry.

**`is_retryable`**: `false`

**Example**: Tool returns 401 Unauthorized when accessing restricted customer records.

---

## Category Decision Table

| Category | Service state | Input state | Retry? | Claude action |
|----------|--------------|-------------|--------|---------------|
| Transient | Down/degraded | Correct | Yes | Wait, retry |
| Validation | Fine | Wrong | Yes (after fix) | Fix input, retry |
| Business | Fine | Correct | No | Explain to user |
| Permission | Fine | Correct | No | Escalate/inform |

## Exam Signal

The exam often presents a failure scenario and asks what Claude should do. The answer always follows from the category:
- "Service timed out" → transient → retry
- "Invalid input format" → validation → fix and retry
- "Policy violation" → business → explain, don't retry
- "Unauthorized" → permission → escalate

The distractor answers are almost always: retrying a business violation (wrong — rules don't change) or failing silently on a transient error by returning empty results (wrong — see [[access-failure-vs-empty-result]]).

Related: [[mcp-error-responses]], [[mcp-error-fields]], [[is-error-flag]]

[Source: 10-cMCoA_MGDdw.md]
