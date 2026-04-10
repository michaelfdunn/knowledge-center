# Hub View — ASCII Layout

Source: Horizon AI-native fulfiller workspace (hub template)

## Screen Structure

```
┌──┬────────────────────────────────────────────────────────────────────────────────┐
│  │                                                                                │
│  │                         Welcome back, Anna                                     │
│  │                                                                                │
│  │               ┌─────────────────────────────────────────────┐                  │
│  │               │  +  Search or ask anything              >   │                  │
│  │               └─────────────────────────────────────────────┘                  │
│  │                 (pill-shaped, centered, max-width 768px)                        │
│  │                                                                                │
│  │           [🛡 Show high risk assets]  [✦ Help me manage assets]  [More ···]    │
│  │                                                                                │
│🏠│  ┌────────────────────────────────────────────────┐  ┌────────────────────────┐│
│  │  │ Accounts needing your judgment  (5)  All (5) ▾ │  │ AI insights        ↗  ││
│📋│  │                                                │  │                        ││
│  │  │  ┌────────────────────────────────────────┐    │  │      ┌──────────┐      ││
│📄│  │  │ ✧ AI recommendation · Expired 2 days   │    │  │     ╱    285     ╲     ││
│  │  │  │ Meridian Health — 2nd broken promise,   │    │  │    │ Total assets │    ││
│👤│  │  │ $180K overdue                           │    │  │     ╲           ╱     ││
│  │  │  │ Promised by Mar 7 · No payment by…     │    │  │      └──────────┘      ││
│🕐│  │  │                          [Review >]     │    │  │  49 NA  108 Lo  85 Me  ││
│  │  │  └────────────────────────────────────────┘    │  │                43 Hi   ││
│⚙ │  │                                                │  │                        ││
│  │  │  ┌────────────────────────────────────────┐    │  │  ┌──────────────────┐  ││
│  │  │  │ ✧ AI recommendation · High risk        │    │  │  │ ✦ Low-risk acct  │  ││
│  │  │  │ Vertex Labs — unresponsive, 3 dunning  │    │  │  │ outreach done…   │  ││
│  │  │  │ cycles, active orders conflict          │    │  │  │ [Review what AI  │  ││
│  │  │  │ No response to any outreach · $120K…   │    │  │  │  handled >]      │  ││
│  │  │  │                          [Review >]     │    │  │  └──────────────────┘  ││
│  │  │  └────────────────────────────────────────┘    │  └────────────────────────┘│
│  │  │                                                │                            │
│  │  │  ┌────────────────────────────────────────┐    │  ┌────────────────────────┐│
│  │  │  │ ✧ AI recommendation · High risk        │    │  │ Inventory          ↗  ││
│  │  │  │ Orion Industries — first late payment   │    │  │ As of today            ││
│  │  │  │ in 18 months, call brief ready          │    │  │                        ││
│  │  │  │                          [Review >]     │    │  │      ┌──────────┐      ││
│  │  │  └────────────────────────────────────────┘    │  │     ╱    285     ╲     ││
│  │  │                                                │  │    │ Total assets │    ││
│  │  │  ┌────────────────────────────────────────┐    │  │     ╲           ╱     ││
│  │  │  │ Task · Expired 1 day ago               │    │  │      └──────────┘      ││
│  │  │  │ Create control attestation              │    │  │                        ││
│  │  │  │ HR employee data 🟢 👤 Miley Simmons   │    │  │  ● 247 Managed AI     ││
│  │  │  │ Ran governance monitoring agent…        │    │  │  ● 38  Unmanaged AI   ││
│  │  │  │                          [Review >]     │    │  │                        ││
│  │  │  └────────────────────────────────────────┘    │  │  ┌──────────────────┐  ││
│  │  │                                                │  │  │ 5 assets from    │  ││
│  │  │  ┌────────────────────────────────────────┐    │  │  │ Moveworks at     │  ││
│  │  │  │ Task · Expired 1 day ago               │    │  │  │ high risk ✦ AI   │  ││
│  │  │  │ Complete PII sensitive data assessment  │    │  │  │ [Review with AI] │  ││
│  │  │  │                          [Review >]     │    │  │  └──────────────────┘  ││
│  │  │  └────────────────────────────────────────┘    │  └────────────────────────┘│
│  │  │                                                │                            │
│  │  └────────────────────────────────────────────────┘                            │
│  │                                                                                │
└──┴────────────────────────────────────────────────────────────────────────────────┘
```

