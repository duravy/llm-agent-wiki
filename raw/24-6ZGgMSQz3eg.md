Welcome back. In part 23, we covered
validation and retry loops for
extraction quality. Let's do a quick
recap before we move on. Tool use
eliminates JSON syntax errors, but not
semantic errors. The data can be
structurally valid but factually wrong.
You need validation beyond schema
enforcement.
Retry with error feedback works by
sending the original document plus the
specific error back to Claude for
correction. The model sees its own
failed attempt and can self-correct.
Retries are only effective for format
mismatches and structural errors if
information is absent from the source
except null. Retrying won't create
missing data and may cause
hallucination.
Self-correction validation extracts both
the stated total and the calculated
total, then flags discrepancies with a
conflict detected boolean. This catches
both extraction errors and source
document errors. The detected pattern
field enables systematic analysis of
dismissed findings.
If developers frequently dismiss a
certain pattern, you can analyze whether
these are false positives and improve
the detection criteria over time.
If this video helped you prepare for the
exam, please give it a like and
subscribe.
Think of the message batches API like
economy shipping versus express
delivery.
You pay half the price, but you wait up
to 24 hours instead of getting an
instant response.
Enthropic offers two ways to call
Claude. The synchronous API gives you an
immediate response at standard pricing.
Perfect for real-time use cases like
chat bots or premerge code checks. The
message batches API costs 50% less, but
has an up to 24-hour processing window
with no guaranteed latency.
It's ideal for non-blocking tasks like
overnight reports or weekly audits.
The exam tests your ability to match the
right API to the right workflow. So, get
this distinction straight.
Here's the critical limitation.
The batch's API cannot execute tools mid
request. Each batch request is a single
turn interaction.
With the synchronous API, Claude can
call a tool, you return the result,
Claude calls another tool, and so on.
With batch, all context must be included
up front, and Claude returns its
complete analysis in one shot.
No tool calls possible during
processing.
This means batch works best for
self-contained tasks where everything
the model needs is already in the
request.
The exam tests your ability to match the
right API to the right workflow. Here
are three scenarios.
A premerge code review blocks a
developer from merging. They're waiting,
so you need a fast response.
Use the synchronous API.
A nightly technical debt report just
needs to be ready by morning. Overnight
processing is fine. Use batch and save
50%.
A weekly security audit needs to be
ready by Monday.
Weekend processing is fine. Use batch
again. If a manager proposes switching
both workflows to batch for cost
savings, the correct answer is batch for
the nightly report only synchronous for
premerge checks.
Think of custom ID like a tracking
number on a package. Each batch request
includes a custom ID that links the
response back to the original request.
When you create a batch, you assign a
custom ID to each request like
docinvo00001.
When results come back, you match by
custom ID to know which response belongs
to which document.
This is essential for tracking and for
identifying failures.
Always include meaningful custom IDs on
every batch request.
Not all requests in a batch succeed.
The key is to resubmit only the failed
documents, not the entire batch.
After the batch completes, check each
results status. If it failed, collect
the custom ID, the error message, and
the original document.
Then analyze the failure before
resubmitting.
If the error is a context limit, the
document was too long, so split it into
chunks and resubmit.
For other errors, resubmit as is. This
saves money by not reprocessing
documents that already succeeded
before submitting thousands of
documents.
Test your prompt on a sample set first.
Phase one uses the synchronous API on 20
to 50 representative documents.
You check extraction quality, fix prompt
issues,
and iterate until quality is acceptable.
Phase 2 switches to the batch API for
the full volume, say 10,000 documents
with the refined prompt.
You get 50% cost savings at scale and a
higher first pass success rate, which
means fewer expensive resubmissions.
This saves money by maximizing quality
before committing to the full batch.
Let's cover the five anti-atterns to
avoid when using the message batches
API.
The exam tests these directly.
Anti-attern one, using the batch API for
blocking workflows.
Premerge checks can't wait 24 hours. The
developer is waiting right now. Use the
synchronous API for blocking tasks.
Batch for overnight processing.
Anti-attern two, resubmitting the entire
batch on partial failure. This wastes
money on documents that already
succeeded.
Use custom ID to identify and resubmit
only the failures.
Anti-attern three, no prompt refinement
before a large batch. This leads to a
low first pass success rate and
expensive resubmissions.
Test on a sample set first, then batch
the full volume. Anti-attern 4, using
batch for multi-turn tool calling tasks.
The batch API doesn't support mid
request tool execution.
Use the synchronous API for agentic
workflows that need tool use.
Anti-attern five, no custom ID on batch
requests.
Without it, you can't correlate
responses to original documents.
Always include a meaningful custom ID
for tracking and failure handling.
Here's the decision framework for the
exam. If someone is waiting for the
result, use synchronous.
If the work can happen overnight or over
the weekend, use batch and save 50%.
Always refine prompts on a sample before
processing large volumes.
Always include custom IDs so you can
track and resubmit only failures.
And remember, batch doesn't support
multi-turn tool calling.
Get these contrasts straight and you own
this topic.
The test is simple. If the exam asks
what guarantees a 50% costsaving for
non-blocking workflows, the answer is
the message batches API.
If it asks what supports multi-turn tool
calling, the answer is the synchronous
API.
The difference between a cost-effective
architecture and a broken one is
matching the right API to the right
workflow. In the next video, we'll cover
multi-instance review architectures.
You'll learn why self-re doesn't work,
how independent review instances catch
issues the generator is blind to, and
the multipass architecture that scales
to large code bases. See you in part 25.
