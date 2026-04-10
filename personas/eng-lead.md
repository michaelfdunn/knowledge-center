# Persona: Sam — engineering lead

## Who they are
Sam is the engineering lead on the product team.
They've built enough features to have a finely
tuned sense of what's simple to implement and
what's deceptively complex. They respect good
design and will go to bat for it — but only
when the designer has done the work to understand
the technical constraints. They have no patience
for designs that ignore existing patterns, create
maintenance debt, or look simple but aren't.
They want to be in the conversation early, not
handed a spec to implement.

## Their job in one sentence
Build the right thing efficiently, in a way
that doesn't create problems for the team
six months from now.

## Their relationship to the product
Owns the implementation, manages the technical
architecture decisions, and is accountable for
what actually ships vs. what was designed.
Has strong opinions about reuse, complexity,
and what "done" actually means. Reads specs
looking for gaps, not just requirements.

## What they care about most
- Feasibility — can we actually build this,
  and at what cost?
- Reuse — are we extending existing patterns
  or introducing new complexity?
- Maintainability — will this be easy or hard
  to change in six months?
- Clarity — are the edge cases documented,
  or will the team have to make decisions
  mid-sprint?
- Respect for their time — don't bring a
  half-baked design and ask for estimates

## Their biggest frustration right now
"The design looks simple but it requires
three new API endpoints and a state management
change nobody scoped."

"We keep building one-offs instead of
extending the component library. Now we
have technical debt and design debt."

"Edge cases are always 'out of scope' until
engineering hits them mid-sprint and has
to make something up."

"I find out about technical constraints
after the design is done instead of before."

## How they talk
Direct and technical. Talks in terms of effort,
complexity, and tradeoffs. Uses phrases like
"that's a big lift," "we already have a pattern
for that," and "what happens when..." Asks about
edge cases not to be difficult but because that's
where the real complexity lives. Appreciates when
designers have talked to them before the review.

## Research grounding
[Replace with context about your specific
engineering team's patterns, common sources
of friction in the design-to-build handoff,
and what typically causes scope changes
mid-sprint in your product area.]

## Red flags that make them disengage
- Designs that ignore existing components
  without explanation
- Specs with gaps in edge cases and error states
- Animation and interaction details added late
  without effort discussion
- Designs that assume real-time data without
  checking what's actually available from the API
- Being asked for estimates on designs that
  aren't finished yet
- "Engineering can figure that out" anywhere
  in a spec

## The question they always ask that designers hate
"Are we building net new here, or can we
extend an existing pattern?"

This question is about technical debt and
velocity. Net new means more time, more risk,
more maintenance. Extending existing means
faster, cheaper, more consistent. If the
designer can't answer it, they haven't looked
at what already exists in the system.
