# Skill: persona-gen

Activate when the designer says "run persona-gen".
Generate a research-grounded persona profile from
raw research the designer provides.

This skill synthesizes — it does not invent.
Every insight must trace back to something in
the research. Gaps are flagged, not filled.

---

## Before starting, ask:

1. "What role are we building a persona for?
   Be specific — not just 'user' but their
   relationship to the product. Example:
   knowledge admin, portal user, new hire,
   developer, HR business partner."

2. "Share your research — paste it in whatever
   form you have it. Interview notes, quotes,
   survey data, usability observations, analytics,
   Slack threads, support tickets. Raw and messy
   is fine. More is better."

3. "Are there any other roles this person
   interacts with that I should factor in?
   Example: a knowledge admin who also has
   to answer to a compliance team."

---

## Grounding rules — apply throughout

Only use what's in the research provided.
If something isn't supported by the research,
do not include it.

When quoting or referencing research directly,
mark it clearly:
[Research: "exact quote or observation here"]

When making an inference from research —
something implied but not stated directly —
mark it:
[Inferred from: brief description of the signal]

When there's a gap — something the template
asks for that the research doesn't cover —
flag it explicitly:
[Gap: no data on this — recommend follow-up
research or usability sessions to fill]

Never write a plausible-sounding assumption
dressed up as a finding. A visible gap is
more useful than a hidden guess.

---

## Persona output format

Use this exact structure. Do not skip sections.
Flag gaps instead of omitting them.

---

# Persona: [Name] — [Role]

## Who they are
2-3 sentences. Specific, not generic. Who is
this person in the context of the product —
not a demographic profile, a behavioral one.
What is their relationship to the system?

## Their job in one sentence
What does success look like for them on a
normal day — before anything goes wrong?

## Their relationship to the product
How often do they interact with it?
In what context — proactive or reactive?
With what level of expertise or confidence?
What do they think of it?

## What they care about most
Their actual priorities. Not what the brief
says they care about — what the research shows
they optimize for in practice.

## Their biggest frustration right now
Specific. Pulled from research. Verbatim quotes
where possible. If multiple frustrations exist,
list them in order of frequency or intensity
as the research supports.

## How they talk
Vocabulary they use naturally.
What they would never say.
Formal or casual? Patient or blunt?
Any phrases that came up repeatedly in research.

## Research grounding
All primary research that informed this persona.
Paste quotes, data points, and observations here
verbatim. This section is the source of truth —
if something isn't here, it shouldn't be in the
persona.

## Red flags that make them disengage
Specific triggers — not general UX principles.
What would make this person immediately lose
trust, give up, or route around the system?
Grounded in observed behavior where possible.

## The question they always ask that designers hate
The one that forces you to have actually thought
something through. The question that, if you
can't answer it, means the design isn't ready.
This should feel uncomfortable. If it doesn't,
it's not the right question.

---

## After generating the persona

Provide a summary section:

**3 strongest behavioral insights this research supports**
What the research says clearly and confidently.
These are the signals worth designing for.

**Biggest gap in what we know about this person**
The most important unknown — the thing that,
if answered, would most change how we design
for them.

**Recommended follow-up research**
1-2 specific questions worth asking in the
next round of interviews or usability sessions
to fill the most critical gap.

**Advisory board fit**
How this persona would behave in a critique
session — their default stance, what they'd
push back on hardest, and their signature
question for design reviews.

---

## After the persona is generated, ask:

"Do you want to save this directly to
resources/personas/? If so, what filename?
Example: kai-knowledge-admin.md"

"Are there other roles you want to generate
from this same research, or do you have
different research for the next persona?"

---

## What not to do
- Do not generate a persona without research —
  if none is provided, ask for it before proceeding
- Do not smooth over gaps with generic UX knowledge
- Do not make the persona sound like a stock photo
  caption — specific and behavioral, not demographic
- Do not skip the advisory board fit section —
  that's what connects this persona to the
  critique skill
