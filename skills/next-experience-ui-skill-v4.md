---
name: next-experience-ui
description: Design UI screens and components using the ServiceNow Next Experience (NX) design system. Use when asked to design, mock up, or generate UI for ServiceNow NX / Now Platform experiences — Guided Setup, Setup Wizard, Product Hub, CBS overview, Core ITSM, Now Assist. Triggers on "Next Experience", "NX design", "now-button", "now-card", "guided setup", "playbook", "setup wizard", "ServiceNow UI". v4 — Figma Mode 1 workflow; confirmed NX component keys (Button, Card, Highlighted Value); NX Icons library with 19 common icon keys; Foundations spacing/radius/shadow scale; NX published variable keys; redesign-from-reference workflow.
disable-model-invocation: false
---

# Next Experience UI Design Skill

## Overview

The ServiceNow **Next Experience (NX)** design system powers the Now Platform's modern UI — including Guided Setup wizards, Product Hub, CBS (Core Business Suite) overviews, and Now Assist configuration screens. The system uses `<now-*>` web components, Tailwind CSS utility classes, and CSS custom properties with a `--now-color_*` / `--now-global-*` token namespace. Audience: platform admins, implementers, and IT/HR/CSM agents configuring ServiceNow products.

### Generation mode — infer from the prompt, do not ask

There are three modes. Pick the wrong one and the output will feel jarring. Infer silently from the user's words — never ask.

| Mode | How to detect | Rule |
|------|--------------|------|
| **Fresh generation** | "Design a…", "Create a…", "Build a new…", no reference provided | Use design system defaults. Canonical layout, standard NX nav structure, documented patterns. |
| **Reference recreation** | "Recreate", "Rebuild", "Match this", "Based on this", provides a Figma URL or image | **Treat the reference as a hard constraint.** Map navigation labels, hierarchy, and active states 1:1. Only swap in the correct NX components and tokens underneath — the *content* stays, the *implementation* gets upgraded. Do NOT reset to design system defaults. |
| **Exploration / iteration** | "Add X", "Try Y", "What if…", "Change X to Y", working from an existing file | Treat the current working file as the reference. New elements inherit the style of what's already there. Never reset existing structure to defaults. |

> **When a Figma URL or image is provided, always default to reference mode** — the user is showing you what they have and expecting it to be respected, not replaced.

### When designing, always:
1. **Determine generation mode first** — fresh, reference, or exploration (see above)
2. **Fetch before you build** — call `get_screenshot` + `get_design_context` on the closest reference screen before writing any code
3. **Use confirmed token values** — colors, radius, and shadows are captured below; do not guess
4. **Use `<now-*>` web component names** in specs and code — never invent component names
5. **Match the layout template** — use the decision tree; 282px left nav at 1440px is mandatory for all Setup screens
6. **Never wrap the page title in a card** — it is the single most commonly broken rule

---

## Getting Started

### Install in 30 seconds

**Claude Code (CLI or desktop app)**

Option A — Settings UI (recommended):
1. Open Settings → Customize → Skills
2. Add this file — Claude registers it automatically
3. Invoke with `/next-experience-ui`

Option B — Chat install:
1. Drag this file into your Claude Code chat
2. Type: `install this as a skill`
3. Invoke with `/next-experience-ui`

**Claude Web / claude.ai**
1. Open or create a Project
2. Go to Project instructions → Edit
3. Drag this file in, or copy-paste the full contents
4. The skill is now always active for that project — no slash command needed

**Connect Figma** (required for Figma mode, one-time setup)
- Claude Code: Settings → MCP Servers → add `plugin:figma:figma` → sign in
- Claude Web: connect Figma via the integrations panel in your Project

### How to invoke

```
/next-experience-ui

Design a CBS multi-service overview with HR and Legal services and Input needed badges.
```

```
/next-experience-ui

Recreate this screen in NX: https://www.figma.com/design/zy4c7wWFtXxztmV8lP4tGq/...?node-id=6-26700
```

### If Figma access breaks

If Claude reports a file is inaccessible, paste the current Figma URL into the chat — Claude will extract the new file key and continue.

---

## Output Modes & Dimensions

> **If the user has not specified a format, always ask before generating:**
>
> *"What format would you like?*
> *1. **Figma design** — push the screen directly into Figma as a frame*
> *2. **HTML prototype** — responsive coded prototype that runs in the browser*
> *3. **Image / static mockup** — screenshot of the closest Figma reference with annotations"*

---

### Mode 1 — Figma Design (push to Figma)
> Use when: designer wants a live editable frame in Figma

> **If no Figma URL or file key is provided**, ask before doing anything: *"Which Figma file and page should I place this on?"* — never push to a default or previously used file without confirmation.

- **Canvas width: 1440px** (fixed — NX desktop standard)
- **Canvas height: 1024px minimum** — extend downward as content requires; 1024px is the visible viewport reference, not a hard limit
- Frozen zones (global nav + sub-header) are placed at the top of the frame; they do not repeat — in Figma they are static elements at y=0 and y=52

#### Required Figma Mode 1 workflow

**Step 1 — Search before building.** Always call `search_design_system` for every interactive or semantic element before drawing it manually. Use the target file key for context. Search terms: "button", "highlighted value", "card container", "input", "badge", "progress".

**Step 2 — Import NX components by key.** Use `importComponentSetByKeyAsync` with the confirmed keys below. Never draw buttons, badges, or form elements as manual frames.

**Step 3 — Find the exact variant.** Match by exact variant string (e.g. `"Variant=primary, Size=md, State=default"`). Use `.find()` with `c.name.includes(...)` as fallback.

**Step 4 — Set text via `setProperties()`.** Use the component property key (e.g. `"Label#35908:169"`) not direct `node.characters` on instances.

**Step 5 — For custom layout containers** (cards with custom slot content): draw a manual frame (white bg, `#d7e1e5` border, 8px radius) and place NX component instances *inside* it. Do NOT detach `<now-card>` unless its visual shell (shadow, sidebar strip) is the specific goal — detaching breaks library link.

**Step 6 — Validate with `get_screenshot`** after each section. Check for: clipped text, wrong variant colors, placeholder text still showing.

### Mode 2 — HTML Prototype
> Use when: developer or designer wants a working prototype in the browser

**Target viewport: 1440px wide.** Design and test at this width. Content must scroll cleanly at 1440px; do not optimize for narrower viewports unless asked.

**Frozen zones — these never scroll with page content:**

| Zone | CSS | Height |
|------|-----|--------|
| Global nav bar | `position: sticky; top: 0; z-index: 100; flex-shrink: 0;` | 52px |
| Guided setup sub-header | `position: sticky; top: 52px; z-index: 99; flex-shrink: 0;` | 64px (Layout A/B/C only) |
| Left nav rail | `flex-shrink: 0; width: 282px; height: 100%; overflow-y: auto;` | Full height, scrolls independently |

**Scrollable zone — main content:**
```css
.main { flex: 1; min-width: 0; overflow-y: auto; padding: 24px 48px; background: #e5edf0; }
```
Content height is never fixed — it grows with content and scrolls within the viewport.

**Full shell:**
```css
.shell      { display: flex; flex-direction: column; width: 100%; min-width: 1440px; min-height: 100vh; }
.header     { position: sticky; top: 0; z-index: 100; flex-shrink: 0; width: 100%; height: 52px; background: #032d42; }
.sub-header { position: sticky; top: 52px; z-index: 99; flex-shrink: 0; width: 100%; height: 64px; background: #ffffff; border-bottom: 1px solid #cfd5d7; }
.body-row   { display: flex; flex: 1; min-height: 0; }
.left-nav   { flex-shrink: 0; width: 282px; overflow-y: auto; background: #ffffff; border-right: 1px solid #d7e1e5; }
.main       { flex: 1; min-width: 0; overflow-y: auto; padding: 24px 48px; background: #e5edf0; }
```
Follow the **High-Fidelity HTML Workflow** below for all remaining steps.

### Mode 3 — Image / Static Mockup
> Use when: stakeholder needs a quick visual reference, not a working prototype

- Call `get_screenshot` on the closest reference node from the Figma layouts file
- Annotate the screenshot with zone labels and token callouts inline
- Note: if no close reference exists, generate the HTML (Mode 2) and instruct the user to screenshot it at 1440px width

### Mode 4 — Spec / Annotation
> Use when: designer or PM needs a written component + token reference without code

- List each zone: component name · NX element · dimensions · token values
- No code or Figma frame generated

---

## What's in this skill vs what needs a live Figma call

| Area | Status | Notes |
|------|--------|-------|
| Layout templates (A–E) | ✅ Captured | Exact dimensions, node IDs, zone breakdowns |
| Component inventory | ✅ Captured | Names + component keys for all known NX components |
| **Color token values** | ✅ **Verified live** | Resolved hex values from `get_design_context` — use directly |
| Typography scale | ✅ Captured | Full `ServiceNow Sans` scale with px, weight, line-height |
| Spacing scale | ✅ Captured | Full `now-global-space--*` scale from Foundations file |
| Border radius scale | ✅ Captured | Developer theme: sm=4px, md=6px, lg=12px — verified from Foundations |
| Shadow tokens | ✅ Partial | Usage patterns captured; exact `rgba` for each tier needs live fetch |
| NX variable keys | ✅ Captured | 9 semantic variable keys for fill/stroke binding |
| **Common icon keys** | ✅ Captured | 19 most-used icons pre-loaded; use directly without searching |
| Button component key + property | ✅ Confirmed | Key + `"Label#35908:169"` property tested in `use_figma` |
| Highlighted Value key + property | ✅ Confirmed | Key + `"Label#37075:0"` property tested in `use_figma` |
| Card container key | ✅ Confirmed | Key tested; use manual frame shell for custom content |
| **Uncommon icon keys** | ⚠️ Fetch live | Search `NNuRiz7WTDl7d9JvvMLFUC` with keyword — thousands of icons exist |
| Property keys for other components | ⚠️ Fetch live | Beyond Button + HV, inspect via `use_figma` componentProperties |
| Component variant names / states | ⚠️ Fetch live | Run `search_design_system` for any component not listed above |
| **Icon/image CDN assets** | 🔴 **Must fetch live** | `get_design_context` returns real CDN URLs — never approximate |

