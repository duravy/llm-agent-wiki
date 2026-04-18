Welcome back. In part 19, we covered
CI/CD
integration.
Quick recap before we move on. When you
give Claude code the minus P flag in a
pipeline, it runs non-interactively and
exits.
Without it, the pipeline hangs waiting
for a human who never shows up.
Structured JSON output with optional
JSON schemas lets your CI scripts parse
findings and post them as inline PR
comments automatically.
Claude.md provides review criteria and
testing standards automatically. So CI
invoked sessions follow the same rules
as interactive sessions.
Session context isolation means a fresh
review session catches issues the code
author missed because a fresh session
has no reasoning bias.
Include prior findings in rear reviews
to avoid duplicate comments that annoy
developers and erode trust in the
automated review system.
If this video helped you prepare for the
exam, please give it a like and
subscribe.
Welcome to domain 4, prompt engineering
and output quality. You already know
what claude code can do. Now, let's
learn how to get reliable, consistent
results every time. This domain covers
the techniques that separate guesswork
from precision. By the end of these
parts, you'll know how to design prompts
with explicit criteria, apply few shot
examples for ambiguous cases, use
structured output with JSON schemas, and
build validation and retry loops for
reliable automated workflows.
Let's start with task 4.1, prompt design
and explicit criteria. The building
block you need to understand is the
false positive. This is when the system
flags something as a problem that isn't
actually a problem. Think of it like a
smoke detector that goes off every time
you make toast. After a few false
alarms, you stop trusting it. And when
there's a real fire, you ignore it.
That's exactly what happens with false
positives in automated code reviews.
Developers stop trusting the tool and
they miss real bugs. In code review,
that means reporting a bug that's
actually correct code. In data
extraction, it means extracting
information that isn't really there.
Here's a critical insight. General
instructions sound reasonable but don't
work. Explicit criteria transform
results. Say check that comments are
accurate and claude flags everything
that could possibly be inaccurate.
80% of findings are false positives and
developers ignore all of them including
the real bugs but say flag comments only
when the comments claim behavior
contradicts the actual code and claude
flags only genuine contradictions.
90% of findings are real issues and
developers trust and act on them. For
example, flag when a comment says
returns null on error but the code
throws an exception or when a comment
says validates email format but no
validation exists.
Do not flag outdated parameter names,
missing comments or style issues.
This brings us to a decision rule that
the exam tests directly.
Instructions like be conservative or
only report high confidence findings
fail because Claude decides what counts
inconsistently each run. What counts as
high confidence?
Claude decides differently each time and
results vary wildly. The fix is
categorical definitions.
Instead of saying high confidence, list
exactly what to report, null pointer to
reference, uncaught async errors, and
SQL injection. And list exactly what to
skip, code style, minor naming, and
missing documentation.
Categorical definitions produce
consistent results.
Subjective thresholds produce
inconsistent results. Know the
difference.
When defining severity levels, provide
code examples for each level so Claude
classifies consistently.
Critical means data loss or security
vulnerability like user input directly
in ASQL query without parameterization.
High means a functional bug that affects
users like a function returning
undefined instead of the expected error
object.
Medium means a code quality issue that
may cause future bugs like catching
exception broadly instead of specific
error types and low means style or
convention issues. Do not report these.
They are handled by llinters. Each
severity level has concrete code
examples and Claude can match new code
against these examples to classify
consistently.
Let's cover the five anti-atterns that
undermine prompt precision. Getting any
of these wrong is a common exam
question. First, saying be conservative
is subjective.
Claude decides inconsistently.
Define explicit categories instead.
Second, only report high confidence
findings means different things each
run. List specific issue types with
concrete code examples.
Third, reporting all possible issues
floods developers with false positives
and destroys trust. Define what to skip
as explicitly as what to report. Fourth,
keeping high false positive categories
undermines trust in accurate categories.
Temporarily disable them, improve
criteria, then reenable.
And fifth, no severity examples leads to
inconsistent classification across
reviews.
Provide code examples for each severity
level.
Let's lock in what you need to know.
Explicit criteria outperform vague
instructions every time. Categorical
definitions work better than subjective
thresholds.
Concrete code examples for each severity
level ensure consistent classification.
Define what to skip as explicitly as
what to report because reducing false
positives is just as important as
catching real issues. And remember, high
false positive rates destroy trust
across all categories.
Disable problematic categories and
improve them before reenabling.
The exam theme here is precision over
recall. It's better to report fewer
accurate findings than many findings
with high false positive rates. In part
21, we'll cover few shot prompting where
two to four targeted examples teach
Claude the reasoning pattern, not just
the action, but why that action was
chosen.
