Welcome back. In part 21, we covered few
shot prompting. Quick recap before we
move on. First, few shot examples beat
pros instructions.
When you give Claude two or three
examples of what you want, you get
consistent results. When you write a
paragraph describing the transformation,
you get different results every time.
Second, include reasoning in your
examples.
show why each action was chosen, not
just the action itself.
Claude copies the judgment pattern so it
can generalize to novel inputs.
Third, edge cases are where few shot
adds the most value. Show at least one
ambiguous scenario so Claude learns to
handle gray areas, not just happy paths.
Today we're moving to task 4.3. We'll
cover why asking for JSON in a prompt is
unreliable and what eliminates syntax
errors entirely.
We'll also look at tool underscore use
with JSON schemas for guaranteed
structured output and how to design
schemas that handle missing data without
hallucination.
If this video helped you prepare for the
exam, please give it a like and
subscribe.
You can tell Claude to respond in JSON
format, but there's a problem. Claude
might produce invalid JSON, missing
braces, trailing commas, unqued keys.
Even when the JSON is valid, the
structure might not match what your code
expects.
Think of it like asking someone to write
their address on a blank sheet of paper.
You get inconsistent results. but give
them a form with labeled boxes for
street, city, state, and zip. That
forces a consistent format.
The solution is tool underscore use with
JSON schemas.
When Claude calls a tool, it must
produce valid JSON matching the tools
input schema. The system enforces this
at the API level.
Claude literally cannot produce
malformed JSON for a tool call. Here's
the pattern.
First, define an extraction tool. It's
not a real tool, just a JSON schema. You
specify field names, types, required
fields, and enums.
Claude fills in the values.
Second, use tool underscore choice to
force Claude to call your extraction
tool instead of returning plain text.
No more sorry. I'll just describe it in
pros.
Third, extract the structured data from
response.content
zero.input.
It's guaranteed valid JSON matching your
schema.
No parsing, no reg x, no cleanup needed.
Let's look at a concrete example. We
define an extract underscore invoice
data tool with fields like vendor
underscore name invoice number total and
a category enum. The required list
includes only fields that truly always
exist in invoices.
Then we call the API with tool_choice
set to force this specific tool. Claude
must call it no text output allowed. The
extracted data comes back in the tool
calls input field guaranteed valid JSON
matching our schema.
Remember from task 2.3 tool underscore
choice controls claude's tool behavior.
Auto means claude may return text
instead of calling it tool. That's fine
for normal conversation but terrible for
extraction.
any means claude must call one of the
tools you provide and it picks which
one. Use this when you have multiple
extraction schemas and don't know the
document type. Force tool selection
means claude must call a specific tool
by name. Use this when you know the
document type and want a particular
extraction schema.
Never use auto for extraction.
Claude may return text instead of
structured data. The exam tests this
directly.
Here's a critical design choice.
What happens when the source document
doesn't contain information for a field?
If that field is required, Claude
fabricates a value. That's
hallucination.
The wrong approach is making all fields
required. If an invoice has no purchase
order number and you require it, Claude
invents one. The right approach is
making fields nullible.
Use the type array with null like type
number or null. Some invoices don't show
tax. Make it nullable so Claude returns
null instead of fabricating.
Also add unclear and other to your
enums.
This prevents forced mclassification of
ambiguous cases. Add a category detail
string field for other cases.
and keep required to only fields that
truly always exist.
Tool use eliminates syntax errors but
not semantic errors. A syntax error
would be missing quotes or invalid JSON.
That's impossible with tool use because
the API enforces valid JSON at the
system level. But a semantic error is
still possible.
valid JSON with wrong content.
For example, the total says $100, but
the line items only sum to 92.
And tax on 92 at 8% is $7.36,
not 8. The JSON is perfectly valid, but
the numbers don't add up. Semantic
validation requires a separate step.
That's task 4.4.
Tool use gives you structure, not
correctness.
Know this for the exam.
Here are the six things you must know
for the exam. One, tool underscore use
with JSON schemas eliminates syntax
errors. It's the most reliable
structured output approach.
Two, tool choice any guarantees claude
calls a tool instead of returning text.
Three, nullable fields prevent
hallucination. Make fields optional when
the source may not contain them. Four,
enums with unclear and other prevent
forced mclassification.
Five, force tool selection ensures a
specific extraction tool runs first when
you know the document type.
And six, tool use eliminates syntax
errors, but not semantic errors. values
can still be wrong even when the JSON is
valid.
You now know why asking for JSON in a
prompt is unreliable.
How to underscore use with JSON schemas
eliminates syntax errors entirely.
And how to design schemas that handle
missing information without
hallucination.
Plaude literally cannot produce
malformed JSON for a tool call. The API
enforces it at the system level. That's
the power of tool use over pros
instructions.
In part 23, we'll cover validation and
retry loops. How to detect semantic
errors. Retry with specific feedback and
build a quality feedback loop that
improves extraction over time.
See you in part 23, validation and retry
loops.