---

## Figma Reference Files

> **If any file key below returns an error, access denied, or "file not found":** stop and ask the user —
> *"The Figma file I need seems to have moved or lost access. Can you share the current Figma link for [file name]?"*
> Extract the new file key from the URL they paste. Do not approximate without live Figma data.

| File | Purpose | File Key |
|------|---------|----------|
| `Next Experience -> Components` | Component library — all NX components | `eB8K8rmfYgbAGaQh2Q1VZ9` |
| `Now Assist for Setup UI Toolkit` | Layout references — guided setup, product hub, CBS, ITSM screens | `zy4c7wWFtXxztmV8lP4tGq` |
| `Next Experience -> Foundations` | Authoritative token definitions — spacing, radius, elevation, typography, themes | `tNs5GTwfggNH91PgFzoyYe` |
| `Next Experience -> Icons` | Full NX icon library — thousands of icons as component sets | `NNuRiz7WTDl7d9JvvMLFUC` |

### Key Node IDs

| File | Node ID | Description |
|------|---------|-------------|
| Components | `807:66061` | **Component Index** — all NX components categorized into Library / Stickersheet / Engineering-specific tiers |
| Components | `2:76` | Getting Started page |
| Components | `102323:786` | Color reference frame |
| Components | `50743:174464` | Typography reference frame |
| Components | `50743:174430` | Templates frame |
| Layouts | `6:26700` | Core ITSM overview — single service (1440×1160) |
| Layouts | `6:85146` | CBS overview — multi-service (1440×1597) |
| Layouts | `10:68280` | Setup task screen (1440px, form in progress) |
| Layouts | `1559:141280` | Product Hub overview (540px preview) |
| Layouts | `1559:141358` | Configuration Console overview (540px preview) |
| Layouts | `104:96285` | Configuration Console — full spec (multiple views: ITOM, ITSM, dark nav) |
| Layouts | `97:89346` | Configuration templates — Form, List, List+form, Add-new-one patterns |
| Layouts | `1584:157690` | Macroponent examples — reusable page patterns (Form, List, Lock-out, Iframe) |
| Layouts | `1574:146854` | Admin home widgets — ITSM, ITOM, CMDB home page layouts |
| Layouts | `1559:53953` | AI patterns and guidelines — Now Assist orchestrator behaviors |

### Design System Library Key
`lk-e2a96b8a6b022723389d7e20cdd84b916a16d8c5340590900d95513cd3554584bfc679b32e8aaa0fafe20486d64ac955cad15153d9fea49a90783ad51f54948c`

---

## Confirmed NX Component Keys (verified working — 2026-04-02)

Use these keys with `importComponentSetByKeyAsync` in `use_figma` scripts. All keys are from the `Next Experience -> Components` library.

### `<now-button>` — key `6dc4fab588de9bac411e6b5b73ca30830dbf428c`

105 variants. Property key for label text: `"Label#35908:169"`

| Use case | Variant string |
|---|---|
| Primary action (e.g. "+ New Rule", "Create") | `"Variant=primary, Size=md, State=default"` |
| Secondary action (e.g. "Use template →", "Cancel") | `"Variant=secondary, Size=sm, State=default"` |
| Large primary CTA | `"Variant=primary, Size=lg, State=default"` |
| Destructive action | `"Variant=primary-negative, Size=md, State=default"` |

```js
const btnSet = await figma.importComponentSetByKeyAsync("6dc4fab588de9bac411e6b5b73ca30830dbf428c");
const primaryMd = btnSet.children.find(c =>
  c.name.includes("Variant=primary") && c.name.includes("Size=md") && c.name.includes("State=default")
);
const btn = primaryMd.createInstance();
btn.setProperties({ "Label#35908:169": "Create New Rule" });
```

### `<now-highlighted-value>` — key `a41e6be6cd1479fecfbdd3d42ac4de76e4378a0a`

324 variants. Property key for label text: `"Label#37075:0"`

Variants: `Size` (sm/md) × `Color` (teal, blue, green, gray, orange, red/critical, yellow/warning, purple, …) × `Variant` (primary/secondary/tertiary) × `Type` (none/dot/icon)

| Use case | Variant |
|---|---|
| Category badge (HR, Ops, All) — text only, subtle | `tertiary` |
| **Status/condition pill with light background fill** | `secondary` ← use this for filled chips |
| Bold status chip (e.g. "Critical", "Active") | `primary` |
| "Low" priority | `Color=green, Variant=secondary` |
| Warning / "Needs setup" | `Color=yellow, Variant=secondary` |
| Critical / error | `Color=critical, Variant=primary` |

> ⚠️ **`tertiary` = text-only (no background fill).** Use `secondary` when you need the light-tinted pill background (e.g. condition rows, status chips). This is the most commonly confused variant.

```js
const hvSet = await figma.importComponentSetByKeyAsync("a41e6be6cd1479fecfbdd3d42ac4de76e4378a0a");
// Badge with light background
const hvSecTeal = hvSet.children.find(c =>
  c.name.includes("Color=teal") && c.name.includes("sm") && c.name.includes("secondary") && c.name.includes("Type=none")
);
const badge = hvSecTeal.createInstance();
badge.setProperties({ "Label#37075:0": "HR" });
// Resize to fill parent width (e.g. condition row)
badge.resize(390, badge.height);
```

### `<now-card>` — key `cac34afa2d36aa9f6a783d2d7270a069293c1f0e`

24 variants. Boolean property `"Sidebar#54110:0"` controls the 4px left accent strip.

| Use case | Approach |
|---|---|
| **Custom card with specific content layout** | Draw a manual frame (white, `#d7e1e5` border, 8px radius) and place NX components inside. Do NOT use `<now-card>` — its slot structure requires detaching for custom content, which breaks the library link. |
| Card that uses the standard NX slot/content system | Use `<now-card>` with `Size=lg, State=default, Hide shadow=false` + toggle `Sidebar#54110:0: true` for the accent strip |
| Accent strip color | Find the "sidebar" rectangle inside the card instance and change its fill after setting `Sidebar=true` |

```js
// Custom card shell (preferred for most use cases)
const card = figma.createFrame();
card.resize(430, 184);
card.fills = [{ type: 'SOLID', color: { r: 1, g: 1, b: 1 } }];
card.cornerRadius = 8;
card.strokes = [{ type: 'SOLID', color: { r: 0.843, g: 0.882, b: 0.898 } }]; // #d7e1e5
card.strokeWeight = 1;
// Then appendChild NX Button, Highlighted Value instances inside
```

---

## NX Icons Library (verified — 2026-04-02)

The NX icon set is published as a separate Figma library. Use `importComponentSetByKeyAsync` with any key below to add icons to frames in `use_figma` scripts. All icons are `COMPONENT_SET` type with size variants.

**Icons library key:** `lk-8764c939c09cecc1cf85ec71ac8c32ec56aaec28169e7190dc3369e887fa5b49497088d62e0ff1f9588adcf01cca5bf435536b92caa0a6c0ce97dc4df432ff46`

### Common icon component keys

| Icon name | Component key | Common use |
|---|---|---|
| `check` | `a9cbb8852ca1352e3a9d97deee09c368dc592781` | Success, completion |
| `check-strong` | `b450059ba2faf6a4ba5a18a55ab1fd06d4f3f81c` | Bold checkmark, stepper |
| `circle-check` | `1927cc9b7de09d3f5c76070987e40349ca847551` | Success state, approved |
| `close` | `39e647a2ad91c64e1db93c2c02a63933c1cfc409` | Dismiss, cancel, delete |
| `circle-close` | `da286a3c4b1c9ba97a7fe6d9fd436adc503d3211` | Error state, incorrect |
| `circle-info` | `9d3021a6e5defba3058edab7d6a5156046225e13` | Info tooltip, help |
| `triangle-exclamation` | `79c561429aee74647e9aa8a719aab1ba06771247` | Warning, alert, hazard |
| `magnifying-glass` | `a05ddeacdaf3731998fcca9966fd737eef4ba46b` | Search, find, lookup |
| `user` | `d6a285e424895824887ae16446b4021dbb5270f4` | Person, profile, avatar |
| `user-group` | `686814bb6880555abc11dcdd1fe3803531cd47e7` | Team, group, people |
| `bell` | `589748d11053f9b1b28af0fb148aac71bab8e57a` | Notifications, alert |
| `home` | `275b5a75401c154e27985987475aaa0126bb6ec5` | Home, main, homepage |
| `gear` | `aecace46140970e95cda3846f6b9f0a0248e5486` | Settings, configuration |
| `pencil` | `43a1a8f83476275680140d5f93108e4cf9c0707a` | Edit, create, write |
| `pencil-page` | `b686d507a9d7127c7d535cbc857553a6234fbbb0` | Edit document, annotate |
| `arrow-right` | `9cc6eac84fca66dc428093a051a298965024a02a` | Navigate forward, next |
| `chevron-right` | `7af8de256e041e35c2b84a8976d55b51c76b799c` | Drilldown, expand |
| `caret-right` | `44f02df0456608a8e40e615bf292cc98b445b5f0` | Tree expand, collapsed |
| `circle-plus` | `6acbe3d78b4348b17fafeb331dae299ec178f7b8` | Add, create new |

### How to use icons in use_figma

