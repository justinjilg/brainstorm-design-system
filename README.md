# @brainstorm/design-system

Shared design tokens and primitives for the **Brainstorm product family** — the "Instrument Console" visual language used across BrainstormRouter, BrainstormMSP, BrainstormGTM, Peer10, EventFlow, and Hive.

This package is the **single source of truth** for color, type, spacing, shadows, motion, and layout dimensions. If two products look different, one of them is wrong.

---

## Philosophy: Precision Monochrome

The chrome is near-colorless. All chroma comes from content — provider logos, chart data, status semantics. The interface itself is cool graphite through bone with no chromatic accent. **Pure white is the rare highlight, used to mark the active, the focused, the committed. When white appears, it means something.**

Status colors (`--bds-sig-ok`, `--bds-sig-warn`, `--bds-sig-err`, `--bds-sig-info`) are reserved for *real signals* about system state — not decoration.

---

## Install

This package is consumed directly from GitHub. Pin to a specific commit sha for reproducible installs:

```json
{
  "dependencies": {
    "@brainstorm/design-system": "github:justinjilg/brainstorm-design-system#<commit-sha>"
  }
}
```

Then `npm install` (or `pnpm install`). npm clones the repo at build time, so the package is available to any build environment with GitHub network access — local dev, CI, DigitalOcean App Platform, Vercel, etc.

### Why a git URL and not a file path or npm registry

- **`file:` paths** work locally but break remote builds — the peer
  directory doesn't exist in the CI workspace. DO App Platform,
  Vercel, and GitHub Actions all only check out the consumer's repo.
- **npm registry** is the cleanest long-term answer, but requires
  auth tokens on every consumer and CI system. Overkill until there
  are ≥3 products consuming it.
- **Git URL** threads the needle: reproducible via commit sha, works
  in every build environment with zero auth, no registry setup.

### Local development on both repos simultaneously

If you're actively editing this package and want changes to appear
immediately in a consumer without committing + pushing:

```bash
# From the design-system repo:
npm link

# From the consumer repo (e.g. brainstormmsp/frontend):
npm link @brainstorm/design-system
```

When done, `npm unlink @brainstorm/design-system` in the consumer and
run `npm install` to restore the git-pinned version.

---

## Use

### Next.js (app router) — BrainstormMSP, GTM, Peer10, EventFlow

In `app/layout.tsx`:

```tsx
import "@brainstorm/design-system/tokens.css";
// Do NOT import fonts.css here — use next/font/google for self-hosting.
```

Then load fonts via `next/font/google`. `tokens.css` declares its font
families by literal name (`"Fraunces"`, `"IBM Plex Sans"`, `"JetBrains Mono"`,
`"Figtree"`), so configure each `next/font` loader to match that exact family
name. The simplest way is to opt into next/font's `adjustFontFallback` and
apply the loaded `className` to `<body>` — the literal names in the tokens then
resolve against the self-hosted fonts:

```tsx
import { Fraunces, IBM_Plex_Sans, JetBrains_Mono, Figtree } from "next/font/google";

// Each loader self-hosts the font; the family name registered by next/font
// matches the literal name referenced in tokens.css (--bds-font-display, etc.).
const fraunces = Fraunces({ subsets: ["latin"] });
const plex = IBM_Plex_Sans({ subsets: ["latin"], weight: ["400", "500", "600", "700"] });
const mono = JetBrains_Mono({ subsets: ["latin"], weight: ["400", "500", "600"] });
const ui = Figtree({ subsets: ["latin"], weight: ["500", "600", "700"] });

// In <body className={...}>, compose the className of whichever font is the
// page default; the others resolve via the literal family names in the tokens.
```

> Note: the tokens reference fonts by literal family name, not by a
> `var(--font-*)` CSS variable. If you prefer next/font's `variable` option,
> you must override the `--bds-font-*` tokens to point at those variables in
> your own CSS (e.g. `--bds-font-display: var(--font-fraunces), serif;`).

### Vite / plain HTML — BrainstormRouter (eventually)

