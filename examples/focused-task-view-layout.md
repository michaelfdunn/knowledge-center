# Focused Task View — ASCII Layout

Source: Horizon AI-native fulfiller workspace (task-view-template.html — default task view)

## Screen Structure

```
┌──────┬──────────────────────────────────────────────────────────────────────┐
│      │  HEADER AREA  (white bg)                                            │
│      │  ┌────────────────────────────────────────────────────────────────┐  │
│      │  │ [clipboard icon] Home                                         │  │
│      │  │                                                               │  │
│  N   │  │ Meridian Health                          Agentic AI  Anna Quinn│ │
│  A   │  │ Last update: 2 days ago by Rick Adams     [●avatar] [●avatar]  │ │
│  V   │  │                                                   [Actions ▾] │  │
│      │  └────────────────────────────────────────────────────────────────┘  │
│  R   │                                                                     │
│  A   │  ┌────────────────────────────────────────────────────────────────┐  │
│  I   │  │     ( Overview )  ( Call log )  ( Details )  ( Account )       │  │
│  L   │  │     ━━━━━━━━━━━                                               │  │
│      │  └────────────────────────────────────────────────────────────────┘  │
│      │                                                                     │
│ ┌──┐ │  CONTENT SCROLL AREA                                                │
│ │⠿⠿│ │  ┌────────────────────────────────────────────────────────────────┐  │
│ │  │ │  │ ░░░░░░░░░░░░░░░ AI SUMMARY BANNER ░░░░░░░░░░░░░░░░░░░░░░░░░ │  │
│ │🏠│ │  │  ✦ AI summary                           [ⓘ] [↻] [▲]         │  │
│ │  │ │  │                                                               │  │
│ │📋│ │  │  Meridian Health owes $127K across 4 past-due invoices.       │  │
│ │  │ │  │  Rick Adams confirmed payment commitment on Jan 8 call but    │  │
│ │📄│ │  │  no remittance has arrived. DSO is 58 days against 30-day     │  │
│ │  │ │  │  target. Recommend escalation to Sales before placing hold.   │  │
│ │👤│ │  └────────────────────────────────────────────────────────────────┘  │
│ │  │ │                                                                     │
│ │🕐│ │  ┌────────────────────────────────────────────────────────────────┐  │
│ │  │ │  │  AI SPECIALIST RECOMMENDATION                          [⤢]   │  │
│ │⚙ │ │  │                                                               │  │
│ │  │ │  │  ┌──────────────────────────────────────────────────────────┐  │  │
│ │  │ │  │  │ ▎ Escalate to regional Sales lead before placing        │  │  │
│ │  │ │  │  │ ▎ account on credit hold.                               │  │  │
│ │  │ │  │  │ ▎                                                       │  │  │
│ │  │ │  │  │ ▎ Meridian is a top-20 account ($1.4M annual). A hold  │  │  │
│ │  │ │  │  │ ▎ without Sales awareness could disrupt a Q1 renewal.   │  │  │
│ │  │ │  │  └──────────────────────────────────────────────────────────┘  │  │
│ │  │ │  └────────────────────────────────────────────────────────────────┘  │
│ │  │ │                                                                     │
│ │  │ │  Key evidence from AI voice call                                    │
│ │  │ │  ┌────────────────────────────────────────────────────────────────┐  │
│ │  │ │  │ ░░░░░░░░░░░ EVIDENCE QUOTE CARD (yellow) ░░░░░░░░░░░░░░░░░░ │  │
│ │  │ │  │  👤 Rick Adams  ·  📅 Jan 8 2026                             │  │
│ │  │ │  │  "We're processing the batch now — expect remittance by       │  │
│ │  │ │  │  end of next week."                                          │  │
│ │  │ │  └────────────────────────────────────────────────────────────────┘  │
│ │  │ │                                                                     │
│ │  │ │  [Show all 3 triggers]  [Audit full transcript]                     │
│ │  │ │                                                                     │
│ │  │ │  ─────────── card actions ──────────                               │
│ │  │ │                [Override AI]  [Loop in Legal]  [Route to Sales >]   │
│ │  │ │                   (red)         (gray)           (dark primary)     │
│ │  │ │                                                                     │
│ └──┘ │                                                                     │
│      │  ┌──────────────────────────────────────────────────────────────┐   │
│      │  │  ▴  Search or ask anything                               ●  │   │
│ [AQ] │  └──────────────────────────────────────────────────────────────┘   │
└──────┴─────────────────────────────────────────────────────────────────────┘

CHAT OVERLAY  (slides up when omnibar is clicked — omnibar hides)
┌──────┬──────────────────────┬──────────────────────────────────────────────┐
│ NAV  │  CHAT PANEL (460px)  │  DETAIL PANEL (full task view content)       │
│      │                      │                                              │
│      │  Chat header + close │  Header area (breadcrumb, entity, assignees) │
│      │  Participants bar    │  Tab bar (pill group)                        │
│      │  Message thread      │  Panel scroll (ALL content from task view:   │
│      │  [AI, user, guest]   │    AI summary, recommendation, evidence,     │
│      │                      │    tables, ghost actions                      │
│      │  Chat input          │    + resolution progress card)               │
│      │                      │  Action bar                                  │
└──────┴──────────────────────┴──────────────────────────────────────────────┘

OVERRIDE MODAL  (centered overlay, 440px)
┌──────────────────────────────────────────┐
│  Override AI recommendation         [✕]  │
│  Explain why you're overriding...        │
│                                          │
│  ○ Inaccurate data                       │
│  ○ Customer relationship context         │
│  ○ Policy exception                      │
│  ○ Other                                 │
│                                          │
│  ┌────────────────────────────────────┐  │
│  │ Add a note (optional)...          │  │
│  └────────────────────────────────────┘  │
│                                          │
│                [Cancel]  [Confirm override] │
└──────────────────────────────────────────┘
```

