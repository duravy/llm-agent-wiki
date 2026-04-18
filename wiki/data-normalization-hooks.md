---
title: Data Normalization via Post-Tool-Use Hooks
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

When an agent talks to multiple backend systems, each returns data in its own format. Post-tool-use hooks solve this by normalizing every tool result into a consistent format before Claude ever sees it. [Source: 06-UTzVVg8Aepc.md]

## The Heterogeneous Format Problem

Five tools, five different ways to represent the same data:

| Backend | What it returns | Problem |
|---------|----------------|---------|
| Order system | Unix timestamp (`1710633600`) | Big integer, Claude sometimes misreads |
| Customer system | ISO date string (`2024-03-17`) | Standard, but inconsistent with others |
| Shipping API | US date format (`03/17/2024`) | Regional convention, different from ISO |
| Legacy database | Numeric status code (`200`) | No label, meaningless to Claude |
| Modern API | String label (`"active"`) | Readable, but inconsistent with legacy |

Without normalization, Claude must juggle all five representations simultaneously — and gets confused, especially with raw Unix timestamps.

## The Solution: Post-Tool-Use Hook Normalization

The hook intercepts every tool result before Claude receives it:

```
Claude calls tool → Tool executes → Hook intercepts → Claude gets clean data
```

Claude never sees the raw, messy original. The conversion happens deterministically, every time, with zero exceptions.

## Normalization Targets (Exam-Tested)

| Raw format | Normalized to |
|-----------|--------------|
| Unix timestamp (integer) | ISO 8601 string (`2024-03-17T00:00:00Z`) |
| US date format (MM/DD/YYYY) | ISO 8601 string |
| Regional date conventions | ISO 8601 string |
| Numeric status code (200, 404) | String label (`"active"`, `"not_found"`) |

## Python Code Pattern

```python
from datetime import datetime, timezone

def post_tool_hook(tool_name: str, raw_result: dict) -> dict:
    result = raw_result.copy()  # never mutate the original

    # Normalize timestamp fields
    for field in ("created_at", "updated_at", "shipped_at"):
        if field in result and isinstance(result[field], (int, float)):
            result[field] = datetime.fromtimestamp(
                result[field], tz=timezone.utc
            ).isoformat()

    # Map numeric status codes to string labels
    status_map = {200: "active", 404: "not_found", 410: "cancelled"}
    if "status" in result and isinstance(result["status"], int):
        result["status"] = status_map.get(result["status"], str(result["status"]))

    return result
```

Key properties:
- **Pure function** — data in, clean data out; no side effects, no network calls
- **Create a copy** — never mutate the original result
- **Fast** — runs on every tool call, so it must add minimal latency

## Why Prompts Are Not Enough for Normalization

Instructing Claude to "always convert Unix timestamps to ISO format" in the system prompt works 95–99% of the time. But [[probabilistic-vs-deterministic|1% of 10,000 requests = 100 failures]]. For data that feeds downstream systems or gets displayed to users, that is 100 silent, hard-to-trace errors. Hooks give you **deterministic normalization — guaranteed by code**, not by hope.

> Analogy: reports from US, UK, and Japan offices all use different units. Before presenting to your boss, you convert everything to one system. That is data normalization.

## Connection to Other Pages

- [[hooks]] — hub page for all hook types and return behaviors
- [[hooks-vs-prompts]] — the full comparison table on reliability and use cases
- [[hooks-decision-framework]] — when to use hooks vs. prompts
- [[probabilistic-vs-deterministic]] — why 1% failures matter at production scale
- [[hooks-anti-patterns]] — Anti-pattern 3: hooks that silently modify data without logging

[Source: 06-UTzVVg8Aepc.md]
