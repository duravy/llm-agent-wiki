---
title: Series Overview — Claude Architect Certification Exam
created: 2026-04-15
last_updated: 2026-04-16
source_count: 30
status: draft
---

A 32-part YouTube video series serving as a study guide for the Claude Architect Certification Exam. The series is organized into 5 domains, each containing 5–6 tasks (subdomains). Each video covers one task in depth, with exam-oriented framing ("the exam tests this directly").

## Structure

| Domain | Topic | Parts (approx) |
|--------|-------|----------------|
| 1 | Core API & Agentic Loops | Parts 1–8 (T1.1–T1.7 confirmed; **complete**) |
| 2 | Tool Design and MCP Integration | Parts 9–13 (T2.1–T2.5 confirmed; **complete**) |
| 3 | Claude Code Configuration | Parts 14–19 (T3.1–T3.6 confirmed; **complete**) |
| 4 | Prompt Engineering and Output Quality | Parts 20–25 (T4.1–T4.6 confirmed; **complete**) |
| 5 | Agentic Systems (long-running, multi-agent) | Parts ~24–32 |

> Note: Domain boundaries will be confirmed as more parts are ingested.

## Known Parts

| Part | Domain | Task | Topic |
|------|--------|------|-------|
| 1 | — | — | Exam overview: 4 domains, weights, study order (teased) |
| 2 | 1 | 1.1 | [[agentic-loop]] — communication stack, messages.create, tools as descriptions, 4-step loop, stop_reason, model-driven agency |
| 3 | 1 | 1.2 | [[multi-agent-coordinator]] — hub-and-spoke, isolated context, coordinator responsibilities, task tool, parallel/sequential, dynamic routing, structured handoffs |
| 4 | 1 | 1.3 | [[raw-api-vs-sdk]] — SDK vs RAW API, [[agent-definition]] (5 fields), [[minimum-tools-principle]], [[context-passing]], [[fork-sessions]], [[goal-oriented-prompting]], [[sdk-anti-patterns]] |
| 5 | 1 | 1.4 | [[probabilistic-vs-deterministic]] — enforcement modes, [[prerequisite-gates]], [[code-vs-prompt-enforcement]], [[structured-escalation-handoff]], [[multiconcern-decomposition]], [[hooks]], [[enforcement-anti-patterns]] |
| 6 | 1 | 1.5 | [[data-normalization-hooks]] — post-tool normalization; [[hooks-vs-prompts]] comparison table; [[hooks-decision-framework]] (1% test); [[hooks-anti-patterns]] |
| 7 | 1 | 1.6 | [[task-decomposition]] — [[prompt-chaining]] (fixed pipeline) vs [[dynamic-decomposition]] (adaptive); [[attention-dilution]] and per-pass decomposition; [[decomposition-anti-patterns]] |
| 8 | 1 | 1.7 | [[session-state]] — named sessions, resume vs. start fresh decision; [[stale-context]] and two mitigations; [[fork-sessions]] four properties; [[session-anti-patterns]]; closes Domain 1 |
| 9 | 2 | 2.1 | [[tool-description-design]] — tool descriptions as only signal; [[five-elements-tool-description]] (purpose, input, output, when-to-use, boundary); [[tool-misrouting]]; [[generic-tool-splitting]]; [[system-prompt-keyword-trap]]; [[tool-naming-strategies]]; [[tool-description-anti-patterns]] |
| 10 | 2 | 2.2 | [[mcp-error-responses]] — `is_error` flag (signal layer) + structured error body (decision layer); [[mcp-error-categories]] (transient/validation/business/permission); [[mcp-error-fields]] (5 fields); [[access-failure-vs-empty-result]]; [[local-error-recovery]]; [[mcp-error-anti-patterns]] |
| 11 | 2 | 2.3 | [[tool-distribution]] — [[too-many-tools-problem]] (4–5 sweet spot); [[tool-scoping]] (least privilege, coordinator gets task_tool only); [[crossrole-tools]] (+40% latency exception); [[tool-choice-modes]] (auto/any/force + ordered pipeline); [[constrained-tool-alternatives]]; [[tool-distribution-anti-patterns]] |
| 12 | 2 | 2.4 | [[mcp-server-integration]] — USB analogy; regular vs MCP tools (auto-discovery); [[mcp-configuration-scopes]] (project vs user level); [[environment-variable-expansion]] (`${VAR}` pattern); [[mcp-resources]] (read-only catalogs, reduce exploratory calls); [[community-vs-custom-servers]]; [[mcp-tool-description-fix]] (CLAUDE.md as fix layer); [[mcp-integration-anti-patterns]] |
| 13 | 2 | 2.5 | [[builtin-tools]] — 6 tools (Grep/Glob/Read/Write/Edit/Bash); [[grep-vs-glob]] (most-tested distinction); [[two-phase-search-strategy]] (Grep entry point → Read); [[edit-vs-write]] (targeted vs full-file; Edit fallback = Read+Write); [[incremental-exploration]] (follow imports step by step); [[builtin-tools-anti-patterns]]; closes Domain 2 |
| 14 | 3 | 3.1 | [[claude-md-hierarchy]] — CLAUDE.md as persistent session config; [[claude-md-levels]] (3 levels: user/project/subdirectory; most specific wins); [[at-import-syntax]] (modular @import references); [[rules-directory]] (.claude/rules auto-loading + YAML frontmatter scoping); [[monolithic-split]] (800 lines → 50-line root); [[memory-command]] (/memory for debugging); [[claude-md-anti-patterns]]; opens Domain 3 |
| 15 | 3 | 3.2 | [[commands-and-skills]] — commands = keyboard shortcuts, skills = plugins; [[custom-commands]] (.claude/commands project vs user scope); [[skills]] (YAML frontmatter: context:fork, allowed_tools, argument_hint); [[context-fork]] (isolated sub-agent; main context stays clean); [[skills-allowed-tools]] (least privilege; read-only skill = [read,grep,glob]); [[skills-vs-claude-md]] (always-loaded vs on-demand decision); [[commands-skills-anti-patterns]] |
| 16 | 3 | 3.3 | [[path-specific-rules]] — `paths:` YAML field in `.claude/rules/` for conditional loading; [[glob-pattern-routing]] (`**/*.test.*`, `src/api/**/*`, OR logic); [[crosscutting-concerns]] (test files spread everywhere → one rule file); [[path-rules-vs-subdirectory-claude-md]] (pattern-bound vs location-bound); [[path-rules-anti-patterns]] (4 anti-patterns; "conventions spread across dirs" exam signal) |
| 17 | 3 | 3.4 | [[plan-mode-vs-direct-execution]] — decision framework + combined mode; [[plan-mode]] (4 scenarios: multi-approach, large scope, architectural, rework risk; human review checkpoint); [[direct-execution]] (4 scenarios: well-defined, single-file, no arch decision, known approach; one-sentence test); [[explore-sub-agent]] (isolated investigation; keeps 45 files + 200 greps out of main context); [[plan-mode-anti-patterns]] |
| 18 | 3 | 3.5 | [[iterative-refinement-techniques]] — prose → inconsistent; [[concrete-examples]] (2–3 I/O pairs define transformation); [[test-driven-iteration]] (write tests → share exact failure → iterate to green); [[interview-pattern]] (clarifying questions before code in unfamiliar domains); [[interacting-vs-independent-issues]] (fix together vs sequential); [[iterative-refinement-anti-patterns]] |
| 19 | 3 | 3.6 | [[cicd-integration]] — [[p-flag]] (`-p` non-interactive; required for all CI); [[structured-output-ci]] (`--output-format json` + `--json-schema`); CLAUDE.md in CI; [[session-isolation-ci]] (fresh session = no generator bias); [[prior-findings-injection]] (prevent duplicate comments + duplicate coverage); [[cicd-anti-patterns]]; closes Domain 3 |
| 20 | 4 | 4.1 | [[prompt-design-explicit-criteria]] — Domain 4 intro (explicit criteria, few-shot, structured output, validation loops); [[false-positive-problem]] (smoke detector analogy; trust erosion cascade); [[categorical-definitions]] (include + exclude lists; subjective thresholds → inconsistent); [[severity-levels-examples]] (Critical/High/Medium/Low with code examples; Low = do not report); [[prompt-design-anti-patterns]] (5 anti-patterns; all exam-tested); opens Domain 4 |
| 21 | 4 | 4.2 | [[few-shot-prompting]] — 2–4 I/O pairs with reasoning; instructions alone fail for ambiguous routing; [[reasoning-in-examples]] (why → generalization, not just action matching); [[gray-area-examples]] (optional chaining vs null bug vs empty catch; null extraction = return null not fabricated); [[example-count-guidelines]] (2–3 sweet spot; 4–5 ambiguous domains; 10+ anti-pattern); [[few-shot-anti-patterns]] (5 anti-patterns: happy-path-only, no-reasoning, too-many, all-same, prose-format) |
| 22 | 4 | 4.3 | [[structured-output-tool-use]] — prose JSON unreliable; [[extraction-tool-pattern]] (define schema as dummy tool; force call; read from `content[0].input`); [[nullable-fields]] (absent data → null, not fabricated); [[enum-escape-hatches]] (`unclear`/`other` + detail field); [[syntax-vs-semantic-errors]] (API enforces syntax; semantic errors still possible); [[structured-output-anti-patterns]] (5 anti-patterns); `tool_choice: auto` = never for extraction |
| 23 | 4 | 4.4 | [[validation-retry-loops]] — complete extract→validate→retry→human-review architecture; [[retry-with-error-feedback]] (original doc + failed extraction + specific errors); [[retriable-vs-nonretriable-failures]] (format errors: retry; absent data: accept null); [[self-correction-dual-totals]] (stated_total + calculated_total + conflict_detected); [[detected-pattern-field]] (false positive trend analysis); [[validation-retry-anti-patterns]] (5 anti-patterns) |
| 24 | 4 | 4.5 | [[message-batches-api]] — economy shipping vs express delivery; [[batch-vs-synchronous-api]] (blocking = sync, non-blocking = batch); batch = single-turn, no tool calls; [[custom-id-tracking]]; [[partial-failure-resubmission]] (resubmit only failures); [[two-phase-batch-strategy]] (sync sample → batch scale); [[batch-anti-patterns]] (5 anti-patterns) |
| 25 | 4 | 4.6 | [[multi-instance-review]] — [[self-review-bias]] (same session retains reasoning, evaluates against intent not code); [[multipass-review-architecture]] (pass 1 parallel per-file, pass 2 cross-file integration, pass 3 confidence verification); [[confidence-calibrated-routing]] (≥4 auto-post, 2–3 human, ≤1 discard); [[multi-instance-anti-patterns]] (5 anti-patterns); closes Domain 4 |
| 26 | 5 | 5.1 | [[context-management]] — context management in long agent conversations |
| 27 | 5 | 5.2 | [[escalation-ambiguity-resolution]] — escalation triggers, sentiment fallacy, ambiguity resolution |
| 28 | 5 | 5.3 | [[error-propagation-multiagent]] — structured errors, graceful degradation, coverage annotations |
| 29 | 5 | 5.4 | [[large-codebase-context]] — context degradation, scratchpad files, subagent delegation, compact, state persistence |
| 30 | 5 | 5.5 | [[human-review-workflows]] — aggregate accuracy trap, per-field confidence, calibration, routing thresholds, stratified sampling, feedback loop |
| 31 | 5 | 5.6 | [[information-provenance]] — attribution loss, claim-source mappings, conflicting sources, publication dates, confidence-structured output, content-type rendering |
| 32 | — | — | [[exam-sample-questions]] — 12 exam-style Q&A across all domains (teased) |

## Exam Style
- Questions are multiple-choice or scenario-based
- Key signals: "If the exam asks about X, the answer is Y"
- Two-answer mnemonics are used: e.g., losing details → case facts blocks; wasting tokens → trim tool outputs

> CONTRADICTION: Part 25 was originally placed in Domain 5 based on inference. The Part 25 source transcript explicitly states "task 4.6 in domain 4." The entry above has been corrected to reflect the authoritative source.

[Source: 26-H8Vpe9j4N5I.md, 02-OCiLc9Frq84.md, 03-KQSSSP0op2M.md, 12-NlTC0XY8orw.md, 13-IN2WntoHedY.md, 18-J2rT6yp-Lak.md, 19-UnN8c0Sshq8.md, 20-EzWRMJz9lXc.md, 21-u6oCkdxU5kw.md, 22-CxwIKbepDwQ.md, 23-wyqmHtVGxAg.md, 24-6ZGgMSQz3eg.md, 25-W7c9wNfoCjU.md]
