# Prompt Cheat Sheet for Designers

Copy-paste these prompts into Claude Code. Fill in the [brackets] with your specifics.

---


## 1. Use Case Breakdown (The Analysis)

```
I have a use case I need to break down for an AI-native workspace.

My use case: [describe it in 1–2 sentences]

Break this down into:
1. The current happy path (3–5 steps)
2. Which steps could be handled by an AI agent (L1)
3. Which steps need human judgment with AI support (L2)
4. Which steps require authority or cross-dept escalation (L3)
5. What triggers escalation between tiers
6. If AI handled everything in L1, how would the specialist's day change?

Format the output as a structured breakdown I can share with my team.
```

---


## 2. Decision Screen Prototype

```
Build me a prototype of the L2 decision screen for this use case.

The persona has 30 seconds and needs to see 3 things:
1. What AI already did (context/summary)
2. What they need to decide right now
3. AI's recommended action (with confidence and reasoning)

```

---


## 3. Hub / Home View (Starting Point)

```
Build a hub view for [persona name], a [role] in [department].

The hub should include:
- "Decision Queue" — cases needing human judgment, ranked by [business metric]
- "On the Horizon" — 2–3 AI-predicted events requiring attention this week
- "AI Handled" — summary of what agents resolved since last session
- Quick stats relevant to this role: [list 2–3 key metrics]

Make it feel like a morning briefing. The specialist opens this and knows exactly what to focus on.
```

---


## 4. The Override Flow (Extend the Prototype)

```
Now show me what happens when the specialist disagrees with AI's recommendation. Update the prototype to include:

1. An override flow — what does the specialist click to disagree?
2. A reason capture — what does the system need to learn from the override?
3. A feedback loop — how does the override improve future AI recommendations?

Keep the same visual style. Show the override as a state change on the existing screen, not a separate page.
```

---

## 5. Queue View (L1 Surface)

```
Build a Queue View for [persona name], a [role] in [department].

The queue should have two sections:
- "Needs you" — [number] cases that require human judgment, ranked by [impact metric]
- "AI handled" — [number] cases auto-resolved overnight, with a reopen option

Each case card should show: priority indicator, department tag, case title, AI-generated one-line summary, estimated time to resolve, and the AI confidence level for auto-resolved items.

Use realistic data for a [department] specialist. Use the Horizon styling from the project.
```

---

## 6. Orchestration View (L3 Surface)

```
Build an Orchestration View for a cross-department case.

Scenario: [describe the multi-department workflow — e.g., "New hire onboarding spanning HR, Legal, Procurement, and Facilities"]

Show:
- Parent case with child tasks across departments
- Status of each department's task (complete, in progress, blocked, not started)
- AI-generated critical path analysis — what's blocking and what's the cascade effect
- Actions the case owner can take (nudge, escalate, reassign)

Include an AI status summary at the top: "Onboarding is X% complete. Y tasks blocked. Estimated risk to target date: [level]."
```

---

## 7. Compare Two Approaches

```
I'm exploring two different design approaches for [describe the screen or flow].

Approach A: [describe]
Approach B: [describe]

Build both as prototypes side by side. Same data, same persona ([name]), same use case. Let me see how the information architecture differs.
```

---

## 8. Persona Day-in-the-Life

```
Write a "day in the life" narrative for [persona name] in the AI-native future.

They arrive at work and open their workspace. Walk me through their first 90 minutes:
- What does the hub show them?
- What did AI handle overnight?
- What's their first decision?
- When does something cross departments?
- Where does AI help vs. where do they rely on their own expertise?

Make it concrete with realistic case details, not abstract.
```

---

## 9. Push Past the Obvious (Exploration Mode)

```
Don't show me the expected layout for this use case. What's a non-obvious way to design this screen — something that challenges how we normally think about [decision queues / AI confidence / override flows / cross-dept visibility]?

Build it and explain:
1. What makes this approach interesting
2. What it sacrifices compared to the default
3. What kind of specialist or workflow it's best suited for
```

---

## 10. Design from Figma (Start from a Visual Reference)

```
Here's a Figma link to a screen I want to use as a starting point: [paste Figma link]

Build an AI-native prototype based on this design, adapted for [persona name], a [role] in [department].

Use case: [describe the task or decision in 1–2 sentences]

Keep the visual structure from the Figma as a reference, but adapt it to follow the project's design principles:
- AI shows its work (confidence + reasoning + evidence)
- Actions, not just information
- Override is a first-class action
- Content adapts to the tier level: [L1 / L2 / L3]

Use realistic data from the persona file. Flag anywhere the Figma design and the AI-native principles conflict — I want to see the tradeoffs.
```

---

## 11. Stress-Test AI Confidence

```
AI is 58% confident in its recommendation for this case. How should the UI change to reflect that uncertainty?

Show me two approaches:
- Approach A: stays minimal — same layout, lower confidence indicator
- Approach B: surfaces more evidence — the UI expands to show what AI doesn't know

Same persona, same use case. Let me see how information architecture changes when certainty drops.
```

