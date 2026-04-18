Welcome back. In part seven, we covered
task 1.6, task decomposition strategies.
The core question was when to use prompt
chaining, a fixed sequential pipeline
versus dynamic decomposition, where the
plan evolves as you go. Prompt chaining
works for structured repeatable tasks
like code review. Dynamic decomposition
handles open-ended investigations where
you can't predict the full scope up
front. Today we move to task 1.7,
session state, resumption, and forking.
How do you preserve and reuse valuable
analysis work across multiple work
periods?
In tasks 1.1 through 1.6, every agent
run started from scratch. A session
changes that. A session is a named,
persistent record of a conversation that
you can return to later. Think of it
like a notebook at your desk. When you
close it and go home, your notes are
still there the next morning. Start a
blank notebook and everything is gone.
Sessions let you keep that notebook open
across days, picking up exactly where
you left off.
Why does this matter? Complex
investigations naturally span multiple
work periods.
Day one, you analyze the codebase
architecture.
Claude reads every relevant file, builds
a mental model, identifies the critical
paths. That analysis lives in the
session history.
Day two, you write tests based on what
was found. Rather than reexplaining the
entire codebase, you resume the session
and Claude remembers exactly what it
analyzed.
Day three, you fix the bugs the tests
revealed. Again, you resume. Claude's
prior context is right there. Without
sessions, you'd reexlain everything from
scratch each day. Sessions turn
multi-day work into one continuous
investigation.
Named sessions use two flags. The double
dash session flag starts a new named
session. You give it a meaningful label
like offer factor. Claude does the work.
You close the terminal. Days later, the
double dash resume flag loads that
session's full conversation history back
into context.
All the tool results, all the analysis,
all the prior messages are still there.
Claude picks up exactly where it
stopped.
The exam tests one critical distinction.
When should you resume and when should
you start fresh?
If the code hasn't changed and you're
continuing analysis from where you
stopped, resume. Prior context is still
valid. You save all the time that
reanalysis would cost. If code was
modified between sessions, resume with
caution. Old tool results in the history
may no longer match what's actually in
the files. You'll need to tell Claude
what changed. If the prior session
filled most of the context window, start
fresh with a summary. Resuming when
context is exhausted means new messages
compete with thousands of old tool
results for attention. You lose the
benefit of the prior session anyway.
If you're exploring a completely
different approach, start fresh. Old
context about the previous direction
would actively mislead the new one. The
exam tests all four of these scenarios.
The stale context problem is the core
concept here. When you resume a session,
all the old tool results are still in
the conversation history, but the actual
files may have changed since those tools
ran. Claude thinks O. py has 45 lines. A
teammate changed it. Now it has 60 with
new validation logic. Claude doesn't
know. It's reasoning from a snapshot.
that's no longer true. That incorrect
mental model propagates into every
analysis that follows.
Solution one, inform the resumed session
about changes.
Resume the session, then immediately
tell Claude what changed. Since our last
session, O. py was updated with new
validation logic. Please reread it
before continuing.
This signals that the old mental model
is outdated, prompting Claude to use
tools to refresh its understanding
before proceeding.
Solution two, start fresh with an
injected summary. Instead of resuming,
open a new session with a structured
briefing of what the prior analysis
found. The code has changed. Please
reanalyze and confirm or revise these
findings.
You preserve the key insights without
carrying stale tool results that could
corrupt the new session's reasoning. The
summary replaces the history cleanly.
Fork session creates an independent copy
of the current session at a branch
point. You've done the shared work,
analyzed the codebase, identified the
problem, narrowed to two possible
approaches.
Then you fork. Each fork gets a complete
copy of everything up to that point.
From there they diverge completely
independently.
You can run them in parallel exploring
different solutions simultaneously. Then
compare results and choose the best
approach.
For properties of forked sessions, you
need to know cold for the exam.
First, each fork gets a complete copy of
the conversation history up to the fork
point. Not a reference, a full
independent copy.
Second, forks are completely
independent.
Changes in fork A have no effect on fork
B. They cannot see each other's work.
Third, forks run in parallel. You're not
blocked waiting for fork A to finish
before starting fork B. Both execute
simultaneously. That's the whole point.
Fourth, the original session remains
unchanged.
Forking is non-destructive.
If both forks lead somewhere bad, the
original baseline is still there
untouched.
When should you use fork underscore
session instead of sequential
exploration?
Use it when comparing two refactoring
strategies before committing.
Fork at the decision point. Implement
each strategy independently. Compare
results, then choose the winner. Use it
when testing different testing
approaches. Unit versus integration
focus. Each fork explores one approach
fully, so you see what each produces
without contaminating one with the
other.
Use it when exploring alternative
architectures where you've done shared
analysis but want to evaluate two
designs without losing your progress.
Never fork for simple linear tasks. That
just adds unnecessary complexity where
none is needed.
Here's the decision framework the exam
tests. Has the code changed
significantly since the session? If no,
resume the context is still valid. If
yes, is the session context window
mostly exhausted? If it is, start fresh
with a summary injection. If it's not,
resume, but explicitly inform Claude
about the specific changes so it rereads
the affected files before continuing.
Five antiatterns to avoid. All of these
appear on the exam.
Resuming sessions when code was heavily
modified. Old tool results lead to
incorrect analysis.
Fix. Start fresh with a summary or
explicitly tell Claude what changed.
Starting fresh when context is still
valid. This throws away good analysis
and wastes time reanalyzing unchanged
code. Fix. Resume when prior context is
accurate. Never using named sessions at
all. You lose all progress when the
terminal closes. Fix named sessions for
any multi-day investigation work.
Forking for simple linear tasks.
Unnecessary complexity. A single thread
would do. Fix. Fork only when genuinely
comparing alternative approaches in
parallel.
Resuming sessions with exhausted context
windows.
New messages compete with thousands of
stale tool results for attention. Fix.
Start fresh. Inject a concise summary of
key findings.
Seven things you must know for the exam
on task. 1.7.
One. Double dash resume continues a
named session with the full conversation
history intact.
Two. Stale context is the biggest risk
of resumption.
tool results from previous sessions may
no longer reflect the current state of
files.
Three, mitigation strategy one, inform
the resumed session about specific
changes so Claude rereads the affected
files.
Four, mitigation strategy two, start a
fresh session and inject a structured
summary of the prior analysis findings.
Five, fork session creates independent
branches from a shared analysis
baseline. Useful for comparing
strategies or architectures in parallel.
Six, start fresh when code has changed
significantly, context is exhausted, or
you're taking a completely new
direction.
Seven, session management is
fundamentally about preserving valuable
analysis while avoiding stale or
overwhelming context.
That wraps up task 1.7, session state,
resumption, and forking. You now
understand how named sessions preserve
work across days, when resumption is
safe versus risky, how to handle stale
context with either mitigation strategy,
and when fork session adds value versus
when it's overkill.
In part nine, we move into domain two,
tool design and MCP integration.
Task 2.1 covers writing effective tool
descriptions, why the description is the
only signal Claude uses to pick a tool,
and how vague descriptions lead to
misouting.
See you there.
