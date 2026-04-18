---
title: MCP Structured Error Fields (D2 T2.2)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five required fields in a structured MCP error response. Know these cold for the exam. Each field serves a specific purpose; missing any one reduces Claude's ability to make the right recovery decision.

## The Five Fields

### 1. `error_category`
**Type**: string enum  
**Values**: `transient` | `validation` | `business` | `permission`  
**Purpose**: Names the failure type so Claude knows which recovery action applies.

Without this field, Claude must guess from the message text — unreliable and exam-tested as an anti-pattern.

---

### 2. `is_retryable`
**Type**: boolean  
**Purpose**: Direct retry signal. `true` = try again. `false` = stop.

This is the highest-leverage field. Setting it correctly eliminates wasted token retry loops on business violations and ensures Claude does retry on recoverable failures.

| Category | Value |
|----------|-------|
| Transient | `true` |
| Validation | `true` (after input fix) |
| Business | `false` |
| Permission | `false` |

---

### 3. `message`
**Type**: string  
**Audience**: Developers, logging systems, debugging  
**Purpose**: Technical detail about the failure — stack trace excerpt, error code, failing condition.

This field is not for users. It feeds logs and helps developers diagnose systemic issues.

---

### 4. `customer_message`
**Type**: string  
**Audience**: End users  
**When included**: Business errors only  
**Purpose**: Human-readable explanation of the business constraint that triggered the error.

For transient, validation, and permission errors, this field is omitted — users don't need to know about infrastructure timeouts or permission model details.

**Example**: `"Refunds over $500 require manager approval. Please contact support to proceed."`

---

### 5. `suggestion`
**Type**: string  
**Purpose**: Tells the agent exactly what action to take next.

This is the actionable field that transforms an error report into a recovery plan. Without it, Claude must infer the next step; with it, Claude acts immediately.

**Examples**:
- Transient: `"Retry after 2 seconds"`
- Validation: `"Order ID must be numeric only — reformat input"`
- Business: `"Inform user of $500 refund limit and offer alternative"`
- Permission: `"Escalate to human agent with user account details"`

---

## Complete Structured Error Example

```json
{
  "is_error": true,
  "error_category": "business",
  "is_retryable": false,
  "message": "RefundAmountExceedsLimit: requested=850, limit=500, account_tier=standard",
  "customer_message": "Refunds over $500 require manager approval. Please contact support.",
  "suggestion": "Inform user of limit and offer to escalate to manager approval workflow"
}
```

## Minimal Structured Error (Transient)

```json
{
  "is_error": true,
  "error_category": "transient",
  "is_retryable": true,
  "message": "DatabaseConnectionTimeout: query timed out after 30s",
  "suggestion": "Retry after 2 seconds"
}
```

Note: `customer_message` is omitted — transient errors are not user-visible.

## Exam Application

If a question asks "what fields must a structured error response include" → the answer is all five. If it asks "which field tells Claude to retry" → `is_retryable`. If it asks "which field is only included for business errors" → `customer_message`.

Related: [[mcp-error-responses]], [[mcp-error-categories]], [[is-error-flag]]

[Source: 10-cMCoA_MGDdw.md]