```js
// Import an icon set and create a default-size instance
const iconSet = await figma.importComponentSetByKeyAsync("a05ddeacdaf3731998fcca9966fd737eef4ba46b");
// Icons have size variants — find "sm" (16px), "md" (24px), or use defaultVariant
const iconMd = iconSet.children.find(c => c.name.includes("Size=md")) || iconSet.defaultVariant;
const icon = iconMd.createInstance();
parentFrame.appendChild(icon);
```

> **Always `search_design_system` first** — if an exact icon name isn't in the table above, search with a keyword (e.g. `"calendar"`, `"filter"`, `"sparkle"`) using the Icons file key `NNuRiz7WTDl7d9JvvMLFUC` to get the correct component key.

---

## Foundations Reference (verified — 2026-04-02)

The Foundations file (`tNs5GTwfggNH91PgFzoyYe`) is the authoritative token source. It is read-only. Key values captured here.

### Spacing scale — `now-global-space--*` (default mode)

| Token | CSS var | Default | Compact mode |
|---|---|---|---|
| `xxs` | `--now-global-space--xxs` | `2px` | `1.5px` |
| `xs` | `--now-global-space--xs` | `4px` | `3px` |
| `sm` | `--now-global-space--sm` | `8px` | `6px` |
| `md` | `--now-global-space--md` | `12px` | `9px` |
| `lg` | `--now-global-space--lg` | `16px` | `12px` |
| `xl` | `--now-global-space--xl` | `24px` | `18px` |
| `xxl` | `--now-global-space--xxl` | `32px` | `24px` |
| `3xl` | `--now-global-space--3xl` | `40px` | `30px` |

> **Note:** Figma does not emit `--now-global-space--*` CSS vars in code output. Use raw `px` values in HTML prototypes. In `use_figma` scripts, bind the variable by key rather than hardcoding.

### Border radius scale — `now-global-border-radius--*`

NX uses the **Developer (Mako)** theme radius values:

| Token | CSS var | Value | Used for |
|---|---|---|---|
| `sm` | `--now-global-border-radius--sm` | `4px` | Small controls, message pills |
| `md` | `--now-global-border-radius--md` | `6px` | Form inputs, nav pills |
| `lg` | `--now-global-border-radius--lg` | `12px` | Badges, highlighted values (pill shape) |

> Cards are hardcoded to `8px` (not a CSS var tier — between sm and md). This is consistent across all NX screens.

### Shadow scale — `now-global-drop-shadow--*`

Tokens exist for `sm`, `md`, `lg`, `xl`, `xxl`. NX screens use:
- **Cards (ITSM/CBS):** no shadow — border only (`#d7e1e5`)
- **Product Hub app cards:** `0px 4px 8px 0px rgba(56, 56, 56, 0.10)` (≈ `md`)
- **Modals / elevated surfaces:** `0px 20px 24px 0px rgba(56, 56, 56, 0.10)` (≈ `xxl`)

---

## NX Published Variables — Bind Instead of Hardcode

The NX library publishes variables for all semantic tokens. **Never hardcode hex values for fills, strokes, or spacing when an NX variable exists.** Binding variables means the design automatically respects the file's active product theme (different products override primary colors — e.g. Assignment Rules uses purple-blue for `--now-color--primary-1`, not the default teal `#0080a3`).

### Why variables beat hex

`frame.fills = [{type:'SOLID', color:{r:0.898,g:0.929,b:0.941}}]` ← hardcoded `#e5edf0`, breaks if theme changes
`frame.fills = [setBoundVariableForPaint(paint, 'color', bgTertiaryVar)]` ← always correct for the active theme

### Step 0 — Discover variable keys from existing NX instances (run once per file)

Before building, run this in `use_figma` to extract all NX variable keys already imported into the target file via component instances:

```js
const page = figma.currentPage;
const varMap = new Map();
for (const node of page.findAll(() => true)) {
  const bv = node.boundVariables;
  if (!bv) continue;
  for (const [, binding] of Object.entries(bv)) {
    const bindings = Array.isArray(binding) ? binding : [binding];
    for (const b of bindings) {
      if (b?.id && !varMap.has(b.id)) {
        const v = await figma.variables.getVariableByIdAsync(b.id);
        if (v?.remote) varMap.set(v.name, { key: v.key, type: v.resolvedType });
      }
    }
  }
}
return Object.fromEntries(varMap);
```

### Confirmed NX variable keys (from NX Components library)

These keys are stable across files. Resolved values are theme-dependent — always import by key, never by hardcoded color.

| Token name | Variable key | Hex (default NX theme) |
|---|---|---|
| `Background/background--primary` | `7a32a3fd07b40cfdf6e97aa1ff404923388e1d5c` | `#ffffff` |
| `Background/background--secondary` | `df476f382c92805cf5290b42497c84d3fd114598` | `#f3f8f9` |
| `Background/background--tertiary` | `0d034b452d883bb69f61283aca9b9f9e000f562c` | `#e5edf0` |
| `Border/border--tertiary` | `5b3ef45633c81cdf705e606518c686af783f0661` | `#d7e1e5` |
| `Border/border--primary` | `e406cffd4b833c597e840d22918767ef6cb8f51a` | `#737f84` |
| `Base/Neutral/--now-color--neutral-0` | `83310f7aeb429fd75d6f80cbda9b7fbd6506ee85` | `#ffffff` |
| `Base/Primary/--now-color--primary-1` | `06d39159ea6783c3fb9f909db1150c8ea62a52a0` | theme-dependent |
| `Component/Highlighted value/Secondary/--now-highlighted-value--secondary_teal--background-color` | `19c03e35fccb11f3362618a160040c5d6833b499` | `#e0f3f7` |
| `Component/Highlighted value/Secondary/--now-highlighted-value--secondary_teal--color` | `355e6dbde8d0d8b04678262f163923d46e6e9b51` | `#0080a3` |

### How to bind variables in use_figma scripts

```js
// Import a variable from the NX library by its key
const bgVar = await figma.variables.importVariableByKeyAsync(
  "0d034b452d883bb69f61283aca9b9f9e000f562c" // background--tertiary
);

// Bind to fill (setBoundVariableForPaint returns a NEW paint — must reassign)
const paint = figma.variables.setBoundVariableForPaint(
  { type: 'SOLID', color: { r: 0, g: 0, b: 0 } }, 'color', bgVar
);
frame.fills = [paint];

// Bind to stroke
const borderVar = await figma.variables.importVariableByKeyAsync(
  "5b3ef45633c81cdf705e606518c686af783f0661" // border--tertiary
);
const strokePaint = figma.variables.setBoundVariableForPaint(
  { type: 'SOLID', color: { r: 0, g: 0, b: 0 } }, 'color', borderVar
);
frame.strokes = [strokePaint];

// Bind to spacing (cornerRadius, padding, etc.)
const radiusVar = await figma.variables.importVariableByKeyAsync("RADIUS_KEY");
frame.setBoundVariable("cornerRadius", radiusVar);
```

> **If `importVariableByKeyAsync` throws**: the file does not have the NX library enabled. Ask the user to add `Next Experience -> Components` as a library in the file's Team Library settings, then retry.

---

## Redesign from Reference Workflow

When given a reference image, Figma URL, or existing screen to redesign into NX:

> **This is reference mode.** Navigation labels, hierarchy, active states, and content structure are constraints — preserve them exactly. Only the visual implementation gets upgraded to NX components and tokens.

1. **Preserve navigation structure** — copy nav labels, hierarchy, and active item exactly from the reference. Do not substitute with design system default nav items.
2. **Preserve page structure** — same sections in the same order. Do not reorganize or consolidate.
3. **Map visual elements to NX equivalents:**
   - Any colored banner/hero header → NX white page header (`#ffffff`, bottom border `#d7e1e5`) + `#032d42` global nav
   - Any brand-colored CTA button → `<now-button>` primary (same label as reference)
   - Any stat/KPI card → NX card frame with NX typography scale (40px Bold for numbers)
   - Any status chip/badge → `<now-highlighted-value>` secondary with appropriate color
   - Any progress bar → NX bar (track `#e2e5e7`, fill `#0080a3`, height 8px, border `#737f84`)
   - Any data table → NX table tokens (header `#4a5e65` 11px Medium all-caps, row divider `#e6eaed`)
4. **Strip all brand colors** — replace with NX token palette (no purple, no custom brand)
5. **Apply NX canvas background** `#e5edf0` to the content area
6. **Use `search_design_system`** to find matching NX components before building any element manually

---

## Technology Stack

- **Web components:** ServiceNow `<now-*>` custom elements — use these names in specs and code
- **CSS framework:** Tailwind CSS utility classes
- **Token pattern:** CSS custom properties with `--now-color_*`, `--now-global-space--*`, `--now-global-border-radius--*` namespaces
- **Colors in code:** Expressed as both `var(--now-color_background--tertiary, #e5edf0)` (with fallback hex) and raw hex — always include the fallback hex
- **Spacing:** Raw `px` values (Figma does not emit `--now-global-space--*` CSS vars in code output; use px directly)
- **No third-party component layer** (unlike Horizon 2.0 which uses DaisyUI)

---

## Confirmed Token Values (verified live — 2026-03-30)

### Background Colors

| Token | Resolved Value | Usage |
|-------|---------------|-------|
| `--now-color_background--primary` | `#ffffff` | Card surface, panel surface, left nav, sub-header, input background |
| `--now-color_background--secondary` | `#e5edf0` | Page/app canvas background |
| `--now-color_background--secondary` (lighter variant) | `#f3f8f9` | App card secondary bg, hover states |
| `--now-color_background--tertiary` (inner items) | `#e2e5e7` | Progress bar track, resource card inner bg |
| **Global top nav bar only** (`--unified-navigation-bg`) | `#032d42` | ServiceNow global header bar — top 52px strip only |
| Tab nav deep (`--now-color_chrome--brand-8`) | `#021f2e` | Product Hub tab navigation bar |
| Tab nav search (`--now-color_chrome--brand-4`) | `#2e5162` | Search bar inside global top nav |
| Tab nav border | `#365a6d` | Bottom divider of tab nav bar |
| Active nav item pill (`--now-color--primary-0`) | `#bddee7` | Left nav active/selected item highlight pill |
| Product Hub header banner | `#bddee7` | Product Hub page title banner strip (90px tall) |

