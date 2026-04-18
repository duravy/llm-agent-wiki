Welcome back. In part 31, we covered
information provenence and uncertainty.
Quick recap before we move on. When sub
agents summarize findings, source
attribution disappears like a journalist
writing experts agree without naming who
said what. The reader has no way to
verify anything.
Structured claim source mappings solve
this.
Every finding carries its URL, source
name, publication date, and excerpt, so
it survives every level of synthesis
without losing verifiability.
When sources conflict, you annotate both
values with their context.
You never arbitrarily pick one side and
hide the contradiction from the reader.
Publication dates turn apparent
contradictions into trends.
87% in 2024 versus 94% in 2025 shows
progress, not disagreement.
And you structure output by confidence
level, well established, contested, and
gaps, so readers know exactly how much
to trust each finding.
If this series is helping you prepare
for the exam, please give it a like and
subscribe. Every like helps more
developers find this content, and your
support keeps me making more.
Today, we're doing something different,
no new concepts,
no new patterns.
We're walking through 12 practice
questions, the exact format and
difficulty level you'll face on the
exam. Each question is grounded in a
real scenario and each answer comes with
a full explanation of why it's right and
why the others are wrong. The exam tests
whether you can pick the most effective
answer, not just a correct one.
We'll cover four scenarios: customer
support resolution, claude code
workflows, multi-agent research systems,
and continuous integration pipelines.
Let's get started.
Scenario one, a customer support
resolution agent. Question one,
production data shows the agent skips
get underscore customer entirely in 12%
of cases, calling lookup order using
only the customer's stated name. This
leads to misidentified accounts and
incorrect refunds.
What's the most effective fix?
The answer is a a programmatic
prerequisite that blocks lookup order
and process_refund
until get_c customer has returned a
verified customer ID. Prompt
instructions are probabilistic 95 to 99%
compliance.
Code enforcement is deterministic 100%.
When errors have financial consequences,
you need guarantees, not best effort
prompts.
Question two. The agent frequently calls
get_c customer when users ask about
orders. Both tools have minimal
descriptions.
What's the first step?
The fix is B. Expand each tools
description with input formats, example
queries, and boundaries explaining when
to use it versus similar alternatives.
Tool descriptions are the primary
mechanism LLMs
use for selection. When they're minimal,
models can't differentiate.
Question three. The agent achieves 55%
first contact resolution, well below the
80% target. It escalates straightforward
cases while attempting complex
situations that need policy exceptions.
What's the most effective fix? The
answer is a add explicit escalation
criteria with few shot examples showing
when to escalate versus resolve
autonomously.
The root cause is unclear decision
boundaries.
Self-reported confidence fails because
LLM confidence is poorly calibrated. The
agent is already incorrectly confident
on the hard cases and sentiment doesn't
correlate with case complexity.
The exam tests this distinction
directly. When a question describes a
calibration problem, easy cases handled
wrong, hard cases handled wrong. The
answer is almost always explicit
criteria with examples, not a new model
or metric.
Scenario two, code generation with cloud
code. Question four, you want a
custom/re command available to every
developer who clones the repo. Where do
you create it? Answer a in
the.cloud/commands/directory
within the project repository.
These commands are version controlled
and automatically available to all
developers.
The home directory is for personal
commands that aren't shared. And
claude.md is for project instructions,
not command definitions.
Question five, you're restructuring a
monolith into microervices, dozens of
files, architectural decisions needed.
What's the correct approach?
The answer is a enter plan mode to
explore the codebase, understand
dependencies, and design an approach
before making changes.
Plan mode is designed for tasks
involving large-scale changes and
multiple valid approaches.
Direct execution risks costly rework
when dependencies are discovered late.
Lock this in. The exam tests plan mode
versus direct execution repeatedly.
Question six. Your codebase has distinct
areas with different conventions. React
components use hooks. API handlers use
async/await.
Database models follow a repository
pattern. Test files are spread
throughout the codebase.
How do you ensure claude automatically
applies the correct conventions?
The answer is a rule files
andcloud/ruule
slash with YAML front matter and glob
patterns. Here's the key distinction.
Claw.md files are directorybound. They
only apply to files in that directory or
below. Rule files with glob patterns
match files anywhere in the project.
When test files are scattered across
many directories, rules win. skills
require manual invocation which
contradicts the requirement for
automatic application.
This is a common exam question. Know the
difference between directorybound claim
MD files and globbas based rule files.
It comes up repeatedly.
Scenario three a multi- aent research
system. The final report covers only
visual arts, completely missing music,
writing, and film. The coordinator
decomposed the topic into digital art,
graphic design, and photography.
What's the root cause? The root cause is
B. The coordinator's task decomposition
is too narrow. The sub agents executed
their assigned tasks correctly. The
problem is what they were assigned.
Overly narrow decomposition is a classic
multi-agent failure mode. Question
eight, the web search sub aent times
out. How should failure information flow
back to the coordinator?
Answer A. Return structured error
context, the failure type, attempted
query, partial results, and alternative
approaches.
This gives the coordinator the
information it needs for intelligent
recovery. A generic status hides
context.
Returning empty as success risks
incomplete outputs
and terminating the entire workflow is
unnecessary when recovery strategies
could succeed.
Question nine. The synthesis agent needs
to verify simple claims adding two to
three round trips and 40% latency.
85% of verifications are simple fact
checks. What's the most effective fix?
The answer is a give the synthesis agent
a scoped verify fact tool for simple
lookups while complex verifications
still go through the coordinator.
This applies the principle of least
privilege just enough capability for the
common case while preserving the
existing coordination pattern for
complex cases.
Full tool access violates separation of
concerns.
Batching creates blocking dependencies
and speculative caching can't reliably
predict synthesis needs.
The exam tests this directly when a
question describes latency from round
trips. Look for the answer that gives
the agent just enough capability for the
common case.
Scenario four. Claude code for
continuous integration.
Question 10. Your pipeline runs claude
code but hangs waiting for interactive
input. What's the fix? The fix is a add
the minus p flag. The minus p or print
flag runs claude code in non-inactive
mode. Process the prompt output to stout
and exit. The other options reference
non-existent features. There is no cl
auds
environment variable and no batch flag.
Question 11. Two workflows, a blocking
premerge check and an overnight
technical debt report. Your manager
proposes switching both to the message
batches API for 50% cost savings. Is
this a good idea?
The answer is a batch for technical debt
reports only. Keep real time for
premerge checks. The batch API has
processing times up to 24 hours with no
guaranteed slot. That's fine for
overnight jobs, but unacceptable for
blocking checks where developers are
waiting. Match each API to its
appropriate use case.
Question 12. A pull request modifies 14
files. A single pass review produces
inconsistent results. Detailed for some
files, superficial for others, obvious
bugs missed. What's the most effective
fix? The answer is a split into focused
passes. Analyze each file individually
for local issues, then run a separate
integration pass for cross-file data
flow. This addresses attention dilution,
the root cause when processing many
files at once. Larger context windows
don't solve attention quality issues and
requiring developers to split P or S
shifts burden without improving the
system.
And that's all 12 questions.
Congratulations. You've completed all 32
parts of this series.
You've covered agentic architecture,
tool design, cloud code configuration,
prompt engineering, and context
management. You've built a rail
foundation. Now go ace that exam. If
this series helped you, please share it
with others preparing alongside you. And
stay tuned. We're working on another
series with more practice questions for
each domain so you can drill deeper into
the areas that matter most to you.
You've earned this. Good luck.
