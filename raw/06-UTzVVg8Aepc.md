Welcome back to zero toclaude certified
architect.
Quick recap of part five. We covered the
safety layer of agenic systems.
Probabilistic versus deterministic
compliance. Prompts are suggestions but
code is enforcement.
Prerequisite gates. These block
downstream tools until earlier steps
finish creating safe execution order.
Structured handoff protocols. Clean
escalation to humans with full context
when claude reaches its limit.
Multiconcern decomposition breaking
complex requests into parallel
investigations for speed and clarity.
And hooks, the pre-tool use and postto
use checkpoints built directly into the
agent SDK.
Today we go deeper into hooks. two
critical jobs. Normalizing messy data
from multiple tools and enforcing
business policies claude must never
violate. Let's get into it.
This is part six, task 1.5. Apply agent
SDK hooks for tool call interception and
data normalization.
We are inside domain one agentic
architecture and orchestration which
carries 27% of the exam. Here is the
full arc. Part two gave you the single
agent loop. Part three added multi-agent
coordination.
Part four wired it in SDK code. Part
five added enforcement patterns. And now
part six makes hooks practical.
So, what problem do hooks actually
solve? When your agent talks to multiple
back-end systems, the data comes back in
wildly different formats.
The order system returns a Unix
timestamp, just a big number
representing seconds since 1970.
The customer system returns an ISO date
string already in standard format.
The shipping API uses US date format,
month, day, year, a completely different
convention from the others. The legacy
database returns a numeric status code,
just a number like 200 with no label.
The modern API returns a string label
like active, readable, but inconsistent
with everything else.
Five tools, five different ways to say
the same thing. If Claude has to juggle
all these formats, it sometimes gets
confused, especially with Unix
timestamps.
That's the gap hooks fill.
Normalization means converting all those
different formats into one consistent
format before Claude ever sees them. On
the left, the raw data, a Unix
timestamp, an ISO string, and a US
format date. All different. On the
right, after normalization,
all three are clean ISO 8601 strings.
Claude does not have to guess. Here is a
good way to remember this. Imagine
reports from US, UK, and Japan offices,
all using different units. Before
presenting to your boss, you convert
everything to one system. That is data
normalization.
You might ask why not just put always
convert timestamps to ISO format in the
system prompt. Claude will do it most of
the time. But remember part five prompt
instructions are probabilistic.
Claude follows them 95 to 99% of the
time. Sounds great.
until you realize that 1% of 10,000
requests is 100 failures. For data that
feeds into downstream systems or gets
displayed to users, you want
deterministic normalization
guaranteed by code, not by hope. That is
what hooks give you.
Let us see how a post tool use hook
works step by step.
First, Claude calls a tool, say, "Get
underscore order." The agent needs order
data to answer the user's question.
Second, the tool executes and returns
the raw result. A Unix time stamp and a
numeric status code. Third, your post
tool use hook intercepts the result
before Claude sees it. It converts the
Unix timestamp to ISO 8601 and maps
status 200 to the string active.
Fourth, Claude receives the clean
normalized result. It never sees the
messy original data. This hook runs
automatically
every single time. Zero exceptions.
Here is the Python code. The function
takes the tool name and the raw result.
First create a copy. Never mutate the
original data. Then loop through known
timestamp fields like created underscore
at and updated underscore at the value
is a number. Convert it to ISO861
using Python's daytime module. Then
check the status field. If it is a
numeric code, map it through a lookup
table. 200 becomes active. 4004 becomes
not underscore found. The hook is a pure
function. Data goes in. Clean data comes
out. No side effects. No network calls.
Fast and reliable.
Now the other direction. Pre-tool use
hooks intercept calls before they
execute.
Real scenario. Your company says refunds
over $500 require a human manager.
Claude wants to call process_refund
with $750.
The pre-tool use hook intercepts.
It checks is the amount over 500. Yes.
The hook engages immediately.
So it blocks the call entirely. The tool
never runs. Instead, the hook redirects
to escalate
manager with a clear explanation.
Claude gets this message and tells the
customer their request has been
escalated.
The refund never processes.
This is not a suggestion. It is a
physical block in code.
The code is straightforward.
The function receives the tool name, the
inputs, and context.
It checks is this process underscore
refund. If the amount exceeds 500,
return a blocked response.
The response includes three things. A
redirect to escalate
manager, a clear message explaining why,
and the full escalation details
including customer ID and amount. For
every other tool, just return blocked
equals false. Let it through the
critical insight for the exam. Claude
cannot override this. The code decides,
not the model.
Let us watch it play out. A customer
asks for a $750 refund.
Step one, Claude decides to call
process_refund
with $750.
Step two, the pre-tool use hook
intercepts.
It checks the amount 750 is greater than
500.
Step three, the hook blocks the call.
The tool never executes.
It redirects to escalate
manager.
Step four, Claude receives the block
message. It now knows exactly what
happened and why.
Step five, Claude tells the customer, "I
have escalated your refund to a manager
who can authorize this amount. They will
follow up shortly."
Clean, safe, deterministic.
The customer gets a helpful response.
The policy is enforced.
No exceptions.
This comparison table is a core exam
concept. Let's walk through each
dimension
how it works. Hooks run code on every
tool call. Prompts are text claude reads
in the system prompt. Failure rate.
Hooks have 0% failure. Code always runs.
Prompts have 1 to 5% failure. Claude may
skip them. When to use hooks for hard
rules that must be followed. Prompts for
soft guidelines that should be followed.
Can Claude override hooks? No. The code
blocks it. Prompts. Yes. Claude may
simply ignore them. Example, hooks block
refunds over $500.
That is non-negotiable.
Prompts say be polite and empathetic.
That is a preference.
Performance hooks add execution time to
every tool call since code runs each
time. Prompts have no extra cost.
They're just text already in context.
The exam takeaway. Whenever a question
says guarantee or ensure compliance, the
answer is hooks, not prompts.
So when do you use which? Here is the
decision framework.
Ask yourself, if this rule fails 1% of
the time, does it matter? Hard rules go
in hooks, financial limits, data format
requirements,
compliance policies,
access controls,
if failure matters, use a hook, soft
preferences, go in prompts, tone,
politeness,
formatting suggestions,
response length. If failure is annoying
but not dangerous, use a prompt. The
exam tests this distinction heavily.
Overengineering soft preferences into
hooks waste performance.
Undergineering hard rules into prompts
creates compliance gaps.
Let us put it all together. A customer
asks, "When was my order shipped and
what is the status?" The agent calls
three tools from different backends.
Get underscore order returns a Unix
timestamp and a numeric status. Get
underscore shipment returns a US format
date. Get underscore tracking returns a
daytime string and an uppercase state.
Without the hook, claude juggles three
date formats and inconsistent statuses.
With the post tool, use hook
intercepting every result that changes.
Every result comes back normalized.
ISO dates lowercase string statuses
consistent across all three tools.
Claude confidently says, "Your order was
created March 17th, shipped that same
day via UPS, and last scanned in transit
on March 18th."
Confident?
Correct. Consistent.
Five anti-atterns the exam will test.
One, relying on prompts for data
consistency.
Prompts fail on edge cases like Unix
timestamps.
Use hooks.
Two, blocking a tool call with no
explanation.
Claude gets stuck and retries endlessly.
Always return a message and an
alternative action.
Three, hooks that silently modify data.
You cannot debug invisible changes. Log
all transformations.
Four, using hooks for soft preferences.
Hooks run on every call and add latency.
Reserve them for hard rules only.
Five, no redirect when blocking. Always
provide an alternative path. Escalation,
a different tool, or human handoff.
Seven exam takeaways.
Lock these in. One, post tool use hooks
normalize heterogeneous data into
consistent formats before Claude
processes them. Two, pre-tool use hooks
block policy violating calls and
redirect to safe alternatives.
Three, hooks are deterministic at 0%
failure. Prompts are probabilistic at 1
to 5%.
Four, always explain why a call was
blocked so Claude knows what to do next.
Five, normalization targets, Unix to ISO
timestamps, numeric to string statuses,
regional to standardized dates.
Six, keep hooks fast. They run on every
tool call.
No network calls, no side effects.
Seven, when the exam says guarantee
compliance, the answer is hooks, not
prompts.
That wraps up domain one, agentic
architecture and orchestration.
All five tasks complete. agentic loops,
multi-agent coordination, sub aent
configuration, enforcement patterns, and
now hooks for data normalization.
In part seven, we move to domain two,
tool design and MCP integration. You
will learn how to write tool
descriptions that Claude selects
reliably, how to structure error
responses, and how to design MCP servers
that connect Claude to your back-end
systems. See you there.
