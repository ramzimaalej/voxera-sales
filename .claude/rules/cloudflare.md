# Cloudflare rules — Astro adapter (Workers) + Wrangler

This site runs on the **`@astrojs/cloudflare` adapter** (`astro.config.mjs` → `adapter: cloudflare()`), deployed to Cloudflare Pages. The adapter compiles SSR routes into a single `_worker.js` in `dist/` — there is **no file-based `functions/` directory** (don't add one). Static assets are served via the `ASSETS` binding (`wrangler.jsonc`). Config lives in `wrangler.jsonc` (`main: "@astrojs/cloudflare/entrypoints/server"`).

## Static-first, opt into SSR per route
- Pages are **prerendered (static) by default**. A route opts into server rendering with `export const prerender = false;` at the top of the `.astro` page or endpoint. Keep marketing pages static; only make a route dynamic when it genuinely needs request-time logic (form POST, geo, A/B).
- API-style endpoints are Astro **endpoints** (`src/pages/api/*.ts` exporting `GET`/`POST` handlers returning a `Response`), not Cloudflare `onRequest` Pages Functions.
- Use Web-standard APIs only (`Request`, `Response`, `fetch`, `URL`, `URLSearchParams`, `crypto.subtle`, `crypto.randomUUID()`). No Node.js APIs (`fs`, `crypto.createHash`) unless `nodejs_compat` is enabled — which needs an ADR.

## Bindings (KV, R2, env, secrets)
- In SSR routes, bindings are on the Astro runtime: `const { env } = Astro.locals.runtime;` (or `context.locals.runtime.env` in endpoints) — **not** `context.env` (that's the old Pages-Functions API).
- Declare bindings in `wrangler.jsonc`; type them via `Env` in `src/env.d.ts` (`type Runtime = import('@astrojs/cloudflare').Runtime<Env>;` extending `App.Locals`).
- Adding a binding: declare in `wrangler.jsonc` → add to the `Env` type → use via `Astro.locals.runtime.env`. All three or it breaks in production.

## Secrets
- Secrets via `wrangler pages secret put <NAME>` (per-environment). Never commit to git.
- Adding a secret: document in `wrangler.jsonc` comments (purpose + how to set it + fallback behavior). Add to `Env` type. Validate at function start; fail loudly if missing in production.

## Deploys
- Push to main → auto-deploy via Cloudflare Pages GitHub integration.
- Local preview: `npm run preview:pages` (builds + runs `wrangler pages dev`).
- Manual deploy: `npm run deploy` (builds + `wrangler pages deploy dist`).
- Preview deploys on PRs: enabled in Cloudflare dashboard; each push to a branch creates a preview URL. Use these to validate before merge.
- Babysitter `breakpointTolerance.alwaysBreakOn` includes `deploy` — the build-feature process will pause before invoking `deploy`.

## Endpoint discipline
- **Validate input.** Every endpoint: validate the request shape (method, content-type, body schema). Return 400 with a clear error on bad input.
- **Verify Turnstile** on public form endpoints. Don't trust the client.
- **Rate limit** at the endpoint level (KV-backed) for endpoints that send email or hit external services. Don't let one bad actor exhaust a third-party quota.
- **Idempotency**: write endpoints that should not double-fire (e.g. signup dedup) need a KV-backed dedup story.
- **Error handling**: catch and return a 500 with a stable error code, never a raw stack trace. Log via `console.error` (visible in `wrangler tail`).
- **CORS**: only set `Access-Control-Allow-Origin` to specific origins, not `*`, on endpoints that mutate state.

## Logging + observability
- `console.log` / `console.error` from SSR routes stream to Cloudflare Logs and `wrangler tail`.
- Log structured (`console.log(JSON.stringify({...}))`) so logs are filterable in the dashboard.
- Don't log PII (email addresses, names) without redaction.
- Don't log API keys, even truncated.

## Caching headers (set in Functions or via `_headers`)
- HTML: `Cache-Control: public, max-age=0, must-revalidate` (re-validate every request).
- Static assets with content-hashed filenames: `Cache-Control: public, max-age=31536000, immutable`.
- API responses: `Cache-Control: no-store` for endpoints that read user-specific data.

## `_headers` and `_redirects`
- Cloudflare Pages supports `public/_headers` and `public/_redirects` Netlify-style files. Use them for site-wide header/redirect rules.
- Security headers (CSP, X-Frame-Options, Referrer-Policy, Permissions-Policy) belong here — apply once, get on every response.

## Compatibility date
- `compatibility_date` is set in `wrangler.jsonc`. Bump it deliberately when a new Workers feature is needed; don't bump speculatively (changes runtime behavior).

## Don't
- Don't run Node.js dependencies that aren't Workers-compatible (most aren't; check before adding).
- Don't write to KV from the build step. KV is runtime-only.
- Don't bundle large dependencies into an SSR route — cold-start cost. Keep the server bundle lean.
- Don't deploy with `wrangler` from a developer machine bypassing Pages CI for "a quick fix" — preview deploys + PR review exist for a reason.
- Don't add a `nodejs_compat` flag without an ADR — opens the door to Node API drift.
