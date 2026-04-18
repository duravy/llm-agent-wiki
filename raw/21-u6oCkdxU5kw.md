Welcome back. In part 20, we covered
prompt design and explicit criteria.
Let's do a quick recap before we move on
to task 4.2.
First, explicit criteria beat vague
instructions every time. When you tell
Claude exactly what to flag, like
comments that contradict the actual
code, you get real issues. When you just
say check comments are accurate, you get
everything and nothing at once. The exam
tests this distinction directly.
Second, categorical definitions work
better than subjective thresholds.
High confidence means different things
each run. A list of specific issue types
with concrete code examples produces
consistent results every time.
Third, severity levels need concrete
code examples.
Without examples showing what a critical
bug looks like versus a medium one,
Claude classifies severity
inconsistently across reviews.
Show the code. Don't just name the
category.
Fourth, define what to skip as
explicitly as what to report. Style
issues belong in llinters, not code
reviews.
Telling Claude what to ignore is just as
important as telling it what to flag.
Fifth, one bad category undermines trust
in all categories.
A high false positive rate in unused
variables makes developers ignore the
accurate null dreference findings too.
The fix is to disable the noisy
category, improve the detection
criteria, and reenable it later.
If this series is helping you prepare
for the exam, please give it a like and
subscribe. Every like helps more
developers find this content.
Here's a fundamental technique the exam
tests repeatedly.
There are two ways to make Claude
produce consistent output and only one
of them actually guarantees it. Today
we're covering task 4.2 fuch prompting.
By the end of this video, you'll know
what fshot prompting is, why it beats
instructions alone for ambiguous tasks,
and how many examples to use without
wasting context tokens.
Let's start with the basics. Few
prompting means including two to four
example input and output pairs in your
prompt so Claude can see the pattern
before processing new data. Think of it
like teaching someone to sort mail.
Zero shot says, "Sort these into the
right bins." Few shot says, "Watch me
sort three letters first." This one goes
to billing because it has an invoice
number. This goes to personal because
it's from a friend. This goes to spam
because it's unsolicited. Now you sort
the rest. The examples communicate
implicit rules you would struggle to
describe in words.
Here's why instructions alone often
fail.
Say you tell Claude, "Route customer
requests to the appropriate tool." A
customer writes, "Check on my order."
Does Claude call get customer lookup
order, or search orders? The instruction
doesn't clarify ambiguous cases. Now
watch what happens with few shot
examples.
Example one. Check my order number 1 2 3
4 5. Claude calls lookup order with that
order number. Example two, check on my
order with no number. Claude calls get
customer first to identify who's asking.
Example three, check orders for John at
email.com.
Claude calls get customer with that
email. See the difference? The examples
teach Claude the reasoning pattern, not
just which action to take, but why that
action was chosen. Claude then
generalizes this reasoning to novel
inputs it has never seen before.
That's the power of fshot prompting.
Fewshot examples are also the most
reliable way to force consistent output
format. If you write return findings in
JSON with location, issue, severity, and
suggested fix, the format drifts across
runs.
Sometimes fields are missing. Sometimes
extra fields appear. But if you show one
complete example, location, issue text,
severity level, suggested fix, and even
a detected pattern field, Claude
produces output matching that exact
structure every time. The detected
pattern field is especially important.
A pros instruction might not convey it
clearly, but a single example makes it
obvious. Show the format, don't describe
it.
Now, here's where few shot examples
become truly powerful. Teaching judgment
for gray areas. These are cases where
the correct action is nonobvious.
Take code review as an example.
If you see if user question mark.name,
name. That's optional chaining,
intentional defensive coding. Skip it.
But if you see if user name and the type
definition says user can be null, that's
a genuine bug. It will throw a type
error at runtime.
Flag it. And here's a tricky one. Catch
E with an empty block and a comment
saying intentionally empty. That looks
like a bug, but the comment tells you
it's acceptable.
Skip it. These examples teach Claude to
distinguish acceptable patterns from
genuine issues, enabling it to
generalize to similar but not identical
code patterns.
Fewshot examples are equally valuable
for document extraction. When you're
pulling data from varied document
formats, examples show claude how to
handle structural differences.
An inline citation like Smith at all
2024 gets extracted as an inline
citation type. A bibliography reference
like reference bracket 3 bracket gets
resolved to the bibliography entry and
extracted as a bibliography reference
type. But here's the critical one. What
if there's no source at all? Revenue
increased by 15% year-over-year. No
citation, no reference.
The example teaches Claude to return
null for the source field instead of
fabricating one.
That's a behavior that's incredibly hard
to describe in pros, but instantly
obvious from a single example.
So, how many examples should you use?
More examples means more context tokens
consumed. So, you need to balance
precision with cost.
Two to three examples is the sweet spot
for most tasks. Cover the common cases
and include one edge case. Four to five
examples when you're in an ambiguous
domain with many gray areas. One example
is only useful for basic format
demonstration. It's not enough to teach
judgment patterns. And zero examples,
zero shot, should only be used when the
task is completely unambiguous, which is
rare.
The anti-attern to avoid using 10 or
more examples that wastes context tokens
with diminishing returns.
Instead, make your two to four examples
structurally diverse. Different formats,
edge cases, and null handling.
Let's cover the five anti-atterns to
avoid. One, only showing happy path
examples.
Claude won't know how to handle edge
cases.
Include at least one edge case and one
null or missing example. Two examples
without reasoning. Claude copies the
action but not the judgment. Always
include why each action was chosen.
Three. Too many examples.
10 or more wastes context tokens. Two to
four targeted examples is enough. Four,
examples that all look the same. Claude
doesn't learn to differentiate.
Make them structurally diverse. And
five, relying on pros instructions for
output format. Format drifts across
runs. Show the exact output format in
your examples instead.
Here's what you must know for the exam.
First, fshot examples are the most
effective technique for achieving
consistent output when instructions
alone produce inconsistent results.
Second, include reasoning why this
action was chosen. So, Claude
generalizes the judgment pattern, not
just the action. Third, show edge cases
and ambiguous scenarios because that's
where few shot examples add the most
value.
Fourth, null and missing field handling
must be demonstrated via examples.
Return null, not fabricated values.
Fifth, two to four targeted examples is
the sweet spot covering common cases,
edge cases, and format.
And sixth, fshot examples enable
generalization to novel patterns, not
just matching the specific examples
shown. In the next video, part 22, we'll
cover structured output in JSON schemas.
How to use tool used to eliminate syntax
errors entirely and handle missing
information without hallucination.
If this video helped you prepare for the
exam, please like and subscribe and I'll
see you in part 22 for structured output
and JSON schemas.
