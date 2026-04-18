Welcome back. In part 10, we covered
task 2.2, structured error responses for
MCP tools. Quick recap before we move to
task 2.3.
For error categories, define how claude
response. Transient means retry.
Validation means fix the input. Business
means explain and stop. Permission means
escalate.
Five structured fields carry the
decision metadata. Error category is
retriable message, customer message, and
suggestion.
Access failures and valid empty results
require completely different handling.
Is error true means the service was
down. Is error false with an empty array
means nothing was found. Local recovery
keeps coordinator context clean. Sub
agents retry transit errors locally
before escalating with partial results
and alternatives
is retriable false on business and
permission errors prevents feudal retry
loops. Claude stops immediately instead
of hammering a service that will never
succeed. That's task 2.2.
Today we move to task 2.3. Distributing
tools across agents and configuring tool
choice.
Task 2.3 starts with a question. How do
you decide which tools go to which
agent? And what happens when you get it
wrong? The answer comes from a research
finding the exam directly tests. Giving
an agent too many tools degrades tool
selection reliability.
Each additional tool is another option
Claude must evaluate.
The more options, the more decision
complexity and the more likely Claude
picks the wrong one. The sweet spot is
four to five tools per agent.
Here is the analogy.
You hire an assistant and give them
access to 12 systems. email, calendar,
CRM, accounting, HR, project management,
design tools, code repo, support
platform, analytics, social media, and
office supplies. When you say schedule a
meeting, they have 12 systems to
consider. They might check the HR system
instead of the calendar. Now, imagine
you give them only email and calendar.
When you say schedule a meeting, no
confusion.
Scope tools eliminate decision
complexity.
The principle of least privilege applied
to tools means each agent gets only what
it needs for its specific role. A web
researcher gets web search and fetch
URL. A document analyzer gets read doc
and extract text. A customer agent gets
lookup customer and process refund. The
coordinator gets the task tool to spawn
sub aents plus nothing else. Agents with
tools outside their role tend to misuse
them. A synthesis agent with web search
starts searching instead of
synthesizing.
Sometimes a sub agent needs limited
access to a tool from another domain.
This is the crossroll tools exception.
Example, the synthesis agent frequently
verifies simple facts, dates, names,
statistics.
Without a verification tool, every fact
check requires a full round trip through
the coordinator, which adds two to three
steps and 40% latency. The solution is a
scoped verify fact tool for simple
lookups only. Its description explicitly
limits it. Use only for simple fact
checks during synthesis.
Complex research still routes through
the coordinator.
Three tool_choice modes control how
Claude selects tools. Each one solves a
different problem. Auto is the default.
Claude may call a tool or return plain
text. Use this for normal operation. Let
Claude decide what fits best.
any forces clawed to call some tool any
of them. Use this when you always need
structured output and cannot accept a
text reply.
Force selection names a specific tool
claude must call. Use this for ordered
pipelines where a specific step must run
first like metadata extraction before
enrichment.
Force tool selection is the key to
building ordered pipelines.
In step one, you force extract metadata.
Claude must call that tool. No skipping
ahead to enrichment before the metadata
exists.
In step two, you switch to auto. Now
Claude decides what to do next based on
what the metadata extraction returned.
This pattern ensures your pipeline steps
run in the right order every time. The
exam tests whether you know when to use
forced versus auto.
Instead of giving agents powerful
generic tools they might misuse, replace
them with constrained alternatives.
Generic fetch URL can access any URL on
the internet. A research agent might use
it to pull random websites instead of
documents.
Constrained load document validates that
the URL points to an approved domain.
only accepts document formats like PDF,
DOCX and HTML and rejects everything
else. Same core functionality, narrower
scope. The exam tests whether you
recognize when a generic tool creates a
misuse risk.
Five antiatterns for task 2.3. These are
the wrong answer choices on the exam.
Every agent gets all tools.
This is the most common mistake. It
maximizes decision complexity and causes
agents to misuse off roll tools.
18 tools per agent. Reliability drops
significantly above four to five tools.
More options means more selection
errors. No crossroll tools at all. This
forces expensive coordinator round trips
for simple operations the sub agent
could handle. directly
using auto when structured output is
required. If you need a tool call every
time, use any. Auto allows claude to
return text instead.
Generic tools like fetch URL with no
constraints.
Replace them with constrained
alternatives that limit scope and
prevent accidental misuse.
Six exam takeaways for task 2.3. Know
these cold. One, too many tools degrades
reliability.
18 tools overwhelm selection. Aim for
four to five per agent.
Two, scope tool access. Each agent gets
only the tools for its specific role.
Off-roll tools create misuse.
Three, crossroll tools are the exception
only for highfrequency operations that
would require expensive coordinator
round trips.
Four, tool underscore choice modes. Auto
for normal operation, any for guaranteed
tool calls, forced for ordered
pipelines.
Five, force tool selection ensures a
specific step runs first. Use it at the
start of ordered pipelines.
Six, replace generic tools with
constrained alternatives that limit
scope to prevent misuse.
That wraps up task 2.3, tool
distribution, and tool choice. You now
know the two many tools problem, scoped
access, the crossroll exception, and the
three tool choice modes. In part 12, we
tackle task 2.4, 4 integrating MCP
servers. You will learn the difference
between project level and user level
configuration, MCP resources as content
cataloges, and when to use community
servers versus custom ones. See you
there.
