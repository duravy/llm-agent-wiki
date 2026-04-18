---
title: Tool Naming Strategies
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Tool names are part of the selection signal. A good name tells Claude exactly what the tool does before it even reads the description. Three patterns cover most naming situations.

## Three Naming Patterns

### 1. Action + Object
Format: `verb_noun`

Use for: standard operations where the action and target are sufficient to distinguish the tool.

Examples:
- `extract_data_points`
- `summarize_content`
- `verify_claim`
- `lookup_order`
- `get_customer`

Best when: the action is specific enough to be unambiguous on its own.

### 2. Verb + Scope
Format: `verb_scope`

Use for: situations where the scope distinction is what differentiates two otherwise similar tools.

Examples:
- `search_web_articles` (vs. `search_internal_documents`)
- `read_pdf_content` (vs. `read_web_content`)
- `extract_structured_data` (vs. `extract_raw_text`)

Best when: you have multiple tools doing the same action on different input types or domains.

### 3. Domain Prefixed
Format: `domain_verb_noun`

Use for: large systems where many tools span different domains — the prefix provides immediate namespace disambiguation.

Examples:
- `billing_lookup_invoice`
- `billing_apply_credit`
- `support_escalate_ticket`
- `inventory_check_stock`

Best when: you have 10+ tools and Claude needs an immediate signal about which subsystem a tool belongs to.

## The Common Thread

Every name must tell Claude **exactly what the tool does**. The name is the first selection filter — Claude narrows candidates by name before reading descriptions in detail.

## Verbs to Avoid

Generic verbs provide zero signal and should never anchor a tool name:
- `process_*` — process what? How?
- `handle_*` — handle how?
- `do_*` — do what?
- `manage_*` — manage in what way?
- `run_*` — run what kind of operation?

Replace with specific action verbs: `extract`, `summarize`, `verify`, `lookup`, `search`, `create`, `update`, `delete`, `check`, `validate`.

## Connection to System Prompt Trap

Tool names interact with system prompt language. A name containing a keyword that appears in the system prompt creates [[system-prompt-keyword-trap|unintended selection bias]]. Naming strategy should include an audit of the system prompt for potential collisions.

Related: [[tool-description-design]], [[five-elements-tool-description]], [[generic-tool-splitting]], [[system-prompt-keyword-trap]], [[tool-description-anti-patterns]]

[Source: 09-TKmhUOlzM6w.md]
