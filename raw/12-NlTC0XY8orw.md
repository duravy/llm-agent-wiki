Welcome back. In part 11, we covered
task 2.3, distributing tools across
agents and configuring tool choice.
Let's recap before moving to task 2.4.
Too many tools degrade reliability.
Research shows Claude's tool selection
becomes unreliable above four to five
tools per agent. 18 tools is the
anti-attern
scope tool access. Each agent gets only
the tools for its specific role. Giving
off roll tools leads to misuse. A
synthesis agent with web search starts
searching instead of synthesizing.
Crossoll tools are the exception. Only
add them for highfrequency operations
that would otherwise require expensive
coordinator round trips.
Three tool underscore choice modes. Auto
lets Claude decide any forces a tool
call. Force names a specific tool. Each
solves a different orchestration
problem.
Constrained alternatives replace generic
tools with scoped versions.
Instead of fetch URL with no limits, use
load document that validates domain and
format. That was task 2.3. Today we move
to task 2.4. Integrating MCP servers.
Task 2.4 starts with a question the exam
likes to test. What is MCP and why does
it matter? MCP stands for model context
protocol, a standard that lets Claude
connect to external services through a
uniform interface.
The USB analogy maps directly to the
problem it solves. Before USB, every
device had its own proprietary
connector. Before MCP, every AI tool
integration required custom code. MCP
standardizes the interface, build or
configure a server once, and Claude
discovers and uses all its tools
automatically at connection time. One
standard any service.
Here is the contrast. The exam will test
regular tools from task 1.1 are defined
in your code as JSON schemas and
executed by your code. You list them
explicitly.
MCP server tools are defined and
executed by the server. Claude autod
discovers all tools at connection time.
You never list them manually. The key
example point when cloud code connects
to an MCP server all tools that server
provides become available
simultaneously.
One connection complete tool set no
repeated declarations.
MCP servers are configured at two
scopes. Getting this wrong is a common
exam question. The scope you choose
determines whether the whole team gets
the tools or just you. Project level
configuration lives in MCP.json at the
project route. Committed to version
control and shared with the entire team.
Everyone who clones the repo gets the
same MCP servers.
Use it for shared team tooling like
Jira, GitHub, and databases.
User level configuration lives in
tilda/.cloud.json
not committed to version control
personal to that developer.
Use it for personal or experimental
servers. Key exam point. If a new team
member isn't receiving expected MCP
tools, check whether the config is user
level instead of project level.
MCP server configs often need API
tokens. The rule is never hardcode them.
The dollar sign bracket syntax tells
claude code to read the value from the
developer environment at runtime. In
this GitHub example, dollar sign bracket
GitHub token bracket stays in the config
file. The actual token lives only in the
developer environment.
The mcp.json A JSON file can be safely
committed to version control because it
contains no actual secrets only variable
references.
This is what the exam means by
environment variable expansion for
secrets.
MCP servers can expose resources in
addition to tools. Resources are
readonly data cataloges.
The difference in agent behavior is
significant.
Without resources, the agent makes
multiple exploratory tool calls to
discover what's available. Searching for
authentication, then off, then login,
guessing every time. With resources, the
MCP server exposes an issue categories
catalog up front. The agent reads, "The
category is authentication.
One precise call, no guessing. resources
reduce exploratory tool calls by giving
the agent visibility before it starts
acting. They expose things like issue
summaries, database schemas,
documentation outlines, and available
report types.
Two options when adding MCP capability,
use a community server or build a custom
one. Community servers cover standard
integrations Jira, GitHub, Slack,
PostgreSQL, and are battle tested,
maintained, and handle edge cases you
haven't thought of yet. Custom servers
are for teamspecific workflows not
covered by any community server, an
internal deployment pipeline, a custom
CRM, a proprietary data source. The
exam's default answer, use community
servers first.
Only build custom when your workflow is
truly unique and no community option
exists.
Building a custom Jira connector when a
community one exists is an anti-attern.
Community MCP servers sometimes have
minimal tool descriptions.
When Claude evaluates which tool to use,
it reads descriptions.
A weak description on an MCP tool versus
a strong description on a built-in tool
like GP means Claude chooses Grep even
if the MCP tool is more capable. The fix
is not to modify the server. The fix is
to add tool preference guidance to your
claw.md.
You write, "When searching for code
patterns, prefer the codebase search MCP
tool over GP because it understands
semantic relationships like function
calls and class hierarchies.
Configuration, not code modification.
The exam tests this as a claw.md
solution.
Five antiatterns for task 2.4. These are
the wrong answer choices on the exam.
hard- coding secrets in MCP.json.
That file is committed to version
control, which means the token is
exposed to everyone who can read the
repo. Always use dollar sign bracket
env_v bracket expansion.
Building custom servers for standard
integrations.
Community servers already handle Jira,
GitHub, and Slack with battle tested
edge case handling. Reinventing them
wastes time and produces worse results.
No MCP resources for content cataloges.
The agent makes repeated exploratory
tool calls to discover what's available
instead of reading a catalog once.
Resources exist precisely to eliminate
this pattern.
Storing shared team MCP config at user
level. Teammates never receive the tools
because user level config stays on your
machine. Shared tooling belongs in
project levelc.json
committed to the repo.
Minimal MCP tool descriptions causing
Claude to skip the capable MCP tool.
Claude picks the built-in tool with the
better description.
Fix this via Claude MD guidance, not by
modifying the server itself.
Six exam takeaways for task 2.4. Know
these cold one. Project level.mmcp.json
is for shared team tooling committed to
git. User level tilda/.claw.json
is personal not shared with teammates.
two environment variable expansion
dollar sign bracket_var bracket keeps
actual secrets out of config files the
mcp.json The JSON is safe to commit.
Three, Claude autod discovers all tools
from a connected MCP server
simultaneously.
You never declare them manually in your
code. Four, MCP resources are readonly
cataloges, issue summaries, database
schemas, documentation outlines.
They reduce exploratory tool calls by
giving the agent upfront visibility.
Five, default to community servers for
standard integrations.
Build custom servers only when the
workflow is truly unique and no
community option covers it.
Six, enhance minimal MCP tool
descriptions via claude.md guidance, not
by modifying the server configuration is
the correct layer for this fix.
That wraps up task 2.4, integrating MCP
servers. You now know the two
configuration scopes, environment
variable expansion for secrets, auto
discovery of all tools at connection
time, MCP resources as content
cataloges, and when to choose community
versus custom servers. In part 13, we
move into domain three, cla code
configuration and workflows.
We start with task 3.1, configuring
claude.md files with the three-level
hierarchy, scoping rules, and modular
organization.
If you are preparing for the exam, you
have completed all of domain 2. See you
in part 13.
