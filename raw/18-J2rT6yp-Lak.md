Welcome back to Zero to Cloud Certified
Architect.
Today, we tackle task 3.5, applying
iterative refinement techniques for
progressive improvement.
In part 17, we covered plan mode versus
direct execution, the two fundamentally
different ways Cloud code can operate
when you give it a task.
Plan mode is for complex tasks,
architectural decisions, multi-file
changes, or when multiple valid
approaches exist.
Cloud explores first, proposes a plan,
and waits for your approval before
editing anything.
Direct execution is for well-scoped
tasks, single-file bug fixes, known
approaches, and changes you can describe
in one sentence.
Cloud starts working immediately, no
planning checkpoint needed.
The explore sub-agent runs investigation
in isolation. Verbose discovery output
stays out of the main conversation,
preserving your context budget for
implementation.
And the best practice, combine both
modes.
Use plan mode to investigate and design,
then switch to direct execution for
implementing each plan step.
If this video helps you prepare for the
exam, please give it a like and
subscribe. It helps others find the
series.
Let's start with the core problem that
iterative refinement solves.
When you give Cloud detailed prose
instructions, convert dates to ISO
format, normalize names to title case,
extract the first email, Cloud may
interpret them inconsistently.
Each run gives you slightly different
results.
The analogy, telling a painter, "Make it
look like autumn," gets you a different
painting every time.
But showing them three photos labeled,
"This is what I want," and three
labeled, "This is NOT what I want," gets
you consistent results.
That difference between prose and
concrete examples is the heart of
iterative refinement.
Now, let's break down concrete input
output examples in two passes, so your
eye can follow the logic.
First, the left panel, the vague prompt.
Extract and normalize contact
information sounds specific, but Cloud
still has to guess formatting rules.
Should it keep titles like doctor?
Should missing values become null or
empty strings?
Should phone numbers stay local or
become international?
This is why vague prose creates unstable
output.
Now, the right panel, concrete examples.
Input one shows full contact data, and
output locks the JSON structure and
international phone format.
Input two includes doctor in the source,
but output removes the title from the
name.
Input three has no name, and output sets
name to null.
Those examples define the transformation
better than a paragraph of instructions.
Next is test-driven iteration, and we'll
review it in two parts, so each step
lands clearly.
Start with the left code block.
You write tests first, then run them
against Cloud's implementation.
Two pass, one fails on the European date
format.
The key move is the failure message.
Expected 2025-03-17,
received 2017-03-25.
That specific diff is what Cloud can
fix.
Then follow the right side loop. Write
tests, implement, share exact failure,
rerun until green.
This works because tests remove
ambiguity.
Instead of saying, "It doesn't work,"
you provide a concrete contract. This
input should produce this exact output.
The third technique is the interview
pattern.
Before implementing a solution in an
unfamiliar domain, you ask Cloud to
interview you.
You say, "Implement a caching layer for
our API responses."
Cloud responds by asking five questions.
What is the cache invalidation strategy?
Time-based, event-based, or manual?
Should cache be per user or shared?
What is the acceptable staleness?
Do we need cache warming on startup?
And on cache failure, do we serve stale
data or hit the origin?
Without this interview, Cloud would
guess on all five questions and
potentially build the wrong thing
entirely.
The interview surfaces design decisions
you must make, and it forces you to make
them before any code is written.
Use the interview pattern whenever you
are implementing in an unfamiliar
domain, or when a task has multiple
valid designs.
The fourth technique is choosing the
right fix strategy based on whether
issues interact.
Interacting issues must be fixed
together.
If a function's type signature changes
from string to string or null, and five
callers depend on that function, fixing
only the signature immediately breaks
all five callers.
You address both issues in a single
message. Update the signature and add
null checks to all five callers at the
same time.
Independent issues can be fixed
sequentially.
Missing error handling in the off module
and unused imports in the billing module
do not interact. Fixing one has no
effect on the other.
You can tackle them one at a time and
review each fix in isolation.
The exam rule, ask yourself whether
fixing issue A changes the correct
behavior of issue B.
If yes, fix them together.
If no, fix them sequentially.
Now, let's apply the same pattern to
edge cases.
Look at the left block first.
Instead of saying, "Handle null values
gracefully," we provide exact input
output pairs.
Null name becomes empty string.
Null age becomes zero.
Empty object maps to both defaults.
This is the executable specification for
edge case behavior.
Then read the right insights. Each
example defines a precise default rule,
and those rules remove ambiguity.
The exam takeaway is simple. Pair broken
input with expected output every time.
Don't describe correctness abstractly.
Show it concretely.
Let's look at the five anti-patterns you
need to recognize for the exam.
Anti-pattern one, only using prose
descriptions.
They are interpreted inconsistently
across runs.
The fix, provide two or three concrete
input output examples.
Anti-pattern two, implementing without
tests.
Without a test suite, you have no way to
verify correctness or catch regressions.
The fix, write the test suite first,
then iterate using failures.
Anti-pattern three, fixing interacting
issues one at a time.
A partial fix breaks other things,
creating cascading failures.
The fix, address interacting issues
together in one pass.
Anti-pattern four, skipping the
interview pattern in unfamiliar domains.
Cloud guesses on all design decisions.
The fix, ask Cloud to interview you
before it writes a single line of code.
Anti-pattern five, vague error reports.
Telling Cloud it doesn't work gives it
nothing to act on.
The fix, share the specific test failure
with the exact expected value and the
exact received value.
Here are the five takeaways you need to
know cold for the exam.
One,
concrete input output examples are more
effective than prose.
They communicate transformation rules
without any ambiguity.
Two,
test-driven iteration. Write tests
first, run them, share failures with
Cloud, iterate until all pass.
Three,
the interview pattern. Have Cloud ask
clarifying questions before implementing
in unfamiliar domains.
Four,
interacting issues must be fixed
together in one message.
Independent issues can be fixed
sequentially.
Five,
specific test cases with expected output
are the most effective way to fix edge
case handling.
That wraps up task 3.5.
You now know how to apply iterative
refinement techniques for progressive
improvement.
We covered why prose instructions
produce inconsistent results.
Concrete input output examples as the
primary tool for communicating
transformations.
Test-driven iteration. Write tests,
share failures, iterate to green.
The interview pattern for surfacing
design decisions before implementation.
In part 18, we will look at how to
integrate Cloud Code into CI/CD
pipelines, including the non-interactive
-p flag, structured JSON output, and
GitHub Actions integration.
