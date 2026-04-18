# Changelog

All notable changes to `@brainstorm/design-system` are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project follows [semantic versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.1] — 2026-04-18

### Changed (PRE-CONSUMER BREAKING)
- **All tokens are now prefixed `--bds-`** (e.g., `--ink-1` → `--bds-ink-1`).
  Discovered during MSP integration audit that 30 tokens collided with MSP's
  existing `globals.css` (`--text-base`, `--space-4`, `--font-sans`,
  `--radius-lg`, `--duration-fast`, `--ease-in-out`, etc.) — every collision
  had a different value, which would silently corrupt either MSP's existing
  components or future BDS-styled components depending on import order.
- This is a pre-consumer breaking change. v0.1.0 was never installed by any
  consumer, so no migration is needed.

### Why prefix
- Self-documenting: reading `--bds-ink-1` in any consumer's code instantly
  signals "comes from the shared design system."
- Zero risk of future collisions as new tokens are added.
- Consumers can drop the prefix locally with their own alias layer if they
  want shorter names — but the package guarantees a clean namespace.

## [0.1.0] — 2026-04-18 (yanked)

### Added
- Initial extraction from `brainstormrouter/site/dashboard`.
- `tokens.css` — full token set: ink/bone/highlight palette, signal colors, paint colors, type scale, spacing, rhythm, shadows, motion, layout dimensions.
- `fonts.css` — Google Fonts loader for Fraunces, IBM Plex Sans, JetBrains Mono, Figtree (Vite/HTML consumers only).
- `package.json` with named exports (`./tokens.css`, `./fonts.css`).
- README documenting philosophy, install, usage per stack, token vocabulary, and versioning policy.