## Layout Zones (Two-Zone Vertical)

```
┌──────┬────────────────────────────────────────────────────────┐
│ NAV  │     FULL-WIDTH TOP SECTION                             │
│ RAIL │                                                        │
│ 64px │     ┌──────────────────────────────────────────┐       │
│      │     │  GREETING  (centered)                    │       │
│dark  │     │  OMNIBAR   (centered, pill, max 768px)   │       │
│bg    │     │  PROMPT PILLS  (centered, wrapped)       │       │
│      │     └──────────────────────────────────────────┘       │
│      │                                                        │
│      │     TWO-COLUMN CONTENT                                 │
│      │     ┌──────────────────────┬──────────────────┐        │
│      │     │  INBOX CARD          │  SIDEBAR         │        │
│      │     │  (flex: 1)           │  (320px fixed)   │        │
│      │     │                      │                  │        │
│      │     │  Action items list   │  AI Insights     │        │
│      │     │  (stacked vertically)│  (donut + legend │        │
│      │     │                      │   + AI callout)  │        │
│      │     │                      │                  │        │
│      │     │                      │  Inventory       │        │
│      │     │                      │  (donut + stats  │        │
│      │     │                      │   + suggestion)  │        │
│      │     └──────────────────────┴──────────────────┘        │
└──────┴────────────────────────────────────────────────────────┘
```

## Component Breakdown

```
NAV RAIL  (left, 64px, rgb(22,50,60) — same on every screen)
├── ServiceNow 4-dot logo (green, teal, orange, dark — 28x28)
├── Home (active)
├── Queue / Work
├── Clipboard
├── Documents
├── People
├── History
├── Settings
├── (flex spacer)
└── User avatar (initials, gradient circle)

FULL-WIDTH TOP SECTION  (centered content, no background card)
├── GREETING
│   └── "Welcome back, [name]"  (28px, font-weight 400, centered)
│
├── OMNIBAR  (inline, centered, max-width 768px)
│   ├── Plus icon (muted)
│   ├── Text input ("Search or ask anything")
│   └── Send button (teal circle, arrow icon)
│
└── PROMPT SUGGESTIONS  (centered row of pill buttons)
    ├── Pill buttons with icon + label (transparent bg, hover: rgba(0,0,0,0.04))
    └── "More" pill with triple-dot icon

TWO-COLUMN CONTENT  (24px padding, 16px gap)
│
├── INBOX CARD  (flex: 1, white, border-radius 24px)
│   ├── HEADER ROW
│   │   ├── Section title ("Accounts needing your judgment")
│   │   ├── Count badge (pill, gray background)
│   │   └── Filter dropdown (right-aligned, "All (6)" + chevron)
│   │
│   └── ACTION LIST  (stacked vertically, max 5 items)
│       │
│       ├── "AI recommendation" cards
│       │   ├── Badges row: green gradient "AI recommendation" pill + red urgency pill
│       │   ├── Title (16px, font-weight 500)
│       │   ├── Meta text (14px, secondary color)
│       │   └── [Review >] button (teal, always present)
│       │
│       └── "Task" cards
│           ├── Badges row: gray "Task" pill + red urgency pill
│           ├── Title (16px, font-weight 500)
│           ├── Agent row (agent name + green presence dot + avatar + assignee name)
│           ├── Meta text (14px, secondary color)
│           └── [Review >] button (teal, always present)
│
└── SIDEBAR  (320px fixed width)
    │
    ├── AI INSIGHTS CARD  (white, border-radius 24px)
    │   ├── Header: title + expand button
    │   ├── Donut chart (160px, 4 segments: NA/Low/Medium/High)
    │   ├── Center label: total count + "Total assets"
    │   ├── Legend row (value + colored dot + label per segment)
    │   └── AI callout card (bordered, sparkle icon + text + action button)
    │
    └── INVENTORY CARD  (white, border-radius 24px)
        ├── Header: title + subtitle ("As of today") + expand button
        ├── Donut chart (160px, 2 segments: Managed/Unmanaged)
        ├── Center label: total count + "Total assets"
        ├── Stat rows (colored dot + label + value, stacked on gray bg)
        └── Suggestion card (bordered, text with AI badge + action button + overflow menu)
```

