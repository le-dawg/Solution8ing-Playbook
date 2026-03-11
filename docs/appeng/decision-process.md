# AppEng Decision Process

Application Engineers are staffed sparsely across projects at Solution8.
This means engineers are often working on unrelated products, code bases and technologies.

Because of this,
we rely on an ever evolving set of intentional structure
to facilitate decision making, share what has/hasn't gone well, and incorporate common patterns into our main playbook.

Due to the variance in projects, we don't expect a one-size-fits-all mentality to decision making.
However, we do hope to account for some prominent cases and highlight when certain applications make sense.

## What To Decide

<Expand> 
- AI greatly accelerates research as well as artifact production, greatly lowering the cost of iterating on anything that can be expressed in natural language or as structured text
- therefore, solution architecture can be treated as a continuous process whose artifacts can be versioned
- this means that we can and should treat solution architecture as a continuously changing aspect of our solutions, just like application code
- This makes decvision making around architecture much easier to track in the form of ADRs
- therefore the decision surface is as follows:
   - Initial
</Expand> 

## How to Make a Decision

At it's core, the FULL decision making process (to be used with very large Brownfield (aka. pre-existing) projects) is as follows:

1. Identify a decision made on a project that is applicable to the whole practice.
1. Write an Architectural Decision Record (ADR) justifying it.
1. Workshop the ADR with a [Domain Group](#domain-groups)
   and create a final draft.
1. Propose it in Microsoft Teams for all of AppEng for review.
1. (Optional) Hold a [meeting](#review-meeting) for review.
1. Commit the [ADR](./adrs) and create [implementation documentation](#output).

In case of smaller projects, where tracking Architecture is not a core need, a reduced best practice has emerged:

1. Identify a decision made on a project that is applicable to the whole practice.
1. Write an Architectural Decision Record (ADR) justifying it.
1. Propose it in Microsoft Teams in the #techcircle for all to review. Make sure all information required to process it is included, ideally using Github Issues or PRs to do so
1. Once a Solution Architect greenlughts the proposal:
   1. Commit the [ADR](./adrs) and create [implementation documentation](#output).

## Writing an ADR (TBD: Publish A Skill that forms part of a web of subagents that all embody the agentic software factory)

ADRs are a lightweight format that allows us
to identify use cases and solutions for problems we face in our engineering.
In AppEng,
we're incorporating ADRs into our practice
because they're the format we commonly communicate with,
advocate for,
and have had success with on projects.

On the practice level,
decision records may look different than those on projects.
While the end result may be similar,
the justification may require further elaboration
around why this decision may benefit as a whole.
More specifically,
what common use case or pattern may this solve for.
(TODO: add guide/clarity on how to do this
along with initial examples that demonstrate it)

More information around process
and structure for ADRs can be found in the
[Architecural Decision Records](../documentation/adr.md)
section of the playbook.

## Domain Groups

In order to organize members across projects,
we've established domain groups
that have responsibility over a certain part of the stack.
Currently these are broken up into:

- **Frontend** - For decisions around browser technology,
  frontend frameworks,
  and client side data modeling.
- **Server** - For decisions that deal with database modeling,
  processing,
  and general API building.
- **General**(better name please) - For decisions that fall outside of the other two groups
  or heavily incorporate both.
  This will likely be broken out into other groups
  as we discover what topics come up.

While guilds are established as topical and cross practice,
domain groups that overlap with guilds may choose to use their time
to prompt new ADRs,
workshop,
or review them.
These are not explicitly 1:1 in overlap
and leadership structures are not synonymous.

### Group Leaders

Groups leaders are accountable for moving forward their group's decisions.
They should seek to delegate and create alignment among the group,
however end decision making power lies with them.
One leader will be assigned per group,
and serve a six month term.
Practice leadership will call for nominees from the practice
and pick leaders in between those terms.
Group leaders should be at a minimum level 3
and have established specialization within their group's domain.
They should also have a year experience at Solution8.

Group leaders are also responsible for connecting practice members
with domain experts that can help them craft decisions best
and reviewers to provide feedback.
Their motivation should be to create a collaborative experience
and effective group output.

The rough expectation for time from a group leader is one hour per week.

## Review Meetings

If a decision is contested
or requires syncronous clarification,
a review meeting should happen.
The author of the ADR
is responsible for scheduling the meeting
and announcing it to #appeng for attendance.
The author,
domain group leader,
and reviewers should be in attendance.

Review meetings should happen at earliest two weeks after an ADR is announced.
This isn't to say folks can't collaborate earlier,
but having asyncronous time to review and incorporate feedback is helpful.

If a clear consensus can be found during the meeting,
the domain group leader for the decision may move it forward.
Otherwise,
the domain group leader is responsible
for following up with the ADR author
and establishing deciders/decision making criteria.
The group leader may delegate decision power,
but is accountable for a decision no later than
one week after the meeting.

## Output

Once a decision is made,
there are three follow ups required:

1. Commit the ADR.
1. Announce the decision in #appeng.
1. Write implementation documentation in this playbook.

ADRs authors are responsible for the corresponding documentation.
Practice leadership will check in on recent ADRs
to ensure documentation has been written to reflect the decision.

## Project Implementation

We recognize that projects vary at Solution8,
and it would be impossible to verbatim follow these decisions.
Some projects will use tools based on client needs,
or some may work in completely different domains.
When Solution8 is responsible for the technology decisions
and applications are similar to a past pattern,
projects should seek to utilize known resources first.
Practice ADR repositories should be consulted
prior to writing a new ADR.

In the end,
it is the Eng Lead's responsibility
to decide when a decision is applicable.
If a relevant ADR is ignored,
the Eng Lead (or a delegate) should write justification within their project's new ADR.
If the decision is succesful/overturns an existing one,
a new ADR should be proposed to the practice using the above process.
