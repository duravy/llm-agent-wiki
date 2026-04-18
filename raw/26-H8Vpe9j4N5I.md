Welcome back. In part 25, we covered
multi-instance review architectures.
Quick recap before we move on. Self-re
is biased. When a claude session reviews
code it just wrote, it retains its
reasoning context and won't question its
own decisions.
Independent review instances, separate
API calls with no shared history, catch
issues the generator is blind to.
Multi-pass architecture splits review
into per file local analysis, cross-file
integration, and optional confidence
verification.
Per file passes avoid attention dilution
by giving each file the model's full
attention on large PRs.
Confidence calibration routes high
confidence findings to automation and
uncertain findings to human review.
If this video helped you prepare for the
exam, please give it a like and
subscribe.
Your agent handles a customer support
conversation.
By turn 15, it's forgotten the order ID
from turn two. The customer repeats
themselves.
Trust drops.
This happens because context windows
fill up, summaries lose numbers and
dates, and tool outputs bury critical
details in hundreds of irrelevant
fields. In domain 5, we fix all of that.
You'll learn to manage conversation
context across long interactions, design
escalation and ambiguity resolution
patterns, handle error propagation in
multi- aent systems, navigate large
codebase context, build human review
workflows, and track information
provenence and uncertainty.
Let's start with domain five, task 5.1,
context management.
Think of the context window as a desk
with limited space. Every document,
system prompt, messages, tool results,
responses takes up room. When the desk
is full and you need to add something,
you have to remove something else. And
you might remove something important.
Here's the catch. Models process
information at the beginning and end of
context more reliably than the middle.
So if critical details are buried deep
in tool results, Claude reads the latest
message clearly but may miss those
details entirely.
The exam tests this directly.
A common approach to managing long
contexts is summarization. Condensing
earlier conversation into a brief
summary. But summarization loses
critical details.
Look at this example. The original has
the customer name, customer ID, order
ID, date, amount, item details, and
delivery deadline. The summarized
version says the customer had an order
issue and expects quick delivery.
Everything is gone. The order ID, the
amount, the deadline, all lost. When the
agent later needs the order ID to
process a refund, it's not there.
Numbers, dates, and identifiers are
especially vulnerable to summarization
loss.
The fix is the case facts pattern.
Instead of relying on summarization,
extract critical transactional facts
into a persistent structured block that
stays outside the summarized history.
Customer ID, order ID, date, amount,
item details, all preserved.
This block is included at the beginning
of every API call right after the system
prompt. That means these facts are
always visible regardless of how long
the conversation gets. The model never
has to dig through history to find them.
Tool outputs often contain far more data
than needed. A customer order lookup
might return 40 or more fields when you
only need five.
internal IDs, warehouse codes, batch
IDs, shipping label, URLs. None of that
helps the agent answer the customer's
question. The fix is to trim tool
outputs and post tool use hooks before
they enter the context window. Reduce
those 40 fields to the five that matter.
Order ID, status, amount, tracking
number, and estimated delivery. This
preserves context budget for information
that actually matters.
To counteract the lost in the middle
effect, organize input strategically.
Place the system prompt at the start
where it's processed well. Put the case
fact summary near the top critical data
right after the system prompt. Organize
detailed tool results with explicit
section headers so the model can
navigate them and put the latest user
message at the end where it's also
processed well. The key techniques are
simple summaries at the beginning
section headers for detailed data and
the most important details at the
beginning and end of long sections.
When sub aents return research findings,
require metadata alongside content.
Think of metadata like a bibliography
card. Without it, you can't verify where
information came from or whether it's
still current. The downstream synthesis
agent needs source, date, URL, and
methodology to make accurate citations.
Without metadata, a 2020 statistic and a
2025 statistic look identical. and the
agent can't tell which one is relevant.
This is especially important when
temporal context matters.
Here are the five anti-atterns to avoid.
Each one wastes context budget or losses
critical information.
First, progressive summarization of
numbers and dates. When you summarize
conversation history, you lose order
ids, amounts, and deadlines.
The correct approach is to extract a
structured case facts block and maintain
it separately.
Second, including full tool output with
40 or more fields.
This wastes context tokens on irrelevant
data. Trim to relevant fields only in
your post tool use hooks.
Third, putting all details in the middle
of context. The lost in the middle
effect means the model may overlook
them.
Place summaries at the beginning and use
section headers.
Fourth, subagent output without
metadata.
Downstream agents can't site sources or
assess recency.
Require date, source, and URL and
structured output.
Fifth, dropping conversation history
entirely.
This loses conversational coherence.
pass complete history but trim tool
output within that history not the
conversation structure itself.
The test is simple. If the exam asks
about losing details in long
conversations, the answer is case facts
blocks. If it asks about wasting context
tokens, the answer is trimming tool
outputs.
Get those two straight and you own this
topic. Lost in the middle means
beginning and end are processed
reliably, not the middle. Progressive
summarization loses numbers, dates, and
ideas.
Trim tool outputs to the fields that
matter. Position summaries at the top
with section headers. Require metadata
and subbasion outputs and pass complete
conversation history for coherence while
trimming tool output within it.
The difference between an agent that
remembers and one that forgets is
deliberate context design, not hoping
the model keeps track. Use case facts
blocks, trim tool outputs, and organize
input position aarly. In the next video,
part 27, we'll cover escalation and
ambiguity resolution. When should an
agent handle things itself, and when
should it pass to a human? If this video
helped you prepare for the exam, please
subscribe and I'll see you in part 27.
