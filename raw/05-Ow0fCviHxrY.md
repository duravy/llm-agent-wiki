Welcome back. In part 4, we built multi-
aent systems with the Claude agent SDK.
Let's do a quick recap before we go
deeper.
First, the raw API is a library you call
it. The claude agent SDK is a framework.
It calls your code. That inversion
changes everything about how you
structure agents.
Second, the task tool spawns sub agents.
And here's what's easy to miss. Task
must appear in the coordinators allowed
tools list or delegation simply won't
work. Third, agent definition has
exactly five fields. Name, description,
system prompt, tools, and allowed
underscore tools. Each one shapes how
the agent behaves.
Fourth, sub aents start completely
blank. They have zero memory of the
coordinator's context.
You must pass every relevant piece of
information explicitly in the task
description.
Fifth, parallel work uses a centio. For
independent tasks,
sequential chains use a weight. When
task B depends on task A's output, they
must run in order.
Today we go deeper. Enforcement and
handoff patterns. the mechanisms that
make multi-step workflows safe and
reliable in production. Let's get into
it.
Here's a fundamental distinction. The
exam tests repeatedly.
There are two ways to make Claude follow
a rule, and only one of them actually
guarantees it. Probabilistic compliance
means relying on Claude's own judgment
to follow your instructions.
You write rules, Claude reads them and
usually follows them. The key word is
usually.
Deterministic compliance means your code
enforces the rule. It doesn't matter
what Claude decides. The check happens
in your code, not in Claude's reasoning.
It cannot be bypassed.
Here's the gap. Claude is a language
model. It can be persuaded. Given a
convincing context, it might skip a step
it normally follows. That's not a bug.
It's how LLMs work.
So, the production rule is this. Safety
critical constraints must be code
enforced.
Never rely on a prompt alone for
anything that causes real harm if
skipped ever.
Now, let's look at the most important
code pattern for enforcement.
Prerequisite gates. A gate blocks one
tool from running until another has
completed successfully.
A gate is a check that prevents tool B
from executing until tool A has
completed successfully and recorded that
in your code state. Implementation. You
write a wrapper function. Before the
downstream tool runs, your wrapper
checks the prerequisite state. If it's
not satisfied, it raises an error, not a
soft message, an actual exception.
Concrete example, payment tool can only
run after validate_card
tool has returned success. The payment
wrapper checks a flag set by the
validator. No flag, no payment, no
exceptions.
And here's the exam key point. Gates are
code enforced.
Claude cannot reason its way around
them. Even if Claude thinks skipping
validation is safe, your gate says no.
That's the whole point.
Okay. When do you use code enforcement
versus a prompt? Here's the decision
framework. One question answers most
exam scenarios.
Use code enforcement when safety
matters, compliance is required,
financial transactions are involved, or
the action is irreversible.
These are non-negotiable code only. Use
prompt guidance when it's a style
preference, tone, non-critical workflow
ordering, or anything a mistake can
correct. Prompts are easier to update
and don't add overhead. The decision
rule comes down to one question. Can a
mistake here be undone? Send an email,
charge a card, delete a record, those
cannot be undone. Code enforcement
required.
Lock this exam framing in. Prompt
guidance is best effort. Code
enforcement is guaranteed.
That single sentence answers the
majority of enforcement questions you'll
see.
Sometimes an agent reaches a decision
point where it needs human judgment.
That's not a failure, it's a feature.
But how you hand off matters enormously.
When should an agent escalate?
Three situations. Confidence drops below
a threshold, instructions are genuinely
ambiguous, or an irreversible action is
about to happen and the agent isn't
certain. What to include in the handoff?
A summary of what was attempted, the
current task state, and the specific
decision the human needs to make, not a
dump of logs, a focus summary.
Format it as structured JSON with four
fields. Action requested, what you need
the human to do. Context, what happened,
options, the available choices, and
recommended underscore action, what the
agent thinks is best.
And the rule you'll be tested on, never
escalate with a vague message like, "I
need help." Always include actionable
decision context.
A human can't act on vague. They need
clear options.
Here's a pattern that makes complex
tasks tractable. Multiconcern
decomposition.
When a single request involves multiple
independent questions, the coordinator
splits them into parallel
investigations.
Take the question, is this transaction
safe? A coordinator can split that into
three separate concern agents running
simultaneously. One checks for fraud
signals. One checks regulatory
compliance. One calculates a risk score.
Each concern agent is isolated. a
failure or timeout in one doesn't block
the others. The coordinator collects all
three results and synthesizes a final
answer. This is faster than running them
sequentially and more robust than a
single agent trying to answer all three
at once. The key design insight. Each
concern gets its own agent with
specialized context and tools, not a
general purpose agent asked to do
everything.
Let's talk about hooks. One of the most
powerful enforcement mechanisms in the
agent SDK. Hooks fire automatically
around every tool call in the agentic
loop. Hooks are regular Python functions
that the SDK calls automatically.
You register them once and they run on
every tool call. You never have to
remember to call them manually.
Pre-tool use hooks fire before
execution.
You can inspect the tool name and input,
block the call by raising an exception,
modify the input before it runs, or just
log it. Post tool use hooks fire after
execution.
You can validate the output matches a
schema, transform or sanitize the result
before Claude sees it, or record the
outcome for auditing.
Common use cases audit logging. Every
tool call recorded automatically.
Rate limiting block if a tool has been
called too many times. Output
sanitization. Strip sensitive data
before results reach clawed. Schema
validation. Reject malformed inputs
before they cause downstream failures.
Here's what hooks look like in code. The
pre-tool hook receives the tool name and
input dict. If the tool is database
write, we call validate schema which
raises if invalid blocking the call.
Then we audit log the attempt returning
none signals allow for the post tool
hook. We log completion and call
sanitize output before returning the
result to claude. Three return behaviors
to memorize for the exam. Return none to
allow the call through unchanged.
raise an exception to block it, the tool
will not execute. Return a modified dict
override the input before execution.
These three behaviors are exactly what
the exam tests on hooks. Know the
difference between returning none,
raising, and returning modified input.
Five anti-atterns that task 1.4 actively
tests on the exam. Know all five. What's
wrong and what the correct approach is?
Anti-attern one prompt only safety.
Writing don't run payment without
validation in a system prompt and
trusting Claude to follow it. The fix a
code gate that physically blocks the
payment tool until validation passes.
Prompts are best effort. Gates are
guaranteed.
Anti-attern two skipping prerequisites.
Claude might call tools out of order
because it thinks it's being efficient.
The fix: enforce order with prerequisite
gate checks and code. Never assume
Claude will respect ordering from a
prompt alone. Anti-attern three vague
escalation
passing. I got confused or I'm not sure
to the human reviewer. The fix
structured handoff with action requested
context and options.
Humans need to act. Give them what they
need to decide.
Anti-attern four, ignoring hook return
values.
Registering hooks but not checking
whether a pre-tool hook signal a block.
The fix. Always propagate hook
decisions. If a hook blocks, the tool
must not run.
Anti-attern five. Running dependent
concerns in parallel. Launching parallel
agents when their work depends on shared
state. The fix only parallelize truly
independent investigations.
Dependent steps must stay sequential.
Seven things to lock in before you sit
the exam. These are the core concepts
from today. One, probabilistic means
claude's judgment prompts.
Deterministic means your code
enforcement.
Know which is which and when each
applies.
Two, safety critical constraints must be
code enforced. Never prompt only. The
exam will give you a scenario where
something could go wrong. The answer
always involves code. Three.
Prerequisite gates block downstream
tools until prerequisites succeed.
Enforced in code, not in prompts. raise
an exception. Don't return a soft error
message.
Four, the irreversibility test. If a
mistake cannot be undone, code
enforcement is required.
This single rule answers half the
enforcement questions on the exam.
Five structured escalation handoffs
always include actionable context.
Action underscore requested context
options recommended underscore action.
Vague handoffs fail the exam. Six,
pre-tool use can block, modify, or
allow. Posttool use can validate,
transform, or record. Know all three
behaviors for each hook type. They're
tested directly.
Seven, multicarnern decomposition.
Parallelize independent concerns. Never
parallelize dependent ones. If one
concern needs the output of another,
they must run in sequence.
That wraps up part five. You can now
explain probabilistic versus
deterministic compliance, implement
prerequisite gates in code, design
structured escalation handoffs, use
hooks to intercept and control every
tool call, and decompose complex
requests into parallel concern
investigations.
In part six, we go deep on hooks, how to
intercept every tool call, normalize
data formats across agents, and
implement the audit trail patterns that
production systems require.
