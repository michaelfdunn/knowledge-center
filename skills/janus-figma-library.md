# Janus Figma Library

**Invocation:** `/janus-figma-library <use-case description>`

Design and generate pixel-perfect Figma screen flows for any employee self-service use case, following the Moveworks/ServiceNow design system. Output is real Figma frames created directly in a target Figma file.

---

## What This Skill Does

1. **Analyzes** the use case to determine which screens are needed
2. **Plans** the full screen flow (states, transitions, panels)
3. **Creates** all frames directly in Figma using `mcp__figma__use_figma`
4. **Applies** exact design system tokens, components, and layout patterns from the reference library

---

## Required Workflow — Follow Every Step

### Step 1 — Parse the Use Case

Identify:
- **Actor**: employee, manager, HR admin, IT staff?
- **Entry surface**: home dashboard, inbox, search, existing chat?
- **Core action**: view info, approve/reject, submit form, search, get AI answer?
- **Resolution**: success state, panel opens, form submitted, notification sent?

Write a one-paragraph internal summary before proceeding.

### Step 2 — Map to Screen Templates

Select screens from the **Screen Template Library** below. Typical flows: 4–8 screens. Required minimum: idle → triggered → AI working → AI response. Add panels and split-views for action-heavy flows.

### Step 3 — Get the Figma Target File

If user has not provided a Figma URL:
1. Call `mcp__figma__whoami` to get plan keys
2. Call `mcp__figma__create_new_file` with `"[Use Case] — Self-Service Flow"` and `editorType: "design"`
3. Note returned `fileKey` — use for all subsequent `use_figma` calls

### Step 4 — Create Each Screen in Figma

For each screen:
- Call `mcp__figma__use_figma` with JavaScript to build that frame
- Use **Design Token Reference** and **Component Patterns** below for ALL values
- Name frames: `"01 — Home Default"`, `"02 — Home Query Typed"`, etc.
- Space frames 80px apart: `x = screenIndex * (1440 + 80)`
- **One screen per `use_figma` call** — never try to build multiple screens in one call

### Step 5 — Verify

After all screens, call `mcp__figma__get_screenshot` on first and last frame. Report Figma file URL to user.

---

## Design Token Reference

**CRITICAL: These are the ONLY values to use. Every token below is from the actual Figma source.**

### Colors (verified from Figma source)
```
Page background:          #f4f3f0   warm off-white (NOT #f8f7f4)
Surface (panel/card):     #ffffff
Input bar outer bg:       #edece9   (NOT #f8f7f4)
Sidebar / nav bg:         #f4f3f0
Border default:           #e3e2df   (NOT #e1e0dd)
Border subtle:            #edece9
Suggestion pill border:   #edece9

Text — primary:           #000000   (pure black for headings)
Text — dark body:         #172b3e   (NOT #0f172a — this is the dark navy text)
Text — secondary:         #656462   (NOT #475569 — sidebar labels, meta)
Text — disabled:          #94a3b8

Active nav item bg:       #c2c1be   (NOT #e1e0dd)
Dark card / CTA buttons:  #172b3e   (NOT #1e293b)
Green (overdue/success):  #4edf61   (NOT #16a34a — bright lime green)
Teal (AI/links):          #1389aa
Amber (warning text):     #ca8a04
Amber bg:                 #fef9c3
Red (error):              #dc2626
Red bg:                   #fee2e2

Notification badge:       #e2161c
AI banner bg:             rgba(19,137,170,0.07)
AI banner border:         #1389aa
User avatar bg:           rgba(0,123,158,0.16)
```

### Figma RGB equivalents (0–1 range for plugin API)
```
#f4f3f0  → { r: 0.957, g: 0.953, b: 0.941 }
#edece9  → { r: 0.929, g: 0.925, b: 0.914 }
#e3e2df  → { r: 0.890, g: 0.886, b: 0.875 }
#c2c1be  → { r: 0.761, g: 0.757, b: 0.745 }
#172b3e  → { r: 0.090, g: 0.169, b: 0.243 }
#4edf61  → { r: 0.306, g: 0.875, b: 0.380 }
#1389aa  → { r: 0.075, g: 0.537, b: 0.667 }
#656462  → { r: 0.396, g: 0.392, b: 0.384 }
#e2161c  → { r: 0.886, g: 0.086, b: 0.110 }
#000000  → { r: 0, g: 0, b: 0 }
#ffffff  → { r: 1, g: 1, b: 1 }
```

### Typography
```
Font family UI:    "Inter" for input text, chips, buttons
Font family UI:    "Inter" for all Figma-generated text (ServiceNow Sans not available)
NOTE: Use "Inter" for ALL text in Figma plugin API — it's the available substitute

Greeting heading:     36px / Regular / #000000    line-height: 40px
Section heading:      18px / Semi Bold / #172b3e  line-height: 25.2px
Body large:           16px / Regular / #172b3e    line-height: 24px
Body:                 14px / Regular / #172b3e    line-height: 19.6px
Label / secondary:    14px / Regular / #656462    line-height: 20px
Caption label:        12px / Regular / #656462    line-height: 20px
Nav button text:      14px / Regular / #000000    line-height: 24px
Disclaimer:           13px / Regular / #656462
```

### Spacing & Shape
```
Page frame:           1440 × 900 px
Nav sidebar:          280px expanded / 40px icon-only collapsed
Content center width: 768px (home omnibar/greeting centered)
Widget grid width:    1118px (3 columns, home dashboard)
Chat column max:      ~520px (left panel in split view)

Border radius:
  Cards:              24px (white cards), 32px (To-dos dark card)
  Pills / nav items:  9999px (rounded-full)
  Input bar outer:    38px
  Input bar inner:    32px
  Suggestion chips:   9999px
  Icon tiles:         16px (action shortcut icons)
  Widget inner items: 16px

Shadows:
  Input bar outer:    0px 20px 25px -5px rgba(0,0,0,0.1), 0px 8px 10px -6px rgba(0,0,0,0.1)
  Input inner:        0px 0px 8px 0px rgba(0,0,0,0.05)
  Right panel/card:   0px 1px 2px rgba(0,0,0,0.05)
```

---

## Screen Template Library

### T-HOME: Home Dashboard (Default)

**Layout**: Sidebar 280px left + content area flex-1

