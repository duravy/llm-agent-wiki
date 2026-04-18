Welcome back. In part 16, we covered
path specific rules. Quick recap before
we move on. First, rule files and
do.claude/ruules
slash use a yaml from matter paths
field. When you edit a file, claude
checks whether it matches any of those
glob patterns and only loads the rule if
it does.
Second, glob patterns like
doublestar/star.est.star
star or src/appi/
doublestar slashstar give you precise
control rules load only for matching
files never for unrelated parts of the
codebase
third path specific rules are better
than subdirectory clawad md for
crosscutting conventions
test files are spread throughout the
codebase one rule file with the right
pattern catches all of them fourth Token
savings. Only the conventions that match
the file you're editing load into
context.
API rules don't consume space when
you're editing a React component.
Fifth, the exam signal. When you see
conventions that span many directories
like test files, the answer is path
specific rules. When you see an isolated
monorreo package, the answer is a
subdirectory cla.md.
That wraps our part 16 recap.
If this video helped you prepare for the
exam, please give it a like and
subscribe.
In this part, we're covering task 3.4 of
domain 3. When to use plan mode versus
direct execution.
Claude code can operate in two
fundamentally different ways. Direct
execution means Claude reads your
request and immediately starts making
changes. No planning step, no waiting.
Plan mode means Claude first explores
the codebase, analyzes the options,
proposes a plan, and waits for your
approval before touching a single file.
The analogy is renovation.
If you say paint that wall blue, the
painter picks up a brush and starts. The
scope is clear. If you say redesign this
room, the designer surveys the space,
sketches layouts, and gets your sign off
before lifting a hammer. By the end of
this part, you'll know exactly when each
mode is appropriate.
Plan mode is designed for four types of
tasks.
First, multiple valid approaches. When
there's no obvious single answer, like
choosing between microservices or a
modular monolith, you need Claude to
explore the options before committing to
one. Second, large scope changes that
touch many files carry a real risk of
discovering problems halfway through,
which forces costly undo work. Third,
architectural decisions. choosing
between integration patterns with
different infrastructure requirements.
Fourth, risk of expensive rework. If
picking the wrong approach means hours
of undoing, the upfront cost of planning
pays for itself immediately.
Let's make this concrete with
microservices restructuring.
Without plan mode, Claude would start
splitting files immediately, then
discover dependency problems halfway
through. It has to undo partial changes
and restart. You get costly rework and a
broken intermediate state. With plan
mode, Claude first explores the code
base and maps the dependencies.
It identifies the natural service
boundaries, proposes a phased
restructuring plan, and then stops. You
review and adjust before a single line
of code changes. Only after you approve
does Claude execute the plan. The key
difference is that human review
checkpoint. It catches design problems
before they become implementation debt.
Direct execution is the right choice for
four types of tasks.
First, well-defined changes where the
task is specific and there are no
architectural choices to make like
adding input validation to a function.
Second, single file or small scope. A
bug with a clear stack trace pointing to
one location. Third, no architectural
decision. Adding a date validation
conditional has one obvious
implementation.
Fourth known approach. Converting a call
back to async/await
is a mechanical transformation.
The rule of thumb is simple. If you can
describe the change in one sentence and
there's only one obvious way to do it,
use direct execution.
If you need to discuss options first,
use plan mode.
Plan mode uses an explore sub agent for
investigation. And this is worth
understanding. The explore sub agent
reads files, searches code, and maps
dependencies all in its own separate
context. The key benefit is isolation.
When Claude reads 45 files and runs 200
GP searches to build the dependency
graph, all of that verbose output stays
inside the explore sub agents context,
not the main conversation.
Your main conversation only sees the
clean, concise plan that comes out of
that investigation.
Without the explore sub agent, all that
discovery would flood your main context
window, consuming your token budget
before implementation even starts. This
is how plan mode keeps the main
conversation focused and preserves
context for the actual work.
The most effective approach in practice
is combining both modes. You use plan
mode for the investigation phase, then
switch to direct execution for
implementing each step. Here's a real
example. Migrating from express to
fastify.
Phase one is plan mode. You ask Claude
to plan the migration. It explores all
route files and middleware, maps the
dependencies, and proposes a phased
migration plan. You review and approve.
No code has changed. Phase two is direct
execution one step at a time. Execute
step one, update package.json
and install fastify. Execute step two,
convert the main server file. Execute
step three, migrate route handlers. Each
direct execution step is well scoped
because the plan already identified
exactly what to change.
Here's the decision framework.
Start by asking is the change well-
definfined and single file? If yes, use
direct execution. You're done. If no,
ask are there multiple valid approaches?
If yes, use plan mode. If no, ask does
it touch many files? If yes, use plan
mode. If no, use direct execution.
This framework covers the exam
scenarios.
A monolith to microservices
restructuring fails the first two checks
and hits large scope plan mode. Adding a
validation check passes the first check,
direct execution.
The goal is to match the mode to the
task's complexity, not to default to one
mode for everything.
Five anti-atterns to avoid.
First, using direct execution for an
architectural change. You risk
discovering problems mid-execution and
having to undo everything. Use plan mode
first. Second, using plan mode for a
single file bug fix. That's unnecessary
overhead. The scope is already clear.
Third, starting over when complexity is
discovered mid execution.
This is avoidable by starting with plan
mode when you suspect the task is
complex.
Fourth, running plan mode without
actually reviewing the plan. The human
review checkpoint is the entire point.
Always review and adjust before
approving.
Fifth, skipping the explore sub agent
for large investigations.
If you let discovery output fill the
main conversation, you'll consume your
context budget before implementation
starts.
Here's what you need for the exam. One,
plan mode is for architectural
decisions, multifile changes, multiple
valid approaches, and library
migrations.
Two, direct execution is for single file
bug fixes, well-defined changes, and
known approaches.
Three, the explore sub agent runs
investigation in isolation. Verbose
discovery output stays out of the main
conversation context.
Four, combine both modes. Plan mode for
investigation, then direct execution for
implementing each plan step. Five. When
the exam describes a monolith to
microservices restructuring or a large
migration, the answer is plan mode
first. When it describes adding a
validation check or fixing a bug with a
clear stack trace, the answer is direct
execution.
Six. Plan mode's value is the human
review checkpoint. Always review and
adjust before approving. In part 18, we
move to iterative refinement where
you'll learn how concrete examples,
test-driven iteration, and the interview
pattern produce consistent, highquality
output. See you there.
