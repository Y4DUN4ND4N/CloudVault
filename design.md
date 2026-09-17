# Design — CloudVault

The locked design system for this app. Every page reads this file before its CSS
is changed. Do not regenerate it per page — extend or amend it when the system
needs to grow. Tokens live in `app/css/tokens.css`; nothing in the app may use a
raw colour or font value that isn't a token.

## Genre

**modern-minimal** — the restrained dev-tool / dashboard register. Confident sans
display, near-white canvas, hairline rules, one signal accent, generous
whitespace, no ornament.

## Theme — Cobalt (light + dark)

Cool near-white paper tinted toward the accent hue, one electric cobalt accent
kept under ~3% of any view, ruler-drawn hairlines, tight 6px radii. Light is the
default; dark is a first-class second mode, not an afterthought.

**Switching.** A `<button data-theme-toggle>` in the topnav (and, on the login
page, inside the card) flips `<html data-theme="light|dark">` and persists the
choice to `localStorage` (`cloudvault_theme`). An inline script in every page's
`<head>`, before `tokens.css` loads, resolves the stored choice — or the OS
`prefers-color-scheme` if none is stored — so there is no flash of the wrong
theme. `CV.initTheme()` (in `util.js`) wires every toggle button on the page and
keeps the sun/moon icon in sync; it runs automatically on load.

**Dark palette.** Same anchor hue (258) as light — only lightness and chroma
move, per the standard recipe: paper 16%, ink 95%, accent brightened to 70% L
and slightly desaturated (0.16 vs 0.19 C) so it doesn't vibrate. Elevation
inverts: canvas is darkest, each raised surface gets ~4% more lightness, so
depth reads without a shadow. Semantic colours and file-type tints get the same
treatment. Shadows switch to tight, neutral black rather than the light theme's
tinted shadows. **Measured contrast in dark mode (WCAG AA, all pass):** ink
15.9:1 · ink-2 7.9:1 · ink-3 5.0:1 · accent 6.8:1 · button text on accent 7.2:1.

| Token | Value | Role |
| --- | --- | --- |
| `--color-paper` | `oklch(98.4% 0.003 258)` | app canvas, side rails |
| `--color-surface` | `oklch(99.7% 0.0015 258)` | cards, chrome, content column |
| `--color-surface-2` | `oklch(96.9% 0.004 258)` | hover, table head, inset |
| `--color-rule` | `oklch(92.6% 0.005 258)` | hairlines |
| `--color-ink` | `oklch(24% 0.018 258)` | primary text |
| `--color-ink-2` | `oklch(46% 0.014 258)` | secondary text |
| `--color-ink-3` | `oklch(55% 0.012 258)` | tertiary / hints |
| `--color-accent` | `oklch(48% 0.19 258)` | links, active nav, primary button, focus |

Semantic colours (`--color-ok` / `--color-warn` / `--color-danger` / `--color-info`)
are deliberately **separate from the accent** so status never reads as brand. The
accent is blue specifically so the amber "duplicate content" signal — which the
app uses heavily — never collides with it.

**Measured contrast (WCAG AA, all pass):** ink 16.2:1 · ink-2 7.1:1 · ink-3 4.8:1 ·
accent 6.6:1 · accent-ink on accent 6.5:1 · dup badge 5.3:1 · danger 6.6:1.

## Typography

Three families — the ceiling. Mono earns its slot: this app displays checksums,
byte counts, and timestamps that must align in columns.

- **Display** — Space Grotesk 600/700, `letter-spacing: -0.018em` → headings, wordmark, KPI figures, breadcrumb
- **Body** — IBM Plex Sans 400/500/600 → all UI text
- **Mono** — JetBrains Mono 400/500 → checksums, sizes, timestamps, tokens, quotas

Headings are **roman only**, never italic. Body/display weight contrast is 300
units (400 vs 700). App UI runs 13–14px for density; 12px is reserved for
uppercase micro-labels. Anything showing columns of digits gets
`font-variant-numeric: tabular-nums`.

## Spacing

4-point named scale, `--space-3xs` (4px) → `--space-3xl` (64px). Pages use named
tokens, never raw px.

## Radii & elevation

Tight and instrument-like: 4 / 6 / 8 / 12px, plus `--radius-pill` for chips and
avatars. Elevation is four soft shadow steps tinted with the accent hue — never
a coloured glow.

## Motion

- Easing: `--ease-out: cubic-bezier(0.16, 1, 0.3, 1)`. Never the browser default, never bounce/overshoot.
- Durations: `--dur-fast` 120ms (colour/hover), `--dur-base` 200ms (drawers).
- Only `transform` and `opacity` animate. Never layout properties.
- **Focus rings are never transitioned** — they appear instantly.
- `prefers-reduced-motion: reduce` collapses everything to ≤150ms opacity.

## Microinteractions stance

- Silent success. Toasts only for actions whose effect isn't visible on screen.
- Hover affordances always have a focus equivalent (row menus reveal on `:focus-within` too).
- Destructive-but-reversible actions (soft delete) act immediately; only
  permanent delete confirms.

## Component voice

- **Primary button** — accent fill, 6px radius, 500 weight, icon + label.
- **Secondary** — white fill, hairline border, subtle shadow.
- **Danger** — white fill, red border + red text; never a red fill.
- **Icons** — one set only (`app/js/icons.js`), Lucide-style geometry at 1.75
  stroke, `currentColor`. **No emoji as icons** — they render differently per OS
  and break the typographic system.

## Page families

- **App pages** (`dashboard`) — grey side rails, white content column, off-canvas
  drawers below 1180px (detail) / 860px (sidebar). No enrichment; function carries it.
- **Report pages** (`reports`, `admin`) — centred max-1180px column, KPI row, then
  white panels with hairline borders. Wide tables scroll inside `.panel-scroll`,
  never the page body.
- **Auth page** (`login`) — centred card on the canvas with a masked engineering
  grid. The only decoration in the app.

## What every page MUST share

The wordmark, the accent and its ≤3% budget, the three font families, button
shape and radius rhythm, hairline rule colour, and the uppercase micro-label
treatment for column/section headers.

## Responsive floor

Verified at 320 / 375 / 414 / 768 / 1280px: no horizontal page scroll, no
clickable text wrapping to two lines, wide tables scroll in their own container,
and the off-canvas sidebar has a real toggle at ≤860px.

**Topnav overflow (≤860px, every page).** `.topnav .links` and the wordmark
text hide; a `#topnavMenuBtn` + `.menu` dropdown appear instead. On the
dashboard, the equivalent job is done by the existing sidebar drawer, which
gets a `.sidebar-pages` block (Files / Reports & Groups / Admin) prepended and
shown only at this breakpoint — no second hamburger. Reports and Admin, which
have no sidebar, get the generic dropdown; `CV.initTopnav()` in `util.js` wires
it once and is safe to call on any page that has the two elements. **Any new
page must include either a sidebar with `.sidebar-pages`, or the
`#topnavMenuBtn`/`#topnavMenu` pair** — omitting both means the user menu
(avatar, sign out, theme toggle) has nowhere to go and gets clipped by the
no-horizontal-scroll rule below 860px, which is exactly the bug this fixed.
