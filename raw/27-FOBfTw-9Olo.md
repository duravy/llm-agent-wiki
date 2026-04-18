Welcome back. In part 26, we covered
context management. Quick recap before
we move on. Quick recap. Context
management is the foundation for
everything we do next. The context
window is like a desk with limited
space. Every document competes for room.
And if the desk is full, you have to
remove something, possibly something
important.
The lost in the middle effect means
models process the beginning and end of
context best.
Important details buried in the middle
may be overlooked.
Summarization loses numbers, dates, and
identifiers.
Use persistent case facts blocks instead
of condensing conversation history. Trim
tool outputs to relevant fields only.
Don't pass 40 fields when you need five.
This preserves context budget for what
actually matters.
Require metadata and sub aent outputs
source date URL methodology.
Without it, downstream agents can't site
sources or assess recency.
Today we cover task 5.2. When to
escalate, how to resolve ambiguity, and
why sentiment is a terrible escalation
proxy.
The exam tests escalation decision rules
directly. Getting these wrong costs
points fast.
If this video helped you prepare for the
exam, please give it a like and
subscribe.
Let's talk about escalation.
Think of a bank teller who tries to
approve loans instead of handing them
off to a loan officer.
This is domain five, task 5.2, Two,
escalation and ambiguity resolution
or a loan officer who handles deposits.
Clear boundaries make everything work
smoothly.
In task 5.2, we focus on the three
triggers that demand escalation, and we
prioritize honoring a customer's request
for a human above all other steps.
Key insight, sentiment-based escalation
is unreliable.
A furious customer might need a simple
tracking lookup. A comm one might need a
policy exception.
The exam tests these decision rules
directly.
There are exactly three triggers for
escalation.
Trigger one, the customer explicitly
requests a human. They say, "I want to
speak to a manager."
Escalate immediately. Do not investigate
first.
Trigger two, a policy gap. The customer
asks you to match a competitor's price,
but your policy only covers on-site
adjustments.
The policy is silent. Escalate.
Trigger three. You've tried multiple
approaches and none resolve the issue.
Continuing wastes everyone's time.
Here's the critical exam distinction.
When a customer explicitly asks for a
human, escalate immediately.
Do not first try to resolve the issue,
even if you think you can. Honoring the
customer's preference is the priority.
Vague escalation guidelines lead to
inconsistent behavior. If you write
escalate complex cases to a human agent,
what counts as complex?
Claude decides differently each time.
Instead, define explicit criteria with
few shot examples.
Escalate when the customer requests a
human. That's immediate. Escalate when a
refund exceeds $500. That's the policy
limit. Escalate when a request isn't
covered in your instructions.
Escalate after three attempts with no
progress. and do not escalate for
standard order lookups, password resets,
or tracking inquiries.
The contrast is clear. Vague criteria
produce random behavior.
Explicit criteria with examples produce
consistent results every time.
Here's a distinction the exam tests
directly.
Sentiment is not a reliable escalation
proxy. Imagine a customer who says,
"This is ridiculous. Where is my
package?"
An agent that escalates because the
customer sounds angry. Is making the
wrong call. This is a simple tracking
query that should be resolved
autonomously.
Now, imagine a calm customer who says,
"I'd like you to wave the restocking fee
because the product description was
misleading."
This needs escalation, not because of
tone, but because restocking fee waivers
require manager approval. The lesson,
escalate based on case complexity, not
emotional tone.
Getting this wrong is a common exam
question.
What happens when a tool returns
multiple matches? Say you search for
John Smith and get three customer
accounts.
The wrong approach is to pick the first
result and proceed. That might be the
wrong person entirely.
The right approach is to ask for
additional identifiers.
Say, I found three accounts under John
Smith. Could you provide your email
address or the last four digits of your
phone number to identify the correct
account?
Never select based on heruristics.
Always ask for clarification.
When an frustrated customer has a
resolvable issue, acknowledge the
frustration but attempt resolution
first. A customer says, "I've been
waiting 3 weeks. This is unacceptable."
The agent responds with empathy, looks
up the order, finds it was delayed at
the distribution center, and offers
expedited shipping on the next order as
compensation.
Only if the customer then says, "No, I
want to talk to a manager." Only then
does the agent escalate.
The agent offered a resolution first and
only escalated when the customer
reiterated their preference for a human.
This is the correct pattern.
Now, let's cover the five antiatterns.
Each of these leads to inconsistent
behavior or wasted human agent time.
Let's go through them one by one.
Anti-attern one, investigating before
escalating. When the customer asks for a
human, the customer says, "I want to
speak to a manager."
The agent tries to resolve the issue
first, thinking they can help. The fix
is simple. Escalate immediately on
explicit request.
Honoring the customer's preference is
the priority, even if you think you can
solve it.
Anti-pattern two, sentiment-based
escalation.
The agent escalates because the customer
sounds angry, but frustration doesn't
correlate with case complexity.
The fix escalate based on policy gaps
and capability limits.
A furious customer might just need
tracking info that you can provide right
now.
Anti-attern three, guessing when
multiple customers match. The tool
returns three John Smith accounts and
the agent picks the first one. That
might be the wrong person. The fix: Ask
for additional identifiers like email or
phone digits.
Never select based on heruristics.
Anti-attern four, escalating resolvable
issues.
Sending a simple password reset or
tracking
inquiry to a human agent waste their
time. The fix: resolve autonomously when
within capability.
Acknowledge frustration, but attempt
resolution first.
Anti-attern five vague escalation
criteria.
Write escalate complex cases and claude
decides differently each time what
counts as complex.
The fix define explicit triggers with
few shot examples.
Show what to escalate and what not to.
Here's the exam framework. If the
customer asks for a human, escalate. If
there's a policy gap, escalate.
If you've tried three times with no
progress, escalate.
Everything else resolve autonomously.
Lock this in for the test.
In the next part, we'll cover error
propagation in multi- aent systems.
When a sub agent fails, how does the
coordinator know? What's the difference
between a generic search unavailable
error and a structured error with
partial results and recovery options?
The quality of error communication
determines whether a multi-agent system
recovers gracefully or fails silently.
We'll learn the two anti-atterns to
avoid silent suppression and immediate
termination.
See you in part 28 where we learn how
agents talk about failure.
