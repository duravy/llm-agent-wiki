Welcome back. In part 13, we covered
task 2.5, Claude Code, six built-in
tools for working with code. Let's recap
before moving to task 3.1.
GP searches file contents for patterns.
Glob searches file names and paths.
Never confuse them. GP does not find
files by name, and Glob does not look
inside files. The two-phase search
strategy. GP first to find entry points,
then read selectively on the files GP
surfaces.
Never read the whole codebase up front.
It fills context with irrelevant code.
Edit for targeted changes. It requires a
unique match. Write for new files and
complete rewrites.
If edit fails due to a non-unique match,
fall back to read the full file, then
write the modified version. Incremental
exploration. Follow imports and function
calls step by step. Each discovery
narrows the next search. Never guess
which files matter. Bash is for build,
test, install, and get operations, not
for file search. Always use the built-in
GP and glob tools instead of shell find
or GP commands. That was task 2.5.
Today, task 3.1, the claude.md
hierarchy.
If this video helped you prepare for the
exam, please give it a like and
subscribe.
It really helps more learners find this
series.
Claude.md is a configuration file that
gives Claude code persistent
instructions.
Every time Claude code starts a session,
it reads claude.md and follows the
instructions inside.
Think of it as a job description you
hand to a new team member. It describes
coding standards, project conventions,
and how things work around here. But
claude.md does not live in just one
place. It exists at three levels. And
each level has a different scope, a
different audience, and different
priorities.
The exam tests whether you know which
level controls what and what happens
when levels conflict.
The three levels of the claude.md
hierarchy from lowest to highest
priority.
User level lives at
tilda/.claude/claude.md.
It is personal to you never shared via
git. Use it for your own preferences and
personal shortcuts.
Project level lives at.cloud/cloud.mmd
or in the project route. It is committed
to get and shared with the whole team.
Use it for team coding standards and
project conventions.
Subdirectory level lives inside a
specific subdirectory. For example,
packages/billing/claude.md.
It is the most specific and has the
highest priority. It is loaded only when
Claude is working inside that
subdirectory.
The key exam point when levels conflict
the most specific level wins.
Subdirectory overrides project.
Project overrides user.
Largeclaw.md
files become unwieldy.
The at import syntax lets you split
instructions into focused files. In the
rootcloud.md,
you write at import followed by the path
to each rule file. Each imported file
contains instructions for one specific
topic, testing, API conventions,
deployment procedures.
This keeps the root claw.md clean. Just
a highle overview and a list of imports.
It also enables selective importing per
package. The billing package can import
the API conventions and testing rules
but skip the general deployment file
because billing has its own deployment
process.
The exam tests this as a modular
organization pattern.
An alternative to add import is
thecloud/ruules
directory. Files placed in this
directory are automatically loaded as
additional instructions. No explicit
import needed.
You create separate markdown files for
each topic. Testing.md for testing
conventions. API-conventions.md
for API design rules. Deployment.md for
deployment procedures.
The advantage over ad import rules files
can have YAML matter with path specific
scoping which is covered in task 3.3.
This makes them more powerful for
projects that need different conventions
in different areas of the codebase.
If your cloud MD has grown to hundreds
of lines covering testing, API design,
deployment, code style, and more, it is
time to split it. Before one monolithic
file, 800 lines covering everything.
After a 50line rootclaw.md with at
import references plus four focused
files including
API conventions deployment and code
style. Each file is short maintainable
and can be selectively imported by
subdirectory cla files. The root file
becomes a readable index not a wall of
text.
When Claude is not following expected
conventions, do not guess. Verify. The
/memory command shows you exactly which
configuration files are loaded in the
current session. If a file is missing
from the list, Claude does not know
about those instructions.
Common causes the file is at user level
when it should be project level, the ad
import path is wrong, or a rules file
has a syntax error in its YAML front
matter. Check slashmemory first before
concluding that Claude is broken.
Five anti-atterns for task 3.1.
These are the wrong answer choices on
the exam. One massive monolithic Claude
MD hard to maintain.
Irrelevant rules dilute important ones.
Claude reads the whole file every
session. split into focused files in
thecloud/ruules
directory.
Team conventions in user level config.
Other team members never get them
because user level config is personal
and not shared via git. Team conventions
belong in project level.cloud/cloud.md.
Personal preferences in project level
config. This imposes your style on
everyone who works in the repo. Personal
preferences belong in
tilda.cloud/cloud.md,
not in the committed project file. No
modular organization at all. Everything
crammed into one file. Use at import or
the rules directory to keep each topic
in its own focused file.
Not checking /memory. When claude
ignores rules, you are guessing about
what is loaded. Always verify with
slashmemory before concluding something
is broken.
Seven exam takeaways for task 3.1
know these cold one three levels user at
tilda/.claude
project at.claude or root subdirectory
inside a specific directory each has
different scope and sharing.
Two, user level is personal, not shared
via git. Project level is shared with
the team. Subdirectory is the most
specific and highest priority.
Three, add import references external
files to keep cla.md modular and
readable. Each import path is relative
to the importing file.
Four, the cloud/ruules directory holds
topic specific rule files loaded
automatically. No explicit import
needed.
Five, split a monolithic claude.md into
focused files organized by topic. The
root file becomes a 50line index with
add import references.
Six /memory verifies which configuration
files are actually loaded. Use it before
concluding that Claude is ignoring your
rules.
Seven, when a team member is missing
expected conventions, check if the
config is at user level instead of
project level. That is the most common
misconfiguration.
That wraps up task 3.1, configuring
claude.md with the three-level
hierarchy.
You now know the three levels and their
scopes. How at import keeps
configuration modular, the rules
directory as an alternative. How to
split a monolithic file and how/memory
diagnoses loading issues. In part 15, we
continue domain 3 with task 3.2
custom/comands and skills. You will
learn how to create team shortcuts,
configure skills with YAML from matter,
use context colon fork for isolated
execution, and apply least privilege
tool access. See you in part 15.