> ⚠️ **Critical distinction:** The left navigation rail (282px) has a **white** background. `#032d42` applies ONLY to the global top nav bar (52px tall). Never apply `#032d42` to the left nav.

### Text Colors

| Token | Resolved Value | Usage |
|-------|---------------|-------|
| `--now-color_text--primary` | `#172b31` | Body text, nav sub-items, card content |
| `--now-color_text--primary` (sub-header title) | `#10171a` | Guided setup sub-header title text (slightly darker) |
| `--now-color_text--secondary` | `#294149` | Secondary text labels |
| `--now-color_text--tertiary` | `#4a5e65` | Subtext, captions, timestamps |
| Nav category label (`--now-color--primary-1`) | `#0080a3` | Expanded parent items in left nav ("Platform", "Core ITSM") |
| Text on dark global nav | `#ffffff` | Nav bar item labels on `#032d42` background |

### Interactive / Brand Colors

| Token | Resolved Value | Usage |
|-------|---------------|-------|
| Active tab underline + text | `#2d7524` | Tab underline, active tab text |
| Link (`--now-color--link-2`) | `#1955be` | Hyperlinks, text links |
| Primary interactive (`--now-color--primary-1`) | `#0080a3` | Progress bar fill, nav category labels, primary actions |
| Brand Wasabi Green | `#62d84e` | Brand accent, AI elements |
| Now Assist AI gradient start | `rgba(134, 246, 115, 0.10)` | AI CTA button / card gradient |
| Now Assist AI gradient end | `rgba(113, 213, 254, 0.10)` | AI CTA button / card gradient |

### Badge / Highlighted Value Colors

| State | Background | Text | Notes |
|-------|-----------|------|-------|
| "Needs setup" / warning | `#fbf7bf` (pale yellow) | `#172b31` | `tertiary_warning` variant |
| "Auto" / info | `#bddcf2` (light blue) | `#172b31` | `tertiary_info` variant, includes ai-sparkle icon |
| Critical / error | `#b61c2d` (red) | `#ffffff` | `primary_critical` variant |
| Badge border-radius | `12px` | — | Pill shape |

### Progress Bar Colors

| Part | Value | Notes |
|------|-------|-------|
| Fill (`--now-color--primary-1`) | `#0080a3` | Active fill color |
| Track background | `#e2e5e7` | Rail background |
| Track border | `1px solid #737f84` | Outer border of track |
| Fill right-edge border | `2px solid #37444a` | |
| Height | `8px` | Hardcoded, not a CSS var |

### Border Colors

| Token | Resolved Value | Usage |
|-------|---------------|-------|
| `--now-color_border--tertiary` | `#d7e1e5` | Card outer border, left nav right edge |
| Container border alias | `#cfd5d7` | Inner panel border (Product Hub context) |
| Sub-header bottom border | `#cfd5d7` | Guided setup sub-header bottom divider |
| Card border width | `1px` | Standard 1px border (NX at 1440px scale) |

### Border Radius

| Usage | Value | Notes |
|-------|-------|-------|
| Cards / Container panels | `8px` | Hardcoded — `rounded-[8px]`, no CSS var |
| Nav item active pill | `6px` | `--now-global-border-radius--md` |
| Badge / highlighted value | `12px` | Pill shape |
| Images / media | `8px` | Image frames, video thumbnails |

### Box Shadows

| Usage | Value | Notes |
|-------|-------|-------|
| Product Hub app cards | `0px 4px 8px 0px rgba(56, 56, 56, 0.10)` | Present on Product Hub card type |
| ITSM / CBS Container cards | None | Border only — no shadow on these card types |
| Large elevation / modals | `0px 20px 24px 0px rgba(56, 56, 56, 0.10)` | |

### Typography — ServiceNow Sans

All text uses `ServiceNow Sans` (no secondary font family).

| Token | Style | Size | Weight | Line-Height |
|-------|-------|------|--------|------------|
| `heading/xx-large` | Display Medium | 64px | 500 | 70.4px |
| `heading/x-large (emphasis)` | Bold | 40px | 700 | 44px |
| `heading/large (emphasis)` | Bold | 32px | 700 | 35.2px |
| `body/large (emphasis)` | Medium | 20px | 500 | 30px |
| `body/large` | Light | 20px | 300 | 30px |
| `body/medium (emphasis)` | Medium | 16px | 500 | 24px |
| `body/medium` | Light | 16px | 300 | 24px |
| `heading/x-small (emphasis)` | Bold | 16px | 700 | 17.6px |

CSS font-family strings (exact, as emitted by Figma):
```css
font-family: 'ServiceNow Sans', sans-serif;
/* Weights accessed as named styles: */
/* Display Medium → font-weight: 500 */
/* Bold          → font-weight: 700 */
/* Medium        → font-weight: 500 */
/* Light         → font-weight: 300 */
```

### Input / Form Field Tokens (verified live — 2026-03-30, all pages audited 2026-03-31)

> **Architecture note:** Border behavior differs by component. `<now-input>` (text input) uses **bottom-border-only** in default state. `<now-textarea>` and `<now-select>` use a **full 4-sided border** in default state. Focus and error states use full border for all three. Selection controls (`<now-checkbox>`, `<now-radio-buttons>`) use their own control-specific border patterns.

#### `<now-input>` — Text input (`<now-input>`, `<now-input-password>`)

#### `<now-input>` border states

| State | Border | Notes |
|-------|--------|-------|
| Default | `1px solid #69838c` (bottom only) | No top/left/right border |
| Hover | `1px solid #0080a3` (bottom only) | Color shifts to primary interactive |
| Focus | `2px solid #248a13` (all 4 sides) | Green — NOT blue |
| Error | `1px solid #ed0000` (all 4 sides) | Red full border |
| Disabled | No border / muted | Greyed-out state |

> ⚠️ **Focus is GREEN (`#248a13`), not blue.** This applies to ALL NX form controls.

#### Input anatomy tokens

| Property | Value | Notes |
|----------|-------|-------|
| Background | `#ffffff` | White, all states |
| Input text color | `#172b31` | `--now-color_text--primary` |
| Placeholder text | `#4a5e65` | `--now-color_text--tertiary` |
| Label color | `#294149` | `--now-color_text--secondary` |
| Label — invalid state | `#cc0000` | Slightly lighter than error border |
| Label font size | `12px` | `body/small` scale |
| Input font size | `16px` | `body/medium` scale |
| Border-radius | `6px` | `--now-global-border-radius--md` |
| Focus ring width | `4px` (md) / `2px` (sm) | Matches border-radius tier |
| Focus ring color | `#248a13` | Green |

#### Form field messaging (below-field state strip)

| Type | Background | Notes |
|------|-----------|-------|
| Critical / error | `#f8c8cd` | Red-tinted strip |
| Warning | `#fbf7bf` | Yellow-tinted strip |
| Info | `#bddcf2` | Blue-tinted strip |
| Positive / success | `#cadfc0` | Green-tinted strip |
| Message pill radius | `2px` | Very tight radius on inline message chip |

#### Input CSS reference

```css
/* Default state — bottom border only */
.nx-input {
  background: #ffffff;
  border: none;
  border-bottom: 1px solid #69838c;
  border-radius: 6px 6px 0 0; /* top corners rounded, bottom square */
  color: #172b31;
  font-family: 'ServiceNow Sans', sans-serif;
  font-size: 16px;
  font-weight: 300; /* Light */
}
.nx-input::placeholder { color: #4a5e65; }
.nx-input:hover { border-bottom-color: #0080a3; }
.nx-input:focus {
  outline: none;
  border: 2px solid #248a13; /* full border on focus */
  border-radius: 6px;
  box-shadow: 0 0 0 4px rgba(36, 138, 19, 0.15); /* focus ring */
}
.nx-input.error { border: 1px solid #ed0000; border-radius: 6px; }

/* Label */
.nx-label {
  color: #294149;
  font-size: 12px;
  font-family: 'ServiceNow Sans', sans-serif;
}
.nx-label.invalid { color: #cc0000; }
```

---

#### `<now-textarea>` — Multi-line text area (node `549:50012`)

> **Architecture: full 4-sided border in default state** — unlike `<now-input>` which is bottom-only.

| State | Border | Background | Notes |
|-------|--------|-----------|-------|
| Default | `1px solid #69838c` (all 4 sides) | `#ffffff` | Full border, not bottom-only |
| Hover | `1px solid #69838c` | `#e5edf0` (light teal tint) | Background tints on hover |
| Focus | `2px solid #248a13` (all 4 sides) | `#ffffff` | Green — same as `<now-input>` |
| Error | `1px solid #ed0000` (all 4 sides) | `#ffffff` | Red full border |
| Read-only | none | `#e2e5e7` (grey fill) | No border, grey bg |
| Disabled | none | muted/greyed | Placeholder text muted |

- Sizes: `md`, `sm`
- Layout variants: vertical (label above), horizontal (label inline)
- Supports character count display (`Characters left: 23`)
- Border-radius: `6px`
- Font: same as `<now-input>` — 16px (md) / 14px (sm) ServiceNow Sans Light

---

#### `<now-select>` — Dropdown select (node `582:53497`)

> **Architecture: full 4-sided border in default state** — this is a Controls component, not Inputs, but used in forms.

