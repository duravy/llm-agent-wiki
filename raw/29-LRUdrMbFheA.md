Welcome back. In part 28, we covered
error propagation in multi-agent
systems. Quick recap before we move on.
Structured error context means sub aents
return failure type, partial results,
and alternative approaches, not just a
generic error message. The coordinator
can then make intelligent recovery
decisions.
and we never silently suppress errors or
terminate on first failure. Both
anti-atterns destroy valuable
information.
Finally, coverage annotations in the
final output mark which areas are well
supported versus which have gaps.
Today we cover task 5.4, managing
context effectively in large codebase
exploration.
We'll look at why context degrades,
scratchpad files for persistent memory,
subagent delegation for exploration, the
compact command, state persistence for
crash recovery, and summarizing between
phases. These are practical techniques
that keep claud code sharp during
extended investigation sessions.
If this video helped you prepare for the
exam, please give it a like and
subscribe.
Imagine
researching a topic by reading 50
articles without taking notes. By
article 40, you can't remember the
details of article 5. You start making
vague statements instead of citing
specific sources.
That's exactly what happens when claude
code explores a large code base over an
extended session. Early on, it gives you
precise answers. The O manager class in
src/off/manager
TS uses JWT tokens with our S256
signing. The verify method on line 45
checks expiry. But late in the session,
responses degrade to vague claims like
the authentication system uses typical
patterns for token verification.
The model has lost the specific details
and is filling in with generic
knowledge.
This is context degradation and the exam
tests your ability to recognize and
prevent it.
The first solution is scratchpad files,
persistent external memory for your
investigation.
Think of a scratch pad like a
detective's notebook. Instead of keeping
every clue in your head, you write
findings down so you can reference them
later. Even after your memory fades, the
agent writes key discoveries to a file
as it finds them. O manager uses JWT
with our S256
verify at line 45 checks expiry but not
signature validation. Token refresh
lives in src/off/refresh.ts
and there's a race condition in
concurrent refreshes.
When the context window fills up, the
agent reads the scratch pad to recover
key findings.
It references specific discoveries
without reanalyzing the same files. The
investigation continues with full
context even though the conversation
history has been compressed.
This is the difference between starting
fresh every time and carrying forward
what matters.
The second solution, sub agent
delegation for exploration.
Instead of the main agent reading 50
files and filling its context with
verbose output, you delegate exploration
to sub aents that return concise
summaries.
Think of it like a manager sending team
members to investigate different areas.
The manager doesn't read every document.
They get briefings.
Here's how it works in practice.
The main agent says, "I need to
understand the refund flow." Sub agent
one reads 12 test files and returns
three test suites found. Coverage is
happy path only. No error path tests.
Sub agent 2 reads eight source files and
returns. The flow goes from API handler
through validation to service layer to
payment gateway to database with no
transaction roll back on gateway
failure.
The main agents context contains only
these two four-line summaries, not 20
raw files. Context is preserved for
actual decisionmaking.
The third tool is the compact command.
Think of it like summarizing your
meeting notes at the end of a long
session.
You compress hours of discussion into
key decisions, freeing your notebook for
new work. When Claude code's context
fills up with verbose tool output, you
type forward/compact.
Claude summarizes the conversation,
freeing context for new work. Use it
proactively when you notice responses
becoming vague or when the session has
accumulated many tool results.
Don't wait until the model is already
confused. That's when it's too late. The
exam tests this directly. If a question
describes vague generic responses during
extended exploration, compact is one of
the correct answers.
The fourth solution, structured state
persistence for crash recovery.
For multi-hour investigations, agents
should export their state to a known
location so work can be recovered if the
session crashes. Think of it like saving
a game checkpoint.
The manifest file tracks the
investigation name, current phase,
completed tasks with their result files,
files still being analyzed, and key
findings discovered so far.
On crash recovery, the coordinator loads
this manifest and injects it into the
new sessions context.
Work continues from exactly where it
left off. No need to reanalyze files
that were already examined. No need to
rediscover findings that were already
documented for a multi-hour
investigation. This saves enormous time
and prevents the frustration of losing
hours of progress.
The fifth technique is summarizing
between phases. When an investigation
has multiple phases, summarize findings
from one phase before spawning agents
for the next. Here's the flow. Phase one
sub agents explore the codebase and
return findings. The main agent creates
a concise summary of key findings.
Phase two sub aents receive that summary
is context not the raw tool output from
phase one. This summarize then inject
pattern keeps downstream agent context
clean and focused.
Passing raw phase 1 output to phase 2
fills the downstream context
unnecessarily.
The summary is compact and targeted,
giving phase 2 agents exactly what they
need without the noise.
Let's cover the five anti-atterns to
avoid when managing context in large
code bases.
Antiattern one, reading all files in the
main agent. This fills context with
verbose output from every file. The fix
is to delegate to sub aents and receive
concise summaries instead. The main
agent should make decisions, not read
source code. Anti-attern two, no
scratchpad for extended investigation.
Key findings are lost as context
degrades over time. The fix is to write
findings to a scratchpad file and
reference them later.
This preserves discoveries across
context boundaries.
Anti-pattern three, ignoring context
degradation signals. When responses
become vague and start citing typical
patterns instead of specific classes,
the context is already degraded. The fix
is to use compact proactively and
delegate to sub aents before the model
loses precision.
Anti-attern four, no state persistence.
A crash loses hours of investigation
work. The fix is to export structured
state to manifest files so work can be
recovered.
This is especially critical for
multi-hour investigations.
Anti-attern 5, passing raw phase 1
output to phase 2. This fills downstream
context with unnecessary detail. The fix
is to summarize phase findings before
injecting them into the next phase. Keep
it compact and targeted. Lock these in.
The exam will present scenarios where
you need to identify which anti-attern
is causing the problem.
Here's the exam framework for this
topic.
Context degrades over time in extended
sessions.
Recognize the signal. vague responses
citing typical patterns instead of
specific findings.
Scratchpad files provide persistent
memory. Write key findings to files and
reference them later. Sub agent
delegation keeps the main agent context
clean. Sub aents read files and return
concise summaries, not raw content. The
compact command compresses context when
it fills with verbose output.
Use it proactively.
Structured state persistence enables
crash recovery. Export manifest files
and load them on recovery and summarize
between phases. Inject concise summaries
into downstream contexts, not raw tool
output.
The test is this. If the exam asks how
to keep context clean during large
codebase exploration, the answer is
subbagent delegation plus scratchpad
files. If it asks about crash recovery,
the answer is structured state
persistence.
Get those two straight and you own this
topic.
Today we covered how to manage context
effectively when cla explores large code
bases. Context degrades in extended
sessions recognize the signals early.
Scratchpad files and sub aent delegation
keep context clean. The compact command
state persistence and phase summaries
complete the toolkit.
In the next video, part 30, we'll cover
human review workflows, confidence
calibration, stratified sampling, and
the human in the loop safety net. See
you in the next video.
