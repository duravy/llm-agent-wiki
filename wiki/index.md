# Wiki Index — Claude Architect Certification Exam

## Series Structure
- [series-overview.md](series-overview.md) — 32-part YouTube series, 5 domains, exam format and style
- [domain-1-overview.md](domain-1-overview.md) — Domain 1: Core API & Agentic Loops — 27% of exam, foundational domain
- [domain-2-overview.md](domain-2-overview.md) — Domain 2: Tool Design and MCP Integration — 20% of exam, T2.1–T2.5
- [domain-3-overview.md](domain-3-overview.md) — Domain 3: Claude Code Configuration — T3.1–T3.6 confirmed; complete
- [domain-4-overview.md](domain-4-overview.md) — Domain 4: Prompt Engineering and Output Quality — T4.1–T4.6 confirmed; complete (Parts 20–25)
- [domain-5-overview.md](domain-5-overview.md) — Domain 5: Agentic Systems — task list and domain intro

## Domain 1: Core API & Agentic Loops (D1)

### Agentic Loop (D1 T1.1)
- [agentic-loop.md](agentic-loop.md) — Hub page: 4-step loop, exam mnemonic S-C-E-A, key distinctions
- [communication-stack.md](communication-stack.md) — 4-layer stack: code → SDK → API → Claude's servers
- [messages-create-parameters.md](messages-create-parameters.md) — 5 parameters of messages.create; model + max_tokens required
- [tools-as-descriptions.md](tools-as-descriptions.md) — Most-tested D1 distinction: Claude sees name + description + schema only
- [agentic-loop-steps.md](agentic-loop-steps.md) — 4-step cycle: Send, Check stop_reason, Execute tools, Append
- [stop-reason.md](stop-reason.md) — Three values: end_turn / tool_use / max_tokens; only reliable control signal
- [model-driven-vs-scripted.md](model-driven-vs-scripted.md) — Claude = planner; your code = executor; vs developer-hardcoded sequences
- [agentic-loop-anti-patterns.md](agentic-loop-anti-patterns.md) — Three exam-tested anti-patterns: text parsing, fixed cap, keyword routing

### Multi-Agent Coordinator (D1 T1.2)
- [multi-agent-coordinator.md](multi-agent-coordinator.md) — Hub page: 9 exam must-knows, when to use multi-agent
- [hub-and-spoke-architecture.md](hub-and-spoke-architecture.md) — 3 hard rules: all comms through coordinator, coordinator owns errors, coordinator controls context
- [isolated-context.md](isolated-context.md) — Most-tested detail: sub-agents start fresh; never assume inherited context
- [coordinator-responsibilities.md](coordinator-responsibilities.md) — D-D-A-E-R: Decompose, Delegate, Aggregate, Evaluate, Respond
- [task-tool.md](task-tool.md) — 4-step delegation: tool_use block → SDK spawns fresh instance → sub-agent loop → tool_result
- [parallel-vs-sequential-execution.md](parallel-vs-sequential-execution.md) — Parallel for independent tasks; sequential when step N+1 needs step N; fork sessions overview
- [dynamic-routing.md](dynamic-routing.md) — Evaluate request first; distinct non-overlapping work partitions
- [iterative-refinement.md](iterative-refinement.md) — Redelegate if gaps; structured handoffs (source + URL + date) on every finding
- [multi-agent-anti-patterns.md](multi-agent-anti-patterns.md) — Five exam-tested anti-patterns: direct comms, inherited context, full pipeline, overlap, raw text

### SDK Implementation (D1 T1.3)
- [raw-api-vs-sdk.md](raw-api-vs-sdk.md) — API vs framework distinction; SDK is a framework, controls the loop
- [agent-definition.md](agent-definition.md) — Exactly 5 fields: name, description, system_prompt, tools, allowed_tools
- [minimum-tools-principle.md](minimum-tools-principle.md) — Each agent gets only what it needs; security + reliability + cost rationale
- [context-passing.md](context-passing.md) — Active construction job; what to pass vs what not to pass; structured task prompt pattern
- [fork-sessions.md](fork-sessions.md) — Third execution pattern: same baseline context, diverging directions
- [goal-oriented-prompting.md](goal-oriented-prompting.md) — Specify goal + output format + constraints; never specify tool call sequences
- [sdk-anti-patterns.md](sdk-anti-patterns.md) — Six exam-tested SDK anti-patterns: missing task, all-tools, history dump, procedural prompts, unstructured handoffs, sequential-as-style