| State | Border | Background | Notes |
|-------|--------|-----------|-------|
| Default | `1px solid #69838c` (all 4 sides) | `#ffffff` | Full border |
| Hover | `1px solid #0080a3` | `#e5edf0` | Teal tint bg on hover |
| Focus / open | `2px solid #248a13` (all 4 sides) | `#ffffff` | Green focus border |
| Error | `1px solid #ed0000` (all 4 sides) | `#ffffff` | Red full border |
| Disabled | muted border | muted bg | |
| Last set | `1px solid #0080a3` | `#ffffff` | Teal border indicating saved state |

- Sizes: `md`, `sm`
- Includes trailing chevron icon
- Border-radius: `6px`
- NX element: `<now-select>`

---

#### `<now-checkbox>` — Checkbox (node `292:15350`)

| State | Appearance | Notes |
|-------|-----------|-------|
| Default unchecked | White box, `1px solid #69838c` border | Square, `6px` radius |
| Default checked | `#0080a3` fill (NX primary interactive) | Blue fill, white checkmark |
| Hovered | Slightly tinted border/bg | |
| Focus | `2px solid #248a13` focus ring (green) | |
| Invalid | Red `#ed0000` border indicator | |
| Disabled | Muted/greyed | |
| Indeterminate | `#0080a3` fill, dash glyph | |

- Sizes: `md` (24px), `sm` (16px)
- Label positions: `end` (label right of checkbox) and `start` (label left)
- Supports `required` prop (shows asterisk)
- NX element: `<now-checkbox>`

---

#### `<now-radio-buttons>` — Radio button group (node `321:14625`)

| State | Appearance | Notes |
|-------|-----------|-------|
| Default unselected | White circle, `1px solid #69838c` border | |
| Selected | `#0080a3` filled dot inside white ring | Blue dot, white ring, teal outer |
| Hovered | Slightly tinted | |
| Focus | `2px solid #248a13` focus ring (green) | |
| Invalid | Red `#ed0000` indicator | |
| Disabled | Muted/greyed | |

- Sizes: `md`, `sm`
- Layout variants: `radio-vertical` (stacked), `radio-horizontal` (inline)
- Entire group wrapped in `Radio group label` with optional info icon
- NX element: `<now-radio-buttons>`

---

#### Shared form control colors (all components above)

| Role | Value |
|------|-------|
| Control border (default/unselected) | `#69838c` |
| Control border (hover) | `#0080a3` |
| Focus border / ring | `#248a13` (green) |
| Error border | `#ed0000` |
| Selected / checked fill | `#0080a3` |
| Label | `#294149` |
| Label — invalid | `#cc0000` |
| Hover background tint | `#e5edf0` |
| Read-only background | `#e2e5e7` |

---

## Layout Templates

Use the closest matching template as the structural skeleton. Always `get_screenshot` + `get_design_context` on the reference node for pixel-accurate details.

---

### Layout A — Guided Setup Overview (Single Service)
**Reference:** `zy4c7wWFtXxztmV8lP4tGq` node `6:26700` (1440×1160)
**Use for:** Core ITSM overview, single-product setup page, playbook progress view

```
┌────────────────────────────────────────────────────────────┐
│  [Header — global nav chrome, 1440×52, bg #032d42]         │
├────────────────────────────────────────────────────────────┤
│  [Guided setup - header, 1440×64 — page title on bg]       │
├────────────────────┬───────────────────────────────────────┤
│  Left Navigation   │  Frame 627779 — main content          │
│  282px × 1046px    │  x=306, width=1110px                  │
│                    │                                        │
│  Playbook activity │  [Page title text — ON BACKGROUND]    │
│  picker (250px)    │  [Container card 1 — white, shadow]   │
│                    │    Section heading + "N/N complete"    │
│  • Overview ←active│    Progress bar (8px)                 │
│  • Platform ▾      │    3× Accordion CTAs (340px each)     │
│    - Brand/theme   │                                        │
│    - I18n locale   │  [Container card 2 — white, shadow]   │
│    - Foundational  │    Same structure as card 1            │
│    - Identity      │                                        │
│    - Users/groups  │  [Stat tiles row — 2× 547px cards]   │
│    - Role assign   │    Big number + label + text link      │
│  • Core ITSM ▾     │                                        │
│    - Intake chnls  │  [Change log section]                  │
│    - Workspace     │    Heading + Badge                     │
│    - IT Portal     │    Presentational list (1110px)        │
│    - AI (+Auto)    │    Pagination control                  │
│    …               │                                        │
└────────────────────┴───────────────────────────────────────┘
```

**Key dimensions:**
- Global nav: 1440×52px, `bg: #032d42`
- Sub-header: 1440×64px (Guided setup - header component)
- Left nav: x=0, y=116, **282×1046px**
- Main content: x=306, y=140, **1110px wide**
- Container cards: 1110px wide, white, `border-radius: 8px`, `border: 1px solid #d7e1e5`, **no box-shadow**
- Accordion CTAs: 3 per row, each ~340.67px wide, 64px tall

---

### Layout B — CBS Overview (Multi-Service)
**Reference:** `zy4c7wWFtXxztmV8lP4tGq` node `6:85146` (1440×1597)
**Use for:** CBS multi-product overview, multiple services in parallel, Input needed tracking

```
┌────────────────────────────────────────────────────────────┐
│  [Header — global nav chrome, 1440×52]                     │
│  [Alert toast — optional, 701px wide, centered at y=60]    │
├────────────────────────────────────────────────────────────┤
│  [Guided setup - header, 1440×64]                          │
├────────────────────┬───────────────────────────────────────┤
│  Left Navigation   │  Main content, x=316, width=1065px    │
│  282px wide        │                                        │
│                    │  [Service summary cards row]           │
│  Segmented control │    2× Container (344px each, gap 16px)│
│  [All | Input needed]   Each: service name + "N Input      │
│                    │    needed" badge + Button + Text link  │
│  • Platform ▾      │                                        │
│  • HR ▾ (Input     │  [Getting started carousel]           │
│    needed badge)   │    3× video/doc cards (440px each)    │
│    - Setup dept    │    Pagination dots + pause Button iconic│
│    - Web branding  │                                        │
│    - Theme engine  │  [Change log / list section]          │
│  • Legal Inquiries │    List header (1065px)               │
│    ▾ (AI badge)    │    Presentational list                │
│    - Configure     │    Pagination control                 │
│    - Assignments   │                                        │
└────────────────────┴───────────────────────────────────────┘
```

**CBS-specific additions vs Layout A:**
- `Segmented control` at top of left nav picker: 250×70px, "All" + "Input needed" tabs
- Service summary cards at top of content area (not a single-product overview)
- `Alert` toast can overlay the sub-header area (x=369, y=60, 701×48px) on success states
- `Highlighted value` badges in nav: amber for "Input needed", blue for "AI"

---

### Layout C — Setup Task / Form Screen
**Reference:** `zy4c7wWFtXxztmV8lP4tGq` node `10:68280` (1440px)
**Use for:** Single-step configuration form, active setup task, player in-progress view

```
┌────────────────────────────────────────────────────────────┐
│  [Header — global nav chrome, 1440×52]                     │
├────────────────────────────────────────────────────────────┤
│  [Guided setup - header, 1440×64]                          │
├────────────────────┬───────────────────────────────────────┤
│  Left Navigation   │  [Large white card — fills right area]│
│  282px wide        │                                        │
│                    │  ┌──────────────────────────────────┐ │
│  (same structure   │  │  [Form area — left ~55%]         │ │
│   as Layout A/B)   │  │    Step title (heading/large)    │ │
│                    │  │    Radio buttons / inputs        │ │
│                    │  │    Form fields                   │ │
│                    │  ├─────────────────────────────────┤  │
│                    │  │  [Illustration/hero — right ~45%]│ │
│                    │  │    Image or diagram              │ │
│                    │  └──────────────────────────────────┘ │
│                    │                                        │
│                    │  [Footer row inside card]             │
│                    │    Button "Previous" + Button "Next"  │
└────────────────────┴───────────────────────────────────────┘
```

**Notes:**
- The active step card may have a **magenta/Now Assist accent border** — distinct from regular cards
- Form + illustration is a split layout within the single card (not two separate cards)
- Navigation buttons are **inside the card**, not floating

---

### Layout D — Product Hub / App Landing
**Reference:** `zy4c7wWFtXxztmV8lP4tGq` node `1559:141280` (540px preview → ~1440px at scale)
**Use for:** Product/BU hub landing page, high-level app overview with tabs

```
┌────────────────────────────────────────────────────────────┐
│  [Header — global nav, bg #032d42, height 52px]            │
│  [Tab Navigation — bg #021f2e, height 18px]                │
│    "Overview" (active, green underline #2d7524)            │
│    "Configuration" (inactive)                              │
├────────────────────────────────────────────────────────────┤
│  [Header Banner — bg #bddee7, height 90px]                 │
│    Page title text — ON COLORED BANNER (not on card)       │
│    Subtitle / description text                             │
├────────────────────────────────────────────────────────────┤
│                [Content area — full width, NO left nav]    │
│                                                            │
│  ┌─────────────────────────────┐  ┌─────────────────────┐ │
│  │ Left column (~740px)        │  │ Right column (~370px)│ │
│  │                             │  │                      │ │
│  │ Tabs-Horizontal             │  │ Auto Install complete│ │
│  │ Services grid (App cards)   │  │ card (white)         │ │
│  │                             │  │                      │ │
│  │ Card container (tasks)      │  │ Next Steps card      │ │
│  │   4× task cards in grid     │  │   Checklist items    │ │
│  │                             │  │   AI CTA Button      │ │
│  └─────────────────────────────┘  └─────────────────────┘ │
└────────────────────────────────────────────────────────────┘
```

**Key differences from Layouts A/B/C:**
- **No left navigation rail** — full-width content area
- Header banner is `#bddee7` (light teal) — title sits ON this colored band, not on white card
- Tab navigation (Overview / Configuration) below the global header
- Two-column content split (roughly 2:1 ratio)

