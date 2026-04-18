Welcome back. In part two, we mastered
agentic loops, the single agent pattern.
Let's do a quick recap before we scale
up. First, the five parameters of
messages.create
model and max tokens are required.
System tools and messages are optional
but critical for agentic behavior.
The four-step loop. Send a request.
Check stop undersc_reason. Execute any
tools. Append the result. Then repeat
until end underscore turn. Tools are
descriptions, not code. Claude sees only
the name and schema. Your code runs the
actual function and returns a tool
underscore result. Stop underscore
reason is the only control signal the
loop reads. In underscore turn means
exit.
tool use means execute. That's the
entire branching logic.
And three anti-atterns to avoid parsing
text to detect tool calls using a fixed
iteration cap as the exit condition and
keyword detection loops.
Today we build on all of that. Here's
what's on the agenda. Six concepts that
cover the full multi- aent coordinator
pattern.
By the end of this video, you'll be able
to design, explain, and reason about
multi-agent coordinator systems at exam
depth. Here are the six things we cover.
First, why move to multi-agent at all?
What are the constraints that make a
single agent insufficient?
Second, the hub and spoke architecture.
One coordinator, multiple sub aents,
three hard rules that define the
pattern. Third, isolated context.
This is the most tested detail. What the
coordinator sees versus what each sub
aent sees.
Fourth, the coordinator's five
responsibilities.
This is the exam's model of what a
coordinator actually does.
Fifth, the task tool. How delegation
actually works under the hood. Four
steps from call to result.
and sixth five anti-atterns.
The exam will present scenarios designed
to trap you with these. Let's make sure
you recognize everyone.
Why move from a single agent to a multi-
aent system? There are four situations
where you have no choice.
The first constraint is the fixed
context window. Long complex tasks
overflow what a single agent can hold.
You need to partition work across agents
with separate contexts.
Second, specialization.
Sub agents get focused system prompts
tailored to a narrow domain. A research
agent, a writing agent, a code agent,
each tuned for its task.
Third, parallelism.
Independent subtasks can run
simultaneously.
Multiple task calls in one response. The
SDK launches them all at once
dramatically faster.
Fourth, and the exam tests this knowing
when one agent is enough. Don't
overengineer
multi-agent only when these constraints
actually apply.
The hub and spoke architecture is the
standard pattern for multi-agent
systems. The coordinator is the hub.
Each sub aent is a spoke. Rule one, all
communication goes through the
coordinator.
Sub aents never send messages to each
other ever. Any design that bypasses
this violates the pattern. Rule two, the
coordinator owns error handling. If a
sub agent fails, the coordinator decides
whether to retry, use a fall back, or
degrade gracefully.
Rule three, the coordinator controls
information flow. It decides exactly
what context each sub aent receives. No
sub aent gets the full coordinator
history unless explicitly passed.
This is the most commonly missed detail
on the exam. Isolated context.
The coordinator sees everything. user
request, its own reasoning, all task
calls, all results.
But each sub agent starts completely
fresh. It only sees its own system
prompt and the prompt the coordinator
explicitly passed. No coordinator
history, no other sub aent results,
nothing. The exam rule, the coordinator
must explicitly include everything a sub
aent needs. Never assume inherited
context.
The coordinator has five distinct
responsibilities.
These run inside its own agentic loop
and they run in order. Responsibility
one, decompose.
Break the user request into non-over
overlapping subtasks with clear distinct
scopes for each sub aent. Responsibility
two, delegate.
Issue task tool calls one per sub agent
with a fully specified prompt that
includes everything the agent needs.
Responsibility three aggregate
collect all sub aent results and
synthesize them into a unified coherent
view. Responsibility four evaluate
check quality and completeness.
If there are gaps, the coordinator
redelegates. It doesn't just accept
whatever came back.
Responsibility five, respond.
Deliver the final answer to the user,
but only after the quality check passes.
The user never sees incomplete work.
How does delegation actually work? The
task tool has four steps from the
coordinator's call to the result coming
back.
Step one, the coordinator's response
includes a tool underscore use block
with type task, the target agent name,
and the fully specified prompt.
Step two, the SDK sees the task call and
launches a fresh claw instance using the
target agents agent definition, system
prompt, tools, model settings.
Step three, the sub agent runs its own
complete agentic loop independently,
including its own tool calls until it
reaches end underscore turn.
Step four, the result comes back into
the coordinator's message history as a
tool underscore result. Critical, the
coordinator's allowed underscore tools
must include task or the SDK will reject
the call.
Once you understand the task tool, you
need to know when to use parallel versus
sequential execution.
Parallel means the coordinator returns
multiple task calls in a single
response. The SDK launches all sub aents
simultaneously.
Use this when tasks are fully
independent.
Sequential means one task call per loop
iteration. The coordinator waits for
each result before issuing the next. Use
this when any step needs the previous
step's output. The test is simple. If
any sub aents prompt needs another sub
agents output, it must be sequential.
Two more patterns the coordinator uses.
Dynamic routing and work partitioning.
Both are about efficiency and
correctness.
Dynamic routing means the coordinator
evaluates the request first, then
decides which sub aents to invoke, not
always all of them. A simple query can
be handled directly. Overinvoking burns
tokens and adds latency.
Work partitioning means each sub agent
has a distinct non-over overlapping
scope. If two agents search the same
data, that's bad decomposition.
Good decomposition. One agent handles
web sources. Another handles internal
data. No overlap.
Two final concepts before the
anti-patterns. Iterative refinement and
structured handoffs.
Both are about quality control.
Iterative refinement. After receiving a
sub aent result, the coordinator
evaluates it. If there are gaps, it
redelegates to the same agent or a
different one. The evaluate step in the
five responsibilities can run more than
once.
Structured handoffs. Each finding from a
sub agent must be tagged with source
name, URL, and date. Raw text is
unverifiable.
Structured output is citable and useful
for the coordinator synthesis step.
Now, the anti-atterns.
The exam will give you scenarios
designed to look reasonable, but each
one has a flaw. Here are all five.
Anti-attern one, direct sub aentto sub
aent communication.
Sub aents never message each other. All
coordination goes through the
coordinator. No exceptions.
Anti-attern two, assuming inherited
context.
Sub agents start fresh. Never assume
they know the user's request or what
other sub aents found unless you
explicitly pass it. Anti-attern three,
always on full pipeline, triggering all
sub aents on every request regardless of
complexity.
Dynamic routing exists precisely to
avoid this. Anti-attern four, overly
narrow decomposition,
splitting tasks. so finely that sub
agents have overlapping scopes. Both do
the same search and produce duplicate
work.
Anti-attern five, raw text handoffs.
Passing unstructured output between
agents with no source attribution.
Always use structured handoffs with
source, URL, and date.
That covers all seven concepts. Here are
the nine things you must be able to
state from memory for the exam.
One, hub and spoke coordinator at
center. Sub aents as spokes. No direct
communication between spokes.
Two, isolated context. Sub aents start
fresh. Coordinator must explicitly pass
all needed context.
Never assume inheritance.
Three. Five. Coordinator
responsibilities. Decompose. Delegate,
aggregate, evaluate, respond.
Four, task tool spawns sub aents as
fresh clawed instances.
The coordinators allowed underscore
tools must include task.
Five, parallel execution, multiple task
calls and one response. SDK runs them
simultaneously for independent tasks.
Six, sequential execution. One task call
per loop iteration used when step n +
one depends on step n's output.
Seven, dynamic routing. Only invoke the
sub aents actually needed. Evaluate the
request first, then decide.
Eight, structured handoffs. Every
finding from a sub aent must carry
source name, URL, and date metadata.
Nine, iterative refinement. The
coordinator evaluates results and
redelegates if there are gaps before
giving the final response.
That covers task 1.2, the multi- aent
coordinator pattern. You now have the
complete mental model. Hub and spoke
architecture, isolated context, the five
responsibilities, the task tool,
parallel versus sequential, dynamic
routing, structured handoffs, and all
five anti-atterns.
In part four, we go from concept to code
with the Claude agent SDK.
We'll build a real coordinator using
agent definition, wire up context
passing, and run a working multi- aent
pipeline in Python. See you there.