### Enforcement and Handoff Patterns (D1 T1.4)
- [probabilistic-vs-deterministic.md](probabilistic-vs-deterministic.md) — Fundamental distinction: prompt compliance vs code-enforced guarantees; 1% of 10k = 100 failures
- [prerequisite-gates.md](prerequisite-gates.md) — Code pattern: block tool B until tool A completes; raise exception, not soft error
- [code-vs-prompt-enforcement.md](code-vs-prompt-enforcement.md) — Decision framework: the irreversibility test; when to use each
- [structured-escalation-handoff.md](structured-escalation-handoff.md) — 4-field JSON handoff: action_requested, context, options, recommended_action
- [multiconcern-decomposition.md](multiconcern-decomposition.md) — Split complex requests into parallel isolated concern agents
- [hooks.md](hooks.md) — Hub page: pre/post hooks; 3 return behaviors; normalization + policy enforcement jobs; performance rule
- [enforcement-anti-patterns.md](enforcement-anti-patterns.md) — Five exam-tested anti-patterns: prompt-only safety, skipping prerequisites, vague escalation, ignoring hooks, dependent parallelism

### Session State, Resumption, and Forking (D1 T1.7)
- [session-state.md](session-state.md) — Hub: named sessions, --session/--resume flags, resume-vs-fresh decision framework
- [stale-context.md](stale-context.md) — Core risk of resumption; two mitigations: inform Claude of changes, or start fresh + inject summary
- [session-anti-patterns.md](session-anti-patterns.md) — Five exam-tested anti-patterns: resuming stale, starting fresh unnecessarily, no named sessions, forking linear tasks, resuming exhausted context

### Task Decomposition Strategies (D1 T1.6)
- [task-decomposition.md](task-decomposition.md) — Hub page: prompt chaining vs dynamic decomposition; decision table; attention dilution
- [prompt-chaining.md](prompt-chaining.md) — Fixed sequential pipeline; per-file passes parallelizable; integration pass uses summaries
- [dynamic-decomposition.md](dynamic-decomposition.md) — Adaptive plan; steps emerge from discoveries; plan insertion mid-execution; higher cost
- [attention-dilution.md](attention-dilution.md) — Middle items get less attention; per-file passes + integration pass solution; same root as lost-in-the-middle
- [decomposition-anti-patterns.md](decomposition-anti-patterns.md) — Five exam-tested anti-patterns: single-prompt review, wrong strategy for task type, narrow subtasks, no integration pass

### Hooks — Tool Call Interception & Data Normalization (D1 T1.5)
- [data-normalization-hooks.md](data-normalization-hooks.md) — Post-tool hook: normalizing Unix timestamps, US dates, numeric status codes to consistent formats
- [hooks-vs-prompts.md](hooks-vs-prompts.md) — Core exam concept: 6-dimension comparison table; failure rates 0% vs 1–5%; override behavior
- [hooks-decision-framework.md](hooks-decision-framework.md) — The 1% test: hard rules → hooks, soft preferences → prompts; performance implication
- [hooks-anti-patterns.md](hooks-anti-patterns.md) — Five exam-tested anti-patterns: prompts for data, blocking without explanation, silent modification, soft-pref hooks, no redirect

## Domain 2: Tool Design and MCP Integration (D2)

### Designing Effective Tool Interfaces (D2 T2.1)
- [tool-description-design.md](tool-description-design.md) — Hub page: description as only signal, five elements, high-leverage fix, D1→D2 connection
- [five-elements-tool-description.md](five-elements-tool-description.md) — Purpose, input format, output, when-to-use, boundary — with examples and exam signal
- [tool-misrouting.md](tool-misrouting.md) — How misrouting happens, canonical example, diagnosis protocol, correct fix pattern
- [generic-tool-splitting.md](generic-tool-splitting.md) — When to split one generic tool into multiple specific tools; naming signal; vs minimum tools principle
- [system-prompt-keyword-trap.md](system-prompt-keyword-trap.md) — Prompt keywords biasing selection; audit process; fix in tool name, not system prompt
- [tool-naming-strategies.md](tool-naming-strategies.md) — Three patterns: action+object, verb+scope, domain-prefixed; verbs to avoid
- [tool-description-anti-patterns.md](tool-description-anti-patterns.md) — Five anti-patterns: minimal, overlapping, generic names, keyword collisions, missing boundaries

