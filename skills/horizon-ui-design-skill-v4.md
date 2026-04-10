---
name: horizon-ui-design
description: Design UI screens and components using the Horizon 2.0 design system. Use when asked to design, mock up, or generate UI for ServiceNow Horizon experiences, or when asked to use the Horizon design system. Triggers on "design a screen", "create a UI", "use Horizon components", "Horizon 2.0 layout", "employee hub", "omni-bar". v3 — includes teammate setup, output modes, and 1440×1024 dimension preference.
disable-model-invocation: false
---

# Horizon 2.0 UI Design Skill

## Overview

Horizon 2.0 is ServiceNow's **ALPHA** next-generation design system for Employee Hub and related experiences. This skill guides you to design screens that are structurally faithful to real Horizon 2.0 Figma references.

### Context
- **Product:** ServiceNow Employee Hub — enterprise portal for employees to manage HR, IT, and workplace requests
- **Users:** Employees, managers, HR/IT agents
- **Tone:** Clean, professional, AI-assisted — the omni-bar is always present as an AI entry point

### Generation mode — infer from the prompt, do not ask

Three modes exist. Infer silently from the user's words — never ask which mode applies.

**Mode: Fresh generation**
Detect: "Design a…", "Create a…", "Build a new…", no reference provided
Rule: Use design system defaults. Canonical layout, standard components, documented patterns.

**Mode: Reference recreation**
Detect: "Recreate", "Rebuild", "Match this", "Based on this", provides a Figma URL or image
Rule: Treat the reference as a hard constraint. Map navigation labels, hierarchy, and active states 1:1. Only swap in the correct components and tokens underneath — the content stays, the implementation gets upgraded. Do NOT reset to design system defaults.

**Mode: Exploration / iteration**
Detect: "Add X", "Try Y", "What if…", "Change X to Y", working from an existing file
Rule: Treat the current working file as the reference. New elements inherit the style of what's already there. Never reset existing structure to defaults.

> When a Figma URL or image is provided, always default to reference mode — the user is showing you what they have and expecting it to be respected, not replaced.

The most jarring failure is treating "recreate" as fresh generation, silently rewriting nav labels and structure the user spent time on. Reference mode fixes this — content is a constraint, only the implementation gets upgraded.

---

### Before running — always clarify if unclear

If the request is missing any of these, **ask before making any Figma tool calls**:
1. **Which page / screen** — if ambiguous (e.g. "design a profile page" but multiple layouts exist)
2. **Output format** — Figma frame, image screenshot, HTML prototype, or spec
3. **Light or dark mode** — if not specified, default to light mode but confirm if designing for the GA release file
4. **Push to Figma destination** — if the user asks to push a design to Figma but provides no URL, ask: *"Which Figma file and page should I place this on?"* Never push to an arbitrary file without confirmation.

Do not approximate or proceed without these. A 10-second clarification avoids a full re-run.

### When designing, always:
1. **Match or compose** a layout from the Screen Layout Templates — use the decision tree, not guesswork
2. **Search Figma first** for any component not in this inventory before inventing one
3. **Use documented tokens** — colors, spacing, typography, radius and shadows are captured in this skill; use them directly
4. **Respect breakpoints** — different nav widths and card column widths at each breakpoint
5. **Fetch component variants** before specifying states (size, style, type) — variant names require a live Figma lookup

---

## Getting Started

### Install in 30 seconds

**Claude Code (CLI or desktop app)**

Option A — Settings UI (recommended):
1. Open Settings → Customize → Skills
2. Add this file — Claude registers it automatically
3. Invoke with `/horizon-ui-design`

Option B — Chat install:
1. Drag this file into your Claude Code chat
2. Type: `install this as a skill`
3. Invoke with `/horizon-ui-design`

**Claude Web / claude.ai**
1. Open or create a Project
2. Go to Project instructions → Edit
3. Drag this file in, or copy-paste the full contents
4. The skill is now always active for that project — no slash command needed

**Connect Figma** (required for Figma mode, one-time setup)
- Claude Code: Settings → MCP Servers → add `plugin:figma:figma` → sign in
- Claude Web: connect Figma via the integrations panel in your Project

### How to invoke

**From a Figma URL** (highest fidelity — recommended):
```
/horizon-ui-design

Implement this screen as a responsive HTML prototype:
https://figma.com/design/FJWsBdS98yzQhHO1O7BmN0/...?node-id=19035-52573
```

**From a description:**
```
/horizon-ui-design

Design a Canvas dashboard with featured apps, quick links, and a requests widget.
```

**For a single component:**
```
/horizon-ui-design

Build the omni-bar-footer as a standalone HTML snippet.
```

### Updating the skill

When a new version is available, replace your local copy via Option A or B above.

### If Figma access breaks

Paste the current Figma URL into the chat:
```
The Figma file has moved. Here is the new link: https://figma.com/design/<new-key>/...
```
Claude will extract the new file key and continue. Never let Claude approximate without live Figma data.

---

## Output Modes & Dimensions

The deliverable type determines dimensions and approach. **Always clarify which mode before generating.**

### Mode 1 — Figma Design Mockup (push to Figma)
> Use when: designer asks for a UI design, screen mock-up, or Figma frame to be created inside Figma

