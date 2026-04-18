Welcome back.
In part eight, we wrapped up domain one
with task 1.7, session state resumption,
and forking.
Let's do a quick recap before we enter a
brand new domain.
Named sessions let you preserve valuable
analysis across multiple work days.
You label a session, close the terminal,
and come back later with the full
history intact.
The resume flag loads a prior session's
conversation back into context.
All tool results, all analysis, right
where you left off.
Stale context is the biggest risk.
If files change since the session, old
tool results may no longer match
reality.
You either inform Claude about changes
or start fresh with a summary.
Fork_session creates independent copies
from a shared analysis baseline. Useful
for comparing refactoring strategies or
architectures in parallel.
And start fresh when context is
exhausted, code changed heavily, or
you're exploring a completely different
direction.
That's domain one, all seven tasks done.
Today, we enter domain two.
Welcome to domain two, tool design and
MCP integration.
This domain makes up 20% of the exam,
one in five questions.
Domain one was about how agents think
and orchestrate.
Domain two is about the tools agents
use.
How you design, describe, distribute,
and integrate tools determines whether
an agent picks the right action or
fumbles.
Five tasks span this domain, and we'll
tackle them one at a time starting
today.
Here's the road map for domain two.
Five tasks, each building on the last.
Task 2.1, designing effective tool
interfaces.
This is where we start today.
The core idea, tool descriptions are the
only signal Claude uses to pick which
tool to call.
Get them wrong, and Claude misroutes
every request.
Task 2.2, structured error responses.
When a tool fails, Claude needs to know
what kind of failure it is, whether to
retry, and what to do next.
Generic error messages leave it
guessing.
Task 2.3, distributing tools across
agents.
Too many tools on one agent degrade
selection accuracy.
You'll learn scope tool access and the
principle of least privilege.
Task 2.4, integrating MCP servers.
The model context protocol lets you plug
external tool servers into Claude code
and agent workflows.
Configuration, not code.
Task 2.5, selecting built-in tools
effectively.
Read, write, edit, bash, grep, glob.
Knowing which built-in tool fits which
situation is a core exam skill.
Let's dive into task 2.1.
Here's the key insight. Claude does not
read your implementation code.
It reads the tool description.
That's it.
When you give Claude five tools and a
user request, it picks the tool whose
description best matches the intent.
Think of a restaurant menu.
If two dishes are both described as just
a dish, you can't choose between them.
But if one says grilled salmon with
lemon butter sauce, and the other says
spicy vegetable curry, vegan, serves
two, now you know exactly what you're
ordering.
Tool descriptions work the same way for
Claude.
Here's what misrouting looks like in
practice.
You have two tools, get_customer,
described as retrieves customer
information, and lookup_order, described
as retrieves order details.
A user says check my order number 12345.
Both tools retrieve things.
The number could be a customer ID or an
order ID.
Claude picks get_customer, wrong tool.
The descriptions don't explain when to
use each one, what inputs they expect,
or how they differ.
Vague descriptions create ambiguity, and
Claude resolves ambiguity by guessing.
A good tool description includes five
elements.
Let's walk through each one.
First, purpose.
What does this tool actually do?
Not a vague verb like analyzes content,
a specific action like extract
structured data from web pages.
Second, input format.
What should the inputs look like?
Include examples.
Instead of just URL as a string, say
full URL including HTTPS.
Example, https://example.com/article.
Third, output.
What comes back?
Page title, main text content,
publication date, and author if
available.
Claude needs to know what the tool
returns so it can plan its next step.
Fourth, when to use.
In what situation should Claude choose
this tool?
Use this when the user asks about
information on a specific web page.
This narrows the selection criteria.
Fifth, boundary.
When should Claude not use this tool?
Do not use for PDF documents. Use
extract_pdf
instead.
Do not use for general web searches. Use
web_search instead.
Boundaries prevent misrouting to
similar-sounding tools.
Let's see the difference side by side.
On the left, a bad tool definition.
The name is analyze_content,
generic, could mean anything.
The description says analyzes content.
That's circular, tells Claude nothing.
The input is just URL as a string with
no guidance.
On the right, the same tool redesigned.
Name, extract_web_results,
specific action plus specific target.
Description covers all five elements.
What it does, what goes in, what comes
out, when to use it, and when not to.
The input includes an example value.
This is the single highest leverage fix
the exam tests.
When you see a misrouting problem, the
first thing to check is the tool
description.
Another common mistake, building one
generic tool that does many things.
Take analyze_document.
It could mean extract data points,
summarize the content, or verify a claim
against the source.
Claude has to guess which behavior the
user wants every single time.
The fix is to split it into three
specific tools, each with a distinct
name and a clear purpose.
Extract_data_points
pulls structured facts like names,
dates, and amounts.
Summarize_content
generates a concise summary of the main
arguments.
Verify_claim_against_source
checks if a specific claim is supported
by the source.
Now Claude can match user intent to the
right tool with zero ambiguity.
Here's a subtle trap the exam tests.
Words in your system prompt can
accidentally influence which tool Claude
selects.
Example, your system prompt says always
analyze customer requests carefully.
You have two tools, analyze_content
and lookup_order.
The user asks, "What's the status of
order 123?"
Claude might pick analyze_content
because the system prompt emphasizes the
word analyze, and that word appears in
the tool name, even though lookup_order
is the correct choice.
The fix, review your system prompt for
words that match tool names.
In this case, renaming analyze_content
to extract_web_content
eliminates the accidental keyword
association entirely.
Three naming strategies to keep in mind.
Action plus object, like
extract_data_points,
works for standard operations.
Verb plus scope, like
search_web_articles,
works when the scope distinction
matters.
Domain prefixed, like
billing_lookup_invoice,
works when you have many tools spanning
different domains.
The common thread, every name should
tell Claude exactly what the tool does.
Avoid generic verbs like process,
handle, or do. They give zero signal
about when to use the tool.
Five anti-patterns to watch for on the
exam.
All of them show up as wrong answer
choices or scenario setups.
Minimal descriptions.
Just saying retrieves data tells Claude
nothing about what makes this tool
different from five other tools that
also retrieve data.
Always include purpose, inputs, outputs,
when to use, and boundaries.
Overlapping descriptions.
If two tools sound alike, Claude picks
randomly.
Make each description explicitly state
what makes it unique.
Generic tool names.
Names like analyze {underscore} document
are ambiguous. They could mean five
different operations.
Split into specific tools with distinct
names.
System prompt keyword collisions.
Words in the system prompt that match
tool names create unintended selection
bias.
Always audit your prompt for accidental
overlaps.
Missing boundary conditions.
Without explicit boundaries, Claude uses
a tool for inappropriate inputs.
State when not to use the tool. For
example, not for PDFs, not for general
searches.
Six takeaways for the exam on task 2.1.
Know these cold.
One, tool descriptions are the primary
mechanism Claude uses for tool
selection.
Minimal descriptions cause misrouting.
Two, five elements of a good
description, purpose, input format,
output, when to use, and when not to
use.
Three, split generic tools into
purpose-specific tools with distinct
input and output contracts.
Four, renaming ambiguous tools is a
high-leverage fix.
Changing analyze {underscore} content to
extract {underscore} web {underscore}
results with a specific description is
the first thing to try.
Five, system prompt keywords can
accidentally bias tool selection.
Audit your prompts for unintended tool
name matches.
Six, when the exam presents a tool
misrouting problem, the first fix to
consider is improving the tool
description.
It's the lowest effort, highest impact
change.
That wraps up task 2.1, designing
effective tool interfaces with clear
descriptions and boundaries.
You now know why descriptions are the
only signal Claude uses, the five
elements every description needs, how to
split generic tools into specific ones,
and the system prompt keyword trap.
In part 10, we tackle task 2.2,
structured error responses for MCP
tools.
When a tool fails, Claude needs to know
what kind of failure it is, whether to
retry, and what to do next.
See you there.