### Distributing Tools Across Agents (D2 T2.3)
- [tool-distribution.md](tool-distribution.md) — Hub page: too-many-tools problem, least privilege, cross-role exception, tool_choice modes, D1↔D2 connection
- [too-many-tools-problem.md](too-many-tools-problem.md) — Research finding: 4–5 sweet spot; 18 tools = reliability drop; description vs count as separate diagnoses
- [tool-scoping.md](tool-scoping.md) — Least privilege per role; coordinator gets task_tool only; off-role tools cause misuse
- [crossrole-tools.md](crossrole-tools.md) — Exception pattern: high-frequency simple ops, +40% latency rationale, constrained scope required
- [tool-choice-modes.md](tool-choice-modes.md) — Three modes: auto (default) / any (guaranteed call) / force (specific tool); ordered pipeline pattern
- [constrained-tool-alternatives.md](constrained-tool-alternatives.md) — Replace generic tools (fetch_url) with scoped versions (load_document); code-level misuse prevention
- [tool-distribution-anti-patterns.md](tool-distribution-anti-patterns.md) — Five anti-patterns: all-tools, 18-tools, no-crossrole, auto-when-any-needed, generic-tools

### Selecting Built-In Tools Effectively (D2 T2.5)
- [builtin-tools.md](builtin-tools.md) — Hub page: 6 tools, workshop analogy, six exam takeaways
- [grep-vs-glob.md](grep-vs-glob.md) — Most-tested distinction: Grep searches contents, Glob searches names/paths
- [two-phase-search-strategy.md](two-phase-search-strategy.md) — Grep for entry points → Read specific files; never read upfront
- [edit-vs-write.md](edit-vs-write.md) — Targeted replacement vs full overwrite; Edit uniqueness rule; Read+Write fallback
- [incremental-exploration.md](incremental-exploration.md) — Follow imports step by step; each discovery narrows the next search
- [builtin-tools-anti-patterns.md](builtin-tools-anti-patterns.md) — Five anti-patterns: read-all-upfront, Grep-for-names, Write-for-one-line, non-unique Edit, Bash-for-search

### Integrating MCP Servers (D2 T2.4)
- [mcp-server-integration.md](mcp-server-integration.md) — Hub page: USB analogy, regular vs MCP tools, auto-discovery, six exam takeaways
- [mcp-configuration-scopes.md](mcp-configuration-scopes.md) — Project level (mcp.json, committed) vs user level (~/.claude.json, personal); team member exam question
- [environment-variable-expansion.md](environment-variable-expansion.md) — `${VAR}` syntax; why never hardcode; mcp.json safe to commit
- [mcp-resources.md](mcp-resources.md) — Read-only content catalogs; without vs with resources; eliminates exploratory tool call loops
- [community-vs-custom-servers.md](community-vs-custom-servers.md) — Default: community first; custom only when truly unique; battle-tested rationale
- [mcp-tool-description-fix.md](mcp-tool-description-fix.md) — Weak community server descriptions; CLAUDE.md as correct fix layer (not server modification)
- [mcp-integration-anti-patterns.md](mcp-integration-anti-patterns.md) — Five anti-patterns: hardcoded secrets, custom for standard, no resources, user-level shared config, minimal descriptions

### Structured Error Responses for MCP Tools (D2 T2.2)
- [mcp-error-responses.md](mcp-error-responses.md) — Hub page: two-layer system, four categories, five fields, access failure distinction, D2↔D5 connection
- [mcp-error-categories.md](mcp-error-categories.md) — Four categories: transient/validation/business/permission; Claude action and is_retryable for each
- [mcp-error-fields.md](mcp-error-fields.md) — Five structured fields: error_category, is_retryable, message, customer_message, suggestion; with examples
- [is-error-flag.md](is-error-flag.md) — Signal layer: is_error: true vs false; why the flag alone is insufficient; two-layer architecture
- [access-failure-vs-empty-result.md](access-failure-vs-empty-result.md) — Most exam-tested distinction: service down (is_error: true) vs search found nothing (is_error: false, empty array)
- [local-error-recovery.md](local-error-recovery.md) — Sub-agent retry pattern before escalation; what to include in escalation reports; D2↔D5 connection
- [mcp-error-anti-patterns.md](mcp-error-anti-patterns.md) — Five anti-patterns: generic errors, empty results for failures, retrying business violations, propagating all errors, no partial results

