Welcome back. In part 27, we covered
escalation and ambiguity resolution.
Quick recap before we move on. First,
three escalation triggers. The customer
asks for a human, there's a policy gap,
or you can't make progress after
retries.
Second, honor human requests
immediately.
Do not investigate first, even if you
think you can help. The exam tests this
directly.
Third, sentiment is unreliable as an
escalation proxy. A frustrated customer
might have a simple issue. Escalate
based on case complexity, not emotional
tone. Fourth, when multiple customer
accounts match, ask for additional
identifiers.
Never guess based on heruristics.
And fifth, explicit escalation criteria
with fewot examples produce consistent
behavior. Vague rules like complex cases
do not.
If this video helped you prepare for the
exam, please give it a like and
subscribe.
Here's a scenario that breaks multi-
aent systems. A sub agent fails
silently. The coordinator never knows
and the final output looks correct but
is completely wrong.
Today we cover task 5.3
error propagation in multi-agent
systems.
You'll learn how sub aents should report
failures with rich context. Why generic
errors are useless, two anti-atterns
that break workflows,
graceful degradation with coverage
annotations, and when to handle errors
locally versus propagating them up.
Think of a nurse discovering an abnormal
lab result. A bad report says there was
a problem that tells the doctor nothing.
A good report says patient X creatinine
4.5 normal is under 1.2 measured at 3
p.m. suggestive of acute kidney injury.
Recommend urgent nefrology consult. Now
the doctor can act immediately.
The same principle applies to
multi-agent systems. The quality of
error communication determines whether
the system recovers gracefully or fails
silently.
Here's what a good error report looks
like. When a sub agent fails, it returns
structured context. the failure type,
the query it attempted, any partial
results it managed to collect, what was
missed, and alternative approaches the
coordinator could try. With this
information, the coordinator can make
intelligent decisions.
Should it retry with a narrower query,
use cached results,
or skip the unavailable source and note
the gap? Without this context, the
coordinator would just see search failed
and have zero recovery options.
The difference comes down to
actionability.
A generic error like search unavailable
tells the coordinator nothing. It
assumes the search found no relevant
results when in reality the service was
completely down.
A structured error tells the full story.
Partial results exist. Alternative
approaches are available. The
coordinator can now decide to proceed
with what it has and flag the gap. This
is the difference between a system that
adapts and one that gives up.
Anti-attern number one, silent
suppression. The sub agent catches a
timeout error and returns an empty
success with zero results.
The coordinator thinks the search found
nothing relevant, but the reality is the
search service was completely down. The
output looks correct, but is silently
wrong.
The fix is simple. Distinguish access
failures from valid empty results.
Return a structured error with the
failure type so the coordinator knows
the difference.
Anti-attern
number two, immediate termination.
One sub agent fails, so the coordinator
kills the entire workflow, but other sub
aents may have completed successfully.
Their partial results get discarded for
nothing. The user gets zero output
instead of partial but useful output.
The fix. Continue with partial results
from successful sub aents and annotate
which areas have gaps. Partial output
beats no output every time.
The correct approach is graceful
degradation. The sub agent reports a
structured error. The coordinator
extracts the partial results and the gap
description. Then it continues the
synthesis, annotating the output with a
coverage note.
The coordinator's job is to proceed with
partial results and annotate coverage
gaps, not abort the entire workflow. The
user gets useful output with clear
transparency about what's missing.
When a sub agent partially fails, the
final synthesis should annotate which
areas are well supported and which have
gaps.
Think of it like a weather report that
tells you which forecasts are based on
satellite data and which are just
educated guesses.
The reader sees diagnostic applications
are well supported with five sources.
Drug discovery has partial coverage
because PubMed was unavailable and
mental health applications are a gap
that needs manual research.
The reader knows exactly which findings
are reliable and which need
verification.
No silent misinformation.
Sub agents should handle transient
errors locally before propagating
anything up. If a search times out,
retry internally.
If the second attempt succeeds, return
clean results.
The coordinator never even knew there
was a hiccup.
But if all retries fail, then and only
then return a structured error to the
coordinator, including how many retries
were attempted. This keeps the
coordinator's context clean and focused
on decisions it actually needs to make.
Let's cover the five anti-atterns to
avoid. Each one breaks multi-agent error
handling in a different way.
One, generic search unavailable errors.
The coordinator can't make recovery
decisions without knowing the failure
type, partial results, or alternatives.
Always include structured context.
Two, returning empty results as success.
The coordinator thinks nothing was found
when the service was actually down.
Distinguish access failures from valid
empty results.
Three, terminating on first sub aent
failure. This discards partial results
from successful sub aents.
Continue with what you have and annotate
the gaps.
Four, no coverage annotations.
The reader doesn't know which findings
are reliable and which aren't. Always
annotate wellsupported versus partial
versus gap areas.
and five propagating every transient
error. This clutters the coordinator
context with noise. Handle transient
errors locally with retries.
Only propagate what can't be resolved.
Here's the decision framework for the
exam. When a sub agent fails, structured
error context must include failure type,
attempted query, partial results, and
alternative approaches.
Never silently suppress errors, and
never terminate on first failure. The
coordinator's job is to proceed with
partial results and annotate coverage
gaps, not abort.
Local recovery handles transient errors,
only propagate unresolvable failures.
The exam tests this directly. Graceful
degradation beats both silent
suppression and immediate termination.
Lock this in.
To wrap up, structured errors give the
coordinator everything needed for
recovery.
Graceful degradation means continuing
with partial results and annotating
gaps, not killing the pipeline. and
local recovery handles transient errors
with retries only propagating what can't
be resolved internally.
In part 29, we'll tackle large codebase
context.
When cloud code explores a big code
base, context fills up fast. We'll cover
scratchpad files for persistent memory,
subagent delegation for exploration, the
compact command, and structured state
persistence for crash recovery.
See you in the next video.
