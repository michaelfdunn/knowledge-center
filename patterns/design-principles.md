# AI-Native Design Principles

## Three Surfaces, One Conversation Layer

The fulfiller workspace has three primary surfaces. Keep it simple.

### 1. The Hub
**Purpose:** The specialist opens this and knows what to focus on.

It answers one question: **What needs me right now?**

The hub should surface:
- Decisions ranked by business impact (not arrival time, not ticket number)
- What AI agents already handled (so the specialist knows what they DON'T need to do)
- Anything predicted to need attention soon

How you lay this out — cards, list, sections, tabs — is a design decision. The principle is: prioritized decisions first, everything else second.

The hub includes an inline omnibar below the greeting. Clicking it opens a chat overlay for conversational AI interaction.

### 2. The Task View
**Purpose:** The specialist is looking at one task and needs to act.

It answers three questions:
1. **What happened?** — AI summarizes the situation so the specialist can orient in seconds
2. **What should I do?** — Clear actions, not just information
3. **Why does AI think that?** — Evidence and reasoning so the specialist can trust or override intelligently

How you structure these three answers is your design space. They could be stacked vertically, split into panels, tabbed, progressive disclosure — that's what prototyping is for. The principle is: summary → action → evidence, in that priority order.

A floating omnibar sits at the bottom of every task view. It's the entry point to chat — clicking it opens the chat overlay (chat panel + detail panel), and the omnibar hides. Closing chat restores the omnibar.

### 3. The Chat View
**Purpose:** Collaborative resolution. AI + specialist + cross-department guests work together in a threaded conversation alongside a focused detail panel.

It answers one question: **How do we resolve this together?**

The chat view is the default when the case requires multi-party collaboration, when the specialist invokes it from the omnibar, or when conversational interaction is the primary mode. The detail panel on the right contains the FULL task view content (AI summary, recommendation cards, counterpoint, evidence, data tables, ghost actions, action bar — everything from the task view's scrollable content area, never condensed) plus a resolution progress card, so the specialist has complete context alongside the conversation.

### The Omnibar → Chat Pattern

The omnibar appears on every surface (except the standalone chat view, which starts with chat open). It's a persistent invitation to start a conversation with AI. The interaction flow:

1. **Omnibar visible** — specialist sees a floating pill ("Search or ask anything")
2. **Specialist clicks or types** — chat overlay slides in, omnibar hides
3. **Chat open** — full chat panel (460px left) + detail panel (right), matching chat-template.html styling
4. **Specialist closes chat** — overlay slides out, omnibar reappears

This means the omnibar and chat are **never visible at the same time**. The omnibar is the dormant state; chat is the active state.

---

## The Tier Model Drives Content, Not Layout

The L1/L2/L3 framework determines **what information the specialist needs and how much AI can do** — it doesn't prescribe screen layouts.

### L1 — AI Handles It
The specialist barely sees these. They show up in the hub as "AI handled" with an option to review or reopen. The design question is: how much visibility does the specialist need into work they didn't do?

### L2 — Human Judges, AI Supports
This is where the task view matters most. AI has done pre-work (gathering context, analyzing data, forming a recommendation) and the specialist applies judgment. The design question is: how do you present AI's analysis in a way that accelerates the specialist's thinking without overriding it?

### L3 — Human Leads, AI Informs
These involve other departments, authority escalations, or high-stakes decisions. The design question is: how does the specialist see what's happening across teams, and what actions can they take to unblock dependencies?

**Don't design three different screen types for three tiers.** The task view adapts its content based on tier — an L2 invoice variance and an L3 fraud escalation might use the same layout with different data density and different actions available.

---

## Design Principles (What to Hold Onto)

These principles should guide prototypes regardless of layout:

**1. Summary → Action → Evidence**
The specialist's reading order. Lead with what AI thinks, make the action obvious, then provide the supporting data for those who want to dig deeper. Don't bury the action under a wall of context.

**2. AI shows its work**
Every AI recommendation needs: what it recommends, how confident it is, and what evidence it used. The specialist decides whether to trust it. Without this, AI becomes a black box that specialists will learn to ignore.

**3. The counterpoint**
Show the strongest argument against AI's recommendation. This builds trust (AI considered alternatives) and supports the specialist's judgment (they see both sides). This isn't a required UI component — it's a content principle. How and where you show it is your call.

**4. Override is a first-class action**
The specialist must be able to disagree with AI as easily as they agree with it. Disagreement should be captured (why did they override?) because that's how the system learns. Don't make agreeing with AI the "easy path" and overriding the "extra work path."

**5. Cross-department visibility when it matters**
When a task touches another department, the specialist needs to see: who's involved, what's blocking, and what they can do about it. This doesn't require a separate screen — it might be a section within the task view that only appears when cross-department dependencies exist.

**6. 30 seconds to orient**
The specialist should be able to open any task and understand the situation in 30 seconds. If your prototype requires more than 30 seconds of reading before the action is clear, the information hierarchy needs work.

---

## What to Explore in Prototyping

These are open design questions, not solved patterns:

- **How much of the hub is predictive vs. reactive?** The "On the Horizon" concept (AI predicts what's coming) is powerful but untested. How much do specialists trust predictive signals?
- **Where does AI confidence change the UI?** When AI is 95% confident, the task view might be minimal. When it's 62% confident, the specialist needs more evidence. Should the layout adapt, or should the same layout just surface more data?
- **How does the task view scale from simple to complex?** An L2 invoice variance might need 3 data points. An L3 investigation might need interviews, evidence, policy references, and timelines. Same surface? Progressive disclosure? Separate depth layers?
- **What does the hub look like when AI handles 70% of cases?** If L1 is mostly invisible, the hub becomes fewer items, higher stakes per item, more context per decision. How does that change the design?
- **How does override feedback close the loop?** The specialist overrides AI today. Next week, AI handles a similar case correctly because of that override. Does the specialist see that improvement? How?

---

## For Claude Code: Prototype Generation Rules

When generating prototypes from this project:

1. **Default to three surfaces with a shared conversation layer:** The hub (prioritized decisions), the task view (summary + action), and the chat view (collaborative resolution). Every surface includes an omnibar that opens chat. This is the starting point, not a hard limit. If the use case or designer prompt suggests a more interesting structure, propose the alternative and explain the tradeoff.

2. **Content adapts to the tier, layout can too.** By default, an L2 and L3 task view use the same layout with different content density. But if a different layout would serve the specialist's cognitive needs better, propose it — and say why.

3. **Use realistic data from the persona files.** Don't generate generic placeholder data. Pull scenario details, metrics, and entity names from the persona that matches the designer's BU.

4. **Match ambition to the question.** If a designer asks what's possible, show what's possible. If they're refining a specific flow, keep it focused. A clean layout with real data is more valuable than a polished UI with generic content — but don't let "don't over-build" become an excuse for safe, predictable output.

5. **Call out design decisions you're making.** When you choose to put evidence in an expandable section vs. inline, say why. When you choose to show 3 actions vs. 2, say why. The designer needs to evaluate the information architecture, not just accept the first layout.

6. **Flag open questions.** If the use case surfaces a design tension (e.g., "should AI confidence change how much context is shown?"), call it out explicitly rather than making a silent choice.

7. **When invited to explore, explore.** If the designer signals they want something unexpected — "what if," "push it," "show me something different" — set aside the defaults and propose something genuinely novel. The framework is a lens, not a cage.
