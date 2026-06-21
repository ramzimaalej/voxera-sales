# Patterns

Reusable, durable implementation patterns for the **Guidaro** site — techniques that outlive any
single feature (e.g. an Astro island-hydration pattern, a German-copy content-collection pattern, a
Cloudflare Pages Function + Turnstile contact-form pattern, an LCP/CLS recipe for the landing page).

This is the **synthesis sink** for `harvest-feature`: when a DONE feature surfaces a reusable
technique, it lands here as a `*.md` pattern (deduped against existing ones), so the next feature
reuses it instead of rediscovering it.

> Auto-loaded build *rules* (the non-negotiable conventions) live in `../../.claude/rules/`
> (`astro.md`, `performance.md`, `accessibility.md`, `cloudflare.md`, `dry.md`). Patterns here are the
> richer "how we tend to do X" companions to those rules. Guidaro brand/voice lives in
> `../../docs/brand/` — keep brand decisions there, engineering patterns here.
