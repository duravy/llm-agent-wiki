Welcome back. In part 29, we covered
managing context in large codebase
exploration.
Quick recap before we move on. When you
do context degradation analysis early,
you get specific, accurate findings.
When you let context fill up without
intervention, responses become vague and
reference typical patterns instead of
actual code. When you delegate
exploration to sub agents, the main
agent gets concise summaries.
When you read all files in the main
agent, context fills with verbose output
and decision quality drops.
When you use the compact command
proactively, you compress verbose
conversation history and free context
for new work. When you ignore
degradation signals, the session keeps
deteriorating.
When you export structured state
manifests, multi-hour investigations
survive session crashes.
Without persistence, a crash loses
everything. When you summarize between
phases, downstream agents get clean,
focused context. When you pass raw tool
output forward, you fill every
downstream agents context unnecessarily.
If this video helped you prepare for the
exam, please give it a like and
subscribe.
Today we cover human review routing,
confidence calibration, stratified
sampling, and feedback loops.
Think of an airport security scanner. It
flags bags for manual inspection. If it
flags every bag, the line is impossibly
long. If it flags none, it misses
threats.
Good calibration flags the right bags
enough to catch threats without
overwhelming inspectors.
The same principle applies to AI
extraction systems.
No AI system is 100% accurate. Even with
structured output, validation and fewot
examples, some extractions will be
wrong. Human review is the safety net,
but human time is expensive.
The goal is to route only the cases that
need human judgment to reviewers and
auto accept the rest. The core question
is how do you know which extractions are
safe to automate and which need a human?
The answer is confidence calibration and
it is harder than it sounds.
A system with 97% overall accuracy
sounds great, but aggregate metrics mask
poor performance on specific segments.
Here is the trap. Standard invoices make
up 90% of volume at 99.5%
accuracy.
Handwritten receipts are 7% of volume at
just 72% accuracy.
Foreign language documents are 3% at
65%.
The 97% is dominated by standard
invoices.
Handwritten receipts and foreign docs
are terrible but hidden.
You must validate accuracy by document
type and field before automating high
confidence extractions.
Never trust a single accuracy
percentage.
Instead of one confidence score per
extraction, have the model output
confidence per field. Vendor name at 98%
confidence.
Invoice date at 95%.
Total amount at 92%.
Tax amount at 60%.
Category at 35%.
Now you can route intelligently.
Fields with confidence at or above 95%
get auto accepted. Fields between 0.5
and 0.95 go to human review.
Fields below 0.5 get flagged as unable
to extract. Do not guess. This targeted
approach saves human time while catching
the uncertain fields that matter.
Here is the catch. Model reported
confidence scores are not automatically
calibrated.
A model might say 0.9 confidence but be
wrong 30% of the time. You need to
measure and adjust.
Step one, create a labeled validation
set of 100 or more documents with known
correct extractions.
Step two, run the extraction model on
all of them.
Step three, compare model confidence to
actual accuracy.
If the model says 0.9 confidence but is
actually correct only 85% of the time,
it is overconfident.
Step four, adjust your thresholds.
If your original threshold was auto
accept above 0.9, the adjusted threshold
might need to be 0.95
because 0.9 was overconfident.
Never trust raw confidence scores.
Always calibrate against known ground
truth before setting automation
thresholds.
Even after deployment, you need ongoing
quality checks. But simple random
sampling has a blind spot. Random 2%
sampling across all document types would
give you about 18 invoice reviews, 1.4
receipt reviews, and 0.6 6 foreign doc
reviews per day. Almost no coverage of
the types that need the most review.
Stratified sampling fixes this. Standard
invoices at 90% of volume get sampled at
2% about 18 reviews per day. Handwritten
receipts at 7% get sampled at 15% about
10 reviews.
Foreign language docs at 3% get sampled
at 30% about nine reviews.
Total roughly 37 human reviews per day.
Stratified sampling over samples rare
and difficult categories to catch
problems where they are most likely to
occur.
Here is the full workflow from
extraction to improvement.
First the model extracts fields with per
field confidence scores.
Then routing kicks in. High confidence
gets auto accepted. Medium confidence
goes to human review. Low confidence
gets flagged as failed. The human
reviewer sees the original document. The
model's extraction and the specific
fields flagged for review along with
their confidence scores.
The reviewer corrects or confirms each
field. But here is the critical step
that most systems miss. Human
corrections feed back into three
systems.
One, the final output gets the corrected
values. Two, calibration data updates
the confidence thresholds.
Three, error pattern analysis improves
the extraction prompts over time. A
system without this feedback loop never
gets better.
Here are five ways to get human review
workflows wrong. Each one looks
reasonable on the surface but causes
real problems in production.
Anti-attern one, relying only on
aggregate accuracy metrics.
This masks poor performance on specific
document types. The consequence is that
you think your system is performing well
overall while certain document types
fail silently. The fix always validate
accuracy by document type and field
separately.
Anti-attern two trusting model
confidence without calibration.
Models are systematically overconfident.
They report higher certainty than their
actual accuracy justifies.
The consequence is you auto accept
extractions that are actually wrong. The
fix: calibrate thresholds with labeled
validation sets before deploying
automation.
Anti-attern three random sampling
without stratification.
Simple random sampling under samples
rare and difficult categories.
The consequence is you almost never
review the document types that fail most
often. The fix. Use stratified sampling
weighted toward difficult types.
Anti-attern four automating all high
confidence extractions immediately.
You have not verified accuracy across
all segments.
The consequence is that high confidence
scores on rare document types may still
be wrong. The fix validate per segment
accuracy before automating any high
confidence path.
Anti-attern five, no feedback loop from
human corrections.
The system never improves because
corrections are not fed back into the
pipeline. The consequence is the same
errors repeat indefinitely.
The fix: Use human corrections to update
calibration thresholds and improve
extraction prompts over time.
Here is how to think about this topic on
the exam. If the question asks about
accuracy metrics, the answer is always
validate by document type and field.
Never trust a single accuracy
percentage.
If it asks about confidence scores,
field level scores enable targeted
review. But raw scores lie. Always
calibrate against labeled validation
sets before setting thresholds.
If it asks about quality monitoring,
simple random sampling misses rare
failures.
The answer is stratified sampling that
over samples difficult document types.
If it asks about the review workflow,
route by confidence, auto accept high,
human review, medium, flag low as
failed. The key is per field routing,
not whole document decisions.
If it asks about system improvement,
human corrections must feed back into
calibration updates and prompt
improvements.
The core principle, human time is
expensive.
Route only the uncertain cases.
Calibrate everything. Measure by
segment, not aggregate.
A brittle system automates everything
and hopes for the best. A robust system
knows its limits, routes uncertain cases
to humans, calibrates its confidence,
and gets better every day through
feedback loops.
Never trust aggregate accuracy.
Never trust raw confidence.
Always validate by segment.
Always close the feedback loop. Up next
in part 31, information provenence and
uncertainty.
We will cover how to preserve source
attribution when multiple agents
synthesize findings and what to do when
sources disagree. See you there.