## Domain 3: Claude Code Configuration (D3)

### CLAUDE.md Hierarchy (D3 T3.1)
- [claude-md-hierarchy.md](claude-md-hierarchy.md) — Hub page: three levels, modular org, /memory debugging, 8 exam takeaways
- [claude-md-levels.md](claude-md-levels.md) — User / project / subdirectory levels; priority: most specific wins; sharing via git
- [at-import-syntax.md](at-import-syntax.md) — @import for explicit modular CLAUDE.md organization; paths relative to importing file
- [rules-directory.md](rules-directory.md) — .claude/rules auto-loaded files; YAML frontmatter for path-specific scoping (T3.3)
- [monolithic-split.md](monolithic-split.md) — Before/after pattern: 800-line file → 50-line root + 4 focused files
- [memory-command.md](memory-command.md) — /memory shows loaded config; first debugging step when Claude ignores rules
- [claude-md-anti-patterns.md](claude-md-anti-patterns.md) — Five anti-patterns: monolithic file, team in user level, personal in project level, no modular org, not checking /memory

### Custom /commands and Skills (D3 T3.2)
- [commands-and-skills.md](commands-and-skills.md) — Hub page: commands = shortcuts, skills = plugins; 6 exam takeaways
- [custom-commands.md](custom-commands.md) — /commands markdown files; project (.claude/commands/) vs user (~/.claude/commands/) scope
- [skills.md](skills.md) — skill.md with YAML frontmatter; context:fork, allowed_tools, argument_hint; project vs user scope
- [context-fork.md](context-fork.md) — Most-tested skill option: isolated sub-agent execution; main context stays clean
- [skills-allowed-tools.md](skills-allowed-tools.md) — Least privilege for skills; read-only skill = [read, grep, glob] only
- [skills-vs-claude-md.md](skills-vs-claude-md.md) — Decision: universal standards → claude.md; task-specific workflows → skills
- [commands-skills-anti-patterns.md](commands-skills-anti-patterns.md) — Five anti-patterns: team in user level, no context:fork, full tool access, universal in skills, same-name override

### Path-Specific Rules (D3 T3.3)
- [path-specific-rules.md](path-specific-rules.md) — Hub page: `paths:` YAML field, conditional loading, token savings, 5 exam takeaways
- [glob-pattern-routing.md](glob-pattern-routing.md) — Glob patterns: `**/*.test.*`, `src/api/**/*`, OR logic; omit paths = loads everywhere
- [crosscutting-concerns.md](crosscutting-concerns.md) — Test files spread across codebase → one rule file with `**/*.test.*`; vs subdirectory claude.md
- [path-rules-vs-subdirectory-claude-md.md](path-rules-vs-subdirectory-claude-md.md) — Pattern-bound vs location-bound; exam signal: "spread across dirs" → path-specific rules
- [path-rules-anti-patterns.md](path-rules-anti-patterns.md) — Four anti-patterns: subdirectory for cross-cutting, no paths field, broad glob, relying on inference

### Plan Mode vs Direct Execution (D3 T3.4)
- [plan-mode-vs-direct-execution.md](plan-mode-vs-direct-execution.md) — Hub page: decision framework, combined mode pattern, 6 exam takeaways
- [plan-mode.md](plan-mode.md) — 4 scenarios + human review checkpoint; monolith-to-microservices = always plan mode
- [direct-execution.md](direct-execution.md) — 4 scenarios + one-sentence rule of thumb; single-file bug = always direct
- [explore-sub-agent.md](explore-sub-agent.md) — Plan mode's isolated investigation sub-agent; keeps discovery out of main context
- [plan-mode-anti-patterns.md](plan-mode-anti-patterns.md) — Five anti-patterns: architectural direct exec, trivial plan mode, mid-exec restart, skip review, skip explore sub-agent

