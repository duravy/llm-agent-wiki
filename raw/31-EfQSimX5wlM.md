Welcome back. In part 30, we covered
human review workflows, field level
confidence scores, calibrated
thresholds, and stratified sampling.
Quick recap before we move on. First,
field level confidence scoring. When you
score each extraction individually
instead of the whole document, you can
route only the uncertain fields to human
reviewers, saving time and money.
Second, calibrate your thresholds.
A model saying 95% confidence does not
mean it is actually right 95% of the
time. You need labeled validation sets
to check. Third, stratified sampling.
When you oversample rare and difficult
document types, you catch problems where
they are most likely to hide instead of
missing them with random sampling.
Fourth, route by confidence.
Auto accept the high confidence
extractions. Send medium confidence to
human review and flag low confidence as
failed. Don't guess. And fifth, feedback
loops. Every human correction should
feed back into your calibration data and
your extraction prompts. That is how the
system gets better over time.
If this video helped you prepare for the
exam, please give it a like and
subscribe.
Think of information provenence like a
journalist's citations.
The exam tests this directly because if
your synthesis agent loses source
attribution, the final report stops
being verifiable.
First, why source attribution disappears
during summarization?
Second, preserve claim source mappings
at every level so every claim stays
attached to its source.
Third, handle conflicting sources
without picking a winner arbitrarily.
Fourth, require publication dates so
newer evidence is not confused with
older evidence.
And fifth, structure the final report by
confidence level so the reader knows how
much trust to place in each claim.
Here is the problem. Each level of
summarization strips away source details
until nothing is verifiable.
At level one, the web researcher still
has the exact source date and number. So
the finding is specific and verifiable.
At level two, the coordinator compresses
the finding and the source name, date,
and exact number disappear.
At level three, the synthesis agent
writes a polished sentence with no
attribution at all. That is attribution
loss during compression.
The solution is structured claim source
mappings.
Think of this like a bibliography that
travels with every finding. You require
every sub agent to output a JSON object
that pairs each claim with its source
metadata. The source name, the URL, the
publication date, a direct excerpt, and
the methodology.
The synthesis agent receives these
structured mappings and must preserve
them when combining findings into the
final report. The claim stays attached
to its source at every level. Nothing
gets compressed away.
This is the single most important
technique for preventing attribution
loss.
Now, what happens when two credible
sources report different numbers?
Nature medicine says 94%.
Jamba says 87%.
The wrong approach is to pick one and
ignore the other. The right approach is
to annotate the conflict. Present both
values with their source context.
Explain why they might differ. In this
case, Jamba used community hospital data
while Nature Medicine used academic
centers. The reader can then evaluate
both claims with full context.
Never resolve a conflict by arbitrarily
choosing a winner that misleads the
reader and destroys trust in the report.
Publication dates matter because without
them a real trend can look like a
contradiction.
Without dates, 87% and 94% look like two
incompatible answers.
With dates, the same numbers become a
timeline. 87% in 2024, then 94% in 2025.
That is the exam anchor. Always require
publication dates in subagasan outputs.
The final report should separate
findings by confidence so the reader can
tell what is well supported, what is
contested, and what still has gaps.
Start with wellestablished findings
where multiple credible sources
independently support the same claim.
Then mark contested findings where
credible sources disagree and you need
to preserve both values with context.
Finally, call out coverage gaps where
the evidence is too thin to make a
confident claim.
Different types of data should be
rendered in their natural format. Think
of it like choosing the right tool for a
job. You would not write a recipe as a
spreadsheet and you would not present
quarterly revenue as a paragraph of
pros. Financial data belongs in tables.
News findings work best as pros
narratives. Technical findings are
clearest as structured lists.
Statistical comparisons need sidebyside
tables with methodology notes.
Converting everything to bullet points
loses the natural clarity of each
content type. The exam tests whether you
know that uniform formatting is an
anti-attern.
Now let us cover the five most common
mistakes that destroy provenence in
multi-agent systems. The exam tests
these directly. So learn each failure
pattern and its fix.
Mistake one, summarizing findings
without claim source mappings.
Mistake two, picking one source when
credible sources conflict.
Mistake three, omitting publication
dates.
Mistake four, forcing every content type
into the same format.
Mistake five, failing to distinguish
wellsupported findings from contested
ones.
Anti-attern number one, summarization
without claim source mappings.
What goes wrong is that sub aents
summarize findings into pros without
attaching source metadata. The synthesis
agent receives vague statements like AI
is transforming health care with no way
to verify which study said what. The
consequence is that attribution is
permanently lost.
The final report contains unverifiable
claims that look authoritative but
cannot be traced back to any source.
The fix is to require structured claim
source mappings from every sub aent
source URL, name, date, excerpt, and
methodology.
Make this a hard output contract, not a
suggestion.
Anti-attern number two, picking one
source when sources conflict.
What goes wrong is that two credible
sources report different numbers and the
synthesis agent arbitrarily picks the
higher or more recent one while silently
dropping the other. The consequence is
that contradictions get hidden from the
reader. The report appears more certain
than the evidence supports, which is
misleading and potentially dangerous for
decision-making.
The fix is to annotate conflicts with
both values and source context.
Explain why they might differ.
Methodology, time frame, sample size.
Let the reader evaluate.
Anti-attern number three, omitting
publication dates.
What goes wrong is that subbasagen
outputs include findings but no
publication dates. The synthesis agent
treats a 2020 study and a 2025 study as
equally current. The consequence is that
temporal differences get misread as
contradictions.
AI accuracy is 75% and AI accuracy is
94% look like a conflict until you
realize one is from 2022 and one from
2025.
The fix is to always require publication
dates and structured output. Dates turn
apparent contradictions into trends.
Anti-attern
number four, uniform format for all
content types.
What goes wrong is that the synthesis
agent converts everything, financial
data, news, timelines, technical specs
into the same bullet point format. The
consequence is that you lose the natural
clarity of each content type. Financial
comparisons are harder to read as
bullets than as tables.
News narratives lose their flow when
reduced to bullet points. The fix is to
render each content type in its natural
format. Tables for financial data, pros
for news narratives, structured lists
for technical specs, and sidebyside
tables for statistical comparisons.
Anti-attern number five, no confidence
distinction.
What goes wrong is that the final report
presents all findings with equal weight.
A claim backed by five peer-reviewed
studies sits right next to a claim from
a single unreed preprint. The
consequence is that the reader cannot
assess which findings are solid and
which are speculative.
This undermines the entire purpose of
multisource research.
The fix is to structure output by
confidence level, wellestablished
findings, contested findings, and
coverage gaps. Each section tells the
reader how much trust to place in the
claims.
The exam question about provenence is
simple. Can a human reader verify every
claim in the final report? If not,
providence was lost somewhere in the
pipeline.
If the exam asks what preserves
attribution, the answer is structured
claim source mappings, source URL, name,
date, excerpt, methodology.
If it asks how to handle conflicts, the
answer is to annotate both values with
context. Never pick one source
arbitrarily.
If it asks about dates, they prevent
temporal differences from being misread
as contradictions.
If it asks about output structure,
distinguish wellestablished contested
and coverage gap findings.
If it asks about rendering, each content
type gets its natural format. Tables for
data, pros for narratives, lists for
specs.
Lock these in for the exam.
Claim source mappings must survive every
level of synthesis from web researcher
to coordinator to final report.
Conflicting sources get annotated with
both values and methodology context
never resolved by picking a winner.
Publication dates turn apparent
contradictions into trends. Always
require them in subagent outputs.
Synthesis output must distinguish
wellestablished, contested, and coverage
gap findings.
And content type rendering means
financial data as tables, news as pros,
technical data as lists.
Do not force uniformity.
The core principle is this. Preserve
providence at every stage so the final
output can be verified by a human
reader. Every claim needs a source.
Every source needs a date.
Every conflict needs context.
If any of these three is missing, the
synthesis has failed, no matter how
polished the final report looks.
Up next, part 32, sample questions and
answers. We will walk through 12 exam
style questions with detailed
explanations covering enforcement versus
prompts, tool descriptions, escalation,
calibration, plan mode, CI/CD
integration, batch processing, and
multipass reviews. Part 32 is your
chance to test everything you have
learned so far.
See you there.
