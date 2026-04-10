# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev       # Start dev server at http://localhost:4321
npm run build     # Production build → dist/
npm run preview   # Preview production build locally
```

## Design System

All design tokens come from the **Women in AI Forum branding library** (Figma file `QhwTuVNTDGHFr9HFgYqzVD`). The main design file is `Z2qwgJQv4Tbfixbi0Az1vD`. CSS custom properties live in `src/styles/global.css` and are the single source of truth for colours, typography, and border radii — always use variables rather than hardcoded values.

**Fonts:**
- `--font-heading`: IBM Plex Serif (Google Fonts, weight 400) — all headings
- `--font-ui`: Px Grotesk (local TTF in `public/fonts/`) — buttons, nav, labels, tag pills
- `--font-body`: Geist (Google Fonts, weights 400/500) — body text, captions

**Colour naming convention:** product colours come in dark/light pairs used for text/background respectively (e.g. `--color-purple-dark` on `--color-purple-light`). The opacity scale (`--color-5` through `--color-40`) represents tints of a mid-grey.

## Architecture

Single-page Astro 6 site. Each page section is a self-contained `.astro` component with scoped `<style>` blocks. The only shared CSS is `src/styles/global.css` (tokens, reset, and reusable utility classes for tag pills, buttons, and form inputs).

**React integration:** `@astrojs/react` is included solely for the `AgentationOverlay.tsx` component (live annotation tool). All page sections are `.astro` — no React needed there. The `AgentationOverlay` mounts via `client:only="react"` in `Layout.astro` and connects to the agentation MCP server at `http://localhost:4747`. Start it with `npx agentation-mcp server --port 4747`.

**Component responsibilities:**
- `NavBar` / `Footer` — site chrome, share the same logo SVG at different sizes (140×40 nav, 112×32 footer)
- `Hero` — contains a placeholder div where the hero image goes; replace with `<img>` once `public/images/hero.jpg` is added
- `LogosSection` — embeds `public/images/logos-bar.svg` (Figma export of the full logos frame)
- `FeaturesSection` — 2×2 card grid on desktop, single column on mobile
- `CTASection` — two-column layout (copy left, form right) that stacks on mobile; body copy is a TODO placeholder

**Responsive breakpoint:** `768px`. Desktop designs are based on 1512px wide frames; mobile on 402px. Content max-widths are 1000px (most sections) or 720px (hero text, features).

**SVG assets** in `public/images/` were exported directly from Figma at 2× and should not be edited manually — re-export from Figma if they need updating.

## Figma Reference

The design uses Auto Layout throughout. Key node IDs for re-fetching layout data via the Figma REST API (`GET /v1/files/{key}/nodes?ids=...`):

| Section | Desktop node | Mobile node |
|---|---|---|
| Nav Bar | `35:18660` | `60:1379` |
| Hero | `35:14093` | `60:1392` |
| Logos | `60:1874` | `60:1672` |
| Features | `35:18760` | `60:1433` |
| CTA / Form | `38:836` | `60:1463` |
| Footer | `35:19162` | `60:1517` |
