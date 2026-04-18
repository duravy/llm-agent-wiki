Welcome back.
In part nine, we enter domain two and
covered task 2.1, designing tool
descriptions that guide Claude to the
right tool.
Quick recap before we move to task 2.2.
Tool descriptions are the only signal
Claude uses to pick a tool.
Vague descriptions cause misrouting.
Claude picks the wrong tool every time.
Good descriptions need five elements:
purpose, input format, output, when to
use it, and explicit boundary conditions
that say when not to.
Split generic tools into specific ones.
Analyze document becomes three focused
tools: extract data points, summarize
content, and verify claim.
System prompt keywords can accidentally
bias tool selection.
Audit prompts for words that match tool
names.
Renaming ambiguous tools is the highest
leverage fix on the exam.
That's task 2.1.
Now we tackle task 2.2.
Task 2.2, structured error responses for
MCP tools.
When a tool fails, what should it
return?
The naive answer, just a text string.
But Claude reads that string and has to
decide, retry or give up.
Escalate or explain.
A plain error string gives Claude no
signal.
Structured error responses give it
exactly what it needs to make the right
call.
Here is the analogy that makes this
click.
You ask a coworker to book a meeting
room.
They come back and say, "I couldn't."
That is useless. You don't know why or
what to try next.
But if they say, "Room A is booked until
3:00 p.m., Room B has a broken
projector, Room C is free at 2:00 p.m."
Now you can decide.
Structured error responses give Claude
that same context.
Not just that something failed, but what
kind of failure it was and what the
options are.
MCP tools signal failures using the is
error flag.
When a result includes is error true,
Claude knows it is an error report, not
success data.
But the flag alone does not tell Claude
what to do.
There are two layers. The is error flag
is the signal layer, something went
wrong.
The structured error body is the
decision layer, here is what kind of
wrong and what to do next.
The exam tests both layers.
For error categories,
each one requires a different action
from Claude.
Transient, timeout or service
temporarily down.
Claude waits and retries.
These are recoverable.
Validation, invalid input like a bad
order ID format.
Claude fixes the input and retries.
The service is fine, the input was
wrong.
Business, policy violation, refund
exceeds the limit.
Claude explains to the user and does not
retry.
Business rules do not change on the next
attempt.
Permission, unauthorized access.
Claude escalates or informs the user.
Retrying with the same credentials will
never succeed.
The structured error format has five key
fields.
Error category names the failure type:
transient, validation, business, or
permission.
Is retryable is a direct signal. True
means try again, false means stop.
Message has technical detail for logging
and debugging.
Customer message is what to show the
user, included only for business errors.
Suggestion tells the agent exactly what
action to take next.
These five fields are what you need cold
for the exam.
The exam tests this distinction heavily,
access failures versus valid empty
results.
An access failure means the database
timed out. Return is error true, error
category transient.
Claude should retry.
A valid empty result means the search
worked and found nothing. Return is
error false with an empty array.
Claude tells the user they have no
orders.
Mix these up and Claude gives wrong
answers when systems are down.
Know which flag combination goes with
each case.
In multi-agent systems, sub-agents
should handle transient errors locally
before escalating.
A research sub-agent hits a timeout.
It retries once, timeout again.
Retries twice, success.
The coordinator never knew.
But if all retries fail, the sub-agent
escalates with rich context, what it
attempted, any partial results it found,
and alternatives the coordinator can
try.
This is local recovery.
The exam question is, "What should a
sub-agent do before escalating?"
Retry locally, then report with partial
results and alternatives.
Five anti-patterns the exam tests.
All of them appear as wrong answer
choices.
Generic errors.
Returning just operation failed gives
Claude no decision data.
Include error category, is retryable,
and suggestion.
Returning empty results for failures.
If a database times out, is error must
be true.
An empty array silently tells the user
nothing exists when the service was just
down.
Retrying business rule violations.
Set is retryable false for business
errors.
Business rules do not change between
attempts.
Propagating all errors to the
coordinator.
Handle transient errors locally in
sub-agents.
Only escalate what you cannot resolve
internally.
No partial results in error reports.
Always include what was attempted and
what alternatives exist.
Partial context enables intelligent
recovery decisions.
Six exam takeaways for task 2.2.
Know these cold.
One, four error categories: transient,
validation, business, permission.
Each requires a different Claude
response.
Two, five structured fields: error
category, is retryable, message,
customer message, suggestion.
Three, access failures and valid empty
results require completely different
handling.
Is error true for down services, is
error false for nothing found results.
Four, local recovery. Sub-agents handle
transient errors with retries before
escalating to the coordinator.
Five, is retryable false on business
violations prevents wasted token retry
loops.
Six, always include attempted query and
alternatives in escalation reports.
Partial context enables intelligent
coordinator recovery.
That wraps up task 2.2, structured error
responses for MCP tools.
You now know the four error categories,
the five structured fields, and why
access failures and empty results
require completely different handling.
In part 11, we tackle task 2.3,
distributing tools across agents.
Too many tools on one agent degrade
selection accuracy.
You will learn scope tool access, the
principle of least privilege, and the
tool choice parameter.
See you there.