```ts
// main.ts
import "@brainstorm/design-system/tokens.css";
import "@brainstorm/design-system/fonts.css";
```

Or reference directly in HTML:

```html
<link rel="stylesheet" href="/node_modules/@brainstorm/design-system/tokens.css" />
<link rel="stylesheet" href="/node_modules/@brainstorm/design-system/fonts.css" />
```

---

## Token vocabulary

Tokens are layered. **Reference the highest-level layer your component needs.** Reaching for raw `--ink-2` when `--color-surface` exists creates fragility.

All tokens are prefixed `--bds-` (Brainstorm Design System) so they never collide with consumer tokens. When you see `--bds-X`, you know it comes from this package.

| Layer | Purpose | Example |
|---|---|---|
| **Primitive** | Raw scale | `--bds-ink-2`, `--bds-bone-mute`, `--bds-space-4` |
| **Semantic** | What it means in UI | `--bds-color-surface`, `--bds-color-text-secondary` |
| **Component** | Specific component need | `--bds-shadow-panel`, `--bds-sidebar-width` |

### Color

- `--bds-ink-0` through `--bds-ink-4` — graphite background steps (sidebar wall → emphasis)
- `--bds-bone`, `--bds-bone-dim`, `--bds-bone-mute`, `--bds-bone-faint` — text hierarchy
- `--bds-hi`, `--bds-hi-glow`, `--bds-hi-line` — pure white accent (use sparingly)
- `--bds-sig-ok`, `--bds-sig-warn`, `--bds-sig-err`, `--bds-sig-info` — status signals
- `--bds-paint-coral` ... `--bds-paint-cream` — chart palette (8 muted colors)

### Type

- `--bds-font-display` — Fraunces (variable serif, for headings/heroes)
- `--bds-font-sans` — IBM Plex Sans (body)
- `--bds-font-mono` — JetBrains Mono (data, code, numerals)
- `--bds-font-ui` — Figtree (page titles only)
- `--bds-text-2xs` ... `--bds-text-4xl` — modular scale anchored at 14px

### Spacing & layout

- `--bds-space-1` (4px) ... `--bds-space-24` (96px) — 4px grid
- `--bds-rhythm-e-*` (element), `--bds-rhythm-c-*` (component), `--bds-rhythm-s-*` (section) — vertical rhythm tiers
- `--bds-sidebar-width` (240px), `--bds-header-height` (56px) — shell dimensions

### Depth

- `--bds-shadow-panel` — standard raised panel (3-layer machined effect)
- `--bds-shadow-raised` — elevated above panel
- `--bds-shadow-flush` — pressed-in effect
- `--bds-shadow-modal`, `--bds-shadow-float` — overlays
- `--bds-shadow-focus` — focus ring (double-ring bullseye)

### Motion

- `--bds-duration-fast` (120ms) ... `--bds-duration-panel` (540ms)
- `--bds-ease`, `--bds-ease-in`, `--bds-ease-in-out`, `--bds-ease-instrument`, `--bds-ease-spring`

---

## Versioning

Follows semver:

- **Patch** (0.1.x) — token value tweaks that don't change semantics
- **Minor** (0.x.0) — new tokens added, no removals
- **Major** (x.0.0) — token rename / removal, breaking change for consumers

Document every change in [`CHANGELOG.md`](./CHANGELOG.md).

---

## Roadmap

- **0.1** — CSS tokens only ← **you are here**
- **0.2** — React primitive components (`Card`, `Button`, `Badge`, `PageHeader`)
- **0.3** — Vanilla TS primitives (BR re-imports its own components from here)
- **0.4** — Chart helpers (Recharts theme, sparkline component)
- **1.0** — All Brainstorm products consume from this package

---

## Origin

Extracted 2026-04-18 from `brainstormrouter/site/dashboard/src/styles/tokens.css` and `brainstormrouter/site/dashboard/index.html`. The brainstormrouter dashboard remains the primary visual reference; this package is the implementation source of truth.
