Welcome to the Claude Certified
Architect Foundations exam overview.
This certification is Anthropic's
official credential for architects and
developers who build production-grade AI
applications using Claude.
To earn it, you'll need to demonstrate
working knowledge across four technology
pillars: Claude Code, the Agent SDK, the
Claude API, and the Model Context
Protocol.
The exam is multiple-choice, with a
passing score of 720 out of 1,000.
This video walks you through exactly
what to expect and how to prepare.
So, what does this certification
actually validate?
It tests your ability to work across
four interconnected technologies.
First, Claude Code, Anthropic's
command-line interface for code
generation, refactoring, and debugging,
configured through claude.md files,
custom skills, and MCP integrations.
Second, the Agent SDK, a framework for
building multi-agent applications with
agentic loops, sub-agent orchestration,
tool calling, and life-cycle hooks.
Third, the Claude API itself, the HTTP
API for programmatic access, including
messages.create,
tool use, structured JSON output, and
batch processing.
Fourth, MCP, the Model Context Protocol,
which is the standard for connecting
Claude to databases, APIs, and services
via tool and resource interfaces.
Critically, this exam is not
theoretical.
Every question is grounded in realistic
production scenarios: customer support
systems, CI/CD
pipelines, research pipelines, and data
extraction workflows.
Who should take this exam?
The ideal candidate is a solution
architect with at least 6 months of
hands-on experience across Claude APIs,
the Agent SDK, Claude Code, and MCP.
You should be comfortable building
agentic applications with multi-agent
orchestration, configuring Claude Code
for team workflows using claude.md
hierarchies and custom skills, designing
MCP tool and resource interfaces for
back-end systems, and engineering
prompts for reliable structured output
using JSON schemas and few-shot
examples.
You should also understand how to manage
context windows across long documents
and multi-agent hand-offs, integrate
Claude into CI/CD
pipelines for automated review and test
generation, and make sound escalation
and reliability decisions in production.
The exam is designed for practitioners,
people doing this work daily, not for
people who have only read the
documentation.
Let's talk about the exam format.
All questions are multiple-choice,
each with one correct answer and three
plausible distractors.
The distractors are specifically
designed to catch incomplete knowledge.
They're written to be convincing to
someone who understands the concept but
hasn't internalized the trade-offs.
The passing score is 720 on a scale of
100 to 1,000. This is a scaled score,
not a raw percentage.
The exam presents four scenarios,
randomly selected from a pool of six.
Each scenario drives a set of questions,
so you must be prepared for any
combination.
There is no penalty for incorrect
answers, which means you should always
provide an answer. Leaving something
blank counts as incorrect.
Use this to your advantage.
Now, let's look at where to focus your
study time.
The exam is divided into five domains,
and they are not equally weighted.
Domain one, agentic architecture and
orchestration, carries the most weight
at 27%.
This covers agentic loops, multi-agent
patterns, sub-agent delegation, hooks,
and task decomposition.
Domain three, Claude Code configuration
and workflows, is next at 20%, covering
claude.md configuration, custom skills,
plan mode, and CI/CD integration.
Domain four, prompt engineering and
structured output, also at 20%, covering
few-shot prompting, tool_use patterns,
JSON schema design, batch API usage, and
validation retry loops.
Domain two, tool design and MCP
integration, is 18%, covering MCP server
design, tool descriptions, tool_choice,
and built-in tools.
Finally, domain five, context management
and reliability, is 15%, covering
escalation decisions, human review
workflows, information provenance, and
error propagation.
The key takeaway, spend the majority of
your preparation on domain one.
Nearly three in 10 questions will test
your agentic architecture knowledge.
The exam uses a scenario-based format.
There are six total scenarios in the
question bank, and four will be randomly
selected for your exam.
Scenario one is a customer support
resolution agent, testing agentic
architecture, tool design, and
reliability.
Scenario two is code generation with
Claude Code, testing Claude Code
configuration and context management.
Scenario three is a multi-agent research
system, again drawing on agentic
architecture, tool design, and
reliability.
Scenario four covers developer
productivity tools, combining tool
design, Claude Code configuration, and
agentic architecture.
Scenario five is Claude Code for CI/CD,
which focuses on Claude Code
configuration and prompt engineering.
Scenario six is structured data
extraction, testing prompt engineering
and context management.
Because you don't know in advance which
four scenarios will appear, you need
solid coverage across all six, but
scenarios one, three, and four are
particularly high-density since they
touch three domains each.
Understanding what is and isn't tested
is just as important as knowing what to
study.
Here's what's in scope.
You need to understand agentic loop
implementation, including stop_reason
handling, tool result processing, and
loop termination conditions.
Multi-agent coordinator and sub-agent
patterns with proper context passing are
tested.
Claude.md hierarchy, the @import syntax,
and path-specific rules using glob
patterns are in scope.
MCP tool and resource design, server
configuration, and structured error
responses are tested.
Structured output via tool_use,
including schema design, tool_choice
configuration, and validation loops, is
fair game.
Finally, escalation decisions, human
review workflow triggers, and
information provenance are tested.
What's not tested? Fine-tuning or custom
model training, API billing or
authentication protocols, deploying or
hosting MCP server infrastructure,
constitutional AI or RLAIF
methodologies, embedding models or
vector database implementation, and
computer use, vision analysis, or
streaming API implementation details.
Focus your energy on the in-scope areas.
There are no trick questions about
topics outside these boundaries.
Here's an eight-step preparation plan to
maximize your chances of passing.
Step one, build an agent with the SDK.
Implement a complete agentic loop with
tool calling, error handling, sub-agent
delegation, and session management.
Hands-on experience is irreplaceable.
Step two, configure Claude Code.
Practice the full claude.md hierarchy,
write path-specific rules in
.claude/rules,
and create custom skills with front
matter options.
Step three, design and test MCP tools.
Write tool descriptions that clearly
differentiate between similar tools, and
implement structured error responses.
Step four, build an extraction pipeline.
Use tool_use with JSON schemas,
implement validation retry loops, and
practice with the message batches API.
Step five, practice prompt engineering,
particularly few-shot examples for
ambiguous scenarios, explicit review
criteria, and multi-pass architectures.
Step six, study context management.
Learn to extract structured facts from
verbose outputs, use scratchpad files
effectively, and understand sub-agent
delegation patterns.
Step seven, review escalation patterns.
Know precisely when a policy gap, a
customer request, or an inability to
make progress should trigger human
review versus autonomous resolution.
And step eight, complete the practice
exam under realistic conditions using
the same scenario format with answer
explanations.
Following this plan systematically will
put you well above the 720 threshold.
Good luck.