## Component Breakdown

```
NAV RAIL  (left, 64px, rgb(22,50,60) — shared with hub)
├── ServiceNow 4-dot logo (28px, green/teal/orange/dark circles)
├── 🏠 Home          ← active: white icon, subtle white bg
├── 📋 Queue
├── 📄 Records
├── 👤 People
├── 🕐 History
├── ⚙ Settings
├── (flex spacer)
└── Avatar circle (32px, initials, gradient bg)

HEADER AREA  (white bg, padding 16px 24px)
├── BREADCRUMB ROW
│   ├── Clipboard icon (16px, stroke)
│   └── "Home" link
├── ENTITY ROW  (flex, space-between)
│   ├── LEFT
│   │   ├── Entity name (24px, 400 weight, rgb(2,6,23))
│   │   └── Meta line (14px, rgb(71,85,105)) — "Last update: 2 days ago by Rick Adams"
│   └── RIGHT
│       ├── Assignees row
│       │   ├── Agentic AI  (gray circle icon + label)
│       │   └── Anna Quinn  (gradient circle icon + label)
│       └── Actions dropdown button (gray pill, 12px, chevron)

TAB BAR  (centered, padding 10px 24px)
└── Pill group container (white bg, 1px border, full-radius)
    ├── Overview    ← active: rgb(23,43,49) bg, white text
    ├── Call log
    ├── Details
    └── Account

CONTENT SCROLL  (flex: 1, overflow-y auto, padding 24px)
├── AI SUMMARY BANNER  (gradient: rgb(210,252,203) → rgb(202,239,255), border-radius 16px)
│   ├── Header row
│   │   ├── Sparkle icon (green stroke) + "AI summary" (18px, 500 weight)
│   │   └── Controls: info, refresh, collapse (32px icon buttons)
│   └── Summary paragraph (16px, line-height 1.5, rgb(30,41,59))
│
├── AI SPECIALIST RECOMMENDATION  (white card, border-radius 24px, shadow)
│   ├── Header row
│   │   ├── "AI Specialist recommendation" (16px, 500 weight)
│   │   └── Expand button (24px icon)
│   └── Content block (gray bg #f4f3f0, border-radius 16px)
│       ├── Action line (16px, 500 weight, bold) — the recommended action
│       └── Reasoning text (16px, rgb(71,85,105)) — why AI recommends this
│
├── EVIDENCE SECTION
│   ├── Label: "Key evidence from AI voice call" (16px, rgb(77,76,74))
│   └── Quote card (yellow bg #fffbc9, border-radius 24px, shadow)
│       ├── Speaker row: avatar icon + name + date with calendar icon
│       └── Verbatim quote text (16px, rgb(2,6,23))
│
└── GHOST ACTIONS  (transparent buttons, 12px, rgb(55,68,74))
    ├── "Show all 3 triggers"
    └── "Audit full transcript"

CARD ACTIONS  (inside recommendation card, border-top separator)
├── [Override AI]         ← red (rgb(226,22,28)), opens override modal
├── [Loop in Legal]       ← secondary gray (rgb(226,232,240))
└── [Route to Sales >]    ← dark primary (rgb(23,43,49)), white text

FLOATING OMNIBAR  (fixed bottom center, z-index 50, offset left 64px for nav)
└── Pill bar (420px, white, full-radius, shadow)
    ├── Chevron-up icon (muted)
    ├── Placeholder text: "Search or ask anything" (14px, muted)
    └── Send button (32px circle, green bg rgb(45,117,36), white arrow)

CHAT OVERLAY  (triggered by clicking omnibar — omnibar hides, overlay slides in)
├── CHAT PANEL (460px, white bg, left side)
│   ├── Header: sparkle icon + title + close button
│   ├── Participants bar (avatar stack + labels + active badge)
│   ├── Messages area (AI, user, guest bubbles + system messages)
│   └── Chat input (24px radius, send button)
└── DETAIL PANEL (flex: 1, right side, #EDECE9 bg)
    ├── Header area (same breadcrumb/entity/assignees as task view)
    ├── Tab bar (same pill group)
    ├── Panel scroll (FULL task view content — never condensed:
    │     AI summary, recommendation card, counterpoint, evidence,
    │     data tables, ghost actions + resolution progress card)
    └── Action bar (pinned bottom, same buttons as task view)

OVERRIDE MODAL  (centered overlay, 440px, border-radius 24px)
├── Header: "Override AI recommendation" + close button
├── Subtitle: explanation prompt
├── Reason options (radio buttons, 1.5px bordered rows, red accent on select)
├── Textarea: "Add a note (optional)..."
└── Actions: [Cancel] secondary + [Confirm override] red
```

