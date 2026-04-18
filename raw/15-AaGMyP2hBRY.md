Welcome back.
In part 14, we covered task 3.1,
configuring clawd.md with the
three-level hierarchy and modular
organization.
Let's recap before moving to task 3.2.
Three clawd.md levels: user at
tilde/.clawd,
personal, not shared; project at .clawd
or root, committed to get, team-wide;
and subdirectory, most specific, highest
priority.
The @import syntax splits clawd.md into
focused topic files.
The .clawd/rules directory auto-loads
rule files without explicit imports and
supports YAML front matter for path
scoping.
Split a monolithic clawd.md into a
50-line index plus focused files in
.clawd/rules.
Each file covers one topic and can be
selectively imported by subdirectory
clawd.md files.
The /memory command verifies which
config files are actually loaded.
If a team member is missing expected
conventions, check whether the config is
at user level instead of project level.
That was task 3.1.
Today, task 3.2, /commands and skills.
If this series is helping you prepare
for the exam, please give it a like and
subscribe.
Every like helps more developers find
this content, and your support keeps me
making more.
Clawd code can be extended with custom
commands and skills.
Both standardize team workflows, but
they work differently.
/commands are quick shortcuts, like
keyboard shortcuts.
You define a markdown file in a commands
directory, and any team member can type
/ followed by the command name to
trigger it.
Skills are more powerful. They are
isolated plugins with their own
permissions, their own execution
context, and their own configuration.
Think of commands as keyboard shortcuts
and skills as plugins.
The exam tests when to use which, how to
configure each, and what anti-patterns
to avoid.
Custom /commands are markdown files in
.clawd/commands for project-level shared
commands or tilde/.clawd/commands
for personal user-level commands.
When a team member types /review, clawd
reads the review.md file and follows its
instructions.
The file is plain markdown, a numbered
checklist, a set of rules, whatever the
workflow requires.
Project-level commands are committed to
get and shared with the whole team
automatically.
User-level commands are personal
shortcuts that are never shared.
This is the same scoping pattern as
clawd.md.
Project versus user scope for /commands.
Getting this wrong is a common exam
question.
Project-level .clawd/commands directory,
committed to get.
Every developer who clones the repo gets
the commands automatically.
Use for standard team workflows, code
review, deploy checks, test runs.
User-level tilde/.clawd/commands
directory, personal and not in get.
Use for your own shortcuts.
Key exam point: if a command is not
available to teammates, check whether it
was placed at user level instead of
project level.
Skills are more powerful than commands.
They live in .clawd/skills
and use a s k i l l .md file with YAML
front matter that configures how the
skill runs.
Three front matter options matter for
the exam.
Context:fork
runs the skill in an isolated sub-agent,
preventing verbose output from polluting
the main conversation context.
Allowed tools restricts which tools the
skill can access, applying least
privilege.
Argument hint prompts the developer for
input when no argument is provided,
improving the developer experience.
These three options are the most tested
front matter fields.
Context:fork is the most important skill
configuration option.
Without it, a verbose skill, like a deep
code base analysis, runs its 50 tool
calls and generates thousands of lines
of output directly in the main
conversation.
That output consumes most of the context
window before your real work begins.
With context:fork,
the skill runs in a separate sub-agent.
The main conversation only receives the
final result, a concise summary.
All the intermediate tool calls and
verbose output are discarded after.
The main conversation stays clean.
Use context:fork for any skill that
involves exploration, analysis, or
brainstorming.
Allowed tools restricts which tools a
skill can use.
An analysis skill that only needs to
read code should declare read, grep, and
glob, and nothing else.
Without this restriction, the skill has
access to edit, write, and bash, which
could accidentally modify files or run
dangerous commands.
This is the principle of least privilege
applied to skills.
A read-only skill cannot write.
A formatting skill cannot run shell
commands.
The exam tests this as the correct way
to prevent accidental damage from skill
execution.
When should you use a skill versus
putting something in clawd.md?
The rule is straightforward.
Clawd.md is always loaded every session
automatically.
Use it for universal standards that
should apply to every interaction.
Always use TypeScript strict mode.
Always write tests before
implementation.
Skills are on demand, invoked explicitly
by name.
Use them for task-specific workflows you
only sometimes need. /analyze for a deep
code review. /deploy check before
release.
If a developer needs to remember to
invoke it, it should be a skill.
If it should always apply, it belongs in
clawd.md.
Five anti-patterns for task 3.2.
These are the wrong answer choices on
the exam.
Putting team commands in
tilde/.clawd/commands.
That location is personal. Teammates
never get the commands.
Team workflows belong in
.clawd/commands,
committed to get.
Skills without context:fork
for verbose operations.
The analysis output fills the main
conversation context before real work
starts.
Add context:fork for any skill that
explores, analyzes, or brainstorms.
Skills with full tool access for
read-only tasks.
Without allowed tools restrictions, a
skill that should only read code can
accidentally edit or delete files.
Always restrict with allowed tools.
Putting universal standards in skills
instead of clawd.md.
If a rule should apply to every
interaction, it must be in clawd.md,
not in a skill that developers must
remember to invoke.
Creating personal skill variants with
the same name as team skills.
Your personal tilde/.clawd/skills
variant overrides the team version for
you.
Use a different name, like my review
instead of review, to avoid overriding
teammates' tools.
Six exam takeaways for task 3.2.
Know these cold.
One, /commands in .clawd/commands are
shared team shortcuts committed to get.
Tilde/.clawd/commands
is personal and not shared.
Two, skills in .clawd/skills support
skill.md with YAML front matter,
context:fork,
allowed tools, and argument hint.
Three, context:fork
isolates skill execution in a sub-agent,
keeping the main conversation clean.
Use it for verbose exploration and
analysis skills.
Four, allowed tools restricts tool
access during skill execution, the
principle of least privilege.
A read-only skill should list only read,
grep, and glob.
Five, skills are on demand, invoked
explicitly.
Clawd.md is always loaded.
Universal standards go in clawd.md.
Task-specific workflows go in skills.
Six, create personal skill variants with
different names to avoid overriding team
skills.
Use my review instead of review, so your
customization does not affect teammates.
That wraps up task 3.2, custom {slash}
commands and skills.
You now know how to create team
commands, scope them correctly,
configure skills with context colon fork
and allowed tools, decide when to use
skills versus claw.md, and avoid the
personal variant naming trap.
You have now completed tasks 3.1 and 3.2
of domain three. The series continues
with task 3.3,
path specific rules for conditional
convention loading.
Thank you for watching. See you in part
16.