---

## Layout Selection Decision Tree

```
Is this a page showing setup progress for ONE product/service?
  → YES → Layout A (Guided Setup Overview — Single Service)

Is this a dashboard showing multiple products/services in parallel with "Input needed" tracking?
  → YES → Layout B (CBS Overview — Multi-Service)
       → Does success/error need to be communicated? → Add Alert toast at y=60

Is this showing a single active configuration step with a form?
  → YES → Layout C (Setup Task / Form Screen)

Is this a high-level product hub or app landing page (not a task/wizard flow)?
  → YES → Layout D (Product Hub — No left nav, colored banner header)

Is this a Configuration Console screen (admin config, not a setup wizard)?
  → YES → Layout E (Config Console — dark `<now-content-tree>` nav, no Guided setup header)
       → Reference: `zy4c7wWFtXxztmV8lP4tGq` node `104:96285` for full spec

None of the above?
  → Compose using chrome: Header (52px) + Guided setup - header (64px) + Left Navigation (282px) + content area
  → For novel content zones, use the component inventory and follow NX layout patterns
```

### Layout E — Configuration Console
**Reference:** `zy4c7wWFtXxztmV8lP4tGq` node `104:96285`
**Use for:** Admin configuration screens, ITOM/ITSM Config Console views

```
┌────────────────────────────────────────────────────────────┐
│  [Header — global nav chrome, 1440×52, bg #032d42]         │
├────────────────────┬───────────────────────────────────────┤
│  Left nav          │  Content area                         │
│  <now-content-tree>│                                        │
│  Dark teal/navy bg │  [Main content — cards, forms, lists] │
│  (same #032d42     │                                        │
│   family)          │                                        │
└────────────────────┴───────────────────────────────────────┘
```

**Key differences from Layouts A/B/C:**
- **No Guided setup sub-header** (64px strip) — content begins directly below the 52px global nav
- Left nav uses `<now-content-tree>` with a dark background (teal-navy family) instead of the white sidebar
- Multiple Config Console variants exist: ITOM, ITSM, single-pane, split-pane
- Fetch `104:96285` live for the specific variant needed

---

## Component Inventory

### Navigation & Chrome

| Component | NX Element | Component Key | Notes |
|-----------|-----------|--------------|-------|
| `Header` | `<sn-canvas-toolbar>` | `86da072b89c89385b1d06c2e37f1e9dc2e65aa17` | Global top chrome, 52px, bg `#032d42` |
| `Primary navigation` | `<sn-canvas-toolbar>` | `0e47d1e8db33b7ad39685bdb67105f8d753249f6` | L1 nav |
| `Page navigation` | `<sn-canvas-tabs>` | `68fb307fc546eeda854228e0a4628876885b7268` | Contextual page tabs |
| `Tabs-Horizontal` | `<now-tabs>` | `9ab1e0a764fa3027ead98dc102e59306c87c7336` | Horizontal tab bar |
| `Tabs-Vertical` | `<now-tabs-vertical>` | `6e2de04955ba6b573e6f54ff7b7c6e22490af4d4` | Vertical tab bar |
| `Content tree` | `<now-content-tree>` | `507e74ca32dc2e3bbd6160ae1495569be1b95d91` | Left nav tree (Config Console) |
| `Breadcrumbs` | `<now-breadcrumbs>` | `ef14a4f4cb9fb1246eb92623834060a6464e81f4` | Breadcrumb trail |
| `Menu` | `<now-menu>` | `376fa3b21df9679a1b9f1143e896fdf922a5111b` | Multi-level primary nav |
| `Stepper` | `<now-stepper>` | `eda396fcf7c63040903fd5f4df2c442bd945d2ac` | Multi-step nav indicator |
| `Pagination control` | `<now-pagination-control>` | `76f7e114834edfbebe21d111e8a50fbbd1088130` | Table/list pagination |
| `Unified Navigation` | `<sn-canvas-toolbar>` + `<now-menu>` | (composite) | Full 52px header chrome combined with multi-level dark-navy flyout menu; shows filter/favorites panel states; use node `6:26700` for reference |

### Actions & Buttons

| Component | NX Element | Component Key | Notes |
|-----------|-----------|--------------|-------|
| `Button` | `<now-button>` | `6dc4fab588de9bac411e6b5b73ca30830dbf428c` | Standard button — text + optional icon |
| `Button iconic` | `<now-button-iconic>` | `65a451ab4506c32bfd2a9c5d5238db4c8e77520c` | Icon-only button |
| `Button bare` | `<now-button-bare>` | `7b68027dd50c61df2e809cd4bf195478f8339f4b` | Minimal padding button |
| `Split button` | `<now-split-button>` | `9f943e319ba9ce3d56e07bf01bcc23ea6998097e` | Primary + dropdown arrow |
| `Grouped button` | `<now-grouped-button>` | `ec3c8a2491302a61635f648651885739605fa245` | Button group |
| `Button stateful` | `<now-button-stateful>` | `dd6571846dfefd840b97c1e4a11bf165d7f1f80d` | Toggleable icon button |
| `Button circular` | `<now-button-circular>` | `07b1df2c6900d00d4d2c128e0ad661eb2e304402` | FAB-style circular button |
| `Button - AI` | `<now-button>` AI variant | `7a0a237fff3dd64cdbe5c88e6252b4325d9d7bed` | AI gradient CTA |
| `Split button - AI` | `<now-split-button>` AI | `78bc98c47dc5e5a3b7a371b5b870002cb0ea35a6` | AI split button |
| `Button iconic - AI` | `<now-button-iconic>` AI | `349caef2c2917855093944587181d778566891b1` | AI icon button |
| `Dropdown` | `<now-dropdown>` | `342ac6889f7e6c011264b6ae2e6ae1ecff5572e2` | Dropdown trigger |
| `Dropdown list` | `<now-dropdown-list>` | `5a27806b8c1bc252feb05af0e84cde1ef14687ff` | Dropdown menu list |

### Cards & Containers

| Component | NX Element | Component Key | Notes |
|-----------|-----------|--------------|-------|
| `Card container` | `<now-card>` | `cac34afa2d36aa9f6a783d2d7270a069293c1f0e` | Primary card container |
| `Card container - AI` | `<now-card>` AI | `2443c3af4c1169a10d5b084fc3b1e07d6d92d9ff` | AI variant card |
| `Card divider` | `<now-card-divider>` | `9fa818aab2bae84ce3b705e930218590cad39f55` | Card internal divider |
| `Card footer` | `<now-card-footer>` | `7e9cd0a8b302df287c6bde2cdcc33eab7568ef59` | Card footer |
| `Contact card` | `<sn-contact-card>` | `6fc11314a557e74b2cfee719f6c8c801d3e4f548` | Avatar + fields + badges |
| `Template Card` | `<now-template-card-assist>` | `8bff0d605a9eb7c82a18fb986134f3751e4749da` | Template card |
| `Nested component/Card header` | — | `22bb35ca903ee04c69be4f432ca15552f0960ad1` | Card header subcomponent |
| `Collapsible container` | `<now-collapsible>` | — | Title header with expand chevron; grey content area when expanded; collapsed/expanded states |
| `Document display` | `<now-document-display>` | — | PDF/HTML document viewer container with search toolbar; 2-up page view; scrollable |
| `Search results container` | `<now-search-results>` | — | Scrollable list of search result cards; includes empty state with illustration |
| `Now evam card content` | `<now-card-evam-content>` | — | Scannable summary of a detailed object; exists only inside `<now-data-set>` and content-set components; shows record title (2 lines max), metadata badge, avatar, label-value pairs; all elements configurable show/hide |
| `Now evam card record` | `<now-card-evam-record>` | — | Same as evam-content but with action buttons (label + primary button); for actionable records in list views |

### Forms & Inputs

| Component | NX Element | Component Key | Notes |
|-----------|-----------|--------------|-------|
| `Input` | `<now-input>` | `a72fa2d91c6b261e61541c3acf681a930c6e37b0` | Text input |
| `Input password` | `<now-input-password>` | `6da44de19ef67da7a39e663a85a60989abe3c99f` | Password input |
| `Textarea` | `<now-textarea>` | `27d7f1906f8e46111f92f0f3cde7946e6bfb07cb` | Multi-line text |
| `Select` | `<now-select>` | `83d1ec365717ef9f124cfd8b1adaed3d6ac8b66b` | Dropdown select |
| `Dropdown - AI` | `<now-dropdown>` AI | `eddd0e724ae808914091d099c7f2295b8913c321` | AI dropdown |
| `Typeahead` | `<now-typeahead>` | `f8eec8fa9f62bf0b49b211887095afef3b242083` | Search/suggest input |
| `Typeahead multi` | `<now-typeahead-multi>` | `2641f609d94e58943f7fec0dd0960ab5d666e07b` | Multi-select typeahead |
| `Checkbox` | `<now-checkbox>` | `95fa8112fdb6b5338931fa17e723a5234a695f0b` | Checkbox |
| `Toggle` | `<now-toggle>` | `c7e0b77868e88f666f9ee7a362100209fbaa87fe` | Toggle switch |
| `Radio button` | `<now-radio-buttons>` | `6e7868c0538bd0ebdec9b7a6b1ee3bd70e88c2ef` | Radio group |
| `Form` | `<sn-form>` | `662a7cf49ae531e894fe2de0a8dc7bbc6921f9f8` | Record data form |
| `Form field messaging` | — | `69fd1eadac5ac081e1a082c9e830e77a6ff64f0e` | Validation messaging |
| `Date time picker` | `<now-date-time-picker>` | — | Calendar popup triggered by input field; same green focus `#248a13` and red `#ed0000` error colors; supports date-only, time-only, and date+time modes |
| `Checklist` | `<now-checklist>` | — | Group of `<now-checkbox>` elements for selecting multiple independent options from a list; used for multi-select scenarios within forms |
| `Date time interval` | `<now-date-time-interval>` | — | Date+time range entry (start + end input fields); calendar types: Day/Week/Month/Quarter/Year; vertical and horizontal layout variants |
| `Input phone` | `<now-input-phone>` | — | Phone number input with country-code selector prefix (flag + dial code); sizes: md/sm; all standard states (default/hover/focus-green/disabled/read-only); align-center and align-right variants |
| `Input URL` | `<now-input-url>` | — | URL input for web addresses; two modes: `view` (shows URL as clickable text link) and `edit` (standard input field); sizes: md/sm; horizontal layout variant |

