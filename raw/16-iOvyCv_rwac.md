Welcome back to the zero toclaude
certified architect series. Today we're
covering task 3.3 path specific rules.
In part 15, we covered slash commands in
the.claude/comands
directory team shortcuts committed to
get and shared automatically with every
developer.
Skills use skill.md files with YAML from
matter to configure context allowed
tools and argument hints.
Context colon fork runs the skill in an
isolated sub aent keeping verbose output
out of the main conversation.
Allowed tools and forces least
privilege. Readon skills should never
get right or bash access.
and the key decision rule. Use skills
for ondemand task workflows and claw.md
for universal standards that must always
be loaded. If this video helps you
prepare for the exam, please give it a
like and subscribe. It really helps more
learners find this series.
Today we tackle path specific rules.
We'll look at the one-sizefits-all
problem, YAML paths, front matter, glob
pattern routing, token savings, and when
to use paths versus subdirectory claw.md
files.
Let's start with the problem that path
specific rules solve. Most code bases
have completely different conventions
for different areas. React components
use functional style and hooks.
API handlers use async await with zod
validation and test files have their own
assertion patterns. If all of these live
in one monolithic claude.m MD, all of
them load for every single file you
edit.
This creates three compounding problems.
First, wasted tokens. API rules consume
context when you're editing a React
component.
Second, irrelevant guidance. Claude
might apply database patterns to
front-end code. And third, the behavior
is non-deterministic.
Claude is inferring which section
applies, not matching it explicitly.
The solution is conditional loading
using the paths field in YAML front
matter. Instead of one giant file, you
create modular rule files in the
do.claude/ruules directory.
For example, API conventions.mmd.
At the very top, you add YAML front
matter with a paths field. That paths
field contains glob patterns. The rule
file only loads into context when Claude
is actively editing a file that matches
one of those patterns.
Editing a React component. The API rules
are simply never loaded. If you omit the
paths field entirely, the rule loads for
every file. The same as putting it in
claude.md directly. Always add a paths
key to scope your rule files.
This is actually the key advantage of
rule files over add import. The add
import directive always loads both files
unconditionally.
Rule files with a paths key load
conditionally only when relevant.
Let's look at common glob patterns and
how they work. Doublestar/star.est
star catches all test files in any
directory with any extension test.ts
test.tsx.test.js.
Double star matches any number of
directory levels.
Star matches any file name within a
level. Pattern two,
src/appi/doublestar/star
matches all files inside the API
directory and any subdirectories.
Pattern three, doublestar/star.tsx
matches all React component files
anywhere in the repo. You can also
provide multiple patterns in the same
array. A rule loads if any pattern
matches. This is useful for API handlers
and middleware that share the same
conventions.
The exam note here is important.
Doublestar/star
test star matches test files in every
directory. One rule file covers your
entire test suite.
This brings us to the key advantage of
path specific rules. solving
crosscutting concerns.
Test files live next to the code they
test, spread throughout your entire
codebase.
Button.test.tsx
sits next to button.tsx.
Handlers.est.tsx
sits next to handlers.ts.
They're in components API, billing,
utils, everywhere.
Without path specific rules, you need a
claude.md in every single directory
containing test files. Duplicate content
a maintenance nightmare.
With one path specific rule using paths
doublestar/star
testar, you catch all test files in the
entire codebase.
One file, zero duplication.
Now let's talk about the architectural
decision when to use pathsp specific
rules versus subdirectory cla.md files.
A subdirectory cla.md is best for
monorreo packages with a distinct
codebase like packages/billing/cloud.md.
It applies to everything inside that
directory and its subdirectories.
It is locationbound.
Path specific rules are best for
conventions that span directories like
all test files, all React components or
all Terraform files regardless of where
they live. They are patternbound, not
locationbound.
Memorize this exam rule. If the question
mentions conventions for files spread
across many directories, the answer is
path specific rules. If it mentions a
specific bounded package, the answer is
subdirectory cla.md.
Let's look at the token savings impact.
To make this concrete
without path specific rules when you
edit button.tsx,
five rule sets load into context.
React conventions are the only relevant
one. API, database, testing, and
Terraform conventions are all wasted
tokens. Roughly 80% of loaded context is
irrelevant.
With path specific rules, editing
button.tsx only loads the React
conventions file because it's the only
one whose paths field matches
doublestar/star.tsx.
The other four rule files don't match.
They simply aren't loaded. 100% of
context is relevant.
Let's review the four anti-atterns you
must avoid.
Anti-attern one, using subdirectory
cla.md for crosscutting concerns.
Test files live everywhere. You can't
put a claw.md in every directory. Use
path specific rules with a glob pattern
instead. Anti-attern two. No paths field
on your rule files. Without a paths key,
the rule loads for every file, which is
identical to putting it in claude.md.
Always add paths front matter to scope
the rule. Anti-attern three, overly
broad glob patterns.
Setting paths to double star/star loads
for every file. It completely defeats
the purpose. Be specific. source/api/
doublestar/star
not just double star/star
anti-attern for relying on claude to
infer which section applies
inference is not pattern matching you
cannot trust claude to apply the right
conventions if you don't use explicit
path scoping always use deterministic
rule loading
here are the five takeaways you need to
know cold for The exam one files
include/ rules with a paths yaml from
matter field enable conditional
convention loading rules only activate
for matching files.
Two glob patterns control when rules
load doublestar/star.est.star
matches all test files.
source/appi/doublestar
slashstar matches all API files.
Three path specific rules beat
subdirectory claw.md for crosscutting
concerns.
Test files are spread everywhere not
confined to one directory.
Four token savings only relevant
conventions load keeping the context
window focused and reducing noise.
Five exam signal. Whenever you see the
phrase conventions for files spread
across many directories, the answer is
always path specific rules, not
subdirectory claw.md.
That wraps up task 3.3.
You now know how to apply path specific
rules for conditional convention
loading. We covered YAML paths front
matter, glob pattern routing, context
optimization, and token savings,
and how path specific rules solve
crosscutting concerns that subdirectory
files cannot.
In part 17, we'll look at plan mode
versus direct execution, including when
to enforce plan mode and how to bypass
it for trivial fixes. See you there.
