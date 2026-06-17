# DRY rules — pragmatic duplication for the Guidaro site

A small Astro site has different DRY economics than a multi-app monorepo. Most "duplication" you'll see is page-to-page boilerplate (hero, CTA section, footer), and most of it is fine. The Guidaro site is a handful of German-language pages (`branchen`, `leistungen`, `ueber-uns`, `kontakt`, `impressum`) — keep each one readable end-to-end.

## When to extract

| Situation | Action |
|---|---|
| 2 similar `.astro` blocks | Leave them. Pages should be readable end-to-end without jumping into shared components. |
| 3+ similar blocks doing the same thing (same hero, same CTA, same nav) | Extract into `src/components/<Name>.astro` and reuse. |
| Inline `<style>` block repeated across 3+ pages | Move to `src/styles/global.css` as a class or custom property. |
| Repeated copy across pages (e.g., trust line, brand promise, the hero claim "Mehr Verträge. Weniger Aufwand. Messbare Ergebnisse.") | Lift it into one source — a shared component, a `src/data/` constant, or `src/styles/global.css` for a CSS-driven label. Single source of truth so the wording never drifts. |

The cost of premature extraction on a marketing site: you trade readable, scan-able page files for "what does `<CtaSection variant='compact' tone='dark' />` render again?" detours. Keep page files visually understandable.

## What counts as duplication worth fixing

- **Copy** repeated across pages (especially CTAs, the trust line, the brand promise) → one source: a component or a `src/data/` constant.
- **Layout patterns** with identical structure across 3+ pages (hero, CTA, footer) → component in `src/components/` (the way `Nav.astro` and `Footer.astro` already are).
- **CSS values** repeated as inline `style="..."` across 3+ pages → CSS custom property in `src/styles/global.css`.
- **Script logic** copy-pasted into multiple `<script>` blocks → small helper in `src/scripts/` or `src/lib/`.

## What is *not* worth fixing

- Two pages with similar but distinct heroes — each tells its own story.
- Markdown-content blog posts that share structure (that's what content collections handle for you).
- Two pages that both use `<Image src="..." alt="..." width="..." height="..." />` — that's the framework primitive; you can't dedupe it without losing readability.
- Decorative SVGs reused across 2 pages — inline both; they're small.

## Single source of truth for brand copy

The site is single-locale German, so there's no i18n table catching drift for you — the discipline has to be manual. The risk is the same one i18n usually solves: a brand string (the hero claim, a CTA, the trust line, a brand verb) hard-coded into three pages, then edited in one and left stale in the other two.

- A copy string used on 3+ pages → put it in **one** place (a shared component or a `src/data/` constant) and reference it.
- Brand voice and the load-bearing strings live in the Guidaro brand guide — `../../docs/brand/brand-guidelines.md`. When the same approved phrase appears on multiple pages, reuse one source rather than retyping it.

When in doubt with a brand-voiced, user-visible string: give it a single home from the start.

## Detection — `jscpd`

For an ad-hoc audit:

```sh
npx jscpd --min-tokens 30 --min-lines 5 src
```

A low token threshold because Astro pages are short; 30 tokens is the floor.

Don't add jscpd to CI for this repo without a cleanup pass first — marketing sites accumulate intentional similarity.

## Don't extract by

- **Mega-components** — `<HeroOrCtaOrBoth variant="...">` with 12 props is harder to understand than 3 inline blocks.
- **Slot abuse** — Astro `<slot>` is great for layouts; if you find yourself passing 5 slots to render what's essentially 5 different components, just make 5 components.
- **Frameworks for the sake of dedup** — bringing in React (or any UI runtime) to render a card 3 ways is more cost than 3 `.astro` blocks. This site ships zero client framework; keep it that way unless a real interaction demands one.

## Code review

When reviewing a PR that adds a new component: ask "is this used 3+ times?" — if no, inline it in the page.

When reviewing a PR that inlines markup that already has a component: ask "is the existing component the right shape for this use?" — if no, copy is fine. If yes, use it.

## See also

- `../../docs/brand/brand-guidelines.md` — Guidaro brand voice and reusable copy (trust line, brand verbs, the hero claim). This is the **local Guidaro** brand — not the Voxera CRM brand; never reference Voxera's brand here.
- `performance.md` — bundle/CSS budgets a stray component or duplicated style can blow.
