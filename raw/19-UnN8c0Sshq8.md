Welcome back.
In part 18, we covered iterative
refinement, practical techniques for
improving Claude's output through
concrete examples, test-driven
iteration, and the interview pattern.
Let's do a quick recap before diving
into today's topic.
First, concrete input and output
examples beat prose descriptions.
When you give Claude two or three
examples of what you want, you get
consistent results.
When you write a paragraph describing
the transformation, you get different
results every time.
Second, test-driven iteration.
You write your test suite first, then
ask Claude to implement.
You run the tests, share the failures
back to Claude, and iterate until
everything passes.
Test failures give Claude specific,
unambiguous feedback about what went
wrong.
Third, the interview pattern.
Before implementing in an unfamiliar
domain, have Claude ask you clarifying
questions.
This surfaces design decisions about
caching strategy, error handling,
concurrency, things you'd otherwise have
Claude guess on.
Fourth, when fixing multiple issues,
check whether they interact.
Interacting issues, where fixing one
breaks another, should be addressed
together in one message.
Independent issues can be fixed one at a
time.
And fifth, to fix edge case handling,
provide specific test cases with
expected output.
The concrete inputs and expected values
eliminate ambiguity about what correct
behavior should be.
That wraps our part 18 recap.
If this video helped you prepare for the
exam, please give it a like and
subscribe.
In this part, we're covering task 3.6 of
domain three, integrating Claude code
into CI/CD
pipelines.
CI stands for continuous integration and
CD for continuous deployment.
This is about running Claude as an
automated worker in your pipeline, not
just as an interactive assistant.
By the end of this part, you'll know how
to run Claude non-interactively, get
structured output for automated
processing, and make sure your CI
reviews are consistent, unbiased, and
non-redundant.
CI/CD
is an automated pipeline that runs every
time code changes. It runs tests, checks
quality, and deploys.
Think of it as an assembly line for
code. Push your changes, and the
pipeline automatically checks, reviews,
and ships them.
The problem is that Claude code is
designed for interactive use.
It expects a human at the keyboard.
Running it in a pipeline as is just
hangs the job. The pipeline waits
forever for input that never comes.
The solution is the minus P flag.
The minus P flag, or dash dash print,
makes Claude code run non-interactively.
Without minus P, running Claude in CI
hangs the job.
It times out waiting for input that
never arrives.
With minus P, Claude processes the
prompt, outputs the result to standard
output, and exits.
You can pipe that output directly to
other tools, like posting it as a pull
request comment with the GH CLI.
This is the foundational flag for all
CI/CD
use of Claude code.
You must use it in every pipeline
invocation.
In CI, you often need machine-parsable
output that other tools can process.
The dash dash output format JSON flag
tells Claude to return structured JSON
instead of prose.
For tighter control, add dash dash JSON
schema and pass a JSON schema. Claude
will return output matching your schema
exactly.
This lets you extract fields like file,
line number, severity, and message, then
post them as inline pull request
comments, update a dashboard, or trigger
conditional logic based on severity.
Structured output turns Claude into a
proper CI component.
When Claude code runs in CI, it reads
the project's Claude.md
just like an interactive session.
This means your testing standards,
review criteria, and conventions
automatically apply to CI-invoked
reviews.
You can document things like flag
functions without error handling as high
severity, do not flag code style issues
since those are handled by linters, use
Vitest for tests, and follow existing
patterns in the test directories.
This is how you get consistent review
criteria between interactive and
automated reviews.
Here's a critical insight.
The same Claude session that generated
code is less effective at reviewing it.
The model retains its reasoning context
and is less likely to question its own
decisions.
Ask a session to review code it just
wrote, and it tends to say it looks good
because it remembers why it made every
decision.
CI-based reviews avoid this problem
entirely.
A fresh pipeline session has no memory
of the generation.
It reviews the code with fresh eyes,
which makes it far more likely to catch
real issues.
When CI runs reviews on updated commits,
it may reflag issues that were already
reported.
These duplicate comments annoy
developers and erode trust in the
automated review.
The solution is to include prior
findings in your prompt and instruct
Claude to report only new issues.
You paste in the list of already
reported findings and say, "Do not
re-report these."
The same principle applies to test
generation.
If you include the existing test file in
context, Claude focuses on uncovered
edge cases instead of duplicating what's
already there.
Let's cover the six anti-patterns to
avoid.
One, running Claude without minus P in
CI, it hangs.
Two, having the same session generate
and review code, the model is biased
toward its own decisions.
Three, no prior findings in reviews, you
get duplicate comments on every commit.
Four, no Claude.md for CI context, your
review criteria aren't applied.
Five, text output in CI, it's hard to
parse programmatically, use JSON format
instead.
Six, not providing existing tests when
generating new ones, Claude duplicates
coverage instead of filling gaps.
Let's summarize what you need for the
exam.
One, the minus P flag runs Claude
non-interactively, required for all
pipeline use.
Two, dash dash output format JSON with
optional dash dash JSON schema produces
structured findings for automated
processing.
Three, Claude.md provides consistent
context to both interactive and CI
sessions.
Four, session isolation, CI reviews are
always fresh, free from the generator's
bias.
Five, include prior findings on reviews
to prevent duplicates.
Six, include existing tests when
generating new ones to avoid duplicate
coverage.
In part 20, we move into domain four and
cover prompt design and explicit
criteria.
See you there.
