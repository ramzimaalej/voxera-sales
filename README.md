# Guidaro — sales-consulting site (`voxera-sales`)

The marketing/landing site for **Guidaro**, a German (DACH) sales-consulting agency.

> **Guidaro is its own brand — not Voxera.** Do not pull in Voxera brand, voice, strategy, or product facts. Brand isolation is enforced by the `brand-voice-reviewer` agent and the `brand-conformance` skill. See `CLAUDE.md`.

Built with **Astro** (static-first), deployed to **Cloudflare Pages** via the `@astrojs/cloudflare` adapter. German-only (no i18n).

## Layout

| Path | What |
|---|---|
| `src/` | Astro pages, components, layouts, styles. |
| `public/` | Static assets (images, favicon). |
| `docs/brand/brand-guidelines.md` | Guidaro's **local** brand guide (voice/essence sections are a work-in-progress skeleton). |
| `decisions/` | Site-engineering ADRs. Numbers are allocated from the workspace registry at `../voxera-os/decisions/ADR-REGISTRY.md`. |
| `features/` | `FEAT-xxx` specs that drive implementation runs. |
| `.claude/` | `agents/` (brand-voice-reviewer), `rules/` (astro, performance, accessibility, cloudflare, dry), `skills/` (brand-conformance). |
| `.a5c/commands.json` | Quality gates (`lint`, `typecheck`, `build`). |

## Governance

- **Engineering processes** are invoked from the shared OS by relative path, never copied:
  `/babysitter:call --process ../voxera-os/.a5c/processes/build-feature.js#process implement features/FEAT-xxx-*.md`
- **Decisions** that constrain this site get an ADR in `decisions/`, numbered from the shared registry.
- **Brand** lives here locally (`docs/brand/`) — it is Guidaro's, and is intentionally separate from `voxera-os/docs/brand/`.

## Commands

All run from the repo root:

| Command | Action |
|---|---|
| `npm install` | Install dependencies |
| `npm run dev` | Local dev server |
| `npm run build` | Production build to `./dist/` |
| `npm run preview` | Preview the build locally |
| `npm run astro check` | Type/diagnostics check |
