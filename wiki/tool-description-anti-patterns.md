---
title: Tool Description Anti-Patterns (D2 T2.1)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Five exam-tested anti-patterns in tool description design. All appear as wrong answer choices or scenario setups in exam questions. Recognizing them quickly is a core D2 T2.1 skill.

## Anti-Pattern Summary

| # | Anti-Pattern | What Goes Wrong | Fix |
|---|-------------|-----------------|-----|
| 1 | Minimal descriptions | Claude can't distinguish this tool from others that also "retrieve data" | Add all five elements: purpose, input, output, when-to-use, boundary |
| 2 | Overlapping descriptions | Claude picks randomly between tools that sound the same | Make each description explicitly state what makes it unique |
| 3 | Generic tool names | `analyze_document` could mean five different operations | Split into specific tools with distinct names |
| 4 | System prompt keyword collisions | Prompt word matches a tool name, biasing selection away from the correct tool | Audit system prompt; rename tool to eliminate the match |
| 5 | Missing boundary conditions | Claude uses the tool for inputs it wasn't designed for | Explicitly state when NOT to use the tool and which tool to use instead |

## Anti-Pattern 1: Minimal Descriptions

**What it looks like**: "retrieves data", "analyzes content", "processes input"

**The problem**: When five tools all retrieve data, Claude has no basis for choosing between them. The description fails to communicate what makes this tool different.

**The fix**: Specify all five elements — especially purpose (what specifically) and boundary (when not this tool).

## Anti-Pattern 2: Overlapping Descriptions

**What it looks like**: Two tools with descriptions that are nearly identical in framing.

**The problem**: Claude picks randomly. In production, this means ~50% misrouting for the overlapping use case.

**The fix**: Each description must explicitly state what makes it unique — the specific situation that triggers it and the explicit exclusion of the other tool's use case.

## Anti-Pattern 3: Generic Tool Names

**What it looks like**: `analyze_document`, `process_request`, `handle_data`

**The problem**: The name gives zero signal. Claude enters description-reading with no prior filter, and a vague name compounds a vague description.

**The fix**: Split into specific tools with action+object or verb+scope names. See [[generic-tool-splitting]] and [[tool-naming-strategies]].

## Anti-Pattern 4: System Prompt Keyword Collisions

**What it looks like**: System prompt says "always analyze customer requests" + tool named `analyze_content` exists.

**The problem**: The keyword "analyze" biases Claude toward `analyze_content` even when a different tool is correct.

**The fix**: Rename the tool to remove the colliding keyword (e.g., `extract_web_content`). See [[system-prompt-keyword-trap]].

## Anti-Pattern 5: Missing Boundary Conditions

**What it looks like**: Description says what the tool does, but not what it does NOT do.

**The problem**: Claude uses the tool for adjacent inputs it wasn't built for (PDFs sent to a web-page extractor, searches sent to a document retriever).

**The fix**: Add explicit "do not use for X — use Y instead" statements. These cross-references between tools make the boundary actionable, not just restrictive.

## Exam Application

When a scenario describes unexpected tool behavior, run through this checklist in order:
1. Are descriptions minimal? (AP1)
2. Do descriptions overlap? (AP2)
3. Are tool names generic? (AP3)
4. Does the system prompt contain tool-name keywords? (AP4)
5. Are boundary conditions missing? (AP5)

Related: [[tool-description-design]], [[five-elements-tool-description]], [[tool-misrouting]], [[generic-tool-splitting]], [[system-prompt-keyword-trap]], [[tool-naming-strategies]]

[Source: 09-TKmhUOlzM6w.md]
