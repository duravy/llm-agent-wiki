Welcome back. In part six, we covered
SDK hooks, how to intercept the agent
loop at pre-tool use and post tool use
events to validate, transform, or block
tool calls before they execute. We
looked at data normalization at the hook
layer, stop decisions, and audit logging
patterns. Today, we go deeper into
domain one, task 1.6, task decomposition
strategies.
Specifically, when do you use a fixed
sequential pipeline versus an adaptive
plan that evolves as you go? Let's dive
in.
Task 1.6 is all about one fundamental
question. How do you decide how to break
a task apart? The first approach is
prompt chaining. A fixed sequential
pipeline where you define all the steps
up front. Step one feeds step two. Step
two feeds step three and the sequence
never changes. You use this when the
task is predictable and repeatable and
every run follows the exact same
structure.
The second approach is dynamic
decomposition where the plan evolves
based on what you discover at each step.
You start with a rough goal and each
step reveals what the next step should
be. You use this for open-ended
investigation tasks where you can't
predict the full scope up front.
Let's go deep on prompt chaining. Three
things to know. Cold. First, it's a
fixed linear pipeline. You design the
steps before the work begins.
Step one's output becomes step two's
input. Step two's output becomes step
3's input, and so on. The pipeline
doesn't change based on what the steps
find. Second, it's best for structured,
repeatable tasks.
Code review is the classic exam example.
Every code review follows the exact same
pipeline per file analysis, then cross
file integration, then final summary.
The steps are identical every time
regardless of what the files contain.
Third, and this is the key insight, the
exam tests, prompt chaining gives each
step a focused context window. When you
review 14 files one at a time, each file
gets the model's full attention. Small
focused prompts beat one giant prompt
every time. That's the whole point.
Here's what that looks like in code.
Step one loops over each changed file
and reviews it in isolation. No other
files in context. These calls can be
parallelized.
Step two takes all the per file reviews
and does a cross file pass looking for
data flow issues and API mismatches
that must run after step one completes.
Step three compiles the final report.
Three steps fixed every time. That's
prompt chaining.
Now let's flip to the other approach.
Dynamic decomposition.
The defining characteristic. The plan
emerges from discoveries.
You cannot write step three until you've
seen what step two found. The scope, the
number of steps, the specific targets,
all of it is determined by what the
agent discovers during execution.
This is the right tool for open-ended
investigation tasks.
Add tests to a legacy codebase is the
canonical example. You know the goal,
comprehensive test coverage, but you
have no idea how many modules there are,
which ones are critical, or which have
complex branching until you actually
explore.
The trade-off is cost. With dynamic
decomposition, steps can multiply as new
discoveries surface.
If the agent finds a shared utility
module that nine other modules depend
on, it needs to spin up a whole new sub
investigation that wasn't in the
original plan. More flexible, yes, but
more expensive.
Here's the canonical example. Phase one,
map the structure. You find 12 modules,
eight without tests. That shapes phase
two. Which of those eight are critical?
Three are off, payments, user data.
Phase 3 digs into O specifically and
finds 15 code paths. Three with complex
branching. Phase 4 generates tests for
exactly those paths. Notice you couldn't
have written phase 4 at the start.
The specific targets only existed after
phase 3 ran. That's dynamic
decomposition in action.
The decision table summarizes it
cleanly. Predictable repeatable steps.
Prompt chaining. Open-ended exploration.
Dynamic.
Linear handoffs between steps. Chaining.
Next step depends on what you find.
Dynamic.
Chaining often allows parallel execution
within a step like reviewing files
simultaneously.
Dynamic is harder to parallelize
and chaining costs less because the step
count is fixed up front. Memorize this
table. Exam scenarios will describe a
situation and ask which approach is
appropriate.
There's one more concept the exam tests
heavily. Attention delution.
What is it? Language models process the
beginning and end of long inputs more
reliably than the middle. Content buried
in the middle of a very long context
window gets less attention. This is
called the lost in the middle effect.
This directly impacts multifile code
reviews.
If you send all 14 files in a single
prompt, files 5 through 10 are buried in
the middle. The model gives them a
shallow pass and misses obvious bugs it
would catch if they were the only file
in context.
The solution is per pass decomposition,
one file per prompt. Each file gets the
model's full attention. Then you do a
separate integration pass using the
summaries from the individual reviews,
not the full file contents.
This is prompt chaining applied
specifically to prevent attention
dilution.
This is the visual the exam loves. On
the left, one massive prompt with all 14
files.
Files one and 14 get thorough reviews.
File seven is lost in the middle and a
critical security bug ships to
production. On the right, 14 separate
passes, one file each. Then a 15th
crossfile integration pass using
summaries.
File 7 gets its own dedicated pass. The
bug is caught. Same task, completely
different quality. The only difference
is how the work was decomposed.
The initial plan has three steps. Map,
prioritis, generate tests.
After step one, the agent discovers a
shared utility module that nine other
modules import. Sticking to the original
plan would be wrong. That utility module
is the highest leverage target in the
entire codebase.
So the agent inserts two new steps.
Analyze the utility module, then test it
first before everything else. Those
steps didn't exist at the start. The
discovery made them necessary.
That's the power of dynamic
decomposition. The plan updates to match
reality.
Now, the anti-atterns.
Five failure modes. The exam will test
you on. Sending all files in a single
review prompt. This is the attention
delution trap. The model misses bugs and
middle files because they're buried in a
long context.
Always decompose into profile passes.
Using dynamic decomposition for a
predictable task. If you know the steps
up front, adaptive planning just waste
tokens replanning something you could
have specified directly. When the steps
are known, use prompt chaining. Using
prompt chaining for an exploratory task.
A rigid fixed pipeline can adapt when
you discover something unexpected mid-
execution. If the scope is open-ended
and discoveries should shape the next
step, use dynamic decomposition.
Overly narrow subtask definitions.
A coordinator tasked with researching
creative industries decomposes it into
only visual art subtasks, digital art,
graphic design, photography, missing
music, writing, and film entirely.
Subtask scope must match the full
breadth of the original task.
No cross-file integration pass. Profile
reviews catch within file bugs, but they
completely miss data flow
inconsistencies between modules and API
contract mismatches.
You always need that second pass to
catch the crosscutting concerns.
Six things to know Cole for the exam.
Prompt chaining equals fixed sequential
pipeline. Steps are known up front. Use
it for predictable, repeatable tasks
like code review or data extraction.
Dynamic decomposition equals adaptive
plan. Use it when the task is open-ended
and discoveries at each step shape. What
comes next?
Split large reviews into profile passes
plus a separate crossfile integration
pass. Never send all files in a single
prompt when accuracy matters.
Attention dilution means models pay less
attention to content in the middle of
long inputs.
Keep each prompt focused to avoid the
lost inthe-middle effect. Adaptive plans
add, remove, or reorder steps based on
intermediate findings.
This is only possible with dynamic
decomposition. A fixed pipeline cannot
adapt.
Prompt chaining has predictable lower
cost because the step count is fixed.
Dynamic decomposition handles complex
scenarios but may be more expensive as
adaptive steps multiply.
That wraps up part seven. We covered the
two decomposition approaches attention
dilution and the lost in middle effect
adaptive investigation plans. five
anti-atterns and six exam takeaways.
In part eight, we tackle task 1.7,
session state, resumption, and forking.
You'll learn how to give an agent
persistent memory across conversations,
how to resume a pause session from
exactly where it left off, and the key
exam question, when do you fork versus
when do you resume. See you in part
eight.
