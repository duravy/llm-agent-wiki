Welcome back. Quick recap from part 24,
badge processing strategies.
First, the message batches API can be
50% cheaper with up to a 24-hour window,
but it cannot run tool calls.
Second, use the synchronous API for
blocking tasks and batch for overnight
workloads.
Third, always include a custom ID so
each response maps back to its original
request.
Fourth, resubmit only failed requests,
never the entire batch.
Fifth, refine prompts on a small sample
before scaling up. Today, we'll cover
why self-re fails, how multipass review
works, and how confidence calibrated
routing improves review quality.
If this video helped you prepare for the
exam, please give it a like and
subscribe.
Here's a scenario you've probably
experienced.
You write a piece of code, then
immediately review it yourself, and it
looks perfect. But the next day, a
colleague spots three bugs you were
completely blind to.
This happens because when the same
clawed session generates code and then
reviews it, the review is biased.
The model retains its reasoning context
from generation and is less likely to
question its own decisions.
Think of it like proofreading your own
essay right after writing it. You read
what you intended to write, not what you
actually wrote. Today we'll cover why
self-re fails, how to build a multi-pass
review architecture for large pull
requests, and how confidence calibration
routes findings to the right reviewer.
This is task 4.6 in domain 4.
Let's see the difference side by side.
When you use the same session, you ask
Claude to write an O module. It
generates code. Then you ask it to
review that code and it says the code
looks correct and well structured. Why?
Because Claude remembers every
assumption it made during generation.
It won't question its own decisions.
Now compare that to a separate session.
Session one generates the O module.
Session two receives only the code with
zero context about how it was built and
it finds three real issues. No rate
limiting on login attempts, a token
expiry check using the wrong comparison
operator and error messages that leak
internal user IDs.
The second session has no reasoning
context from generation. So it evaluates
the code objectively from first
principles.
The exam tests this distinction
directly.
But what happens when you're reviewing a
large pull request with 14 changed
files? A single pass through all of them
has serious problems.
First, attention dilution.
The model focuses heavily on the early
files and the late files, but the ones
in the middle get shallow treatment.
It's like reading a long document where
you remember the introduction and
conclusion clearly, but the middle
chapters blur together.
Second, inconsistent depth. Some files
get detailed, thorough feedback, while
others get just a superficial comment.
Third, contradictory findings.
The model might flag a coding pattern as
problematic in one file, then approve
that exact same pattern in another file
without noticing the inconsistency.
The solution is to split the review into
focused passes, each with a specific
job.
Here's the three pass architecture.
Pass one is per file local analysis.
Each file gets reviewed completely
alone, giving the model its full
attention.
All 14 files can run in parallel as
separate instances.
Pass two is cross-file integration.
You gather all 14 files plus the
summaries from pass one. And this single
instance focuses only on integration
issues, data flow inconsistencies
between modules, API contract
mismatches, and missing error
propagation across boundaries.
Pass three is optional but powerful.
It's a verification pass that rates
every finding from one to five for
confidence.
Findings rated four or higher get
autoposted as pull request comments.
Findings rated two to three get routed
to a human reviewer. Findings at one or
below get discarded.
This prioritizes limited human review
capacity on findings that actually need
judgment.
Let's visualize the full pipeline.
Your 14 change files enter the system.
Pass one spins up 14 parallel instances,
one per file, each producing a detailed
local review. The summaries from all 14
flow into pass two a single instance
that examines cross file integration
data flow API contracts error
propagation.
All findings from both passes then enter
pass three where one instance rates
confidence for each finding. High
confidence findings get autoposted to
the poll request.
Medium confidence findings go to a human
reviewer for judgment.
Low confidence findings get discarded to
reduce noise.
This architecture ensures every file
gets deep attention while also catching
the integration issues that only appear
when you look at the full picture.
Now let's cover the five anti-atterns to
avoid. The exam will present scenarios
where each of these looks tempting but
is wrong. Anti-attern one using the same
session to generate and review code.
This is wrong because the review is
biased by the reasoning context from
generation. The correct approach is to
use a separate independent review
instance with no shared history.
Anti-attern two running a single pass
for 14 or more files.
This causes attention dilution and
inconsistent review depth. Split into
profile local analysis plus cross-file
integration passes instead.
Anti-attern three using extended
thinking as a substitute for a fresh
review. Extended thinking still operates
within the same reasoning context, so it
stays biased.
Use a genuinely separate instance
instead. Anti-attern four, autoposting
all findings as pull request comments.
Low confidence findings create noise and
reduce trust in the review system. Add a
verification pass with confidence
routing to filter appropriately.
Anti-attern five, skipping the cross
file pass entirely.
Without it, you miss data flow issues
and API contract mismatches between
modules. Always include integration
analysis after the local passes.
Here's the decision framework for the
exam. When you see a scenario where a
review misses issues in a large pull
request, the answer is multi-pass review
architecture.
Self-re is biased because the same
session retains reasoning context and
won't question its own decisions.
Independent review instances with
separate API calls and no shared history
catch issues the generator is blind to.
Per file passes avoid attention delution
by giving each file the model's full
attention.
The cross file pass catches data flow
issues, API mismatches, and inconsistent
patterns.
and confidence calibration routes high
confidence findings to automation while
sending uncertain findings to human
review. When the exam describes
inconsistent review quality on large
PRS, the answer is multi-pass review
architecture.
Lock that in.
The test is simple. If the exam asks
what catches issues that a code
generator is blind to, the answer is a
separate independent review instance.
If it asks how to handle inconsistent
review quality on large pull requests,
the answer is multipass review
architecture.
Get those two straight and you own this
topic. In the next video, part 26, we'll
cover context management. How to manage
conversation context to preserve
critical information across long
interactions. Avoid the lost in the
middle effect and keep your agents from
forgetting important facts.
See you there.
