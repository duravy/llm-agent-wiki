Welcome back. In part 22, we covered
structured output in JSON schemas. Quick
recap before we move on. First, tool use
with JSON schemas eliminates syntax
errors entirely.
Claude literally cannot produce
malformed JSON for a tool call. No
missing braces, no trailing commas.
Second tool underscore choice set to any
guarantees structured output. Claude
must call a tool. It never returns plain
text instead.
Third, nullable fields prevent
hallucination.
When the source document may not contain
certain information, make that field
optional. Don't force Claude to invent a
value. Fourth, enums with unclear and
other prevent forced mclassification.
If a document doesn't fit your
categories, Claude can say so instead of
guessing wrong. And fifth, the critical
one, tool use eliminates syntax errors,
but not semantic errors. The JSON is
structurally perfect, but the values
inside can still be wrong.
If this series is helping you prepare
for the exam, please give it a like and
subscribe. Every like helps more
developers find this content. Here's
a fundamental distinction. The exam
tests repeatedly.
Think of a spell checker versus a fact
checker. A spell checker catches typos.
Writing Tuesday instead of Tuesday,
but it won't catch the meeting is on
Wednesday when it's actually Thursday.
Tool uses the spell checker. It
guarantees valid JSON syntax,
but it doesn't verify the facts inside
that JSON. Here's a concrete scenario.
The vendor is Acme Corp. The total is
100. The JSON is valid, but the line
items only sum to $92.
And tax on 92 at 8% is $7.36,
not 8.
The JSON is structurally perfect. The
numbers are wrong. That's a semantic
error and it requires a separate
validation step.
So what do you do when validation
catches an error? The most powerful
pattern is retry with error feedback.
When validation fails, you send the
original document plus the specific
errors back to Claude for correction.
Here's how it works. You make the first
extraction call. You validate the
result. If there are errors, you create
a new messages call with three entries.
The original user prompt with the
document Claude's failed response and a
new user message that lists the specific
errors. Line items total 92, but you
reported 100. tax is null, but the
document shows $7.36
and includes the original document.
Again, the model sees its own failed
attempt and the specific errors. It can
self-correct because it knows exactly
what went wrong.
But retries are not always the answer.
There are two categories of extraction
failures and only one of them retries
can fix.
Format mismatches and structural errors
are retriable.
If Claude extracted a date as 03/172025
instead of the ISO format, it can
reformat that. If it missed a line item
in a table, it can read it again more
carefully. But information absent from
the source is not retriable.
If the document genuinely doesn't
contain a tax field, retrying will
either return null again which is
correct or fabricate a tax amount which
is hallucination and actually worse.
Same with external references see
appendix B but appendix B isn't
provided. The information is elsewhere.
Key insight. Before retrying, check if
the missing information actually exists
in the source.
If it doesn't, accept null.
Here's a pattern that improves your
extraction system over time. The
detected underscore pattern field. Think
of it like a tag on a warning. It tells
you what kind of issue was flagged. For
example, a finding might flag a broad
exception catch at line 45 with the
pattern identifier broad exception
catch. If developers frequently dismiss
these findings, you can analyze the
pattern. Are these false positives?
Should the detection criteria be
improved? This feedback loop improves
the system over time.
For numerical data, there's a powerful
self-correction technique.
Extract both the stated value and the
calculated value, then flag
discrepancies.
Here's a concrete scenario. The document
says total $100,
but the line items only sum to $92.
Your extraction includes stated
underscore total of 100, calculated
underscore total of 92 and conflict
detected set to true. Your validation
code flags this for human review.
Is the document wrong or did the
extraction misalign item? This pattern
catches both extraction errors and
source document errors.
Let's put it all together. Here's the
feedback loop architecture.
First, extract the document into
structured data. Second, validate the
result. If there are no errors, accept
it. If errors are found, ask is this
retryable?
If yes, retry with specific error
feedback, then revalidate.
If the retry passes, accept. If it fails
again, send a human review.
If the error is not retriable, meaning
the information genuinely isn't in the
source, except null or flag for review.
This is the complete loop. Every
extraction should follow this flow.
Now let's cover the anti-atterns.
Number one, retrying when data is absent
from the source. This encourages
hallucination.
If the document doesn't show tax,
retrying will either return null again
or fabricate a value. The correct
approach, check if the information
exists before retrying and accept null
if it doesn't. Number two, generic retry
without specific errors. Saying try
again gives no guidance. The model
doesn't know what to fix. Always include
the specific validation errors in the
retry prompt.
Anti-attern three, no detected
underscore pattern field. Without
pattern identifiers, you can't analyze
false positive trends or improve
detection criteria over time. Always
include them for systematic dismissal
analysis.
Anti-attern four, only checking
structural validity. This misses
semantic errors, totals that don't add
up, dates that are inconsistent.
The JSON is valid, but the data is
wrong. Implement semantic validation by
cross-checking calculations.
Anti-attern five, unlimited retries.
This wastes tokens on unfixable issues.
If the information genuinely isn't in
the source, no number of retries will
find it. The correct approach, max one
to two retries, then send to human
review. And here's the exam anchor. The
exam tests this distinction directly.
Know when to accept null when data is
genuinely missing versus when to retry
when data exists but was missed or
misformatted.
Lock this in.
Here's what you must know for the exam.
One, retry with error feedback means
including the original document, the
failed extraction, and the specific
validation errors in the retry prompt.
Two, retries only work for format
mismatches and structural errors, not
for information absent from the source.
Three, self-correction means extracting
both stated underscore total and
calculated underscore total. Then
flagging conflicts with a conflict
underscore detected boolean.
Four, semantic validation. Do totals
match? Are dates consistent? Is a
separate step from schema validation? Is
the JSON valid? and five max one to two
retries then human review.
Unlimited retries waste tokens on
unfixable issues.
The difference between a brittle
extraction system and a robust one comes
down to this. Does it catch its own
mistakes?
Schema enforcement guarantees valid
JSON.
Validation loops guarantee correct data.
You need both. In the next part, we'll
cover batch processing strategies.
When you need to process thousands of
documents overnight, the message batches
API gives you 50% cost savings.
You'll learn when to use batch versus
synchronous and how to handle partial
failures.
See you in part 24.