### Data Display

| Component | NX Element | Component Key | Notes |
|-----------|-----------|--------------|-------|
| `Presentational list` | `<now-list>` | `ca5501c0c39c84ef2470c5af3a7118bb467896b3` | Interactive list / table |
| `Presentational list - Gallery layout` | `<now-list>` | `ed067f019e8e4050cdb736e17fe5eaa4e076f275` | Gallery / card layout |
| `Badge` | `<now-badge>` | `03b949ddbb791684d1fe7c081c5a37c167890833` | Numeric notification badge |
| `Highlighted value` | `<now-highlighted-value>` | `a41e6be6cd1479fecfbdd3d42ac4de76e4378a0a` | Status chip — "Input needed" / "Auto" / "AI" |
| `Pill` | `<now-pill>` | `0bda1e95f9ff604cb3a314bf1f3b537ac2009f5d` | Removable pill tag |
| `Avatar` | `<now-avatar>` | `a3d6331ef8b303410e84c0b16c92797764e0767c` | User avatar |
| `Heading` | `<now-heading>` | `088a7d22f54cfea35dd3f680b5d1cc3887bc7f43` | Semantic heading element |
| `Label value stacked` | `<now-label-value-stacked>` | `40b9599d31435fde462d573ae7ab466c2c55e52e` | Label + value pair |
| `Label value tabbed` | `<now-label-value-tabbed>` | `54a7fab304ac02994e542063a072092b2dd43cc9` | Tabbed label/value |
| `Progress bar` | `<now-progress-bar>` | `6064ed50873490a7a7cf64bd6dff37d7f80401d5` | Horizontal progress bar |
| `Alert` | `<now-alert>` | `673c6874f67d95745009f1bce4940d5d18cbbcd5` | Dismissable notification toast |
| `Alert - AI` | `<now-alert>` AI | `66c732d925becd3a4930b9ae02b1c52623d5046c` | AI variant alert |
| `Alert list` | `<now-alert>` list | `4ea2f5cd3397433ebf2d6a428edc837c4ed57fe9` | Alert list |
| `Accordion` | `<now-accordion>` | `4589e4d3590b2f70873c832d42c7005fa7c01e44` | Expandable section |
| `Tooltip` | `<now-tooltip>` | `dca1ff736bf3d42f43ebeaa07393ee047770fadf` | Hover tooltip, wraps at 320px |
| `Empty State` | `<now-empty-state>` | — | Illustration + message heading + description + optional CTA button; multiple icon variants (paperclip, search, cloud); used inside search results, lists, containers when no data present |
| `Message` | `<now-message>` | — | Configurable message framework; all content zones are slots; supports 3-column layout variant; used for structured informational blocks |
| `Label value inline` | `<now-label-value-inline>` | — | Single horizontal label–value pair; layout-unaware, placed directly inside existing layouts |
| `Pill list` | `<now-pill-list>` | — | Collection of removable `<now-pill>` elements in wrapping layout; supports "Related tags:" labeled variant |
| `Record header` | `<now-record-header>` | — | Record page header: primary item (title + description + status) with field label/value pairs below; used at top of record detail pages |
| `Analytics Q&A` | `<sn-analytics-qa>` | — | Seismic component; natural-language questions about data ("What do you want to see?"); shows results as charts with Table/Aggregation/Grouped-by/Conditions metadata; "How can I improve my results?" link |
| `Dashboards Overview` | `<sn-dashboards-overview>` | — | Dashboard catalog component for Analytics center; left Filters panel (search by name/owner) + grid of dashboard tiles (title + author); layouts: full paginated, "See all" compact, card flyout; includes pagination control |
| `Data row` | `<now-data-row>` | — | Horizontal scrollable row of evam cards; shows "Title" heading + "View all →" link; density variants: compact/summary/extended; supports horizontal and vertical orientation |
| `Form record presence` | `<now-form-record-presence>` | — | Avatars showing concurrent record viewers; shows up to 2 individual avatars; 3rd+ becomes "+N" overflow trigger |
| `Record tags` | `<now-record-tags>` | — | Tags panel on a record; "Add tags" input; "Me" section (private) + "Everyone" section (shared); Edit Tag modal: tag name + "Viewable by" select + Cancel/Save |

### Overlays & Dialogs

| Component | NX Element | Component Key | Notes |
|-----------|-----------|--------------|-------|
| `Modal` | `<now-modal>` | `65b8e30d7c44f4acaff0fcf329d0c2bb51a3c465` | Full modal, disables background |
| `Modeless dialog` | `<now-modeless-dialog>` | `a588de6bb0bcc5085adb7517a3ddbcecd4786e79` | Overlay, background stays interactive |
| `Contextual Sidebar` | `<now-contextual-sidebar>` | `82473feb4d45fb507eef3837a2f10b7056152239` | Sidebar with tab buttons |
| `Flyout menu` | `<now-flyout-menu>` | `1ea09744aa43a92e48cf6e3e9f207ba462fe0830` | Flyout menu panel |
| `Popover` | — | — | Use `get_design_context` for specs |

### Controls

| Component | NX Element | Component Key | Notes |
|-----------|-----------|--------------|-------|
| `Segmented control` | `<now-segmented-control>` | — | Tabbed selector, 2–4 segments; CBS left nav uses 250×70px "All \| Input needed" version |
| `Toggle` | `<now-toggle>` | `c7e0b77868e88f666f9ee7a362100209fbaa87fe` | Toggle switch — same border/focus colors as Checkbox |
| `Date time calendar` | `<now-date-time-calendar>` | `26a15430233f8c2c0beef156cac68f2742a428ec` | Persistent standalone calendar picker (no input trigger) |
| `Mini Calendar` | `<now-record-mini-calendar>` | `264e0ad4cb5aca00da6d0291bb2fac5c78e1b822` | Compact inline calendar, often inside cards |
| `Filter` | `<sn-component-filter>` / `<now-filter>` | `e35b0841b51182cefde924105b9fcb86bafeabf8` | Unified filter for Reporting and Performance Analytics |
| `Text link` | `<now-text-link>` | — | Inline hyperlink component; sizes `md`/`sm`; variants `primary` (`#1955be`) and `secondary` (`#4a5e65`); `underline: true/false`; optional icon at start or end; states: default / hover / pressed |
| `Active call` | `<sn-active-call>` | — | Native Voice Controls for CCaaS phone call management; shows call timer, mute/hold/transfer controls, contact info; many state variants (active, multiple calls, hold, transfer); workspace-specific engineering component |
| `Color picker` | `<sn-color-picker>` | — | Two modes: 25-color category grid (6×5 swatches + category dropdown) or 16M-color full picker (gradient canvas + hue/opacity sliders + hex input); "Custom colors" row; Cancel/Save buttons; Seismic component |
| `Color selector` | `<sn-color-selector>` | — | Selects named color variables for UI theming (hex/RGB/HSL input); simpler than Color picker; shows named color variable grid; used for theming configuration |
| `List selector` | `<now-list-selector>` | — | List Collector / Slushbucket equivalent; "Available items" panel (search + expand hierarchy) on left + "Selected items" panel (× remove) on right; empty state: paperclip illustration + "No fields selected yet" |
| `Presence icon` | `<now-presence-icon>` | — | User availability indicator dot; 4 states: Available (green checkmark), Away (orange slash), Busy (red minus), Offline (hollow grey circle); sizes: xl/lg/md/sm/xs (decreasing from icon to dot) |
| `Search facets` | `<now-search-facets>` | — | Faceted filter panel for search results; collapsed tree view of facet groups; selected state shows checkboxes + selected facets as pills above; "Clear all" / "Clear (N)" per group |

### Records & Content

| Component | NX Element | Component Key | Notes |
|-----------|-----------|--------------|-------|
| `Data set list view` | `<now-data-set>` | `6767fae7f4c49767a482c1997bc97c6e26de12a7` | List of records with configurable views + filter options |
| `KPI details template` | `<sn-kpi-details>` | `3209196179ff0cb425cf8a4149d7e1886e926360` | KPI details layout for workspaces |
| `Attachments` | `<now-record-common-attachments-connected>` | `6233e86e69e5f49eeac140a4b7f2e9aa59199ef3` | Full attachment panel — add/preview/download/remove |
| `Compact Attachments` | `<now-record-common-attachments-connected>` | `67698fd7f78c6f63cd711a3cb30bc52c1a015fb9` | Compact version, for use inside cards/modals |
| `Attachment upload preview modal` | `<now-record-common-attachments-connected>` | `1929aca7be8ff4104df08312f3d1f1dcb6115688` | Modal for uploading + previewing attachments |
| `Template card omnichannel` | `<now-template-card-omnichannel>` | `91a983bb6936cea4da59a43290e56466c0a0ca49` | Omnichannel attachment template card |

### Loaders & Feedback

