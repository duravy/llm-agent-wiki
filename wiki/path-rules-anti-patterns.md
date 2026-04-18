---
title: Path-Specific Rules Anti-Patterns (T3.3)
created: 2026-04-16
last_updated: 2026-04-16
source_count: 1
status: draft
---

Four exam-tested anti-patterns for Task 3.3. These are the wrong answer choices on the exam.

## Anti-pattern 1: Subdirectory claude.md for Cross-Cutting Concerns

**Wrong**: Using a subdirectory `claude.md` to enforce test file conventions.

**Why it's wrong**: Test files live next to the code they test — spread across components, API, billing, utils, everywhere. You can't put a `claude.md` in every directory. You get duplicate content and a maintenance nightmare.

**Correct approach**: One path-specific rule with `paths: ["**/*.test.*"]` covers the entire codebase. See [[crosscutting-concerns]].

---

## Anti-pattern 2: No `paths:` Field on Rule Files

**Wrong**: Creating a rule file in `.claude/rules/` but omitting the `paths:` YAML frontmatter field.

**Why it's wrong**: Without a `paths:` field, the rule loads for every single file — identical to putting it in `claude.md` directly. You get no conditional loading benefit at all.

**Correct approach**: Always add a `paths:` field to scope the rule. See [[glob-pattern-routing]].

---

## Anti-pattern 3: Overly Broad Glob Patterns

**Wrong**: Setting `paths: ["**/*"]` on a rule file.

**Why it's wrong**: `**/*` matches every file. The rule loads for every file — exactly the same as having no `paths:` field. This completely defeats the purpose of conditional loading.

**Correct approach**: Be specific. `src/api/**/*` not `**/*`. Match only the files that actually need the rule.

---

## Anti-pattern 4: Relying on Claude to Infer Which Section Applies

**Wrong**: Keeping all conventions in one monolithic `claude.md` and expecting Claude to infer which section is relevant for the current file.

**Why it's wrong**: Inference is not pattern matching. Claude is a probabilistic system — it can be wrong about which convention applies, and you cannot guarantee it picks the right section. This is the same [[probabilistic-vs-deterministic]] distinction from D1 T1.4: inference is probabilistic; `paths:` matching is deterministic.

**Correct approach**: Use explicit path scoping. The rule either loads or it doesn't — no inference required.

---

## Summary Table

| # | Anti-pattern | Correct approach |
|---|-------------|-----------------|
| 1 | Subdirectory `claude.md` for cross-cutting concerns | One path-specific rule with glob pattern |
| 2 | No `paths:` field (loads for every file) | Always add `paths:` frontmatter |
| 3 | Overly broad glob (`**/*`) | Use specific patterns (`src/api/**/*`) |
| 4 | Relying on Claude to infer applicable section | Explicit path scoping — deterministic, not probabilistic |

[Source: 16-iOvyCv_rwac.md]