**Sidebar (expanded, 280px × 900px, bg #f4f3f0, right border #e3e2df)**:
- Top: "servicenow" text logo left + hamburger icon right, row h=40px, pl=12px
- Primary nav items (h=40px each, rounded-full, pl=10px, pr=12px):
  - "Home" — active → bg `#c2c1be`
  - "Calendar Management"
  - "World Knowledge"
- "Recent" label (12px, `#656462`) + pencil-edit icon right at 32px
- Recent items (h=40px, rounded-full, pl=10px):
  - "New Employee Hub Navigation"
  - "It's Time to Register for Benefits"
  - "Don't forget to finish your requir..."
  - "See all" → with chevron right icon
- Horizontal divider `h-px bg-[#e3e2df]`
- "My Apps" label (12px, `#656462`)
- My Apps items (h=40px, rounded-full, pl=10px):
  - "Enterprise Search" (search icon)
  - "My canvas" (grid icon)
  - "Activity hub" (pulse icon)
  - "Org chart" (tree icon)
- Bottom section: "Notifications" with red badge "1" + "Renee Simmons" user row

**Content area (bg #f4f3f0, pt=124px, px=24px)**:
- Greeting: "Good afternoon, [Name]! 👋" — 36px, regular, #000000, centered
- Omnibar (768px wide, centered):
  - Outer: bg `#edece9`, border `#edece9`, rounded-[38px], shadow `0px 20px 25px -5px rgba(0,0,0,0.1), 0px 8px 10px -6px rgba(0,0,0,0.1)`, p=8px
  - Inner: bg white, border `#edece9`, rounded-[32px], shadow `0px 0px 8px rgba(0,0,0,0.05)`, h=120px
  - Text area (top 56px): placeholder "Find answers, automate tasks, or create content instantly" — 16px Inter Regular `#656462`
  - Bottom row (64px): attachment icon left (40×40, bg transparent) + send button right (40×40, bg `#c2c1be` when empty, rounded-full)
- Suggestion chips row (gap 12px, centered):
  - Each chip: bg white, border `#edece9`, rounded-full, h=40px, px=16px, py=8px, text 14px Inter Regular black
  - "Improve writing" | "Summarize" | "Find information" | "Research events"
- 3-column widget grid (1118px wide, gap 16px):
  - **Col 1 — To-dos dark card** (bg `#172b3e`, rounded-32px):
    - Header "To-dos" 18px white
    - 3 pill columns: Open 12 / Due soon 3 / Overdue 3
    - Pill bg: `#22405c` (inner dark circle bg `#172b3e`)
    - Overdue pill bg: `#4edf61` with text `#172b3e`
    - Summary text 14px white light weight
    - Below card: 2-column quick action grid (Report an issue / Ask an HR question / etc) white card, icon tiles bg `#f4f3f0` rounded-16px
  - **Col 2 — News card** (bg white, rounded-24px):
    - Image placeholder top (16:9 ratio, rounded-16px, bg `#e3e2df`)
    - Title 24px, body 14px light
    - 2 sub-story rows with 80×80 image thumbs
  - **Col 3 — Suggested actions + Holiday card**:
    - Suggested actions (bg white, rounded-24px): 2 items, each with white icon tile + text
    - Holiday card (bg `#172b3e`, rounded-24px): date in `#4edf61` pill + "Next Holiday" title white

### T-HOME-QUERY: Home — Query Typed State

Identical to T-HOME but:
- Sidebar shows typed conversation in Recent: first item highlighted `bg-[#c2c1be]`
- Omnibar shows typed text — 16px Inter Regular `#000000` (not placeholder gray)
- Send button active: bg `#172b3e` rounded-full
- Sidebar shows "Team approvals" or use-case specific item at top of Recent (highlighted)

### T-NOTIFY: Notifications Panel Overlay

Rendered on top of T-HOME (home visible behind at ~60% opacity):
- Panel: x=195px (left of sidebar edge), width=304px, full height, bg white, shadow right
- Header row (h=56px, px=16px): "Notifications (2)" 16px SemiBold + X button
- Notification items (bg white, border-b `#edece9`, px=16px, py=12px):
  - **Announcement item**: megaphone icon + "General announcement" + "2h" timestamp
    - Body text 14px
    - CTA button: "Try it out now!" — outline pill, border `#e3e2df`, rounded-full
  - **Approval item (expanded)**: file icon + "Approval Request" + "3h"
    - "New Pending Approval Request" + ticket ID
    - Description block 14px
    - "Approve" button: outline pill
  - **Second Approval item**: same pattern, collapsed/summary

### T-CHAT-WORKING: AI Assistant — Processing State

**Layout**: Same sidebar (280px) + content area full width

**Chat header bar** (h=64px, px=24px, bg #f4f3f0, border-b #e3e2df):
- "Moveworks Assistant" — 18px SemiBold `#172b3e`, left
- MOVEWORKS logo text "MOVEWORKS from ServiceNow" — right, small

**Chat area** (flex-1, overflow-y-auto, px=24px, py=16px):
- **User message block** (mb=24px):
  - Row: user avatar circle left (24×24, bg green `#4edf61`, initial letter, font 10px) + name "Jason Wang" + timestamp gray
  - Message text: 16px Regular `#172b3e`, mt=4px, ml=32px (indented past avatar)
- **Assistant message block**:
  - Row: Moveworks avatar circle left (24×24, bg `#172b3e`) + "Moveworks Assistant" + timestamp gray
  - Working state text (ml=32px, mt=4px), 16px `#172b3e`:
    - Line 1: "⏳ Working on: "[query text in quotes]""
    - Line 2: "✓ [step 1 complete]" (text `#656462`)
    - Line 3: "→ [step 2 in progress]" (text `#172b3e`)
  - NOTE: NO white card, NO progress bars — just plain inline text with emoji/symbols

**Input bar** (pinned bottom, mx=24px, mb=16px):
- Same as T-HOME omnibar: outer `#edece9` rounded-[38px] with xl shadow, inner white rounded-[32px] with soft shadow
- Placeholder text: "Find answers, automate tasks, or create content instantly"

### T-CHAT-RESPONSE: AI Assistant — Response

Same as T-CHAT-WORKING layout but assistant message shows response:
- Response text: numbered list or paragraphs, 16px Regular `#172b3e`, ml=32px
- Reference cards below response (bg white, rounded-24px, p=12px, border `#e3e2df`, gap=8px):
  - Source logo/icon (Notion N, etc.) + "Source Title" — 14px Medium `#172b3e`
  - Snippet — 14px `#656462`
  - Timestamp label — 12px `#656462`
  - 2–3 cards side by side (each ~220px wide) or stacked
- Reaction row (mt=12px, ml=32px): 👍 👎 ℹ️ + "Get help" text 14px `#656462`
- Disclaimer: "Check references as AI generated responses may contain inaccuracies." — 13px `#656462`

### T-CHAT-SPLIT: AI Chat + Right Panel

**Layout**: Collapsed sidebar (icon strip 40px) + chat area (~530px) + right panel (flex-1)

**Collapsed sidebar** (40px wide, full height, bg `#f4f3f0`, border-r `#e3e2df`):
- Hamburger icon at top (40×40)
- Icon-only nav items (40×40 each, centered): Home, Calendar, World Knowledge, Recent/clock, Search, Canvas/grid, Brush, OrgChart
- Bottom: bell icon with red badge + user avatar

**Chat area** (~530px, full height, bg `#f4f3f0`):
- Chat header: "Moveworks Assistant" + MOVEWORKS logo
- Conversation history (user + assistant messages, plain text style)
- Input bar pinned bottom

**Right panel** (flex-1, full height, bg `#f4f3f0`, pt=8px, pr=0):
- Panel: bg white, rounded-tl-[24px] rounded-tr-[24px], h=full, shadow
- Panel header (h=56px, px=24px, border-b `#edece9`): title + expand icon (↗) + X close icon (right)

**Right panel content variants:**
- **Approvals panel**:
  - AI Insights banner: bg `rgba(19,137,170,0.07)` border `#1389aa` rounded-[20px], "✦ AI Insights" teal + body text
  - Items: user avatar + name + date range + "Approve" outline button + 3-dot menu
  - Footer: "Reject all" outline pill + "Approve all" dark `#172b3e` pill
- **Form panel**:
  - Form fields in bordered container `#e3e2df` rounded-2xl
  - AI-prefilled badge `bg-[#fef9c3] text-[#ca8a04]`
  - Submit CTA `bg-[#172b3e] text-white rounded-full`
- **Knowledge Article panel**:
  - Title 24px SemiBold
  - Metadata row: "Updated N ago" chip + Language dropdown + "Copy link" button
  - Author line: name link + role
  - Hero image 100% width, rounded-16px
  - Body text sections

### T-INBOX: Inbox — To-Do List

**Layout**: Same sidebar (280px expanded) + content area

**Content area** (px=40px, pt=40px):
- "Inbox" — 30px Regular `#172b3e`
- Tabs: "To-dos (N)" | "Everything else (N)" — active tab has bottom border `#1389aa` teal
- Sort label: "Prioritized by AI" 13px `#656462`
- Item rows: bg white, rounded-24px, px=20px, py=16px, border `#e3e2df`, mb=8px
  - Priority badge left: Overdue = `bg-[#fee2e2] text-[#dc2626]` / Due soon = `bg-[#fef9c3] text-[#ca8a04]`
  - Title 15px Medium `#172b3e` + description 13px `#656462`
  - Right: "Decline" outline pill + "Approve" dark `#172b3e` pill

---

## Component Code Patterns (Figma Plugin API)

These are verified, production-accurate patterns. Use exactly as shown.

**CRITICAL RULE: NEVER use `figma.createEllipse()` for any node that needs `appendChild()` or child text.**
`EllipseNode` has no `appendChild` method — calling it causes `TypeError: not a function`.
Always use `figma.createFrame()` with `cornerRadius = 9999` (pill/circle) or `cornerRadius = 12` (24px avatar) instead.

**CRITICAL RULE: Paint opacity goes on the Paint object, NOT inside the color.**
WRONG: `{ type:'SOLID', color:{ r, g, b, a:0.07 } }` — `a` is ignored/invalid in color objects.
CORRECT: `{ type:'SOLID', color:{ r, g, b }, opacity:0.07 }` — `opacity` is a top-level Paint field.

### Helper: Create Text Node
```js
async function mkText(parent, content, x, y, size, style, hex) {
  await figma.loadFontAsync({ family: "Inter", style });
  const t = figma.createText();
  t.fontName = { family: "Inter", style };
  t.fontSize = size;
  t.characters = content;
  t.x = x; t.y = y;
  const r = parseInt(hex.slice(1,3),16)/255;
  const g = parseInt(hex.slice(3,5),16)/255;
  const b = parseInt(hex.slice(5,7),16)/255;
  t.fills = [{ type:'SOLID', color:{r,g,b} }];
  parent.appendChild(t);
  return t;
}
```

### Page Frame
```js
const frame = figma.createFrame();
frame.name = "01 — Home Default";
frame.resize(1440, 900);
frame.x = 0; // use screenIndex * 1520 for subsequent screens
frame.fills = [{ type:'SOLID', color:{ r:0.957, g:0.953, b:0.941 } }]; // #f4f3f0
```

### Navigation Icon SVG Reference (sn-aix-icon system)

All icons are 20×20 viewBox. Used with `figma.createNodeFromSvgAsync()`.

```js
const NAV_ICONS = {
  'home-fill': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" width="20" height="20"><path fill="#343331" d="M10.707 2.293a1 1 0 00-1.414 0l-7 7a1 1 0 001.414 1.414L4 10.414V17a1 1 0 001 1h4a1 1 0 001-1v-3h2v3a1 1 0 001 1h4a1 1 0 001-1v-6.586l.293.293a1 1 0 001.414-1.414l-7-7z"/></svg>`,
  'calendar-outline': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" width="20" height="20" fill="none" stroke="#343331" stroke-width="1.5"><rect x="3" y="4" width="14" height="14" rx="2"/><path stroke-linecap="round" d="M3 8h14M7 2v4M13 2v4"/></svg>`,
  'globe-outline': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" width="20" height="20" fill="none" stroke="#343331" stroke-width="1.5"><circle cx="10" cy="10" r="8"/><path d="M10 2C10 2 7 6 7 10s3 8 3 8M10 2c0 0 3 4 3 8s-3 8-3 8M2 10h16"/></svg>`,
  'magnifying-glass-outline': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" width="20" height="20" fill="none" stroke="#343331" stroke-width="1.5"><circle cx="9" cy="9" r="6"/><path stroke-linecap="round" d="M15 15l3 3"/></svg>`,
  'grid-three-outline': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" width="20" height="20" fill="none" stroke="#343331" stroke-width="1.5"><rect x="2" y="2" width="7" height="7" rx="1"/><rect x="11" y="2" width="7" height="7" rx="1"/><rect x="2" y="11" width="7" height="7" rx="1"/><rect x="11" y="11" width="7" height="7" rx="1"/></svg>`,
  'lightning-outline': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" width="20" height="20" fill="none" stroke="#343331" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M11.5 2L4.5 11H10L8.5 18.5L16.5 9H11L11.5 2Z"/></svg>`,
  'tree-outline': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" width="20" height="20" fill="none" stroke="#343331" stroke-width="1.5"><rect x="8" y="1" width="4" height="4" rx="1"/><rect x="2" y="14" width="4" height="4" rx="1"/><rect x="14" y="14" width="4" height="4" rx="1"/><path d="M10 5v4M4 14V11h12v3"/></svg>`,
  'bell-outline': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" width="20" height="20" fill="none" stroke="#343331" stroke-width="1.5"><path stroke-linecap="round" d="M10 2a6 6 0 00-6 6v3l-1.5 2.5h15L16 11V8a6 6 0 00-6-6z"/><path d="M8.5 17.5a1.5 1.5 0 003 0" stroke-linecap="round"/></svg>`,
  'menu-close-outline': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" width="20" height="20" fill="none" stroke="#343331" stroke-width="1.5"><path stroke-linecap="round" d="M4 6h12M4 10h12M4 14h12"/></svg>`,
  'chevron-right-outline': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" width="16" height="16" fill="none" stroke="#656462" stroke-width="1.5"><path stroke-linecap="round" d="M6 4l4 4-4 4"/></svg>`,
  'document-edit': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 14.001 14" width="14" height="14" fill="#343331"><path d="M5.833 2.33a.389.389 0 010 .778H5.055c-1.111 0-1.905.001-2.508.082-.593.08-.943.23-1.2.488-.257.257-.407.607-.487 1.2-.08.603-.082 1.397-.082 2.508V8.94c0 1.111.001 1.905.082 2.508.08.593.23.943.488 1.2.257.257.607.407 1.2.487.603.08 1.397.082 2.508.082h1.555c1.111 0 1.905-.001 2.508-.082.593-.08.943-.23 1.2-.488.257-.257.407-.607.487-1.2.081-.603.082-1.397.082-2.508v-.777a.389.389 0 01.778 0v.777c0 1.089.001 1.945-.09 2.612-.088.678-.28 1.22-.696 1.636-.416.416-.958.608-1.636.697-.667.09-1.523.089-2.612.089H5.055c-1.089 0-1.945.001-2.612-.09-.678-.088-1.22-.28-1.636-.696C.391 13.072.199 12.53.11 11.852.02 11.185.02 10.33.02 9.24V7.685c0-1.09-.001-1.945.09-2.612.088-.678.28-1.22.696-1.636.416-.416.958-.608 1.636-.697.667-.09 1.523-.089 2.612-.089h.778z"/><path d="M10.596.584a1.834 1.834 0 012.822 0 1.834 1.834 0 010 2.822L7.07 9.754a.389.389 0 01-.154.099l-3.035 1.012a.389.389 0 01-.491-.491l1.012-3.035a.389.389 0 01.099-.154L10.596.584zM4.863 7.418L4.003 10l2.582-.86 5.278-5.283-1.72-1.72-5.28 5.281zm8.004-6.283a1.056 1.056 0 00-1.45 0l-.455.453 1.72 1.72.453-.453a1.056 1.056 0 000-1.45z"/></svg>`,
};
```

### Nav Item (expanded sidebar button with icon)
```js
// ALWAYS pass an iconKey — never build a nav item without an icon
async function navItem(sidebar, label, y, isActive, iconKey) {
  const item = figma.createFrame();
  item.name = `nav-${label}`;
  item.resize(248, 40);
  item.x = 16; item.y = y;
  item.cornerRadius = 9999;
  item.fills = isActive
    ? [{ type:'SOLID', color:{ r:0.761, g:0.757, b:0.745 } }] // #c2c1be
    : [];
  sidebar.appendChild(item);

  if (iconKey && NAV_ICONS[iconKey]) {
    const iconNode = await figma.createNodeFromSvgAsync(NAV_ICONS[iconKey]);
    iconNode.name = `icon-${iconKey}`;
    iconNode.x = 8; iconNode.y = 10;
    item.appendChild(iconNode);
  }

  await figma.loadFontAsync({ family:"Inter", style:"Regular" });
  const t = figma.createText();
  t.fontName = { family:"Inter", style:"Regular" };
  t.fontSize = 14;
  t.characters = label;
  t.fills = [{ type:'SOLID', color:{ r:0, g:0, b:0 } }];
  t.x = iconKey ? 36 : 10; t.y = 10;
  item.appendChild(t);
  return item;
}
```

### buildExpandedSidebar() — USE THIS FOR ALL SCREENS WITH FULL SIDEBAR
```js
// Call once per screen. Replaces all inline sidebar code.
// activeItem: "Home" | "Calendar Management" | "World Knowledge"
// recentItems: string[] (up to 3)
async function buildExpandedSidebar(frame, activeItem, recentItems) {
  const sidebar = figma.createFrame();
  sidebar.name = "Sidebar";
  sidebar.resize(280, 900); sidebar.x = 0; sidebar.y = 0;
  sidebar.fills = [{ type:'SOLID', color:{ r:0.957, g:0.953, b:0.941 } }]; // #f4f3f0
  sidebar.strokeWeight = 1; sidebar.strokeAlign = 'OUTSIDE';
  sidebar.strokes = [{ type:'SOLID', color:{ r:0.890, g:0.886, b:0.875 } }]; // #e3e2df
  frame.appendChild(sidebar);

  // Logo + hamburger
  await mkText(sidebar, "servicenow", 28, 18, 14, "Semi Bold", "#172b3e");
  const burgerBtn = figma.createFrame();
  burgerBtn.resize(40, 40); burgerBtn.x = 232; burgerBtn.y = 0;
  burgerBtn.cornerRadius = 8; burgerBtn.fills = [];
  sidebar.appendChild(burgerBtn);
  const burgerIcon = await figma.createNodeFromSvgAsync(NAV_ICONS['menu-close-outline']);
  burgerIcon.x = 10; burgerIcon.y = 10;
  burgerBtn.appendChild(burgerIcon);

  // Primary nav — icons always included
  await navItem(sidebar, "Home",               56,  activeItem === "Home",               'home-fill');
  await navItem(sidebar, "Calendar Management", 96,  activeItem === "Calendar Management", 'calendar-outline');
  await navItem(sidebar, "World Knowledge",    136, activeItem === "World Knowledge",    'globe-outline');

  // Recent section
  await mkText(sidebar, "Recent", 26, 192, 12, "Regular", "#656462");
  const newChatBtn = figma.createFrame();
  newChatBtn.resize(32, 32); newChatBtn.x = 232; newChatBtn.y = 188;
  newChatBtn.cornerRadius = 8; newChatBtn.fills = [];
  sidebar.appendChild(newChatBtn);
  const newChatIcon = await figma.createNodeFromSvgAsync(NAV_ICONS['document-edit']);
  newChatIcon.x = 9; newChatIcon.y = 9;
  newChatBtn.appendChild(newChatIcon);

  const items = (recentItems && recentItems.length)
    ? recentItems
    : ["New Employee Hub Navigation", "It's Time to Register for Benefits", "Don't forget to finish your requir..."];
  for (let i = 0; i < Math.min(items.length, 3); i++) {
    const ri = figma.createFrame();
    ri.resize(248, 40); ri.x = 16; ri.y = 216 + i * 40;
    ri.cornerRadius = 9999; ri.fills = [];
    sidebar.appendChild(ri);
    await mkText(ri, items[i], 10, 10, 14, "Regular", "#000000");
  }

  // See all + chevron
  const seeAllRow = figma.createFrame();
  seeAllRow.resize(248, 40); seeAllRow.x = 16; seeAllRow.y = 336;
  seeAllRow.cornerRadius = 9999; seeAllRow.fills = [];
  sidebar.appendChild(seeAllRow);
  await mkText(seeAllRow, "See all", 10, 10, 14, "Regular", "#000000");
  const chevron = await figma.createNodeFromSvgAsync(NAV_ICONS['chevron-right-outline']);
  chevron.x = 220; chevron.y = 12;
  seeAllRow.appendChild(chevron);

  // Divider
  const divider = figma.createRectangle();
  divider.resize(248, 1); divider.x = 16; divider.y = 380;
  divider.fills = [{ type:'SOLID', color:{ r:0.890, g:0.886, b:0.875 } }];
  sidebar.appendChild(divider);

  // My Apps
  await mkText(sidebar, "My Apps", 26, 392, 12, "Regular", "#656462");
  await navItem(sidebar, "Enterprise Search", 412, false, 'magnifying-glass-outline');
  await navItem(sidebar, "My canvas",         452, false, 'grid-three-outline');
  await navItem(sidebar, "Activity hub",      492, false, 'lightning-outline');
  await navItem(sidebar, "Org chart",         532, false, 'tree-outline');

  // Notifications — bell + badge Rectangle (NEVER Ellipse)
  const notifRow = figma.createFrame();
  notifRow.resize(248, 40); notifRow.x = 16; notifRow.y = 820;
  notifRow.cornerRadius = 9999; notifRow.fills = [];
  sidebar.appendChild(notifRow);
  const bellIcon = await figma.createNodeFromSvgAsync(NAV_ICONS['bell-outline']);
  bellIcon.x = 8; bellIcon.y = 10;
  notifRow.appendChild(bellIcon);
  await mkText(notifRow, "Notifications", 36, 10, 14, "Regular", "#000000");
  const badgeRect = figma.createRectangle(); // Rectangle NOT Ellipse
  badgeRect.resize(12, 12); badgeRect.x = 22; badgeRect.y = 6;
  badgeRect.cornerRadius = 6;
  badgeRect.fills = [{ type:'SOLID', color:{ r:0.886, g:0.086, b:0.110 } }]; // #e2161c
  notifRow.appendChild(badgeRect);

  // User row — Frame avatar (NEVER Ellipse)
  const userRow = figma.createFrame();
  userRow.resize(248, 40); userRow.x = 16; userRow.y = 860;
  userRow.cornerRadius = 9999; userRow.fills = [];
  sidebar.appendChild(userRow);
  const avatarFrame = figma.createFrame(); // Frame NOT createEllipse
  avatarFrame.resize(24, 24); avatarFrame.x = 8; avatarFrame.y = 8;
  avatarFrame.cornerRadius = 12;
  avatarFrame.fills = [{ type:'SOLID', color:{ r:0, g:0.482, b:0.620 }, opacity:0.16 }]; // rgba(0,123,158,0.16)
  userRow.appendChild(avatarFrame);
  await mkText(avatarFrame, "RS", 3, 6, 9, "Regular", "#172b3e");
  await mkText(userRow, "Renee Simmons", 40, 10, 13, "Regular", "#172b3e");

  return sidebar;
}
```

### buildCollapsedSidebar() — USE THIS FOR SPLIT-VIEW SCREENS
```js
// 40px icon-only strip for T-CHAT-SPLIT screens
async function buildCollapsedSidebar(frame) {
  const sc = figma.createFrame();
  sc.name = "Sidebar Collapsed";
  sc.resize(40, 900); sc.x = 0; sc.y = 0;
  sc.fills = [{ type:'SOLID', color:{ r:0.957, g:0.953, b:0.941 } }]; // #f4f3f0
  sc.strokeWeight = 1; sc.strokeAlign = 'OUTSIDE';
  sc.strokes = [{ type:'SOLID', color:{ r:0.890, g:0.886, b:0.875 } }]; // #e3e2df
  frame.appendChild(sc);

  const burger = await figma.createNodeFromSvgAsync(NAV_ICONS['menu-close-outline']);
  burger.x = 10; burger.y = 10;
  sc.appendChild(burger);

  const icons = [
    { key:'home-fill',              y:56  },
    { key:'calendar-outline',       y:96  },
    { key:'globe-outline',          y:136 },
    { key:'magnifying-glass-outline', y:196 },
    { key:'grid-three-outline',     y:236 },
    { key:'lightning-outline',      y:276 },
    { key:'tree-outline',           y:316 },
  ];
  for (const { key, y } of icons) {
    const ico = await figma.createNodeFromSvgAsync(NAV_ICONS[key]);
    ico.x = 10; ico.y = y;
    sc.appendChild(ico);
  }

  // Bell + badge Rectangle (NEVER Ellipse)
  const bell = await figma.createNodeFromSvgAsync(NAV_ICONS['bell-outline']);
  bell.x = 10; bell.y = 820;
  sc.appendChild(bell);
  const badgeR = figma.createRectangle();
  badgeR.resize(8, 8); badgeR.x = 22; badgeR.y = 818;
  badgeR.cornerRadius = 4;
  badgeR.fills = [{ type:'SOLID', color:{ r:0.886, g:0.086, b:0.110 } }]; // #e2161c
  sc.appendChild(badgeR);

  // User avatar Frame (NEVER Ellipse)
  const avaF = figma.createFrame();
  avaF.resize(20, 20); avaF.x = 10; avaF.y = 860;
  avaF.cornerRadius = 10;
  avaF.fills = [{ type:'SOLID', color:{ r:0, g:0.482, b:0.620 }, opacity:0.16 }];
  sc.appendChild(avaF);

  return sc;
}
```

### Omnibar Input Bar
```js
// typed: string | null — pass null for placeholder (idle) state
async function createOmnibar(parent, x, y, width, typed) {
  const outer = figma.createFrame();
  outer.name = "Omnibar";
  outer.resize(width, 136); outer.x = x; outer.y = y;
  outer.cornerRadius = 38;
  outer.fills = [{ type:'SOLID', color:{ r:0.929, g:0.925, b:0.914 } }]; // #edece9
  outer.strokes = [{ type:'SOLID', color:{ r:0.929, g:0.925, b:0.914 } }];
  outer.strokeWeight = 1;
  outer.effects = [
    { type:'DROP_SHADOW', color:{ r:0, g:0, b:0, a:0.1 }, offset:{x:0,y:20}, radius:25, spread:-5, visible:true, blendMode:'NORMAL' },
    { type:'DROP_SHADOW', color:{ r:0, g:0, b:0, a:0.1 }, offset:{x:0,y:8},  radius:10, spread:-6, visible:true, blendMode:'NORMAL' },
  ];
  parent.appendChild(outer);

  const inner = figma.createFrame();
  inner.name = "OmnibarInner";
  inner.resize(width - 16, 120); inner.x = 8; inner.y = 8;
  inner.cornerRadius = 32;
  inner.fills = [{ type:'SOLID', color:{ r:1, g:1, b:1 } }];
  inner.strokes = [{ type:'SOLID', color:{ r:0.929, g:0.925, b:0.914 } }];
  inner.strokeWeight = 1;
  inner.effects = [{ type:'DROP_SHADOW', color:{ r:0, g:0, b:0, a:0.05 }, offset:{x:0,y:0}, radius:8, spread:0, visible:true, blendMode:'NORMAL' }];
  outer.appendChild(inner);

  await figma.loadFontAsync({ family:"Inter", style:"Regular" });
  const txt = figma.createText();
  txt.fontName = { family:"Inter", style:"Regular" };
  txt.fontSize = 16;
  txt.characters = typed || "Find answers, automate tasks, or create content instantly";
  txt.fills = typed
    ? [{ type:'SOLID', color:{ r:0, g:0, b:0 } }]
    : [{ type:'SOLID', color:{ r:0.396, g:0.392, b:0.384 } }]; // #656462
  txt.x = 24; txt.y = 20;
  txt.resize(width - 80, 24);
  inner.appendChild(txt);

  // Send button — Rectangle with cornerRadius (NEVER Ellipse — EllipseNode has no appendChild)
  const sendBtn = figma.createRectangle();
  sendBtn.name = "SendButton";
  sendBtn.resize(40, 40); sendBtn.x = inner.width - 48; sendBtn.y = 72;
  sendBtn.cornerRadius = 20;
  sendBtn.fills = typed
    ? [{ type:'SOLID', color:{ r:0.090, g:0.169, b:0.243 } }] // #172b3e active
    : [{ type:'SOLID', color:{ r:0.761, g:0.757, b:0.745 } }]; // #c2c1be idle
  inner.appendChild(sendBtn);

  return outer;
}
```

### Suggestion Chips Row
```js
async function createChips(parent, x, y, labels) {
  let cx = x;
  for (const label of labels) {
    const chip = figma.createFrame();
    chip.name = `chip-${label}`;
    chip.resize(label.length * 7 + 32, 40);
    chip.x = cx; chip.y = y;
    chip.cornerRadius = 9999;
    chip.fills = [{ type:'SOLID', color:{ r:1, g:1, b:1 } }];
    chip.strokes = [{ type:'SOLID', color:{ r:0.929, g:0.925, b:0.914 } }]; // #edece9
    chip.strokeWeight = 1;
    parent.appendChild(chip);

    await figma.loadFontAsync({ family:"Inter", style:"Regular" });
    const t = figma.createText();
    t.fontName = { family:"Inter", style:"Regular" };
    t.fontSize = 14;
    t.characters = label;
    t.fills = [{ type:'SOLID', color:{ r:0, g:0, b:0 } }];
    t.x = 16; t.y = 10;
    chip.appendChild(t);
    cx += chip.width + 12;
  }
}
```

### To-dos Dark Card
```js
async function createTodosCard(parent, x, y) {
  const card = figma.createFrame();
  card.name = "To-dos Card";
  card.resize(350, 270);
  card.x = x; card.y = y;
  card.cornerRadius = 32;
  card.fills = [{ type:'SOLID', color:{ r:0.090, g:0.169, b:0.243 } }]; // #172b3e
  parent.appendChild(card);

  await figma.loadFontAsync({ family:"Inter", style:"Semi Bold" });
  const title = figma.createText();
  title.fontName = { family:"Inter", style:"Semi Bold" };
  title.fontSize = 18;
  title.characters = "To-dos";
  title.fills = [{ type:'SOLID', color:{ r:1, g:1, b:1 } }];
  title.x = 24; title.y = 24;
  card.appendChild(title);

  const cols = [
    { label:"Open",    count:"12", x:24,  isOverdue:false },
    { label:"Due soon",count:"3",  x:120, isOverdue:false },
    { label:"Overdue", count:"3",  x:216, isOverdue:true  },
  ];
  for (const col of cols) {
    const pill = figma.createFrame();
    pill.resize(80, 147); pill.x = col.x; pill.y = 64;
    pill.cornerRadius = 9999;
    pill.fills = col.isOverdue
      ? [{ type:'SOLID', color:{ r:0.306, g:0.875, b:0.380 } }] // #4edf61
      : [{ type:'SOLID', color:{ r:0.133, g:0.251, b:0.361 } }];
    card.appendChild(pill);

    const inner = figma.createFrame();
    inner.resize(80, 77); inner.x = 0; inner.y = 0;
    inner.cornerRadius = 9999;
    inner.fills = [{ type:'SOLID', color:{ r:0.090, g:0.169, b:0.243 } }];
    pill.appendChild(inner);

    await figma.loadFontAsync({ family:"Inter", style:"Semi Bold" });
    const num = figma.createText();
    num.fontName = { family:"Inter", style:"Semi Bold" };
    num.fontSize = 24;
    num.characters = col.count;
    num.fills = [{ type:'SOLID', color:{ r:1, g:1, b:1 } }];
    num.x = col.count.length === 2 ? 26 : 32; num.y = 24;
    inner.appendChild(num);

    await figma.loadFontAsync({ family:"Inter", style:"Regular" });
    const lbl = figma.createText();
    lbl.fontName = { family:"Inter", style:"Regular" };
    lbl.fontSize = 12;
    lbl.characters = col.label;
    lbl.fills = col.isOverdue
      ? [{ type:'SOLID', color:{ r:0.090, g:0.169, b:0.243 } }]
      : [{ type:'SOLID', color:{ r:1, g:1, b:1 } }];
    lbl.x = col.isOverdue && col.label.length > 5 ? 4 : 16; lbl.y = 88;
    pill.appendChild(lbl);
  }

  // opacity on Paint object — NOT a: inside color
  await figma.loadFontAsync({ family:"Inter", style:"Regular" });
  const summary = figma.createText();
  summary.fontName = { family:"Inter", style:"Regular" };
  summary.fontSize = 13;
  summary.characters = "Bianca is waiting on PTO approval, Alfonso is waiting on a MacBook Pro approval.";
  summary.fills = [{ type:'SOLID', color:{ r:1, g:1, b:1 }, opacity:0.8 }]; // opacity on Paint, NOT a:0.8 in color
  summary.x = 24; summary.y = 224;
  summary.resize(302, 36);
  card.appendChild(summary);

  return card;
}
```

### White Card with Shadow
```js
const card = figma.createFrame();
card.name = "Card";
card.resize(350, 180);
card.cornerRadius = 24;
card.fills = [{ type:'SOLID', color:{ r:1, g:1, b:1 } }];
card.effects = [{
  type:'DROP_SHADOW',
  color:{ r:0, g:0, b:0, a:0.05 },
  offset:{ x:0, y:1 }, radius:2, spread:0,
  visible:true, blendMode:'NORMAL'
}];
```

### Chat Message Block
```js
// Uses Frame+cornerRadius for avatar — NEVER figma.createEllipse (no appendChild on EllipseNode)
async function addChatMessage(parent, isUser, name, time, messageLines, y) {
  const block = figma.createFrame();
  block.name = `msg-${name}`;
  block.resize(520, 20 + messageLines.length * 24 + 32);
  block.x = 0; block.y = y;
  block.fills = [];
  parent.appendChild(block);

  // Avatar — Frame with cornerRadius (NOT createEllipse)
  const avatar = figma.createFrame();
  avatar.resize(24, 24); avatar.x = 0; avatar.y = 0;
  avatar.cornerRadius = 12;
  avatar.fills = isUser
    ? [{ type:'SOLID', color:{ r:0.306, g:0.875, b:0.380 } }] // #4edf61
    : [{ type:'SOLID', color:{ r:0.090, g:0.169, b:0.243 } }]; // #172b3e
  block.appendChild(avatar);

  await figma.loadFontAsync({ family:"Inter", style:"Semi Bold" });
  const nameT = figma.createText();
  nameT.fontName = { family:"Inter", style:"Semi Bold" };
  nameT.fontSize = 14;
  nameT.characters = name;
  nameT.fills = [{ type:'SOLID', color:{ r:0.090, g:0.169, b:0.243 } }];
  nameT.x = 32; nameT.y = 0;
  block.appendChild(nameT);

  await figma.loadFontAsync({ family:"Inter", style:"Regular" });
  const timeT = figma.createText();
  timeT.fontName = { family:"Inter", style:"Regular" };
  timeT.fontSize = 12;
  timeT.characters = time;
  timeT.fills = [{ type:'SOLID', color:{ r:0.396, g:0.392, b:0.384 } }];
  timeT.x = 32 + name.length * 8; timeT.y = 2;
  block.appendChild(timeT);

  for (let i = 0; i < messageLines.length; i++) {
    await figma.loadFontAsync({ family:"Inter", style:"Regular" });
    const line = figma.createText();
    line.fontName = { family:"Inter", style:"Regular" };
    line.fontSize = 16;
    line.characters = messageLines[i];
    line.fills = [{ type:'SOLID', color:{ r:0.090, g:0.169, b:0.243 } }];
    line.x = 32; line.y = 24 + i * 24;
    line.resize(480, 24);
    block.appendChild(line);
  }
  return block;
}
```

### Reference Card (below AI response)
```js
async function refCard(parent, x, y, title, snippet, source) {
  const card = figma.createFrame();
  card.name = `ref-${title.slice(0,20)}`;
  card.resize(220, 90);
  card.x = x; card.y = y;
  card.cornerRadius = 16;
  card.fills = [{ type:'SOLID', color:{ r:1, g:1, b:1 } }];
  card.strokes = [{ type:'SOLID', color:{ r:0.890, g:0.886, b:0.875 } }]; // #e3e2df
  card.strokeWeight = 1;
  parent.appendChild(card);

  await figma.loadFontAsync({ family:"Inter", style:"Semi Bold" });
  const t = figma.createText();
  t.fontName = { family:"Inter", style:"Semi Bold" };
  t.fontSize = 13;
  t.characters = title;
  t.fills = [{ type:'SOLID', color:{ r:0.090, g:0.169, b:0.243 } }];
  t.x = 12; t.y = 12;
  t.resize(196, 18);
  card.appendChild(t);

  await figma.loadFontAsync({ family:"Inter", style:"Regular" });
  const s = figma.createText();
  s.fontName = { family:"Inter", style:"Regular" };
  s.fontSize = 12;
  s.characters = snippet;
  s.fills = [{ type:'SOLID', color:{ r:0.396, g:0.392, b:0.384 } }]; // #656462
  s.x = 12; s.y = 36;
  s.resize(196, 32);
  card.appendChild(s);

  await figma.loadFontAsync({ family:"Inter", style:"Regular" });
  const src = figma.createText();
  src.fontName = { family:"Inter", style:"Regular" };
  src.fontSize = 11;
  src.characters = source;
  src.fills = [{ type:'SOLID', color:{ r:0.396, g:0.392, b:0.384 } }];
  src.x = 12; src.y = 72;
  card.appendChild(src);

  return card;
}
```

### Chat Header Bar
```js
async function createChatHeader(parent, y) {
  const header = figma.createFrame();
  header.name = "ChatHeader";
  header.resize(parent.width, 64);
  header.x = 0; header.y = y;
  header.fills = [{ type:'SOLID', color:{ r:0.957, g:0.953, b:0.941 } }]; // #f4f3f0
  header.strokes = [{ type:'SOLID', color:{ r:0.890, g:0.886, b:0.875 } }]; // #e3e2df
  header.strokeWeight = 1;
  header.strokeAlign = 'OUTSIDE';
  parent.appendChild(header);

  await figma.loadFontAsync({ family:"Inter", style:"Semi Bold" });
  const title = figma.createText();
  title.fontName = { family:"Inter", style:"Semi Bold" };
  title.fontSize = 18;
  title.characters = "Moveworks Assistant";
  title.fills = [{ type:'SOLID', color:{ r:0.090, g:0.169, b:0.243 } }];
  title.x = 24; title.y = 20;
  header.appendChild(title);

  await figma.loadFontAsync({ family:"Inter", style:"Regular" });
  const logo = figma.createText();
  logo.fontName = { family:"Inter", style:"Regular" };
  logo.fontSize = 11;
  logo.characters = "MOVEWORKS\nfrom ServiceNow";
  logo.fills = [{ type:'SOLID', color:{ r:0.396, g:0.392, b:0.384 } }];
  logo.textAlignHorizontal = 'RIGHT';
  logo.x = header.width - 130; logo.y = 16;
  header.appendChild(logo);
  return header;
}
```

### Pill Button (Dark CTA)
```js
const btn = figma.createFrame();
btn.name = "Button Primary";
btn.resize(140, 40);
btn.cornerRadius = 9999;
btn.fills = [{ type:'SOLID', color:{ r:0.090, g:0.169, b:0.243 } }]; // #172b3e
```

### Pill Button (Outline/Ghost)
```js
const btn = figma.createFrame();
btn.resize(120, 40);
btn.cornerRadius = 9999;
btn.fills = [{ type:'SOLID', color:{ r:1, g:1, b:1 } }];
btn.strokeWeight = 1;
btn.strokes = [{ type:'SOLID', color:{ r:0.890, g:0.886, b:0.875 } }]; // #e3e2df
```

### AI Insights Banner
```js
// CORRECT: opacity is a top-level Paint field — NOT inside the color object
const banner = figma.createFrame();
banner.fills = [{ type:'SOLID', color:{ r:0.075, g:0.537, b:0.667 }, opacity:0.07 }]; // rgba(19,137,170,0.07)
banner.strokeWeight = 1;
banner.strokes = [{ type:'SOLID', color:{ r:0.075, g:0.537, b:0.667 } }]; // #1389aa
banner.cornerRadius = 16;
```

### Right Panel Container (split view)
```js
const panel = figma.createFrame();
panel.name = "RightPanel";
panel.resize(contentArea.width, 900);
panel.x = 0; panel.y = 0;
panel.fills = [{ type:'SOLID', color:{ r:0.957, g:0.953, b:0.941 } }]; // #f4f3f0
parent.appendChild(panel);

const panelInner = figma.createFrame();
panelInner.name = "PanelInner";
panelInner.resize(panel.width, 892);
panelInner.x = 0; panelInner.y = 8;
panelInner.topLeftRadius = 24;
panelInner.topRightRadius = 24;
panelInner.bottomLeftRadius = 0;
panelInner.bottomRightRadius = 0;
panelInner.fills = [{ type:'SOLID', color:{ r:1, g:1, b:1 } }];
panelInner.effects = [{ type:'DROP_SHADOW', color:{r:0,g:0,b:0,a:0.05}, offset:{x:0,y:1}, radius:2, spread:0, visible:true, blendMode:'NORMAL' }];
panel.appendChild(panelInner);
```

---

## Use Case → Flow Mapping

| Use Case Type | Screens to Use |
|---|---|
| Information lookup / FAQ | T-HOME-QUERY → T-CHAT-WORKING → T-CHAT-RESPONSE |
| Approval management | T-HOME-QUERY → T-CHAT-WORKING → T-CHAT-RESPONSE → T-CHAT-SPLIT (approvals panel) |
| Submit a form / raise a case | T-HOME-QUERY → T-CHAT-WORKING → T-CHAT-RESPONSE → T-CHAT-SPLIT (form panel) → success |
| Search enterprise knowledge | T-HOME → T-CHAT-WORKING → T-CHAT-RESPONSE → T-CHAT-SPLIT (KB article) |
| Notification-driven approval | T-HOME → T-NOTIFY → T-CHAT-SPLIT (approvals panel) |
| IT request / HR case tracking | T-HOME → T-INBOX → T-CHAT-SPLIT (form) → case confirmed |
| Agentic multi-step task | T-HOME-QUERY → T-CHAT-WORKING (multi-step) → T-CHAT-RESPONSE → T-CHAT-SPLIT |

---

## Naming Conventions

```
01 — Home Default
02 — Home Query Typed
03 — Assistant Working
04 — Assistant Response
05 — Split View — [Right Panel Name]
06 — Success / Confirmed
```

---

## Critical Rules for Screen Generation

1. **Background color is `#f4f3f0`** — NOT `#f8f7f4`
2. **Dark accent is `#172b3e`** — NOT `#1e293b`
3. **Green is `#4edf61`** — NOT `#16a34a`
4. **Borders are `#e3e2df`** — NOT `#e1e0dd`
5. **Input bar outer bg is `#edece9`** — NOT `#f8f7f4`
6. **Active nav item is `#c2c1be`** — NOT `#e1e0dd`
7. **Chat messages are PLAIN TEXT** — no white card bubbles around messages
8. **Working state = bullet list** with ⏳ emoji and ✓/→ markers — no progress bars
9. **Split view sidebar COLLAPSES to 40px icon strip** — not 72px
10. **Always `await figma.loadFontAsync`** before any text operation
11. **Set `layoutSizing = 'FILL'` AFTER `parent.appendChild(child)`**
12. **Return ALL created node IDs** at end of every script

---

## Output Checklist

- [ ] All frames are 1440×900px
- [ ] Frames spaced 1520px apart on X axis (1440 + 80)
- [ ] Background color is `#f4f3f0` (not `#f8f7f4`)
- [ ] Dark cards/CTAs use `#172b3e` (not `#1e293b`)
- [ ] Sidebar uses correct active state `#c2c1be`
- [ ] Input bar outer bg is `#edece9` with XL shadow
- [ ] Chat messages use plain text style (no card bubbles)
- [ ] Split view has 40px collapsed sidebar
- [ ] Screenshot verified on first and last frame

---

## Source Reference (Figma file `EEdQtgQ9VVscCfZtKsFdGU`)

| Template | Reference Node ID |
|---|---|
| T-HOME (expanded sidebar) | 434-72953 |
| T-HOME-QUERY (collapsed sidebar) | 434-81221 |
| T-NOTIFY (2 notifications) | 434-69553 |
| T-CHAT-WORKING (single step) | 434-73002 |
| T-CHAT-WORKING (multi-step) | 434-73105 |
| T-CHAT-RESPONSE | 434-73210 |
| T-CHAT-SPLIT (approvals) | 434-72867 |
| T-CHAT-SPLIT (KB article) | 434-78583 |
| T-INBOX | 434-85181 |
| T-CANVAS | 434-81086 |