### Iterative Refinement Techniques (D3 T3.5)
- [iterative-refinement-techniques.md](iterative-refinement-techniques.md) — Hub page: 4 techniques, 5 anti-patterns, 5 exam takeaways; note naming conflict with D1 T1.2
- [concrete-examples.md](concrete-examples.md) — 2–3 input/output pairs replace prose; lock every formatting decision; works for edge cases too
- [test-driven-iteration.md](test-driven-iteration.md) — Write tests first; share exact failure diff (expected/received); iterate to green
- [interview-pattern.md](interview-pattern.md) — Ask Claude to interview you before implementing in unfamiliar domains; surfaces design decisions before code
- [interacting-vs-independent-issues.md](interacting-vs-independent-issues.md) — Interacting issues: fix together; independent: fix sequentially; exam rule: does A affect correct behavior of B?
- [iterative-refinement-anti-patterns.md](iterative-refinement-anti-patterns.md) — Five anti-patterns: prose-only, no tests, serial interacting fixes, skip interview, vague error reports

### CI/CD Integration (D3 T3.6)
- [cicd-integration.md](cicd-integration.md) — Hub page: 6 techniques, 6 anti-patterns, 6 exam takeaways; closes Domain 3
- [p-flag.md](p-flag.md) — `-p`/`--print` flag: non-interactive mode; required for every CI invocation; without it, hangs
- [structured-output-ci.md](structured-output-ci.md) — `--output-format json` + `--json-schema`; structured findings for pipeline tools
- [session-isolation-ci.md](session-isolation-ci.md) — Fresh CI session = no generator bias; self-review bias explained; isolation pattern table
- [prior-findings-injection.md](prior-findings-injection.md) — Include prior findings to prevent duplicate comments; include existing tests to prevent duplicate coverage
- [cicd-anti-patterns.md](cicd-anti-patterns.md) — Six anti-patterns: no `-p`, same-session review, no prior findings, no CLAUDE.md, text output, no existing tests

## Domain 4: Prompt Engineering and Output Quality (D4)

### Prompt Design and Explicit Criteria (D4 T4.1)
- [prompt-design-explicit-criteria.md](prompt-design-explicit-criteria.md) — Hub page: explicit vs. general instructions, precision over recall, 5 exam takeaways
- [false-positive-problem.md](false-positive-problem.md) — Smoke detector analogy; trust erosion cascade; across-all-categories effect
- [categorical-definitions.md](categorical-definitions.md) — Exam decision rule: explicit include/exclude lists vs. subjective thresholds; both halves required
- [severity-levels-examples.md](severity-levels-examples.md) — Critical/High/Medium/Low with code examples; Low = do not report (linters handle)
- [prompt-design-anti-patterns.md](prompt-design-anti-patterns.md) — Five anti-patterns: subjective thresholds, all-possible-issues, no severity examples; all exam-tested

### Few-Shot Prompting (D4 T4.2)
- [few-shot-prompting.md](few-shot-prompting.md) — Hub page: 2–4 I/O pairs with reasoning; routing example; output format consistency; 6 exam takeaways
- [reasoning-in-examples.md](reasoning-in-examples.md) — Always include *why*; enables generalization to novel patterns (not just action copying)
- [gray-area-examples.md](gray-area-examples.md) — Highest-value use: optional chaining vs null bug vs empty catch; null extraction = return null not fabricated
- [example-count-guidelines.md](example-count-guidelines.md) — 2–3 sweet spot; 4–5 ambiguous domains; 1 format-only; 10+ anti-pattern; diversity rule
- [few-shot-anti-patterns.md](few-shot-anti-patterns.md) — Five anti-patterns: happy-path-only, no-reasoning, too-many, all-same, prose-format

### Structured Output via Tool Use (D4 T4.3)
- [structured-output-tool-use.md](structured-output-tool-use.md) — Hub page: prose JSON unreliable; tool use enforces JSON at API level; 6 exam must-knows
- [extraction-tool-pattern.md](extraction-tool-pattern.md) — 3-step pattern: define schema as dummy tool; force call; read from `content[0].input`
- [nullable-fields.md](nullable-fields.md) — Required field + absent data = hallucination; nullable types enforce null-or-value at schema level
- [enum-escape-hatches.md](enum-escape-hatches.md) — Add `unclear` + `other` to every enum; add `category_detail` string for `other` cases
- [syntax-vs-semantic-errors.md](syntax-vs-semantic-errors.md) — Tool use eliminates syntax errors (API-enforced); semantic errors (wrong values) still possible
- [structured-output-anti-patterns.md](structured-output-anti-patterns.md) — Five: prose JSON, all-required, closed enums, auto for extraction, no semantic validation