- **Canvas size: 1440 × 1024px** (fixed width, designers' standard)
- **Height: flexible** — let content determine the actual frame height; 1024px is the starting point, extend downward as needed for full-page designs
- Use `generate_figma_design` or compose with real Figma component keys
- Output must use Horizon 2.0 component keys and exact token values
- Do not use a fixed 810px height — that is only a viewport clipping reference, not a design canvas constraint

### Mode 2 — Image / Screenshot Export
> Use when: user wants a quick visual reference, shareable image, or screenshot of an existing Figma frame

- Call `get_screenshot` on the target node
- No code generated — output is the rendered image
- Use to verify layout before generating HTML, or as a standalone deliverable

### Mode 3 — HTML Prototype / Simulation
> Use when: developer or designer asks for a coded prototype, HTML mockup, or interactive simulation

- **No fixed canvas dimensions** — layout must be fully responsive
- Shell: `width: 100%; height: 100vh` (fills the browser window at any size)
- **Frozen components** (always fixed position — do not scroll with content):
  - `side-navigation` — always `position: relative; flex-shrink: 0; height: 100%`
  - Page/section headers (e.g. `_card-header`) — sticky at top of their panel
- **Scrollable content** — main panel content area scrolls vertically; widget cards, lists, articles flow naturally
- Side nav: `flex-shrink: 0; width: 52px`
- Main panel: `flex: 1; min-width: 0; overflow-y: auto`
- Add breakpoints: 2-col grid at ≤860px, 1-col at ≤600px
- Follow the **High-Fidelity HTML Workflow** below

### Mode 4 — Spec / Annotation
> Use when: designer or PM needs a written component + token reference

- List each zone: component name · variant · dimensions · token references
- No code or Figma frame generated

> **If the user does not specify a mode**, ask before running anything:
> *"What format would you like the output in?*
> *— Figma design (I'll push a 1440×1024px frame into Figma)*
> *— Image / screenshot (quick visual reference)*
> *— HTML prototype (responsive, runs in browser)*
> *— Spec (written component + token reference)"*

---

### What's in this skill vs what needs a live Figma call
| Area | Status | Notes |
|------|--------|-------|
| Layout templates | ✅ Captured | 5 real layouts with exact dimensions |
| Component inventory | ✅ Captured | Names + Figma keys for all known components |
| Grid / breakpoints | ✅ Captured | Column widths, nav widths, all breakpoints |
| **Color/radius/shadow tokens** | ✅ **Verified live** | See "Confirmed Token Values" — real hex/px/rgba values |
| Spacing tokens | ✅ Captured | Full `$sp-space--*` scale with px values |
| Typography | ✅ Captured | Font: `ServiceNow Sans` (headings), `Inter` (UI) |
| DaisyUI class mapping | ✅ Captured | Component → DaisyUI class table below |
| **Icon/app/image assets** | 🔴 **Must fetch live** | `get_design_context` returns real CDN URLs — never approximate |
| Component variant names | ⚠️ Fetch live | Run `search_design_system` per component |
| Web component code (sn-aix-*) | ⚠️ Partial | Accordion + text-input confirmed; others need `get_design_context` |
| Novel page layouts | ✅ Guided | Use the Flexible Layout Composition rules |

---

## Figma Reference Files

> **If any file key below returns an error, access denied, or "file not found":** stop and ask the user —
> *"The Figma design system file I need seems to have moved or lost access. Can you share the current Figma link for [Components / the layout you're referencing]?"*
> Do not attempt to approximate or proceed without live Figma data. The user can paste a new `figma.com/design/…` URL and you can extract the new file key from it.

| File | Purpose | File Key |
|------|---------|----------|
| `[ALPHA] Horizon 2.0 -> Components` | Component library source of truth | `SPmKj8MGJIDvOJhvDbNIZO` |
| `Home-page-widgets` | Dashboard / widget layout references | `U7p5PANFPEjSZZq4F0FFZ5` |
| `Employee Hub – Canvas for GA May 2026` | Employee Hub app layouts | `FJWsBdS98yzQhHO1O7BmN0` |
| `Employee Hub – Employee Communications` | Comms / document detail layouts | `TsM6m7HpyGTsk19CoJxVOo` |
| `Employee Hub – GA May Release (Layout Gallery)` | Full collection of all page layouts in light + dark mode. Use to look up any page type that isn't in the templates above. Note: this file supports multiple themes — color tokens may differ from the confirmed values; use layout/structure data only unless the user explicitly targets this file's theme. | `mplcJSFS3G8LLZkNCHm4q0` |

### Design System Libraries

| Library | Purpose | Library Key |
|---------|---------|-------------|
| `[ALPHA] Horizon 2.0 -> Components` | UI components | `lk-73b4524d7b41a07d4f8ac35c2d50471e1453d972cd883533817a15a188b16a212153165386302f2f5bffec5c178c214a9c9bbc0e0e2fb7c84cc5b18ffb49dca4` |
| `[ALPHA] Horizon 2.0 -> Foundations` | Typography, tokens | `lk-0ed3561d4a90d19c48176fb4135f419a36f2d19eae66e164cb1e261cc605631d06de21b19c8253d10ee3d846712a4632ec2a0bc97c1be01735d7c588851b5ff1` |
| `[ALPHA] Horizon 2.0 -> Icons` | Icons | `lk-dff6270ac2ed60b2bad847fa1d65f0eb364804b8856787b0ced2a0274ad8299b9655ee6e4701e9480093c63a3122b40d34bd277dd20034adaa0799949b3b42b2` |

---

## Technology Stack

Horizon 2.0 is built on **Tailwind CSS + DaisyUI** (confirmed from Guidelines page `23:612`).

- **CSS framework:** Tailwind CSS utility classes
- **Component layer:** DaisyUI — use DaisyUI class names for all component styling
- **Web components:** ServiceNow `sn-aix-*` custom elements for complex interactive components
- **Colors:** CSS custom properties (`--color-*`) following DaisyUI's semantic system
- **Border radius:** DaisyUI size tokens (`--radius-badge`, `--radius-btn`, `--radius-box`)
- **Shadows:** Tailwind v4 shadow utilities

> When generating code, **always prefer DaisyUI class names** over custom CSS. Only reach for `sn-aix-*` web components for AI/chat elements (accordion, omni-bar).

---

## Design Tokens

### Color Tokens (CSS custom properties)

These are DaisyUI semantic CSS variables — apply as `bg-[--color-primary]`, `text-[--color-primary]`, or via `oklch()` values in Tailwind config.

**Semantic (core)**
| Token | Usage |
|-------|-------|
| `--color-primary` | Primary brand action color |
| `--color-primary-content` | Text/icon on primary background |
| `--color-secondary` | Secondary action color |
| `--color-secondary-content` | Text/icon on secondary background |
| `--color-accent` | Accent/highlight color |
| `--color-accent-content` | Text/icon on accent background |
| `--color-neutral` | Neutral/muted color |
| `--color-neutral-content` | Text/icon on neutral background |
| `--color-base-100` | Page background (lightest) |
| `--color-base-200` | Slightly elevated surface |
| `--color-base-300` | Card/panel surface |
| `--color-base-content` | Default text color |

**Status**
| Token | Usage |
|-------|-------|
| `--color-info` | Informational |
| `--color-info-content` | Text on info |
| `--color-success` | Success state |
| `--color-success-content` | Text on success |
| `--color-warning` | Warning state |
| `--color-warning-content` | Text on warning |
| `--color-error` | Error/destructive state |
| `--color-error-content` | Text on error |

**Extended semantic (background + text)**
| Token | Usage |
|-------|-------|
| `--color-background-primary` | Primary page background |
| `--color-background-secondary` | Secondary background layer |
| `--color-background-tertiary` | Tertiary background layer |
| `--color-background-inverted` | Inverted (dark) background |
| `--color-text-primary` | Primary text |
| `--color-text-secondary` | Secondary/muted text |
| `--color-text-tertiary` | Tertiary/disabled text |
| `--color-text-inverted` | Text on dark background |

> Both light and dark mode values are defined in the Figma Colors section. Token names are the same in both modes.

---

### Spacing Tokens

| Token | Value | Tailwind equiv |
|-------|-------|---------------|
| `$sp-space--xxs` | `2px` | `gap-0.5` / `p-0.5` |
| `$sp-space--xs` | `4px` | `gap-1` / `p-1` |
| `$sp-space--sm` | `8px` | `gap-2` / `p-2` |
| `$sp-space--md` | `12px` | `gap-3` / `p-3` |
| `$sp-space--lg` | `16px` | `gap-4` / `p-4` |
| `$sp-space--xl` | `24px` | `gap-6` / `p-6` |
| `$sp-space--xxl` | `32px` | `gap-8` / `p-8` |
| `$sp-space--3xl` | `40px` | `gap-10` / `p-10` |

Use spacing tokens for: padding, gap, margin. Never hardcode pixel values.

---

### Typography Scale

Font family: System/app default. Weight: Normal for headings/labels, Light for body.

**Headings** (Desktop: 1024–1536px+)
| Token | Size | Line height | Weight |
|-------|------|------------|--------|
| `heading-01` | 48px | 48px | Normal |
| `heading-02` | 36px | 40px | Normal |
| `heading-03` | 30px | 36px | Normal |
| `heading-04` | 24px | 28px | Normal |
| `heading-05` | 20px | 24px | Normal |
| `heading-06` | 18px | 20px | Normal |

**Body**
| Token | Size | Line height | Weight |
|-------|------|------------|--------|
| `body-lg` | 18px | 28px | Light |
| `body-md` | 16px | 24px | Light |
| `body-sm` | 14px | 20px | Light |
| `body-xs` | 12px | 16px | Light |

**Button** (letter-spacing: 0%, line-height: 180%)
| Token | Size | Weight |
|-------|------|--------|
| `button-xl` | 22px | Normal |
| `button-lg` | 18px | Normal |
| `button-md` | 14px | Normal |
| `button-sm` | 12px | Normal |
| `button-xs` | 11px | Normal |

**Label**
| Token | Size | Line height | Weight |
|-------|------|------------|--------|
| `label-xl` | 18px | 24px | Normal |
| `label-lg` | 16px | 24px | Normal |
| `label-md` | 14px | 24px | Normal |
| `label-sm` | 12px | 20px | Normal |
| `label-xs` | 10px | 16px | Normal |

---

### Border Radius (DaisyUI tokens)

| DaisyUI token | CSS var | Used for |
|--------------|---------|---------|
| `rounded-badge` | `--radius-badge` | Small components: checkbox, toggle, badge |
| `rounded-btn` | `--radius-btn` | Medium components: button, input, select, tab |
| `rounded-box` | `--radius-box` | Large components: card, modal, dropdown panel |

---

### Box Shadow (Tailwind v4 classes)

| Class | Use |
|-------|-----|
| `shadow-none` | Flat, no elevation |
| `shadow-2xs` | Barely lifted |
| `shadow-xs` | Subtle card lift |
| `shadow-sm` | Default card |
| `shadow-md` | Floating element |
| `shadow-lg` | Dropdown / popover |
| `shadow-xl` | Modal |
| `shadow-2xl` | Overlay panel |
| `inset-shadow-2xs` | Input inner shadow |
| `inset-shadow-xs` | Input inner shadow (stronger) |
| `inset-shadow-sm` | Deep inset |

---

## Grid & Breakpoints

All breakpoints from the Guidelines page (`23:612` → Breakpoints section).

### Desktop — 1440px+ (icon nav)
```
[nav 52px] [content area 1388px]
           [gutter 54px] [inner 1280px] [gutter]
                         3 columns × 374px, gap 16px
```
- `side-navigation` collapsed: **52px wide**
- `omni-bar-footer`: **766px wide**, centred

### Wide — 1280px (expanded nav)
```
[side-navigation 288px] [panel 998px]
                         [gutter 44px] [inner 896px]
                         3 columns × 288px, gap 16px
```
- `side-navigation` expanded: **288px wide**
- `omni-bar-footer`: **896px wide**, centred

### Tablet — 1024px
- Collapsed nav; 2-column card grid; single-column content zones
- Use `mobile-navigation` patterns for sub-1024px

### Tablet — 768px
- Single-column layout; stacked cards
- Nav becomes bottom tab bar (`mobile-navigation`)

### Mobile — 640px and below
- Full-width single column
- `mobile-navigation` always at bottom
- No `omni-bar-footer`; use `omni-bar-home` inline if needed

### Card column widths by breakpoint
| Breakpoint | Canvas | Nav | Panel | Card col width | Cols |
|-----------|--------|-----|-------|---------------|------|
| 1536px+ | 1536 | 52 | ~1484px | ~470px | 3 |
| 1440px | 1440 | 52 | 1334px | 374px | 3 |
| 1280px | 1286 | 288 | 982px | 288px | 3 |
| 1024px | 1024 | 52 | ~972px | ~472px | 2 |
| 768px | 768 | 0 | 768px | 100% | 1 |
| 640px | 640 | 0 | 640px | 100% | 1 (mobile) |

---

## Screen Layout Templates

Use the closest matching template as the structural skeleton. Fetch the Figma node for pixel-accurate details.

---

### Layout A — Home Dashboard (AI-first)
**Reference:** `U7p5PANFPEjSZZq4F0FFZ5` node `2678:18222`
**Use for:** Home page, landing screen, AI-first entry point

```
┌────────────────────────────────────────────────────────┐
│ [nav 52px]  │  [content area 1388px]                   │
│             │                                          │
│  logo       │   ┌──────────────────────────────────┐   │
│  nav icons  │   │  Greeting text (Hi {name}…)      │   │
│             │   │  [omni-bar — 768px wide]          │   │
│             │   │  [quick action chips row]         │   │
│  ─────────  │   └──────────────────────────────────┘   │
│             │                                          │
│  settings   │   ┌──────┐  ┌────────────────┐  ┌──────┐│
│  profile    │   │ col1 │  │ col2 (carousel │  │ col3 ││
│             │   │ 416px│  │ + quick links) │  │ 416px││
│             │   │      │  │ 416px          │  │      ││
│             │   └──────┘  └────────────────┘  └──────┘│
└────────────────────────────────────────────────────────┘
```

**Structure:**
- Top: Greeting + `omni-bar-home` (768px, centered in content)
- Quick action chips below omni-bar (icon + label, horizontal row)
- 3-column widget grid (416px each, 16px gap):
  - **Col 1:** Inbox summary — `Badge` + task list with avatar
  - **Col 2:** `promo-card` carousel (image + text + dot pagination) stacked above quick-links grid (2×2 icon shortcuts)
  - **Col 3:** Popular content card + `upcoming-meeting-card` / holiday widget
- `omni-bar-footer` NOT used on this layout (omni-bar is inline at top)

---

### Layout B — Employee Hub Dashboard (widget grid)
**Reference:** `FJWsBdS98yzQhHO1O7BmN0` nodes `19035:52573` (1440) and `19035:31332` (1280)
**Use for:** Main app dashboard, personalized widget hub

```
┌────────────────────────────────────────────────────────┐
│ [side-navigation]  │  [main panel]                      │
│ 52px (collapsed)   │  ┌─────────────────────────────┐  │
│ or 288px (expanded)│  │ _card-header (hidden)       │  │
│                    │  ├─────────────────────────────┤  │
│                    │  │ page-heading-block (88px)   │  │
│                    │  ├──────┬──────┬───────────────┤  │
│                    │  │ col1 │ col2 │ col3          │  │
│                    │  │ 374px│ 374px│ 374px         │  │
│                    │  │      │      │               │  │
│                    │  │ apps │ ql/g │ requests      │  │
│                    │  │ popu │ rec  │ ql/grid       │  │
│                    │  │ ql/g │ reqs │               │  │
│                    │  └──────┴──────┴───────────────┘  │
│                    │  [gradient fade 80px]               │
│ [omni-bar-footer centered at y=710]                     │
└────────────────────────────────────────────────────────┘
```

**Structure:**
- `side-navigation` full height (collapsed or expanded per breakpoint)
- Main panel is a card with 8px inset
- `page-heading-block` at top (88px)
- 3-column scrollable card grid (gap 16px):
  - **Col 1:** `apps-card` → `popular-content-card` → `quick-links-card / grid`
  - **Col 2:** `quick-links-card / grid` → `recent-conversations-card` → `requests-card`
  - **Col 3:** `requests-card` → `quick-links-card / grid`
- Gradient fade at bottom (80px, masks scroll overflow)
- `omni-bar-footer` floating, horizontally centered, at y=710

---

### Layout C — Side Panel Overlay
**Reference:** `FJWsBdS98yzQhHO1O7BmN0` node `19355:68443`
**Use for:** Detail panels, record detail, contextual side drawers

```
┌───────────────────────────────────┬────────────┐
│  [main content — Layout B or D]   │ _side-panel│
│                                   │  340px     │
│                                   │            │
│  [scrim overlay on main content]  │            │
└───────────────────────────────────┴────────────┘
```

**Structure:**
- `_side-panel` component (340px wide) slides in from the right
- Scrim overlays the main content behind it
- Panel contains its own header + scrollable body

---

### Layout D — Chat + Document Detail (split)
**Reference:** `TsM6m7HpyGTsk19CoJxVOo` node `8244:97183`
**Use for:** AI chat alongside article/policy/KB content, comms detail view

```
┌───────────────────────────────────────────────────────┐
│ [side-nav 52px] │ [chat panel 484px] │ [doc panel 904px] │
│                 │                    │                    │
│                 │ _chat-header       │ _card-header       │
│                 │                    │ ─────────────────  │
│                 │ chat thread        │ title + Badge      │
│                 │ (scroll)           │ Dropdown + button  │
│                 │                    │ ─────────────────  │
│                 │                    │ [content sections] │
│                 │                    │  media / image     │
│                 │                    │  text paragraphs   │
│                 │                    │  badge-cards (3col)│
│                 │                    │  tabs + tab-content│
│                 │                    │  timeline/stepper  │
│                 │                    │  accordion FAQ     │
│                 │                    │  attachments       │
│                 │                    │  related resources │
│ [omni-bar-footer 472px at y=710]    │                    │
└───────────────────────────────────────────────────────┘
```

**Structure:**
- `side-navigation` (52px collapsed)
- **Left panel (484px):** Chat thread + `omni-bar-footer` (472px)
  - `_chat-header` (56px)
  - `chat conversations` (scrollable)
  - Gradient fade above omni-bar
- **Right panel (904px):** Document/content view
  - `_card-header` (56px)
  - Header row: title + `Badge` group + name label + `Dropdown` + `button`
  - Scrollable content sections (see Content Section Patterns below)

---

### Layout E — Widgets Catalog
**Reference:** `U7p5PANFPEjSZZq4F0FFZ5` node `1610:11456`
**Use for:** Widget management, add/configure widgets page

- Full-width section per widget type with label + example instances
- Each widget shown at 416px wide with its live preview
- Section headers label the widget category

---

## Content Section Patterns (for Layout D scrollable body)

These patterns appear inside the document/detail panel. Use in sequence as needed:

| Pattern | Components | Notes |
|---------|-----------|-------|
| **Media** | image frame + play icon | Hero image or video thumbnail |
| **Text paragraph** | body text | Full-width prose |
| **Highlight section** | `highlight section` instance | Callout / featured info box |
| **Section header + text** | heading text + body text | Standard content block |
| **3-column badge cards** | card frames + `Badge` + text | Approval tiers, categories |
| **Tabs + content** | `tabs` + tab-content frame | Tabbed data (expenses, vendors, visas) |
| **Numbered timeline** | numbered circles + title + subtitle + `Badge` | Step-by-step process |
| **FAQ accordion** | Collapse rows (chevron-down icon) | Use `Accordion` component |
| **Attachments row** | `media-card` + document icon + `button iconic` | File list |
| **Related resources** | 2-column cards + quote text | Links/resources at bottom |
| **PDF embed** | iframe container + PDF toolbar | Inline document viewer |

---

## Component Inventory

### Navigation & Chrome

| Component | Type | Component Key | Notes |
|-----------|------|--------------|-------|
| `side-navigation` | component_set | `52669afebe2f4fb167810a47137373ed7686c83f` | Collapsed (52px) or expanded (288px) |
| `mobile-navigation` | component | `abe30638baf6e94f79176946b4cc182ad507965a` | Mobile bottom nav |
| `header` | component_set | `28417961af4b880b93ef5b8969e3c206d02ade92` | Top app bar |
| `pagination` | component_set | `6d866a5840a2ba40c8353fa6629cd349c5556bdd` | Page/carousel pagination |
| `_nav-button-text` | instance | — | Icon nav item (used inside side-navigation) |
| `_profile` | instance | — | Profile avatar button in nav footer |
| `page-heading-block` | instance | — | Page title block (88px tall) |

### AI & Omni-bar

| Component | Type | Component Key | Notes |
|-----------|------|--------------|-------|
| `omni-bar-home` | component | `b1d16503a287450c3b787fa590065c7a8e4ba7ad` | Inline AI bar (home layout, 768px) |
| `omni-bar-footer` | component_set | `fde4edbcf7105d044075e37a7353cc2e70ddc69b` | Floating bottom AI bar |
| `ask-question` | component | `16994ea7925785b2be7317b56d0a5588e3177632` | Ask-question entry point |
| `_chat-header` | instance | — | Chat panel header |
| `chat conversations` | instance | — | AI conversation thread |

### Actions & Controls

| Component | Type | Component Key | Notes |
|-----------|------|--------------|-------|
| `button` | component_set | `766e61ed41043c3071c80f9f7e87420630c7dcb1` | Primary action button |
| `button iconic` | component_set | `8ef170aefb1f0aa9b4980cf2f0ed1f8fb56430bd` | Icon-only button (32px or 40px) |
| `Dropdown` | component_set | `2db3c35820c36711b04b9eeb43cdf147c102b509` | Menu trigger button |
| `checkbox` | component_set | `a1c0f8a0f1450dad2e2907d82b800067c8544e87` | Custom-coloured checkbox |
| `tabs` | instance | — | Horizontal tab bar |

### Forms & Inputs

| Component | Type | Component Key | Notes |
|-----------|------|--------------|-------|
| `textarea/input-box` | component_set | `dbc34d61a101521011bf10c2d75c69633fc3ee89` | Multi-line text input |
| `text-input fieldset` | component | `c2bc63ea40ff32f49d857aa122a27f28de8b00ed` | Labelled text input |

```html
<!-- Text input HTML pattern -->
<fieldset class="fieldset">
  <legend class="fieldset-legend">Title</legend>
  <input type="text" class="input" placeholder="Input text" />
</fieldset>
```

### Cards & Widgets

| Component | Type | Component Key | Notes |
|-----------|------|--------------|-------|
| `card/card-actions` | component_set | `90306f3ad1a9636c4924c4e5bfecb3a8d51a3f8f` | Card with action buttons |
| `promo-card` | component_set | `d60f510847de4e06a584445bc629792543397794` | Image carousel card (company announcements) |
| `history-card` | component | `10a002a886d78bde3fb59e7368975b157afebd68` | Activity history item card |
| `upcoming-meeting-card` | component_set | `98d67667bd5a6611137b19889ce3219002deccfa` | Calendar/meeting card |
| `quick-links-card / list` | component | `ed0576502d0ef77cd55cd9bbdc079a5e9fc6f707` | Quick links as a list |
| `quick-links-card / grid` | instance | — | Quick links as 2×2 icon grid |
| `apps-card` | instance | — | Application shortcuts card |
| `popular-content-card` | instance | — | Top KB articles + service catalog |
| `recent-conversations-card` | instance | — | Recent AI conversations |
| `requests-card` | instance | — | Open service requests / inbox |
| `card/text-block` | instance | — | Text block inside a card |
| `_card-header` | instance | — | Card chrome header bar (56px) |
| `_side-panel` | instance | — | Slide-in right panel (340px) |
| `figure / start` | instance | — | Media/image figure in card |
| `highlight section` | instance | — | Highlighted callout block |
| `media-card / vertical` | instance | — | Vertical image + text card |

### Data Display

| Component | Type | Component Key | Notes |
|-----------|------|--------------|-------|
| `List` | component_set | `0b39ad53ab16bc07b3b5012016ec04e680bcfb07` | List of items |
| `Badge` | component_set | `dd63a1f9e9a136dbaf84448bfebf3dcbde618568` | Status badge (semantic colour) |
| `Calendar` | component | `07e1a2273417337f59b5f5df59bfa969ca8ab0eb` | Calendar widget |
| `Progress` | component_set | `ab0ea0b214c4936a2165dd9ada4971ef1bbeb854` | Progress bar |
| `avatar` | component_set | `2b335923b92eecd38de285a800753c1523198522` | User avatar |

### Disclosure & Overlays

| Component | Type | Component Key | Notes |
|-----------|------|--------------|-------|
| `Accordion` | component_set | `357e566a0f31c091280886bfa1e74c5d9956018d` | Expandable FAQ / sections |
| `Popover` | component_set | `5d7a26577f4be066ac87dcdeb09800ec18ba2026` | Contextual overlay |
| `notifications-content` | component | `f1e8544d308aef669de9b8f80b48f68aacd81c0b` | Notifications panel content |

```html
<!-- Accordion web component -->
<sn-aix-accordion-group id="myAccordion"></sn-aix-accordion-group>
<script>
  document.getElementById('myAccordion').items = [
    { title: 'Section 1', content: 'Content here.', open: true },
    { title: 'Section 2', content: 'More content.' },
  ];
</script>
```

### DaisyUI Class Mapping

When generating code, use these DaisyUI class names for Horizon 2.0 components:

| Horizon component | DaisyUI class(es) | Notes |
|------------------|-----------------|-------|
| `button` | `btn btn-primary` / `btn-secondary` / `btn-ghost` | Add size: `btn-sm` `btn-md` `btn-lg` |
| `button iconic` | `btn btn-square btn-ghost` | Icon-only, add size modifier |
| `Dropdown` | `dropdown` + `btn` + `dropdown-content` | Wraps trigger + menu |
| `checkbox` | `checkbox` | Customised colours via `--color-*` |
| `textarea/input-box` | `textarea textarea-bordered` | |
| `text-input fieldset` | `fieldset` + `input input-bordered` | See HTML pattern below |
| `Badge` | `badge` + semantic variant | `badge-info` `badge-success` `badge-warning` `badge-error` |
| `card/card-actions` | `card` + `card-body` + `card-actions` | |
| `List` | `menu` or `list` | |
| `Accordion` | `sn-aix-accordion-group` (web component) | Not a DaisyUI class |
| `Popover` | `popover` or `dropdown` | |
| `Progress` | `progress` | Add `value` and `max` attrs |
| `avatar` | `avatar` + `placeholder` | |
| `tabs` | `tabs` + `tab` + `tab-active` | |
| `pagination` | `join` + `btn join-item` | |
| `omni-bar-footer` | Horizon-specific — use component key | No DaisyUI equiv |
| `side-navigation` | Horizon-specific — use component key | No DaisyUI equiv |

---

## Flexible Layout Composition (for layouts not in the templates)

When the request doesn't match any template exactly, **compose** from these primitives:

### Chrome (always present)
Every screen has:
1. `side-navigation` — leftmost, full height (52px collapsed / 288px expanded)
2. A main panel — fills remaining width, 8px inset card
3. `omni-bar-footer` — floating at y=710, centred, **not** full-width

### Content zone types — mix and match
| Zone | Width pattern | Typical components |
|------|-------------|-------------------|
| Full-width header | 100% of panel | `page-heading-block`, `_card-header`, title + actions row |
| 3-col grid | 3 × equal cols, 16px gap | Any cards, consistent per breakpoint |
| 2-col grid | 2 × equal cols, 24px gap | Badge cards, media cards, related resources |
| Single wide column | 100% of panel inner | Paragraphs, tabs, timeline, accordion, attachments |
| Split (left panel + right panel) | 484px + 904px | Chat + document (Layout D) |
| Right overlay | 340px from right | `_side-panel` + scrim |

### Rules for novel layouts
- **Forms:** Use `text-input fieldset` and `textarea/input-box` in a single column (max ~600px). Add `button` row at bottom.
- **Table / list views:** Use `List` component + `pagination` at bottom. Header row uses `Dropdown` (filter) + `button` (actions).
- **Empty states:** Centre content in the panel; use an icon (48px), heading text, body text, and a single `button`.
- **Settings / profile:** Single column, section headers, `checkbox` rows, `button` save row — no cards needed.
- **Scrollable long-form content:** Follow Layout D's content section patterns. Always add a gradient fade (80px) at the bottom of the visible area.
- **Never place omni-bar-footer inside a card** — it always floats at the frame level.

### Adding a new layout from a Figma URL
When the user shares a new reference URL:
1. `get_metadata(nodeId, fileKey)` → extract frame name, dimensions, instance names, column structure
2. Add a new Layout entry to "Screen Layout Templates" using the ASCII diagram format
3. Update the decision tree with the new case

---

## Page Layout Gallery (GA May Release)

All layouts below are in file `mplcJSFS3G8LLZkNCHm4q0`, node `419:24768` (Executive Summary page). Each section contains light mode, dark mode, and instance screenshots. **Use light mode section IDs by default.**

> Use these to look up a page type that isn't covered by the templates above. Run `get_screenshot` or `get_design_context` on the section node to see the full layout.

| Page | Light mode section ID | Dark mode section ID | Notes |
|------|----------------------|---------------------|-------|
| Navigation states | `938:23031` | `938:24460` | All nav variants: collapsed, expanded, side panel |
| Homepage | `419:24769` | `909:109456` | AI-first entry with omni-bar + 3-col widgets |
| Content authoring | `419:107643` | `909:115083` | Rich text editor layout |
| Canvas | `419:26395` | `909:115915` | Widget dashboard (matches Layout B) |
| Canvas admin | `419:32576` | `909:118068` | Admin view of canvas |
| People finder | `419:41555` | `909:118922` | Directory / org chart view |
| Profile | `419:42594` | `938:24478` | User profile page |
| Inbox | `419:47758` | `909:120556` | Inbox with 2-col layout |
| Catalog | `419:48718` | `909:121492` | Service catalog browse |
| KB article | `419:110083` | `909:122290` | Article detail with chat (matches Layout D) |
| Admin | `419:92161` | — | Admin configuration screens |

**How to use:**
```
get_screenshot(fileKey: "mplcJSFS3G8LLZkNCHm4q0", nodeId: "<section-id>")
→ visually pick the exact frame you want

get_design_context(fileKey: "mplcJSFS3G8LLZkNCHm4q0", nodeId: "<frame-node-id>",
  clientLanguages: "html,css", excludeScreenshot: true)
→ extract structure, layout, and asset URLs
```

---

## Layout Selection Decision Tree

```
Is this a home / landing page with an AI prompt?
  → YES → Layout A (Home Dashboard) or Homepage section in Layout Gallery

Is this a dashboard with configurable widgets (no prominent AI bar at top)?
  → YES → Layout B (Employee Hub Dashboard) / Canvas section in Layout Gallery
       → Does the user open a detail record? → Add Layout C (Side Panel)

Is this showing a document / KB article / policy alongside an AI chat?
  → YES → Layout D (Chat + Document Detail) / KB article section in Layout Gallery

Is this a catalog or gallery of widgets to browse/add?
  → YES → Layout E (Widgets Catalog) / Catalog section in Layout Gallery

Is this a people directory / org chart / people finder?
  → YES → People finder section in Layout Gallery (`419:41555`)

Is this a user profile page?
  → YES → Profile section in Layout Gallery (`419:42594`)

Is this an inbox / notifications / task list?
  → YES → Inbox section in Layout Gallery (`419:47758`)

Is this a content editor / authoring tool?
  → YES → Content authoring section in Layout Gallery (`419:107643`)

Is this an admin or configuration screen?
  → YES → Admin section in Layout Gallery (`419:92161`) or Canvas admin (`419:32576`)

None of the above?
  → Check the Layout Gallery first (get_screenshot on parent node `419:24768` to browse all pages)
  → Then use Flexible Layout Composition rules to compose from zone primitives
```

---

## Confirmed Token Values (from live `get_variable_defs` — verified 2026-03-27)

These are the **real resolved values** from `SPmKj8MGJIDvOJhvDbNIZO`. Use these directly in HTML/CSS output:

| Token | Value | Usage |
|-------|-------|-------|
| `background/primary` | `#ffffff` | Card, panel, input surfaces |
| `background/secondary` | `#f3f3f3` | Omni-bar outer shell |
| `background/tertiary` | `#eeeeed` | App background (the page canvas) |
| `text/primary` | `#000000` | Headings, primary body text |
| `text/secondary` | `#1e293b` | Secondary text, card titles |
| `text/tertiary` | `#656462` | Placeholder, timestamps, captions |
| `primary` | `#007b9e` | Primary action color (teal) |
| `secondary` | `#e1e1e0` | Badge background, secondary button bg |
| `secondary-content` | `#1e293b` | Text on secondary surfaces |
| `neutral/200` | `#e2e8f0` | Borders, dividers |
| `neutral/400` | `#c2c1bf` | Active nav item pill background |
| `info` | `#1b59bb` | "In progress" badge color |
| `accent` | `#68e353` | Mic/voice button (bright green) |
| `border-radius/rounded-sm` | `4px` | Inputs, small elements |
| `border-radius/rounded-md` | `8px` | (`rounded-lg` in Figma) Buttons, nav icons |
| `border-radius/rounded-lg` | `12px` | (`rounded-xl`) Cards inner, link items |
| `border-radius/rounded-xl` | `16px` | (`rounded-2xl`) List items, chat bubbles |
| `border-radius/rounded-2xl` | `24px` | (`rounded-3xl`) Cards, main panels |
| `border-radius/rounded-full` | `9999px` | Badges, avatars, pill buttons |
| `box-shadow/card` | `0 1px 2px rgba(0,0,0,.10), 0 1px 2px rgba(0,0,0,.10)` | Widget cards |
| `box-shadow/card-raised` | `0 1px 2px rgba(0,0,0,.10), 0 1px 2px -1px rgba(0,0,0,.10)` | Popovers, chat items |
| `box-shadow/omni` | `0 20px 25px -5px rgba(0,0,0,.10), 0 8px 10px -6px rgba(0,0,0,.10)` | Omni-bar floating |
| `box-shadow/inner` | `0 0 8px rgba(0,0,0,.05)` | Omni-bar inner white ring |
| Font (headings) | `ServiceNow Sans` (fallback: `Inter`) | Page title, subtitle |
| Font (body/UI) | `Inter` | Card titles, body text, badges |

> **Note:** Figma calls `8px` radius `rounded-lg` and `24px` `rounded-3xl` — the values above map Figma naming → actual px for use in CSS.

---

## High-Fidelity HTML Workflow

> Use this when the deliverable is an **HTML file** that matches the Figma design with pixel accuracy (real icons, real app images, real colors).

The key insight: **this skill doc is an index and guide — it does not replace live Figma data.** Always call `get_design_context` on the real layout node to get actual asset URLs, resolved CSS values, and correct component structure. Never approximate icons, app logos, or images.

### Step-by-step

**Step 1 — Screenshot the target**
```
get_screenshot(fileKey: "<layout-file-key>", nodeId: "<layout-node-id>")
```
This is your visual source of truth. Keep it visible throughout.

**Step 2 — Get full design context for the layout**
```
get_design_context(
  fileKey: "<layout-file-key>",
  nodeId: "<layout-node-id>",
  clientLanguages: "html,css",
  clientFrameworks: "unknown",
  excludeScreenshot: true   ← saves context window (already got screenshot above)
)
```
The response contains React+Tailwind reference code with:
- **Real `https://www.figma.com/api/mcp/asset/…` URLs** for every icon, logo, avatar, and image
- **Resolved CSS values** (actual hex colors, px sizes, shadow RGBA values)
- **Exact component structure** (nesting, flex/grid layout, spacing)

> ⚠️ **Output is often 80–100KB.** It will be saved to a tool-result file. Read it in chunks using `offset` + `limit` (30 lines at a time) or use python3 to parse it as JSON.

**Step 3 — Extract assets and structure**
From the JSON output (array of text items):
- Item `[0]` — the main React+Tailwind code (~98KB). Contains all `const imgXxx = "https://…"` asset URLs
- Items `[3–6]` — style descriptions, Code Connect hints, component docs, asset expiry note

Collect all asset URL constants. They expire in **7 days**.

**Step 4 — Convert React → HTML/CSS**
Translate the Figma output to plain HTML:
- Replace `className=` → `class=`
- Replace Tailwind arbitrary values with inline CSS or CSS variables (e.g. `var(--foundation/px/2-(8px),8px)` → `8px`)
- Keep `position: absolute` only where the Figma code uses it for overlay elements (omni-bar, gradient, active indicator)
- Use `display: flex` + `flex: 1` for the main layout — **never fixed px widths for the outer shell**

**Step 5 — Make the layout responsive**
Always structure as:
```css
.desktop   { display: flex; width: 100%; height: 100vh; }
.side-nav  { flex-shrink: 0; width: 52px; height: 100%; }
.main-panel { flex: 1; min-width: 0; height: 100%; padding: 46px 44px 0;
              display: flex; flex-direction: column; overflow: hidden; }
```
Cards column grid uses `flex: 1 0 0` on each column (already in Figma output).
Add breakpoints: 2-col at ≤860px, 1-col at ≤600px.

**Step 6 — Position the omni-bar**
```css
.omni-bar-footer {
  position: fixed;
  bottom: 20px;
  left: calc(52px + (100vw - 52px) / 2);   /* center in main area */
  transform: translateX(-50%);
  width: min(766px, calc(100vw - 52px - 80px));
}
```

**Step 7 — Inject asset URLs via JS**
Store all Figma CDN URLs in a JS object, set `img.src` on `DOMContentLoaded`. Never embed URLs as `src=` attributes directly (easier to batch-update when they expire).

**Step 8 — Layout rule: header sits on background, not on a card**
The page heading ("Canvas", subtitle, action buttons) renders directly on `background/tertiary` (`#eeeeed`). Only the widget cards have `background: white` with `box-shadow`. Do **not** wrap the entire main panel in a white card.

---

## Design Workflow

### Step 1 — Understand the request
Ask (or infer): What is the page for? Who uses it? What is the primary action? What breakpoint?

### Step 2 — Pick or compose a layout
Use the decision tree. If no template fits exactly, use the Flexible Layout Composition rules.

### Step 3 — Fetch live Figma data (always — this is the primary source of truth)

**For the layout frame:**
```
get_screenshot(nodeId: "<layout-node>", fileKey: "<file-key>")
get_design_context(nodeId: "<layout-node>", fileKey: "<file-key>",
  clientLanguages: "html,css", excludeScreenshot: true)
```

**For individual components (if layout context is truncated):**
```
get_metadata(nodeId: "<layout-node>", fileKey: "<file-key>")
→ identify child node IDs
get_design_context(nodeId: "<child-node-id>", fileKey: "<file-key>")
```

**For component variant names:**
```
search_design_system(query: "<component name>", fileKey: "SPmKj8MGJIDvOJhvDbNIZO")
```
Never guess variant names (size, style, state) — retrieve them.

### Step 4 — Use confirmed tokens
Color, spacing, radius, and shadow values are in **"Confirmed Token Values"** above — use those directly. To verify or find newer tokens:
```
get_variable_defs(nodeId: "23:612", fileKey: "SPmKj8MGJIDvOJhvDbNIZO")
```

### Step 5 — Compose the screen
- Start from chrome: `side-navigation` + `omni-bar-footer`
- Place content zone(s) with correct column widths for the breakpoint
- Fill with components using real asset URLs from `get_design_context`

### Step 6 — Output
| Deliverable | Dimensions | Approach |
|------------|-----------|--------|
| **Figma design mockup** | **1440 × 1024px** (height flexible, extends with content) | Use `generate_figma_design` with component keys + token names |
| **Image / screenshot** | As-is from Figma | `get_screenshot` on target node |
| **HTML prototype / simulation** | Responsive — `100vw × 100vh`, no fixed px | Follow the High-Fidelity HTML Workflow above |
| **Production code** | Match target framework conventions | Run `get_design_context` per component, then adapt |
| **Spec / annotation** | N/A | List each zone: component name · variant · width · token references |

> **Not sure which the user wants?** Ask before generating — see Output Modes & Dimensions section above.

---

## Known Icon Names (from reference layouts)

Use these names when placing icons inside components. Full set is in the Icons library — search by keyword.

`menu` · `microphone` · `send` · `plus` · `circle-plus` · `calendar-clock` · `checkbox-checked` · `eye` · `fire` · `play` · `document` · `documents` · `chevron-down` · `chevron-left` · `chevron-right` · `magnifying-glass` · `circle-info` · `circle-question-outline-32` · `pencil-ai-sparkle` · `code` · `calendar` · `clone` · `thumbs-up` · `thumbs-down` · `sort-ascending` · `sort-descending`

Search for more:
```
search_design_system(
  query: "<keyword>",
  fileKey: "SPmKj8MGJIDvOJhvDbNIZO",
  includeLibraryKeys: ["lk-dff6270ac2ed60b2bad847fa1d65f0eb364804b8856787b0ced2a0274ad8299b9655ee6e4701e9480093c63a3122b40d34bd277dd20034adaa0799949b3b42b2"]
)
```

---

## Rules

1. **Never use deprecated components** — skip anything prefixed `⛔ deprecated_`
2. **Figma mockups are 1440×1024px** — width fixed at 1440px, height starts at 1024px and extends downward with content; never use 810px as a design canvas height (that is only a viewport clipping reference)
3. **HTML prototypes are always responsive** — use `width:100%; height:100vh` for the shell; never hardcode fixed pixel dimensions on the outer container; content scrolls vertically
4. **Match the breakpoint** — 374px card columns at 1440px; 288px at 1280px
3. **omni-bar-footer position** — always at `y=710`, horizontally centred, never full-width, never inside a card
4. **side-navigation state** — icon-only (52px) at 1440; expanded (288px) at 1280
5. **No invented components** — search Figma first; if nothing found, compose from listed primitives
6. **Token-first always** — use `--color-*` CSS variables and `$sp-space--*` spacing; no raw hex or px values
7. **DaisyUI classes first** — use DaisyUI class names (`btn`, `card`, `badge`, `input`, `checkbox`, etc.) before writing custom CSS
8. **Fetch variants before specifying** — never assume button size/style/state names; run `search_design_system`
9. **ALPHA caution** — Figma is the source of truth; this doc may lag behind live changes
10. **Broken Figma access → ask the user** — if any `get_design_context`, `get_screenshot`, or `get_variable_defs` call fails with an access/not-found error, immediately ask: *"The Figma file seems to have moved or lost access. Can you share the current Figma link for [file name]?"* Extract the new file key from the URL they provide (`figma.com/design/<fileKey>/…`). Never proceed by approximating from this skill doc alone.
10. **Flexible over rigid** — if the user's context doesn't match a template exactly, compose using the zone primitives
11. **Always surface what you fetched** — include retrieved token values / variant names in output so the user can verify

---

## Quick Reference Cheatsheet

```
FRAMEWORK: Tailwind CSS + DaisyUI + sn-aix-* web components

OUTPUT MODES (ask user if not specified):
  Mode 1 — Figma mockup  → 1440 × 1024px (push to Figma via generate_figma_design)
  Mode 2 — Image/screenshot → get_screenshot on target node
  Mode 3 — HTML prototype → responsive: width:100%; height:100vh (no fixed px on shell)
  Mode 4 — Spec/annotation → written component + token reference

HTML PROTOTYPE — FROZEN vs SCROLLABLE:
  Frozen (fixed, does not scroll): side-navigation, page/section headers (_card-header)
  Scrollable: main panel content, widget cards, article body, list items

Chrome skeleton (every screen):
  side-navigation [52px or 288px] + main panel + omni-bar-footer [y=710, centred]

3-col grid (1440px):  3 × 374px, gap 16px  (Tailwind: gap-4)
3-col grid (1280px):  3 × 288px, gap 16px

SPACING TOKENS:
  xxs=2px  xs=4px  sm=8px  md=12px  lg=16px  xl=24px  xxl=32px  3xl=40px

COLOR TOKENS (CSS vars — DaisyUI semantic):
  --color-primary  --color-secondary  --color-accent  --color-neutral
  --color-base-100/200/300  --color-base-content
  --color-info  --color-success  --color-warning  --color-error
  --color-background-primary/secondary/tertiary/inverted
  --color-text-primary/secondary/tertiary/inverted

BORDER RADIUS:
  rounded-badge → small (checkbox, toggle)
  rounded-btn   → medium (button, input, tab)
  rounded-box   → large (card, modal)

SHADOWS:  shadow-sm (card) · shadow-lg (dropdown) · shadow-xl (modal)

TYPOGRAPHY:
  heading-01(48) → heading-06(18)
  body-lg(18) → body-xs(12)  [Light weight]
  label-xl(18) → label-xs(10)
  button-xl(22) → button-xs(11)  [180% line-height]

Key component keys (Horizon 2.0 Components):
  button            766e61ed41043c3071c80f9f7e87420630c7dcb1
  button iconic     8ef170aefb1f0aa9b4980cf2f0ed1f8fb56430bd
  Badge             dd63a1f9e9a136dbaf84448bfebf3dcbde618568
  side-navigation   52669afebe2f4fb167810a47137373ed7686c83f
  omni-bar-footer   fde4edbcf7105d044075e37a7353cc2e70ddc69b
  Accordion         357e566a0f31c091280886bfa1e74c5d9956018d
  Dropdown          2db3c35820c36711b04b9eeb43cdf147c102b509
  avatar            2b335923b92eecd38de285a800753c1523198522
  Progress          ab0ea0b214c4936a2165dd9ada4971ef1bbeb854
  Popover           5d7a26577f4be066ac87dcdeb09800ec18ba2026

Variant lookup:
  search_design_system(query, fileKey: "SPmKj8MGJIDvOJhvDbNIZO",
    includeLibraryKeys: ["lk-73b4...dca4"])

LAYOUT GALLERY (GA May Release):
  fileKey: mplcJSFS3G8LLZkNCHm4q0  node: 419:24768 (browse all)
  Homepage 419:24769 · Canvas 419:26395 · Inbox 419:47758 · Catalog 419:48718
  KB article 419:110083 · People finder 419:41555 · Profile 419:42594
  Content authoring 419:107643 · Admin 419:92161
  (append 909:* IDs for dark mode variants — see Page Layout Gallery section)
```