| Component | NX Element | Notes |
|-----------|-----------|-------|
| `Loader` | `<now-loader>` | Animated teal spinner; sizes `sm`/`md`; optional "Loading..." label below; optional "Retry" button; use inside cards or content zones during data fetch |
| `Loader custom` | `<now-loader-custom>` | Lottie animation loader; sizes `sm`/`md`/`lg`/`xl`; supports optional progress bar + label; dark background variants available; use for full-page or major transition loading states |
| `Skeleton loader custom` | `<now-skeleton-loader>` | Animated grey skeleton placeholder; box sizes 1×1 through 4×4; configurable for text, image, card, and mixed content patterns; use while async content loads to preserve layout |
| `Carousel text` | `<now-carousel-text>` | Text label with animated transitions; inline variant (small, within a line) and full-page loader variant; used to cycle through status messages during long operations |

---

### Utilities & Animation

| Component | NX Element | Notes |
|-----------|-----------|-------|
| `Actionable` | `<now-actionable>` | Animated bar/button element for interactive chart-like controls; sizes: xxl/xl/lg/md/sm/xs; variants: Primary (teal `#0080a3`), Primary utilitive (dark green), Primary negative (red), Secondary, Tertiary; states: default/active/selected |
| `Actionable bare` | `<now-actionable-bare>` | Minimal version of Actionable with reduced visual weight; same size/variant/state structure |

---

### Component Tiers (from Figma Components Index — node `807:66061`)

The NX Components file classifies all components into three tiers. This determines which ones to use for design:

| Tier | Description | When to use |
|------|-------------|------------|
| **Library Components** | First-class components shared as a library across all projects; browsable in the Figma Assets panel | ✅ Use these in all designs |
| **Stickersheet-only** | Coming soon to the NX Library as first-class components | ⚠️ Use sparingly — not yet in the design library |
| **Engineering specific / Single use case** | Components engineering needs to build experiences; represented as static images with links to Developer Portal docs; no plan to convert to first-class | ❌ Do not use in UI designs — reference Developer Portal docs instead |

> **Note:** Specialized workspace components like Active call, Agent chat, Inbox, Kanban board, Calendar, Code editor, SLA timer, etc. are Engineering specific — they are built by engineering teams from ServiceNow's Seismic framework and are not design-time NX library components. Do not attempt to replicate them from first principles; reference the Developer Portal for specs.

---

### Layout Guidelines (from Figma Guidelines pages — verified 2026-03-31)

These are NX layout system rules documented in the Components Figma file. Follow them for all screen compositions.

#### Grid System

- **12-column grid** at 1440px — the standard desktop breakpoint
- Column count reduces at mobile breakpoints (see grid reference page for breakpoint values)
- Always align card edges to grid columns; do not use arbitrary `px` offsets for the main content width

#### Spacing Scale

NX uses an 8-point spacing scale. Common values (color-coded in Figma spacing reference):

| Step | Value | Typical usage |
|------|-------|--------------|
| 1 | `4px` | Internal element gap, icon padding |
| 2 | `8px` | Compact item spacing |
| 3 | `12px` | Small padding |
| 4 | `16px` | Standard gap between grouped items |
| 5 | `20px` | Medium padding |
| 6 | `24px` | Card internal padding, section gap |
| 7 | `32px` | Large section spacing |
| 8 | `40px`+ | Page-level section separation |

> Use raw `px` values in code — Figma does not emit `--now-global-space--*` CSS vars; they exist in the design token namespace but are not surfaced in code output.

---

### Guided Setup Specific

These components appear only in guided setup / playbook screens and are not in the NX component library — they are custom compositions in the layout file:

| Component Name | Where Used | Approx Dimensions |
|---------------|-----------|------------------|
| `Guided setup - header` | All setup screens | 1440×64px, y=52 |
| `Left Navigation` (frame) | All setup screens | 282×1046px |
| `Playbook activity picker` | Inside Left Navigation | 250px wide, 16px left padding |
| `Segmented control` | CBS left nav top | 250×70px — "All" + "Input needed" |
| `Container` (white card) | Main content area | Variable height, 8px radius, border only |
| `Nested component/Progress bar` | Container cards | 1078px wide, 8px tall |

---

## High-Fidelity HTML Workflow

> Use when the deliverable is Mode 2 (HTML prototype). Always follow all steps — do not skip any.

**Step 1 — Screenshot the target**
```
get_screenshot(fileKey: "zy4c7wWFtXxztmV8lP4tGq", nodeId: "<screen-node-id>")
```
Keep visible throughout as your visual ground truth.

**Step 2 — Get full design context**
```
get_design_context(
  fileKey: "zy4c7wWFtXxztmV8lP4tGq",
  nodeId: "<screen-node-id>",
  clientLanguages: "html,css",
  clientFrameworks: "unknown",
  excludeScreenshot: true
)
```
> ⚠️ Output is often 50–120KB. It will be saved to a tool-result file. Parse with python3:
> ```python
> import json
> with open('<tool-result-file-path>') as f:
>     data = json.load(f)
> print(data[0]['text'][:5000])  # read in 5000-char chunks
> ```
> `data[0]['text']` = main React+Tailwind code block with all asset URLs
> `data[1:]` = hints, component docs, asset expiry note

**Step 3 — Extract all asset URLs**
Collect every `const imgXxx = "https://www.figma.com/api/mcp/asset/…"` line.
These expire in **7 days**.

**Step 4 — Identify outer shell structure from the code**
Look for the chrome pattern: top nav div + sub-header div + flex row (left nav + main panel).

**Step 5 — Convert React → HTML**
- Replace `className=` → `class=`
- Resolve Tailwind arbitrary values: `var(--now-color_background--tertiary, #e5edf0)` → use `#e5edf0` directly in CSS
- Keep `position: absolute` only for true overlay elements (Alert toast, modals)

**Step 6 — Apply the shell (use exactly this structure)**
```css
.shell      { display: flex; flex-direction: column; width: 100%; min-width: 1440px; min-height: 100vh; }
.header     { position: sticky; top: 0; z-index: 100; flex-shrink: 0; width: 100%; height: 52px; background: #032d42; }
.sub-header { position: sticky; top: 52px; z-index: 99; flex-shrink: 0; width: 100%; height: 64px; background: #ffffff; border-bottom: 1px solid #cfd5d7; }
.body-row   { display: flex; flex: 1; min-height: 0; }
.left-nav   { flex-shrink: 0; width: 282px; overflow-y: auto; background: #ffffff; border-right: 1px solid #d7e1e5; }
.main       { flex: 1; min-width: 0; overflow-y: auto; padding: 24px 48px; background: #e5edf0; }
```
- `.header` and `.sub-header` are sticky — they freeze at the top as the user scrolls
- `.left-nav` scrolls independently if its content overflows
- `.main` scrolls freely; height grows with content — never set a fixed height on it
- For Layout D (Product Hub — no left nav): omit `.left-nav`, add `.header-banner { height: 90px; background: #bddee7; }` below the global nav (no sub-header)

**Step 7 — Position the Alert toast (Layout B only)**
```css
.alert-toast {
  position: absolute;
  top: 60px;
  left: 50%;
  transform: translateX(-50%);
  width: 701px;
  z-index: 10;
}
```

**Step 8 — Inject asset URLs via JS**
```js
const assets = {
  img1: "https://www.figma.com/api/mcp/asset/...",
  // ... all extracted URLs
};
document.addEventListener('DOMContentLoaded', () => {
  document.querySelectorAll('[data-asset]').forEach(el => {
    el.src = assets[el.dataset.asset];
  });
});
```
Use `data-asset="img1"` attributes on `<img>` elements — easier to refresh when URLs expire.

**Step 9 — Cards structure rule**
Container cards in Guided Setup / ITSM / CBS screens:
```css
background: #ffffff;
border-radius: 8px;
border: 1px solid #d7e1e5;
/* NO box-shadow — border only for ITSM/CBS Container cards */
padding: 24px;
```
Product Hub app cards (Layout D):
```css
background: #ffffff;
border-radius: 8px;
border: 1px solid #cfd5d7;
box-shadow: 0px 4px 8px 0px rgba(56, 56, 56, 0.10);
padding: 16px;
```

---

## Rules

1. **The page title in setup screens sits directly on the `#e5edf0` background** — it is NOT inside a white card. Only the "Container" panels for content sections are white cards. In Layout D (Product Hub), the title sits on the `#bddee7` banner strip — not on a card and not directly on the page canvas. Never invent a white card wrapper for page titles.

2. **Never use deprecated components** — skip any component prefixed `⛔️DEPRECATED DO NOT USE/`.

3. **Left nav is 282px at 1440px** — `flex-shrink: 0; width: 282px`. Do not make it responsive to a different width without checking the breakpoint reference.

4. **No floating elements** — this design system has no persistent omni-bar, FAB, or bottom bar. Buttons are always anchored inside cards or header bars. The Alert toast is the only absolute-positioned element and it is transient.

5. **Card border-radius is 8px** — verified live from 1440px screens. Nav item active pills use 6px. Badge pills use 12px. Do not use 3px.

6. **Card borders are 1px** — `border: 1px solid #d7e1e5` on Container cards at 1440px. The 0.375px value was a Figma preview-scale artifact from the 540px preview frames.

7. **All typography is ServiceNow Sans** — no secondary font family. Use `font-family: 'ServiceNow Sans', sans-serif` with `@import` or local font. Fallback to `sans-serif` only.

8. **AI elements use the gradient treatment** — any "Button - AI" or AI CTA card should use `background: linear-gradient(110deg, rgba(134, 246, 115, 0.10) 7%, rgba(113, 213, 254, 0.10) 140%)`.

9. **Asset URLs expire in 7 days** — re-run `get_design_context` on the same node to refresh. Store in JS object (Step 8), never as `src=` attributes.

10. **Broken Figma access → ask immediately** — do not approximate. Paste the new Figma URL and extract the updated file key.

11. **Never write code before completing Steps 1–3 of the HTML Workflow** — `get_design_context` is mandatory before any implementation.