### Validation and Retry Loops (D4 T4.4)
- [validation-retry-loops.md](validation-retry-loops.md) — Hub page: complete extract→validate→retry→human-review architecture; 5 exam must-knows
- [retry-with-error-feedback.md](retry-with-error-feedback.md) — 3-part retry: original doc + failed extraction + specific errors; enables self-correction
- [retriable-vs-nonretriable-failures.md](retriable-vs-nonretriable-failures.md) — Most exam-tested T4.4 distinction: format errors retry; absent data → accept null (retry = hallucination risk)
- [self-correction-dual-totals.md](self-correction-dual-totals.md) — stated_total + calculated_total + conflict_detected; catches extraction errors AND source document errors
- [detected-pattern-field.md](detected-pattern-field.md) — Tag findings with pattern IDs; enables false positive trend analysis and prompt improvement over time
- [validation-retry-anti-patterns.md](validation-retry-anti-patterns.md) — Five: retry absent data, generic retry, no pattern field, structural-only validation, unlimited retries

### Batch Processing Strategies (D4 T4.5)
- [message-batches-api.md](message-batches-api.md) — Hub page: 50% savings with 24-hour window; no mid-request tool calls; decision framework; 5 anti-patterns
- [batch-vs-synchronous-api.md](batch-vs-synchronous-api.md) — Core distinction: blocking = sync, non-blocking = batch; tool call constraint as second gate
- [custom-id-tracking.md](custom-id-tracking.md) — Tracking number analogy; required on every request; enables targeted resubmission
- [partial-failure-resubmission.md](partial-failure-resubmission.md) — Resubmit only failed documents; analyze error type before resubmitting; never resubmit whole batch
- [two-phase-batch-strategy.md](two-phase-batch-strategy.md) — Phase 1: sync on 20–50 sample docs; Phase 2: batch full volume with refined prompt
- [batch-anti-patterns.md](batch-anti-patterns.md) — Five: batch for blocking, resubmit whole batch, no prompt refinement, batch for tool-calling, no custom ID

### Multi-Instance Review Architecture (D4 T4.6)
- [multi-instance-review.md](multi-instance-review.md) — Hub page: self-review bias, three-pass architecture, confidence routing; closes Domain 4
- [self-review-bias.md](self-review-bias.md) — Why same-session review fails; reasoning context retained; extended thinking is not a fix
- [multipass-review-architecture.md](multipass-review-architecture.md) — Pass 1 (parallel per-file), Pass 2 (cross-file integration), Pass 3 (confidence verification)
- [confidence-calibrated-routing.md](confidence-calibrated-routing.md) — ≥4 auto-post, 2–3 human review, ≤1 discard; prevents noise in PRs
- [multi-instance-anti-patterns.md](multi-instance-anti-patterns.md) — Five: same-session review, single pass, extended thinking substitute, auto-post all findings, skip cross-file pass

## Domain 5: Agentic Systems

### Context Management (D5 T5.1)
- [context-management.md](context-management.md) — Core problem, patterns, and exam mnemonics for D5 T5.1
- [case-facts-pattern.md](case-facts-pattern.md) — Persistent structured block for transactional data; prevents summarization loss
- [lost-in-the-middle-effect.md](lost-in-the-middle-effect.md) — LLM attention gap in long contexts; beginning and end processed reliably
- [tool-output-trimming.md](tool-output-trimming.md) — Post-tool-use hooks to reduce 40-field responses to 5 relevant fields
- [strategic-input-organization.md](strategic-input-organization.md) — Layout: system prompt → case facts → tool results → user message
- [subagent-metadata.md](subagent-metadata.md) — Required fields (source, date, URL, methodology) on subagent outputs
- [context-anti-patterns.md](context-anti-patterns.md) — Five exam-tested anti-patterns in context management

### Escalation & Ambiguity Resolution (D5 T5.2)
- [escalation-ambiguity-resolution.md](escalation-ambiguity-resolution.md) — Hub page: three triggers, exam framework, correct resolution pattern
- [escalation-triggers.md](escalation-triggers.md) — The exactly-three triggers: human request, policy gap, three failed attempts
- [sentiment-escalation-fallacy.md](sentiment-escalation-fallacy.md) — Why emotional tone is a bad escalation proxy; use case complexity instead
- [ambiguity-resolution.md](ambiguity-resolution.md) — Multiple account matches: always ask for identifiers, never guess
- [explicit-escalation-criteria.md](explicit-escalation-criteria.md) — Vague vs. explicit criteria; few-shot examples in system prompts
- [escalation-anti-patterns.md](escalation-anti-patterns.md) — Five exam-tested anti-patterns in escalation design

