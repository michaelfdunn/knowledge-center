---
name: styling
description: >
  Complete visual system. Combines design tokens (colors, spacing, typography,
  solid card system), density/context mapping, and content writing rules.
  Single reference for Step 4 of the reasoning process.
---

# Styling Reference

---

# Part 1: Design Tokens

## Background
Solid: `background: #EDECE9;`

No background image. No gradient fallback. The page background is a flat warm gray.

## Nav Rail
`background: #EDECE9; width: 52px; border-right: 1px solid #C2C1BE;` Full viewport height, fixed left.
Logo: ServiceNow 4-dot logo at top, centered in rail.
Nav icons: black (`#000000`) outlined strokes, 1.5px width. Hover/active background: `#E3E2DF`.

## Cards
Card default: `background: #fff; box-shadow: 0 1px 2px rgba(0,0,0,0.1), 0 1px 2px -1px rgba(0,0,0,0.1); border-radius: 24px;`
Card hover: `background: rgb(249,250,251);`
Active/selected: `background: #fff; border: 1px solid rgba(45,117,36,0.3);`
Floating (modals, dropdowns): `background: #fff; box-shadow: 0 8px 24px rgba(0,0,0,0.15); border-radius: 24px;`

No `backdrop-filter`. No transparency. Cards are solid white.

## Font
`font-family: 'ServiceNow Sans', -apple-system, BlinkMacSystemFont, sans-serif;`

## Colors
**Brand:** primary (buttons, nav) `rgb(23,43,49)`, green accent (active states, AI sparkle) `rgb(45,117,36)`, green tint `rgba(45,117,36,0.1)`
**Text:** primary `rgb(2,6,23)`, secondary `rgb(30,41,59)`, tertiary `#656462`, muted `rgb(115,127,132)`, inverse white, link `rgb(45,117,36)`
**Borders:** default `rgb(226,232,240)`, subtle `rgba(226,232,240,0.5)`
**Status:** error/critical `rgb(226,22,28)`, warning `rgb(253,202,110)` / bg `#FFFBEB`, success `rgb(45,117,36)` / bg `#f1fcf5`, info `rgb(23,43,49)`, muted `rgb(115,127,132)`
**AI:** summary gradient `linear-gradient(180deg, rgb(210,252,203), rgb(202,239,255))` with `border-radius: 16px`, recommendation badge `linear-gradient(171deg, rgb(221,252,216) 10%, rgb(215,243,255) 90%)` with `border: 1px solid #7de56b`

## Buttons
Primary: `background: rgb(23,43,49); color: white; border-radius: 12px;`
Secondary: `background: rgb(226,232,240); color: rgb(2,6,23); border-radius: 12px;`
Destructive/error: `background: rgb(226,22,28); color: white; border-radius: 12px;`

## Tabs
Centered group inside a container (`background: #f9f8f6; border: 1px solid #e3e2df; border-radius: 16px; padding: 4px;`).
Active tab: `background: #4d4c4a; color: white; border-radius: 12px;`
Inactive tab: `background: transparent; color: #656462;`

## Omnibar
Inline below greeting on hub view (not fixed to bottom). Centered, pill-shaped (`border-radius: 9999px`), white background, `max-width: 768px`, with `border: 1px solid rgb(226,232,240)`.

## Spacing
xs: 4px, sm: 8px, md: 16px, lg: 24px, xl: 32px

## Layout Dimensions
Card gap: 16px, content margins: 24px, nav width: 52px, nav button: 40px

## Typography
Headings: lg 20px/700, md 16px/500, sm 14px/600
Body: default 16px/400, small 14px/400, xs 12px/400
Metrics: large 30px/700, label 14px/400

## Border Radius
Default: 12px. sm: 4px, lg: 24px (cards), pill: 9999px

## Elevation
0: none. 1: `0 1px 2px rgba(0,0,0,0.1), 0 1px 2px -1px rgba(0,0,0,0.1)`. 2: `0 4px 6px rgba(0,0,0,0.07)`. 3: `0 8px 24px rgba(0,0,0,0.15)`. 4: `0 20px 25px rgba(0,0,0,0.15)`

## Icons
All icons: same outlined stroke family, 1.5px width. Never mix. AI Sparkle (✦): 4-point star SVG, green gradient (`rgb(45,117,36)`).

## Transitions
fast: `all 0.2s ease`, normal: `all 0.25s ease`, slow: `all 0.35s ease`

## Breakpoints
sm: 640px, md: 768px, lg: 1024px, xl: 1280px, 2xl: 1536px

---

# Part 2: Density & Context Mapping

## Density Modes
**Default:** padding 24px, gap 16px, row 48px, list-item 80px
**Compact:** padding 16px, gap 8px, row 36px, list-item 56px
**Spacious:** padding 32px, gap 24px, row 56px, list-item 96px

## Density by Persona
Power user (agent, dispatcher) → Compact. Regular user (manager, ops) → Default. Occasional user (requester) → Spacious. Mobile → Default or Spacious.

## Color by Characteristic
priority critical → red border + solid badge. priority high → warning border. agent-running → green pulse. AI content → sparkle icon. threshold-breached → red 10% bg. sla-approaching → warning 10% bg.

## Element Mapping
Card: elevation-1, radius-lg (24px). Button/Input: radius default (12px). Badge/Search/Avatar: radius-pill (9999px). Tab container: radius 16px. Tab pill: radius 12px. Transitions: buttons/cards fast, panels normal, pages slow.

## Card Overflow Rules
Cards must never clip their own content. Follow these rules:
- **Cards:** Never set `overflow: hidden` on a card or its content sections. Let height grow naturally with content.
- **Text:** Always set `word-wrap: break-word; overflow-wrap: break-word;` on text containers so long strings (invoice numbers, supplier names, email addresses) wrap instead of overflowing.
- **Fixed-height elements only:** Use `overflow: hidden` only on elements with an intentional fixed height — avatars, image thumbnails, icon containers.
- **Scrollable regions:** Panels that scroll (the full content area, a sidebar) use `overflow-y: auto`, never `overflow: hidden`.
- **Line clamping:** If truncating multi-line text is intentional (e.g., a preview snippet), use `-webkit-line-clamp` with a "Show more" toggle — never silent clipping.
- **Min-height, not max-height:** If a content block needs a minimum size, use `min-height`. Never use `max-height` without `overflow-y: auto`.

## Precedence
`Platform default → App Shell → Layout Pattern → BU override → Element (most specific wins)`

---

# Part 3: Content Writing Rules

## Sentence Case Everywhere
"Priority incidents" not "Priority Incidents." Exceptions: product names, proper nouns, acronyms.

## Buttons: Verb + Object
"Submit request" not "Submit." Destructive: "Delete this incident" not "Delete."

## Headings: Be Specific
"Open incidents" not "Items." Metrics include context: "12 critical incidents" not "12."

## Empty States
What would be here + why empty + what to do. "No open incidents. You're all caught up." Not "No data."

## Errors: What + Why + What to Do
"Can't save. Connection timed out. Try again." Not "Error."

## Positive Framing
Frame around what users can do. "Save to continue" not "You can't proceed without saving."