## Information Hierarchy (Reading Order)

```
1. GREETING            → "Who am I?" (centered, lightweight, sets context)
2. OMNIBAR             → "I can search or ask AI anything right now" (click to open chat overlay)
3. PROMPT SUGGESTIONS  → "Here are starting points if I'm not sure what to do"
4. INBOX CARDS         → "What needs my judgment?" (ranked action items, max 5)
   a. AI recommendations → "AI has a recommendation — review and decide"
   b. Tasks             → "Assigned work from agents and teammates"
   c. Every item has a [Review >] action — the specialist always has a clear next step
5. AI INSIGHTS         → "What's the risk landscape?" (sidebar — awareness, not action)
6. INVENTORY           → "What's my asset posture?" (sidebar — monitoring)
7. AI CALLOUT / SUGGESTION → "What did AI handle or discover?" (nested inside sidebar cards)
```

## Key Design Decisions in This Screen

- **Omnibar is inline above content, not fixed at bottom.** The omnibar sits centered below the greeting as the primary interaction surface. This positions search and AI conversation as the first thing a specialist can do — before even scanning the inbox. Clicking the omnibar opens a chat overlay (chat panel + detail panel) that slides over the hub content. The omnibar hides while chat is open. Closing chat restores the omnibar. This is the same omnibar→chat pattern used on the task view — the omnibar and chat are never visible at the same time.

- **No stats row pills.** The old layout used three metric pills (exceptions count, match rate, expiring discounts) as a portfolio health snapshot. The new layout removes this in favor of letting the inbox cards themselves communicate urgency through badges and ordering. Portfolio-level metrics move to the sidebar donut charts instead.

- **Two card types in the inbox replace the uniform queue.** Instead of identical decision-queue cards with colored left borders, the inbox distinguishes "AI recommendation" cards (green gradient badge, AI has a suggested action) from "Task" cards (gray badge, assigned work with agent context). This makes the nature of work immediately visible without opening the card.

- **All inbox items have equal visual weight.** No single card is highlighted with a glow or special border. Priority is communicated through ordering (most urgent first) and urgency badges, not visual emphasis on one item. This keeps the queue scannable and avoids anchoring the specialist on a single card.

- **Every item has a Review action.** Every card in the inbox includes a [Review >] button so the specialist always has a clear next step. The hub is a decision queue — every item is there because it needs judgment, so every item should be actionable.

- **Max 5 items in the inbox.** The hub shows at most 5 items to keep the decision queue focused. This is a morning briefing, not a full backlog. If there are more than 5 items needing judgment, the filter dropdown indicates the total count.

- **Sidebar uses donut charts, not stat tiles.** The old sidebar had a simple count-based "AI handled" summary (812 auto-matched, 7 duplicates, etc.) and cross-department dependency cards. The new sidebar uses visual donut charts for risk distribution (AI Insights) and asset posture (Inventory), with AI callouts nested inside each card. This is richer at a glance but more domain-specific.

- **No cross-department dependency cards.** The old layout surfaced blocked cross-department workflows (Procurement, Finance) with direct action buttons in the sidebar. The new template replaces these with domain-focused insight cards. Cross-department visibility would need to be added back as a third sidebar card or integrated into the inbox items if the use case requires it.

- **Prompt suggestion pills replace contextual subtitle.** The old greeting included a contextual subtitle ("Thursday, Mar 19 · Accounts Payable · Payment run in 2d") that framed the session. The new layout uses prompt suggestion pills below the omnibar instead — these are actionable rather than informational, guiding the specialist toward common starting queries.

- **Background is solid, not glassmorphic.** The background is flat #EDECE9 rather than a gradient or blurred image. Cards are white with border-radius 24px and subtle shadows. This is a cleaner, more restrained visual treatment than the glassmorphism described in the Horizon styling guide — appropriate for a data-dense hub where readability matters more than atmosphere.

- **No faded/deprioritized items.** With a max of 5 items, every card in the inbox earns its place and gets full visual weight. Lower-priority items simply don't appear — they're accessible through the full queue view, not dimmed in the hub.
