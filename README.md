# @brainstorm/design-system

Shared design tokens and primitives for the **Brainstorm product family** — the "Instrument Console" visual language used across BrainstormRouter, BrainstormMSP, BrainstormGTM, Peer10, EventFlow, and Hive.

This package is the **single source of truth** for color, type, spacing, shadows, motion, and layout dimensions. If two products look different, one of them is wrong.

---

## Philosophy: Precision Monochrome

The chrome is near-colorless. All chroma comes from content — provider logos, chart data, status semantics. The interface itself is cool graphite through bone with no chromatic accent. **Pure white is the rare highlight, used to mark the active, the focused, the committed. When white appears, it means something.**

Status colors (`--sig-ok`, `--sig-warn`, `--sig-err`, `--sig-info`) are reserved for *real signals* about system state — not decoration.

---

## Install

This package is consumed via local `file:` paths from sibling directories in `~/Projects/`. No registry, no publishing — yet.

In any consumer project's `package.json`:

```json
{
  "dependencies": {
    "@brainstorm/design-system": "file:../brainstorm-design-system"
  }
}
```

Then `npm install` (or `pnpm install`). The dependency is symlinked, so edits propagate without rebuilding.

---

## Use

### Next.js (app router) — BrainstormMSP, GTM, Peer10, EventFlow

In `app/layout.tsx`:

```tsx
import "@brainstorm/design-system/tokens.css";
// Do NOT import fonts.css here — use next/font/google for self-hosting.
```

Then load fonts via `next/font/google`:

```tsx
import { Fraunces, IBM_Plex_Sans, JetBrains_Mono, Figtree } from "next/font/google";

const fraunces = Fraunces({ subsets: ["latin"], variable: "--font-display-loaded" });
const plex = IBM_Plex_Sans({ subsets: ["latin"], weight: ["400", "500", "600", "700"], variable: "--font-sans-loaded" });
const mono = JetBrains_Mono({ subsets: ["latin"], weight: ["400", "500", "600"], variable: "--font-mono-loaded" });
const ui = Figtree({ subsets: ["latin"], weight: ["500", "600", "700"], variable: "--font-ui-loaded" });
```

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

| Layer | Purpose | Example |
|---|---|---|
| **Primitive** | Raw scale | `--ink-2`, `--bone-mute`, `--space-4` |
| **Semantic** | What it means in UI | `--color-surface`, `--color-text-secondary` |
| **Component** | Specific component need | `--shadow-panel`, `--sidebar-width` |

### Color

- `--ink-0` through `--ink-4` — graphite background steps (sidebar wall → emphasis)
- `--bone`, `--bone-dim`, `--bone-mute`, `--bone-faint` — text hierarchy
- `--hi`, `--hi-glow`, `--hi-line` — pure white accent (use sparingly)
- `--sig-ok`, `--sig-warn`, `--sig-err`, `--sig-info` — status signals
- `--paint-coral` ... `--paint-cream` — chart palette (8 muted colors)

### Type

- `--font-display` — Fraunces (variable serif, for headings/heroes)
- `--font-sans` — IBM Plex Sans (body)
- `--font-mono` — JetBrains Mono (data, code, numerals)
- `--font-ui` — Figtree (page titles only)
- `--text-2xs` ... `--text-4xl` — modular scale anchored at 14px

### Spacing & layout

- `--space-1` (4px) ... `--space-24` (96px) — 4px grid
- `--rhythm-e-*` (element), `--rhythm-c-*` (component), `--rhythm-s-*` (section) — vertical rhythm tiers
- `--sidebar-width` (240px), `--header-height` (56px) — shell dimensions

### Depth

- `--shadow-panel` — standard raised panel (3-layer machined effect)
- `--shadow-raised` — elevated above panel
- `--shadow-flush` — pressed-in effect
- `--shadow-modal`, `--shadow-float` — overlays
- `--shadow-focus` — focus ring (double-ring bullseye)

### Motion

- `--duration-fast` (120ms) ... `--duration-panel` (540ms)
- `--ease`, `--ease-in`, `--ease-in-out`, `--ease-instrument`, `--ease-spring`

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
