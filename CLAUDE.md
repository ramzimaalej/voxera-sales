# guidaro-sales

Code for the **Guidaro** marketing site — a German-language sales-consulting agency for the DACH market (specialist for Pflegebox & Hausnotruf sales). Astro + Cloudflare Pages.

> **Guidaro is its own brand.** It is *not* Voxera. Do not pull in Voxera brand, voice, strategy, or product facts, and never reference the Voxera CRM here. Guidaro's identity is owned locally in this repo (see below). The two share an engineering OS, nothing else.

## What this repo is

A small, static, German-first marketing site. Hero promise: **"Mehr Verträge. Weniger Aufwand. Messbare Ergebnisse."** Pages live in `src/pages/` (all German):

- `index.astro` — landing
- `branchen.astro` — industries served
- `leistungen.astro` — services
- `ueber-uns.astro` — about
- `kontakt.astro` — contact
- `impressum.astro` — legal imprint (German legal requirement)

Copy is German. Keep it German. There is no EN locale.

## Canonical context lives **here**, locally

Unlike the other code repos, Guidaro does **not** read its brand or strategy from a command/OS repo:

- **Brand:** `docs/brand/brand-guidelines.md` — Guidaro's own brand (voice, palette, writing rules). It is the single source of truth for all copy and visual decisions on this site. It's currently a draft skeleton; flesh it out here, don't borrow from elsewhere.
- **Specs:** feature and bug specs live locally under `features/` (`FEAT-xxx-<slug>.md`, `BUG-xxx-<slug>.md`). If a spec and the code disagree, the spec wins — fix the code or update the spec.

**Do not** reference the Voxera brand (`../voxera-os/docs/brand/`) or any Voxera product/strategy doc. If you need a fact about Guidaro that isn't written down yet, write it into `docs/brand/` (or a spec) — don't import it from a sibling brand.

## Working with babysitter

The engineering OS (processes + ADR registry) is shared and lives in **`../voxera-os/`**. Like every code repo in the workspace, Guidaro *invokes* those processes by relative path so the run + journal land in this repo:

- Implement a feature:
  `/babysitter:call --process ../voxera-os/.a5c/processes/implement-feature.js#process — spec features/FEAT-xxx-<slug>.md`
- Fix a bug:
  `/babysitter:call --process ../voxera-os/.a5c/processes/fix-bug.js#process — spec features/BUG-xxx-<slug>.md`

(You can also just say `/babysitter:call implement features/FEAT-xxx-<slug>.md` and let the skill pick the process.)

Never copy a process into this repo — invoke the shared one. The processes are repo-agnostic and read this repo's quality gates from `.a5c/commands.json`.

## Quality gates for this repo

The babysitter processes read gate commands from **`.a5c/commands.json`** (`qualityGate.commands`, `passingScore`). Current gate:

- `npm run lint` — ESLint (`eslint . --max-warnings=50`, with the astro + jsx-a11y plugins)
- `npm run typecheck` — `astro check`
- `npm run build` — `astro build`

`passingScore: 90`. There is no test runner yet (`test` is `null`); the gate is lint + check + build. First-time setup runs `npm install` to bring in the Astro + ESLint + wrangler stack.

## Local rules, agent, skill

Under `.claude/`:

- **`.claude/rules/`** — auto-loaded every session:
  - `astro.md` — static-first render, island hydration priority, scripts/styles discipline
  - `performance.md` — Core Web Vitals targets, JS budget, images/fonts
  - `accessibility.md` — WCAG 2.2 AA floor, semantic HTML, keyboard + focus, contrast
  - `cloudflare.md` — Pages deploy, Web APIs (not Node) in any Functions, bindings + secrets via wrangler
  - `dry.md` — share layout/components (`src/layouts/`, `src/components/`); don't duplicate page scaffolding across the German pages
- **brand-voice-reviewer** (`.claude/agents/`) — subagent for the Task tool. Reviews copy-bearing changes (`src/pages/**/*.astro`, `src/components/**/*.astro`) against this repo's **local Guidaro** brand (`docs/brand/brand-guidelines.md`). Returns [block]/[warn]/[info] findings. Run it before merging copy changes; it can propose rewrites for blocking findings when asked explicitly. This is the `brandCheck` agent wired in `.a5c/commands.json` — it reviews German copy against Guidaro, never Voxera.
- **brand-conformance** (`.claude/skills/`) — read-only lint of copy against Guidaro's brand guidelines (forbidden words, voice, CTA patterns). Guidaro's own rules, not Voxera's.

## Decisions (ADRs)

Real engineering decisions for this site get an ADR in this repo's local **`decisions/`** folder. Numbers are **global across the whole workspace** — allocate the next free number from the shared registry at **`../voxera-os/decisions/ADR-REGISTRY.md`**, create the file here from this repo's `decisions/_template.md`, then add your row back to that registry in the same change. Numbers are never reused or renumbered.

Strategy/brand/company-wide ADRs are not this repo's concern. Only site-engineering decisions live here.

## Deploy

Cloudflare Pages, via **wrangler**. Build + deploy: `npm run deploy` (`astro build && wrangler pages deploy dist`). Local Pages preview: `npm run preview:pages`. Config in `astro.config.mjs` (`@astrojs/cloudflare` adapter) and `wrangler.jsonc` (project `guidaro-sales`, assets served from `./dist`). Pause for human approval before any production deploy.

## On completion

When a run finishes it should: update the spec's `status` in `features/`, and — if a real site-engineering choice was made — open an ADR in `decisions/` (number from `../voxera-os/decisions/ADR-REGISTRY.md`). The shared `implement-feature.js` / `fix-bug.js` processes do this in their final phases. Run **brand-voice-reviewer** on any changed German copy before merging.