## Information Hierarchy (Reading Order)

```
1. BREADCRUMB         →  "Where did I come from?" (navigation context)
2. ENTITY NAME        →  "Who is this about?" (account/case identity)
3. META + ASSIGNEES   →  "When was this touched? Who's on it?"
4. TAB BAR            →  "What view am I in?" (Overview is the AI-curated default)
5. AI SUMMARY         →  "What's the situation?" (full context in one paragraph)
6. RECOMMENDATION     →  "What should I do and why?" (AI's suggested action + reasoning)
7. EVIDENCE           →  "What proof supports this?" (verbatim source material)
8. GHOST ACTIONS      →  "Is there more?" (progressive disclosure to deeper data)
9. CARD ACTIONS       →  "What are my choices?" (override / escalate / route)
10. OMNIBAR           →  "I can ask AI anything about this case" (entry point to chat)
```

### Chat Overlay Reading Order (when omnibar is clicked)
```
1. CHAT THREAD        →  "What's the conversation so far?"
2. DETAIL PANEL       →  "Full task context alongside the conversation"
                         (identical content to main task view — never condensed)
3. CHAT INPUT         →  "How do I contribute?" (type a message or question)
```

## Key Design Decisions in This Screen

- **Entity name leads, not the task action.** The specialist orients on
  *who* first (Meridian Health), then gets AI-assembled context about
  *what* and *why*. This is an account-centric model — the entity is the
  anchor, and the AI summary provides the situational briefing.

- **AI summary is a gradient banner, visually distinct from the task
  content.** The green-to-blue gradient signals "this is AI-generated
  context" without requiring a label to carry that weight alone. The
  sparkle icon and "AI summary" title reinforce the source. Controls
  (info, refresh, collapse) give the specialist agency over the AI
  content.

- **Recommendation and evidence are separate cards.** The AI's suggested
  action lives in a white card with a gray inset block. Evidence lives
  in a yellow quote card below. This separation means the specialist
  reads the recommendation first, then checks the evidence — Summary,
  Action, Evidence reading order.

- **Evidence uses verbatim quotes on yellow.** The yellow background
  (#fffbc9) visually distinguishes primary source material from AI
  interpretation. Speaker name and date give provenance. This supports
  "AI shows its work" — the specialist can verify the recommendation
  against the actual words spoken.

- **Ghost actions for progressive disclosure.** "Show all 3 triggers"
  and "Audit full transcript" are low-chrome text buttons that don't
  compete with the primary reading flow. The default view shows only
  what's needed for the decision. Deeper data is one click away.

- **Three actions in the bottom bar, not two.** Override (red), Loop
  in Legal (secondary), Route to Sales (primary). This reflects an
  L2/L3 decision where the specialist has multiple valid paths — not
  just approve/reject. The primary action (Route to Sales) is the
  AI-recommended path. Override is red to signal it contradicts AI.

- **Override opens a modal with reason capture.** The override modal
  asks *why* the specialist disagrees — radio options plus a free-text
  note. This is the learning loop: every override is training data.
  The red accent on selection reinforces that this is a deliberate
  departure from the AI recommendation.

- **Floating omnibar opens the chat overlay.** The omnibar sits at
  the bottom center as a persistent invitation to chat with AI. When
  clicked, a full chat overlay slides up (chat panel left + detail
  panel right), and the omnibar hides. Closing chat restores the
  omnibar. This means the omnibar and chat are never visible at the
  same time — the omnibar is the dormant state, chat is the active
  state. Action buttons move into the recommendation card to keep
  the bottom clear for the omnibar.

- **No impact banner, step indicators, or 3-block task card.** The
  previous layout used an impact banner above the content, step
  progress (1 of 3), and a monolithic task card containing title,
  context, summary, reasoning, and actions. This template breaks
  those into separate, independently scannable components: banner,
  recommendation card, evidence card. The reading flow is flatter
  and the specialist can focus on each block independently.

- **Tabs separate AI-curated vs. raw views.** "Overview" is the
  AI-assembled view (default active). "Call log," "Details," and
  "Account" provide raw data access. The specialist can always drop
  out of the AI view into the underlying record.

- **Background is warm gray (#EDECE9), not white.** The page
  background creates contrast with the white header area and white
  content cards. The AI summary banner's gradient and the yellow
  evidence card both pop against this neutral tone.
