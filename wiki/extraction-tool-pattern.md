---
title: Extraction Tool Pattern (D4 T4.3)
created: 2026-04-17
last_updated: 2026-04-17
source_count: 1
status: draft
---

The three-step mechanism for getting guaranteed valid structured JSON from Claude. The key insight: a "tool" does not need to have real behavior — it can be a pure JSON schema that Claude is forced to fill in. [Source: 22-CxwIKbepDwQ.md]

## The Three Steps

### Step 1: Define an extraction tool (schema only)

Define a tool with a JSON schema but no actual implementation. Specify:
- Field names and types
- Required fields (only fields that truly always exist in the source document)
- Enums with escape hatches (`unclear`, `other`) — see [[enum-escape-hatches]]
- Nullable types for optional fields — see [[nullable-fields]]

```python
extract_invoice_data = {
    "name": "extract_invoice_data",
    "description": "Extract structured data from an invoice document",
    "input_schema": {
        "type": "object",
        "properties": {
            "vendor_name":    {"type": "string"},
            "invoice_number": {"type": "string"},
            "total":          {"type": ["number", "null"]},   # nullable
            "tax":            {"type": ["number", "null"]},   # nullable
            "category": {
                "type": "string",
                "enum": ["supplies", "services", "software", "unclear", "other"]
            },
            "category_detail": {"type": "string"}  # populated when category = "other"
        },
        "required": ["vendor_name", "invoice_number", "category"]
    }
}
```

### Step 2: Force the tool call

Set `tool_choice` to force the specific tool (when document type is known) or `any` (when document type varies and you have multiple extraction schemas):

```python
response = client.messages.create(
    model="claude-opus-4-6",
    max_tokens=1024,
    tools=[extract_invoice_data],
    tool_choice={"type": "tool", "name": "extract_invoice_data"},
    messages=[{"role": "user", "content": invoice_text}]
)
```

No text output allowed. Claude must call the named tool.

### Step 3: Extract the result

```python
extracted = response.content[0].input
# Guaranteed valid JSON matching the schema.
# No parsing, no regex, no cleanup needed.
```

The data lives in `response.content[0].input`. It is valid JSON by API guarantee — the system enforces this, not a prompt instruction. [Source: 22-CxwIKbepDwQ.md]

## Why This Works

Claude must produce valid JSON to call any tool — the API enforces this at the system level. A dummy extraction tool hijacks this mechanism: the "tool" is never executed, it's just a schema. Claude fills in the values; the API ensures valid JSON. This is fundamentally different from a prompt instruction like "respond in JSON" — the latter is probabilistic; the former is enforced.

## Exam Signal

> **"Claude literally cannot produce malformed JSON for a tool call. The API enforces it at the system level."** [Source: 22-CxwIKbepDwQ.md]

Common exam question: What do you read to get the extracted data? Answer: `response.content[0].input` — not `response.content[0].text`.

## Design Choices

- **Required list:** Only fields that truly always exist. If an invoice sometimes has no PO number, do not require it — Claude will fabricate it. See [[nullable-fields]].
- **Enums:** Always include `unclear` and `other`. See [[enum-escape-hatches]].
- **`tool_choice: auto`:** Never use for extraction — Claude may return text. See [[tool-choice-modes]].

Related: [[structured-output-tool-use]], [[nullable-fields]], [[enum-escape-hatches]], [[syntax-vs-semantic-errors]]

[Source: 22-CxwIKbepDwQ.md]