### Error Propagation in Multi-Agent Systems (D5 T5.3)
- [error-propagation-multiagent.md](error-propagation-multiagent.md) — Hub page: core problem, two anti-patterns, graceful degradation, exam framework
- [structured-error-reporting.md](structured-error-reporting.md) — Required fields: failure type, query, partial results, gap, alternatives
- [graceful-degradation.md](graceful-degradation.md) — Continue with partial results + annotate gaps; partial beats zero
- [coverage-annotations.md](coverage-annotations.md) — Label findings as well-supported / partial / gap in final output
- [local-vs-propagated-errors.md](local-vs-propagated-errors.md) — Retry transient errors locally; only propagate unresolvable failures
- [error-propagation-anti-patterns.md](error-propagation-anti-patterns.md) — Five exam-tested anti-patterns in multi-agent error handling

### Large Codebase Context (D5 T5.4)
- [large-codebase-context.md](large-codebase-context.md) — Hub page: context degradation, five techniques, exam mnemonics
- [context-degradation.md](context-degradation.md) — Precision loss signal: vague responses citing "typical patterns"; causes and mitigations
- [scratchpad-files.md](scratchpad-files.md) — Write key findings to file during exploration; read back when context fills
- [subagent-delegation-exploration.md](subagent-delegation-exploration.md) — Sub-agents read files and return summaries; main agent stays for decisions
- [compact-command.md](compact-command.md) — /compact compresses verbose context; use proactively before degradation
- [state-persistence-crash-recovery.md](state-persistence-crash-recovery.md) — Manifest file tracks phase, tasks, findings; enables crash recovery
- [phase-summarization.md](phase-summarization.md) — Summarize-then-inject: phase 2 gets summary of phase 1, not raw output
- [large-codebase-anti-patterns.md](large-codebase-anti-patterns.md) — Five exam-tested anti-patterns in codebase context management

### Human Review Workflows (D5 T5.5)
- [human-review-workflows.md](human-review-workflows.md) — Hub page: aggregate accuracy trap, routing thresholds, exam framework
- [aggregate-accuracy-trap.md](aggregate-accuracy-trap.md) — 97% overall can hide 65% accuracy on rare segments; never trust a single %
- [confidence-calibration.md](confidence-calibration.md) — Per-field scores; models are overconfident; calibrate against labeled validation sets
- [human-review-routing.md](human-review-routing.md) — Thresholds: ≥0.95 auto accept / 0.5–0.95 review / <0.5 flag failed; per-field routing
- [stratified-sampling.md](stratified-sampling.md) — Over-sample rare/difficult types; random sampling misses the types that fail most
- [feedback-loop-improvement.md](feedback-loop-improvement.md) — Human corrections → calibration updates + prompt improvements; "most systems miss this step"
- [human-review-anti-patterns.md](human-review-anti-patterns.md) — Five exam-tested anti-patterns in human review design

### Information Provenance & Uncertainty (D5 T5.6)
- [information-provenance.md](information-provenance.md) — Hub page: attribution loss, claim-source mappings, exam framework, core principle
- [attribution-loss.md](attribution-loss.md) — Three-level degradation: web researcher → coordinator → synthesis agent strips all sources
- [claim-source-mappings.md](claim-source-mappings.md) — Required fields: source name, URL, date, excerpt, methodology; hard output contract
- [conflicting-sources.md](conflicting-sources.md) — Annotate both values with context; never pick a winner arbitrarily
- [publication-dates-provenance.md](publication-dates-provenance.md) — Dates turn apparent contradictions into trends; always required in subagent outputs
- [confidence-structured-output.md](confidence-structured-output.md) — Well-established / contested / coverage gap tiers in the final report
- [content-type-rendering.md](content-type-rendering.md) — Natural format per content type; uniform bullet points is an exam-tested anti-pattern
- [provenance-anti-patterns.md](provenance-anti-patterns.md) — Five exam-tested anti-patterns in information provenance

### Other Topics
- [exam-sample-questions.md](exam-sample-questions.md) — STUB: Part 32 — 12 exam-style Q&A across all domains
