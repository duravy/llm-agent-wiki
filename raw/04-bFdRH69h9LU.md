Welcome back. In part three, we built
our first multi-agent coordinator.
Let's lock in those key patterns before
we go deeper. Hub and spoke
architecture. The coordinator sits at
the center and specialist sub aents live
at the spokes. Each one owns a single
skill. Isolated context. Each sub aent
starts completely blank. It has zero
memory of the coordinator's
conversation.
You must pass everything it needs. The
coordinator has exactly five jobs.
Decompose, delegate, aggregate,
evaluate, and respond. That order
matters and the exam tests it. Parallel
means a centinio.
Sequential means an await chain. You
choose based on data dependencies, not
preference.
Dynamic routing beats a full pipeline
every time.
Only invoke agents that are actually
needed. Don't spin up everyone on every
request.
Today we go hands-on with the SDK. How
sub agents are actually configured and
spawned in code. Four topics. The task
tool, agent definition, context passing,
and execution patterns.
When working with sub aents, you have
two levels to understand. the RAW API
and the Claude agent SDK.
Let's see how they relate. The RAW API
is direct HTTP calls to the slash
messages endpoint. You're talking to
Claude, but you manage every loop, every
retry, every context window yourself.
The Claude agent SDK is a Python
framework built on top of that raw API.
It gives you structured building blocks,
agents, tools, sessions instead of raw
messages.
Think of the SDK as a framework. It
enforces structure for agent loops, tool
handling, and sub agent spawning. You
don't reinvent the wheel.
Here's the key distinction the exam
loves. An API is a library. You call it
whenever you want. A framework calls
your code. It's in control of the loop.
The SDK is a framework.
So, how do sub agents actually get
created in the SDK? The answer is one
special tool called task. Let's break it
down. Task is a special built-in tool in
the claude agent SDK.
When the coordinator calls it, the SDK
creates a brand new agent instance on
the spot. To use task, the coordinators
allowed underscore tools list must
include the string task.
This is the single most tested gotcha in
the entire sub aent section. Don't
forget it. Each task invocation creates
an isolated sub aent with its own fresh
context.
It sees nothing from the coordinator
except what you explicitly pass in. The
task description you write becomes the
sub agent's very first user message.
That's its only starting point. So make
it count.
When the sub agent finishes, its result
flows back to the coordinator as a tool
underscore result in the messages array.
Exactly like any other tool response.
Before you can spawn a sub aent, you
define it with agent definition. It has
exactly five fields and the exam tests.
First, name. This is the identifier for
the agent. Something like research agent
or summarizer.
Keep it descriptive.
Second, description. This tells the
coordinator what the agent does. The
coordinator reads this when deciding
which agent to route a task to. Third,
system prompt. These are the
specialization instructions. The agents
role, personality, and behavioral
constraints.
This is what makes it a specialist.
Fourth, tools. The list of tool
definitions the agent can use. These are
JSON schemas describing available
functions just like in the RAW API.
Fifth, allowed underscore tools, which
tools the agent is actually permitted to
call at runtime. This must be a subset
of tools. This is where you enforce the
minimum tools principle.
Here's a principle straight from the
exam. Minimum tools per agent. On the
left, the wrong approach. Every agent
gets every available tool. On the right,
the right approach. Each agent gets only
the specific tools it needs for its job.
Why does this matter? Security. Fewer
tools means a smaller attack surface.
Reliability. The agent can't
accidentally call the wrong tool. And
cost and use tool descriptions still
consume tokens in every request.
Design every agent with the minimum set
it actually needs. Nothing more.
Context passing is the coordinator's
most critical job and the most common
source of bugs. Here's why. Every sub
agent starts blank. It has no memory of
the coordinator's conversation, no prior
context, no shared state. Zero. So the
coordinator must construct a task
description that includes everything the
sub aent needs to succeed.
This is an active construction job, not
a copy paste. What to pass, relevant
facts, constraints. The sub agent must
respect the exact output format you need
and results from prior sub aents if this
task depends on them.
What not to pass the entire conversation
history. That's lazy and expensive. Be
selective.
Extract only the signal that's relevant
to this specific task.
Here's a concrete example of structured
metadata in a task prompt.
Notice the pattern. A clear goal at the
top, a structured JSON block with
source, date, and confidence score, the
actual data, and an explicit output
format at the bottom. Three things make
this work. First, include the source.
The sub agent needs to know where data
came from to assess trust.
Second, include a format spec. Don't let
the sub aent guess what output you need.
Third, include constraints. Tell it what
it cannot do, not just what it should
do. This pattern is testable. Learn it.
The SDK supports two execution patterns
for sub aents, sequential and parallel.
Choosing correctly is an exam question.
Sequential, you await task one, get the
result, then await task two using that
result.
Use this when task two needs task one's
output to proceed.
Parallel Assio.gather runs multiple
tasks at the same time. Use this when
the tasks are independent. They don't
need each other's output to start. The
SDK determines the mode purely by how
you await. Sequential awaits wait one at
a time. A gather await runs them all at
once.
You control execution mode through your
own code.
Exam rule. If tasks share no data
dependencies, parallel is always the
right answer. Choosing sequential when
tasks are independent is a correctness
error, not just a performance issue.
There's a third execution pattern that
the exam covers. Fork sessions. Here's
the idea.
A fork spawns multiple sub aents from
the same baseline context, but each one
explores a different direction.
Same starting point, diverging paths.
Classic use cases. Brainstorming
multiple approaches to a problem,
running AIB tests on different
strategies, or investigating multiple
hypotheses in parallel.
The coordinator collects the results
from all forks and synthesizes them into
a final decision. Fork is essentially
parallel execution with a shared origin
and a deliberate divergence.
How you write task prompts determines
whether sub aents succeed or fail. The
exam tests a specific philosophy here.
Goal oriented design. Always specify the
goal. what you want the sub agent to
produce and always specify the output
format, JSON, bullet list, structured
report, whatever you need. Always
specify constraints, what the sub agent
must not do or access.
Negative constraints are just as
important as positive goals. Don't
specify exact tool call sequences.
You tell Claude what you want, not how
to get there.
Claude decides the execution path.
That's the whole point of using an
agent.
Don't over constrain.
Micromanaging an agent's execution
defeats the purpose of using one. Give
it room to reason. If you need to
specify every step, use a script, not an
agent.
Let's run through the six anti-atterns
the exam specifically tests.
Each one is something developers do
wrong in real code. Anti-attern one,
forgetting task in the coordinators
allowed underscore tools. The fix is
simple, always include it. But
forgetting it means the coordinator can
never spawn sub agents at all.
Anti-attern two, giving all agents all
tools. The right move is minimum tools
per agent. Every extra tool is a
security risk and a token cost.
Anti-attern three, dumping the entire
conversation history into the task
prompt. Extract and pass only what the
sub agent actually needs. Precision over
volume. Anti-attern four procedural
prompts that specify every step. Write
goal oriented prompts instead. Let
Claude choose the execution path.
Anti-attern five, unstructured data
handoffs. Passing raw text blobs between
agents with no metadata.
Always include source, date, and
confidence so downstream agents can
reason about trust.
Anti-attern six, treating parallel
versus sequential as a style choice.
It's a correctness decision based on
actual data dependencies.
Get this wrong and you break task
sequencing.
Let's lock in the eight things you
absolutely need to know from part four.
One, task tool creates isolated sub
aents and task must be in the
coordinators allowed underscore tools.
Two, agent definition has five fields.
name, description, system underscore
prompt, tools, and allowed underscore
tools. Three, minimum tools principle.
Each agent gets only what it needs.
Four, sub aents start blank. You must
pass all needed context in the task
description.
Five, sequential equals a weight chain
for dependent tasks. Parallel equals
essentio. rather for independent ones.
Six, fork equals multiple sub aents from
the same baseline exploring different
directions.
Seven, specify goals and output format.
Don't specify tool call sequences.
Eight, always include structured
metadata, source, date, and confidence
in all cross agent handoffs.
In part five, we add the safety layer,
prerequisite gates, deterministic
enforcement hooks, and the structured
handoff patterns that make Agentic
Systems production safe.
