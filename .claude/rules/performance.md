# Performance rules — Core Web Vitals first

This is a marketing site. Visitors form an impression in the first 2 seconds. Performance is brand.

## Targets (75th percentile, mobile)
- **LCP** (Largest Contentful Paint): < 2.0s
- **INP** (Interaction to Next Paint): < 200ms
- **CLS** (Cumulative Layout Shift): < 0.05
- **TTFB**: < 600ms (Cloudflare edge — should be easy)
- **Lighthouse Performance**: ≥ 90 on every public page

If a change risks these, the change needs a justification.

## LCP discipline
- Hero image / heading is the LCP element on most pages. Preload it.
- Hero image: `<Image>` from `astro:assets` with `loading="eager"` + `fetchpriority="high"`. Width/height attributes mandatory (prevents CLS).
- Web fonts: `font-display: swap` + `<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>`. Only load the weights actually used (see brand guidelines).
- Don't put a hero behind JS that has to hydrate first.

## INP / JS budget
- Aim for **< 50 KB JS gzipped** per route. Most marketing pages should be < 20 KB.
- Audit before merging: `astro build` output shows per-page JS. If a page jumps > 30 KB, justify it.
- Islands: prefer `client:visible` over `client:load` (defers hydration).
- Don't ship a framework runtime (React, Vue) for non-interactive content. Vanilla `<script>` is fine for tiny interactions.
- Don't add analytics / chat widgets that block the main thread on load. Async + deferred only.

## CLS prevention
- Every `<img>`, `<video>`, `<iframe>`: explicit `width` + `height` attributes (or aspect-ratio CSS).
- Reserve space for late-loading content (ads, embeds, fonts).
- Don't inject content above existing content after first paint. Banners / cookie notices either render server-side or use `position: fixed` so they don't push layout.

## Images
- Use `astro:assets` `<Image>` and `<Picture>` for all content images. Astro generates responsive sources + modern formats (AVIF, WebP).
- Decorative images: `loading="lazy"` + `decoding="async"`.
- SVGs: inline small ones (< 4 KB) for zero HTTP overhead; reference larger ones from `public/`.
- Source the smallest acceptable size. Don't ship 3000×2000 photos when 1200×800 suffices on a 1440px viewport.

## Fonts
- WOFF2 only.
- Subset by language and weight. Don't ship Cyrillic glyphs to an EN/DE site.
- 3 weights per family max in production.
- `font-display: swap` so text is visible during font load.

## CSS
- Single hand-written stylesheet (no preprocessor); aim for < 50 KB total.
- Inline critical CSS for above-the-fold content if FCP needs it.
- Don't load print/utility CSS on every page if it's only used on one.

## Network
- Cloudflare Pages: HTTP/3 + Brotli are on by default.
- Caching: static assets get long `Cache-Control: public, max-age=31536000, immutable`. HTML stays revalidated. The defaults are fine; don't disable.
- Don't add a CDN in front of Cloudflare. It is the CDN.

## Verification
- Before merging perf-affecting changes: run `npm run build` and inspect `dist/` JS / CSS sizes.
- For substantive changes: Lighthouse CI or PageSpeed Insights on a deployed preview.
- The `web-perf` skill (global) can measure deployed pages — use it before / after.

## Don't
- Don't add unused npm dependencies "to try" — they bloat the build even if tree-shaken (codegen overhead, lockfile churn).
- Don't lazy-load above-the-fold content. Defeats the purpose.
- Don't add a service worker for "offline support" on a marketing site — it traps users in stale builds.
- Don't ship source maps to production for the main bundle (Cloudflare Pages serves what's in `dist/`).