---

## 12. The Messy Case (Edge Case Stress Test)

```
Show me the worst-case version of this task view.

The specialist inherited a messy case with:
- Conflicting data from two sources
- A missed SLA (3 days overdue)
- A pending cross-department action that's blocking resolution
- AI confidence at 61% with no clear recommended action

How does the screen adapt? What does the specialist see first? What actions are available when AI can't give a clean recommendation?
```

---

## 13. Challenge the Design

```
You just built a prototype. Now argue against it.

What's the strongest case for a completely different approach? What assumptions did the current design make that might be wrong? What would a specialist in this role say is missing or misleading?

Don't be polite — stress-test your own output.
```

---


## 14. Storyboard — Title Frames Only (Narrative Flow)

```
Take this use case / day-in-the-life narrative and turn it into a storyboard prototype.

Use case or narrative: [paste your scenario here]

Build an HTML storyboard. Layout: horizontal scroll, frames side by side, 6–8 frames.

Each frame has two parts:
1. A white box with only the frame title centered inside (no wireframe, no UI elements — just the name of the screen or moment, e.g. "Morning Hub — decision triage")
2. A light blue section below the box with two labeled fields:
   - Narrative: what the specialist is doing or experiencing at this moment (1–2 sentences)
   - User experience: what they see or interact with (1–2 sentences)

Style: match this layout exactly —
- White background page, "Storyboard" section header top-left
- Frame boxes: white fill, 1px gray border, fixed height (~180px), title text centered in gray
- Info section: light blue background (#dbeafe or similar), small bold labels ("Narrative", "User experience"), regular body text below each
- Frames sit in a single horizontal row with consistent spacing
- No glassmorphism, no shadows, no icons — this is a planning artifact, not a UI prototype

Story arc: left to right, start with the specialist opening their workspace, move through key decision moments (at least one L1 and one L2 beat), end at resolution or handoff.

After the storyboard, add a one-paragraph "Future Day" statement: how is this specialist's role different because AI handles the routine work?
```

---


## 15. Visual Storyboard (Future-State Flow)

```
Take this use case / day-in-the-life narrative and turn it into a visual storyboard prototype.

Use case or narrative: [paste your scenario here — can be a paragraph, a bullet list, or a "day in the life" story]

Build an HTML storyboard with one frame per key moment. Each frame should have:
- A screen sketch or wireframe (not a finished UI — show the shape and key elements, not full polish)
- Frame title: the purpose of this screen (e.g., "Morning Hub — decision triage", "Invoice mismatch — AI flags variance")
- Narrative: what the specialist is doing or experiencing at this moment
- User experience: what they see, hear, or interact with on screen

Layout: horizontal scroll, frames side by side, 6–8 frames total.
Style: clean and minimal — white frame boxes, light gray background, small readable labels. Don't apply full Horizon glassmorphism here; this is a communication artifact, not a clickable prototype.

Storyboard should read left to right as a coherent story arc:
- Start: specialist arrives, opens workspace
- Middle: the key decision moments (include at least one L1, one L2 beat)
- End: resolution or handoff state

After the storyboard, add a one-paragraph "Future Day" statement: how is this specialist's role different from today because AI handles the routine work?
```

---


## Tips for Better Results

- **Want a faster prototype? Ask for HTML.** Say "Build this as a single HTML file, no React" or just "HTML prototype, no build step." It opens directly in a browser and generates much faster. Use React only when you need component reuse or complex state.
- **Styling is automatic.** The project has the Horizon visual system loaded (`patterns/horizon-styling.md`). You don't need to specify colors, fonts, or glassmorphism — Claude Code applies them. If a prototype comes out dark-themed or generic, say "Use the Horizon styling from the project."
- **Start from a template to go faster.** The `templates/` folder has a hub, task view (with omnibar + chat), and chat skeleton. Say "Use the task view template and fill it in for [persona] working on [use case]" to skip building from scratch.
- **Every prototype includes an omnibar and chat.** The omnibar is the entry point to chat — it appears on hub and task views. Clicking it opens a chat overlay; the omnibar hides while chat is open. You don't need to ask for this; it's built into the templates by default.
- **Be specific about your persona.** "AP specialist handling invoice variances" gets better results than "someone in finance."
- **Include a realistic scenario.** "Invoice #7842 for $118K against a $100K PO" grounds the prototype in real data.
- **Push back on Claude Code.** If the output is generic, say "Make the data more specific to [your scenario]" or "That's not how it works in my BU — [explain what's different]."
- **Iterate in the same conversation.** Don't start over. Say "Now change the recommendation confidence to 62% and see how the UI should adapt."
- **Ask for the counterpoint.** After any prototype, ask "What's the strongest argument against this design?" Claude Code will stress-test its own output.
