Welcome back.
In part one, we mapped the entire exam.
Four domains weighted by importance.
Let's do a quick recap before we dive
in.
The single most important number from
part one, domain one, is worth 27% of
the exam.
That's why we start here.
Claude comes in three tiers. Opus for
deep reasoning, Sonnet for balance,
Haiku for speed.
The exam tests when to use each one.
The SDK wraps the API.
One Anthropic client handles
authentication, retries, and response
parsing automatically.
Questions are scenario-based. The exam
tests your applied judgment, not
memorized definitions.
Study order matters. Domain one first.
Everything else builds on the agentic
loop concepts we cover today.
And here's what's on today's agenda. Six
things that completely cover task 1.1.
Let's go.
By the end of this video, you'll be able
to design, explain, and reason about
agentic loops at exam depth.
Here's what we cover. Six concepts.
First, the communication stack. What
actually happens between your code and
Claude's servers across four layers.
Second, messages.create,
the one function you call in every
agentic loop.
Five parameters, two of them required.
Third, the most tested distinction in
domain one. Tools are descriptions, not
code.
We'll unpack exactly what that means.
Fourth, the four-step loop itself. The
repeating pattern that powers every
Claude-driven product.
Fifth, model-driven agency versus
scripted automation. A distinction the
exam tests consistently.
And sixth, three anti-patterns. Patterns
the exam expects you to recognize and
reject.
Let's start with the stack.
Before we write any loops, you need to
understand what actually happens when
your code talks to Claude.
There are four layers.
At the top is your code, Python or
JavaScript.
You write logic and call a single
function.
That's all you interact with directly.
Below that is the Anthropic SDK, a
library you install that wraps all the
HTTP complexity into simple, readable
method calls.
The SDK talks to the Claude API, which
handles authentication with your API key
and routes the request to the right
model.
At the bottom, Claude's servers run the
language model and return a structured
response object, not plain text, but a
structured object you can reason about.
Think of it like a restaurant. You're
the customer, the SDK is the waiter, the
API is the kitchen ticket system, and
Claude's servers are the kitchen.
One line of code creates a client that
handles all four layers automatically.
You never write raw HTTP.
There's one function you'll call in
every agentic loop.
Let me walk through its five parameters.
First, model, required.
This specifies which Claude processes
your request, Opus, Sonnet, or Haiku.
Second, max_tokens,
also required.
A hard cap on how long Claude's response
can be.
Set it too low and responses get cut
off, which creates a distinct stop
reason we'll cover shortly.
Third, system, optional but powerful.
Persistent instructions that shape every
response.
Think of it as a permanent job
description for Claude in this session.
Fourth, tools, optional but essential
for agentic behavior.
This is where you declare what actions
Claude is allowed to request.
More on this in the next slide.
Fifth, messages, the full conversation
history.
Every user and assistant message
exchanged so far.
This is what makes the loop work.
The agentic power lives in tools and
messages together. Tools tell Claude
what it can do. Messages carry the
growing record of what it's already
done.
This is one of the most tested
distinctions in domain one. Tools are
not code, they're descriptions.
When you give Claude a tool, you give it
three things, a name, a description, and
an input schema.
That's all Claude ever sees.
Claude never sees your actual
implementation.
When Claude decides to use a tool, it
responds with a structured block saying,
"Call this tool by name with these
inputs."
Your code receives that instruction and
runs the actual implementation, whether
that's calling a weather API, querying a
database, or reading a file.
The result feeds back to Claude as a
tool result message.
The pattern is clean. Claude decides
what to call.
Your code decides how to execute it.
For the exam, any question about giving
Claude the ability to perform an action,
the answer always involves a tool
description plus a separate function
that does the work.
Every call to messages.create returns a
structured response object.
Two fields matter in every loop
iteration.
First is stop_reason.
Why Claude stopped generating.
End_turn means Claude is done.
Tool_use means Claude wants your code to
act first.
Max_tokens means the response was cut
off before Claude finished.
Second is the content array, a list of
blocks.
A text block contains Claude's narration
or final answer.
A tool_use block contains a tool Claude
wants to call.
A single response can contain both.
Each tool_use block has three fields. An
ID that uniquely identifies this call,
the name of the tool Claude wants to
use, and the input object Claude chose.
That ID is critical. When your code
sends the result back, you must
reference the exact same ID.
Tool results always go back as user role
messages wrapped in a tool_result block.
Now we put it all together.
The agentic loop is four steps repeated
until Claude signals it's done.
Step one, call messages.create with the
complete messages array and your tool
definitions.
Claude receives the full conversation
history, every prior message, every tool
call, every result.
Step two, check stop_reason.
This is the only decision point.
If it's end_turn, exit and return the
answer.
If it's tool_use, continue to step
three.
Do not try to infer completion from
Claude's text.
Step three, execute every tool Claude
requested.
Scan the content array, find all
tool_use blocks, run each
implementation, collect the results with
their IDs.
Claude may request more than one tool.
Execute all of them before continuing.
Step four, append Claude's full response
as an assistant message, then append all
tool results as a user message, and loop
back to step one.
Both additions are required.
Claude needs to see what it said and
what your code returned before it can
decide the next step.
Let's make this concrete with a real
walk-through.
The task, get the weather in both Tokyo
and Paris.
Iteration one. The messages array has
one item, the user's question.
Claude responds, requests get_weather
for Tokyo.
Stop reason is tool_use, so we continue.
Your code calls the weather
implementation and gets a result.
Iteration two. The messages array now
has three items.
Claude reads all of it, sees it has
Tokyo's weather, and now requests
get_weather for Paris.
Same pattern, execute, collect result,
append.
Iteration three. Messages has five
items.
Claude reads the full history, sees it
has both cities, and responds with a
text block summarizing both answers.
Stop reason is end_turn.
Loop exits.
The key insight. You never explicitly
told Claude to check Paris next.
Claude figured that out itself by
reading the growing conversation
history, knowing what it already fetched
and what was still missing.
That's model-driven agency in action.
Let's contrast two ways to build a
multi-step workflow.
In a preconfigured approach, your code
hardcodes the sequence. Step one is
always search, step two is always
calendar check, step three is always
payment.
The order is fixed regardless of what
the user needs.
If the task only needs two steps, your
code still runs all three.
You, the developer, are the planner.
Claude just fills in the blanks.
This is automation, not agency.
In the model-driven approach, Claude
reads the conversation context at each
iteration and decides what tool to call
next based on what it knows so far.
If the search returns nothing, Claude
might try a different query.
If the user's request only needs one
step, Claude calls one tool and signals
end_turn.
Claude is the planner.
Your code is only the executor.
For the exam, when asked how a Claude
agent should decide which API to call,
the correct answer is that Claude reads
the tool descriptions and conversation
context and decides.
Not that your code checks a condition
and routes.
Let's go deep on stop_reason
because it's the most important
structured field in every response and
the only reliable way to control the
loop.
Tool_use means Claude is not done.
It's identified a tool it needs to call
to continue.
Your response, find all tool_use blocks
in the content array, execute every one
of them, and loop back.
End_turn means Claude is done.
It has all the information it needs and
has produced a final answer.
Your response, break out of the loop and
return the text to the user.
Max_tokens means the response was cut
off before Claude finished because it
hit the token limit you set.
The answer or tool call may be
incomplete.
Handle this carefully.
And a critical warning, never parse
Claude's text output to decide whether
to continue.
Checking for words like done or finished
is fragile and unreliable.
Stop_reason is structured,
deterministic, and always present.
Use it.
The exam actively tests whether you can
recognize these three anti-patterns and
explain what's wrong with each one.
Anti-pattern one, text parsing for loop
control.
This is when your code checks Claude's
output text for words like done or
finished to decide if the task is
complete.
The problem, Claude uses natural
language, which changes.
Claude might say it's done with step one
while still having three steps left.
Stop_reason is always there, always
accurate, never ambiguous.
Use it.
Anti-pattern two, fixed iteration cap as
primary loop control.
Using for i in range of five as the main
mechanism.
A cap of five cuts off a six-step task
at the worst possible moment.
A safety counter is fine and you should
have one as a fallback.
But the primary exit condition must
always be stop_reason equals end_turn.
Anti-pattern three, keyword detection
for routing.
Checking whether Claude's text contains
certain words to decide which function
to call next.
This is fragile in two ways. Claude's
wording varies and Claude might mention
a tool in context without actually
requesting it.
Read the structured tool_use blocks in
the content array. They tell you exactly
which tool Claude requested with which
inputs reliably every time.
Let me close with the seven things you
must be able to recall and apply on the
exam.
One, the loop pattern, send, check
stop_reason,
execute tools, append, repeat until
end_turn.
Two, tools are descriptions.
Name, description, schema.
Claude never sees your implementation.
Three, stop_reason is the only control
signal.
Tool use means keep going.
End turn means stop.
Never parse Claude's text.
Four, the messages array every
iteration.
Claude sees the full history at each
step. That's how it knows what it
already did and what it still needs to
do.
Five, model-driven means Claude decides
what to do next.
Your code is only the executor of
Claude's decisions, not the planner of
the workflow.
Six, tool results go back to Claude as
user role messages wrapped in a
tool_result block with the matching
tool_use ID from Claude's request.
Seven, use while true with a safety
counter as a fallback.
Never use a fixed range as the primary
exit condition.
That covers task 1.1 completely.
You can now explain the full agentic
loop, the communication stack, the five
parameters, the tool description
pattern, the four-step cycle, and how to
distinguish model-driven agency from
scripted automation.
In part three, we scale this up.
Instead of one agent in a loop, we build
multi-agent systems, a coordinator that
spawns specialized sub-agents and
synthesizes their results.
You'll learn the hub-and-spoke
architecture, the task tool for
delegation, and how to design agent
boundaries that keep systems
maintainable.
See you in the next video.
