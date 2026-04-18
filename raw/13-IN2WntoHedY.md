Welcome back. In part 12, we covered
task 2.4, integrating MCP servers into
cloud code and agent workflows.
Let's recap before moving to task 2.5.
MCP stands for model context protocol, a
standard that lets Claude connect to
external services through a uniform
interface.
like USB for devices. One standard
replaces every proprietary connector.
Two configuration scopes project
levelcp.n
is committed to get and shared with the
whole team. User level tilda/.claw.sn
is personal and never shared.
Environment variable expansion keeps
secrets safe. Wrapping a variable name
in curly braces with a dollar prefix in
the config file makes it pull the actual
value from the environment at runtime so
the config file itself can be safely
committed.
MCP resources are readonly data
catalogs, issue summaries, database
schemas, documentation outlines.
They eliminate exploratory tool calls by
giving the agent upfront visibility into
what is available.
Community servers handle standard
integrations Jira, GitHub, Slack,
PostgreSQL, and are battle tested. Only
build a custom server when your workflow
is truly unique and no community option
exists. That was task 2.4.
Today, task 2.5, built-in tools.
Claude Code ships with six built-in
tools for working with code. Each has a
specific strength, and picking the wrong
one wastes time or produces poor
results. Think of them as specialized
instruments in a workshop. You would not
use a hammer to turn a screw. GP
searches file contents for patterns.
Glob finds files by name or path
pattern. Reloads a full file. Write
creates a new file or completely
overwrites one. Edit makes targeted text
replacements in an existing file. Bash
runs shell commands for build, test, and
get operations.
Six tools, six distinct jobs. The exam
tests whether you can match the right
tool to the right task.
The single most common exam mistake,
confusing Grep and Glob. They sound
similar but search completely different
things. Glob searches file names and
paths. You give it a pattern like
star.est.tsx
and it returns every file whose path
matches that pattern. GP searches file
contents. You give it a text or reg x
pattern like describe open pend and it
returns every file that contains that
string inside it. The rule is simple. If
you know the naming pattern, use glob.
If you know what the code contains, use
gp. A glob for star.test.tsx
gives you all test files. A GP for
describe open pan finds all files that
actually contain test suites which could
include non-EST utility files as well.
When exploring an unfamiliar codebase,
use a two-phase search strategy. Phase
one, use GP to find entry points. Search
for the function or concept you need.
For example, process refund and GP
returns every file that contains it.
Phase two, use read on the specific
files that GP surfaced. You now know
exactly which files are relevant. So you
are not loading the entire codebase into
context.
Why not just read everything up front?
You do not know which files matter until
you search. Reading blindly fills the
context window with code that has
nothing to do with your task. Why not
Grep for everything? GP finds matches
but does not show the surrounding
context you need to understand the code.
Read gives you the full picture once you
know where to look.
Edit and write are both for modifying
files but they operate at completely
different scales. Edit makes a targeted
text replacement. You provide the exact
old string and the new string and only
that portion of the file changes.
Everything else stays intact.
Write replaces the entire file with new
content. The whole file is overwritten.
Use edit for changing one specific part
of an existing file. Use write for
creating new files or for complete
rewrites.
Edit has one critical requirement. The
old string you provide must be unique in
the file. If the same text appears more
than once, edit fails. It cannot know
which occurrence to change. When that
happens, include more surrounding
context to make the match unique.
When edit cannot find a unique match,
the fallback is read plus write. Step
one, read the entire file to get its
current content. Step two, write the
full modified content back, replacing
the file. This two-step pattern is more
expensive than a direct edit. You are
loading and replacing the whole file,
but it always works when targeted
replacement cannot find a unique anchor.
The exam tests this fallback. If a
question asks what to do when edit
fails, the answer is read then write.
Do not try to read the entire codebase
up front. Build understanding
incrementally.
Step one, GP for the entry point. Find
where the API routes are defined.
Step two, read the routes file and
discover that routes call handlers in a
handler's directory.
Step three, read the specific handler
you care about and discover it imports
from a billing service. Step four, read
the service to understand the full data
flow. Each step narrows your focus based
on what you found in the previous step.
You only read files that are actually
relevant. This uses the context window
efficiently. No wasted tokens on code
that has nothing to do with your task.
The exam calls this incremental
exploration.
Five anti-atterns for task 2.5.
These are the wrong answer choices on
the exam. Reading all files up front.
This fills the context window with
irrelevant code before you know which
files matter. Always start with GP to
find entry points. Then read
selectively.
Using GP to find files by name. GP
searches content, not file names. For
file name or path patterns, use glob.
Using write to modify one line. Write
overwrites the entire file. Use edit for
targeted changes. It only replaces the
match string. Using edit when the match
is not unique. Edit fails when the old
string appears more than once.
Include more surrounding context to make
the match unique or fall back to read
plus write.
Using bash for file search. The find and
grab shell commands are less optimized
and less safe than the built-in GP and
Glob tools. Always prefer the built-in
tools for file and content search.
Six exam takeaways for task 2.5.
Know these cold. One. GP searches file
contents.
Glob searches file names and paths.
Never confuse them. The exam will try.
Two, the search strategy is GP first to
find entry points, then read to
understand the code. Never read
everything up front. Three, edit for
targeted changes requiring a unique
match. Write for new files or complete
rewrites.
Four, when edit fails due to a
non-unique match, the fallback is read
the full file, then write the modified
version back.
Five, incremental exploration. Follow
imports and function calls step by step.
Each discovery narrows the next search.
Do not guess which files matter.
Six. Bash is for build, test, install,
and get operations, not for file search.
GP and Glob are the right tools for
discovery.
That wraps up task 2.5, the six built-in
tools. You now know when to use GP
versus Glob, the two-phase search
strategy, when edit beats write, and
when to fall back, and how to explore a
code base incrementally.
In part 14, we move into domain three
and start with task 3.1, configuring
cloud.md files with the three-level
hierarchy, scoping rules, and modular
organization with add import and the
rules directory. If you found this video
helpful, please give it a like and
subscribe to follow the full series. See
you in part 14.
