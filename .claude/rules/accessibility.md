# Accessibility rules — WCAG 2.2 AA

A marketing site that excludes keyboard, screen-reader, or low-vision users is excluding paying customers. Plus EU accessibility act compliance matters for a DACH-targeting business.

## Baseline
- Target **WCAG 2.2 AA** as the floor.
- Run `astro check` + an a11y linter (axe DevTools or Lighthouse a11y audit) before merging.

## Semantic HTML
- Use the right element for the job. Buttons are `<button>`, not `<div onclick>`. Links are `<a href>`, not `<span onclick>`.
- Headings in order: one `<h1>` per page, then `<h2>` etc. — no skipping levels for styling reasons.
- Landmarks: `<header>`, `<nav>`, `<main>`, `<footer>`, `<aside>` on every page. Screen readers navigate by these.
- Lists are `<ul>`/`<ol>`/`<dl>`. Don't simulate with `<div>`s.

## Keyboard
- Every interactive element reachable + activatable via keyboard. Tab/Shift-Tab order matches visual order.
- Visible focus indicator on every focusable element. Don't `outline: none` without replacing with something equally visible.
- Skip-link (`<a href="#main">Skip to content</a>`) as the first element on pages with substantial nav.
- Modal / drawer / dropdown traps: focus trapped inside while open; restored to the trigger on close; Esc closes.

## Screen-reader support
- All non-decorative `<img>` need a meaningful `alt`. Decorative images: `alt=""` (empty, not omitted).
- Form inputs: every `<input>` has a programmatically-associated `<label>` (`for`/`id` or wrapping).
- `aria-label` only when visible text isn't possible. Visible text beats an aria-label every time.
- Don't sprinkle `aria-*` attributes without understanding them — wrong ARIA is worse than no ARIA.
- Live regions (`aria-live="polite"`) for status messages (form submitted, error appeared).

## Color + contrast
- **Text contrast** ≥ 4.5:1 for body, ≥ 3:1 for large text (≥ 18.66px regular or ≥ 14px bold).
- **UI component contrast** (borders, focus rings, button outlines) ≥ 3:1 against adjacent colors.
- Verify every text/background and accent color combination against these thresholds when adding new combinations to the palette.
- Don't use color as the only signal. Errors are red AND have an icon AND say "Error:". Success is green AND a checkmark AND says "Success:".

## Forms
- Every form field labeled.
- Required fields indicated visually AND with `required`/`aria-required`.
- Errors associated with their field via `aria-describedby`. Don't put errors in a separate region with no link to the input.
- Validation: don't disable submit on invalid; let it submit and announce errors.
- If you use a CAPTCHA / anti-spam widget (e.g. Cloudflare Turnstile): pick an accessible-by-design one, and if you customize it, preserve the keyboard + SR behavior.

## Motion + animation
- Respect `prefers-reduced-motion`. Any reveal / scroll / transition animations should short-circuit when this preference is set.
- No autoplay video with audio.
- No flashing > 3 times per second (seizure trigger).

## Language + locale
- `<html lang="de">` (or the page's actual language) on every page. If a locale switcher exists, it must update the `lang` attribute when the locale changes.
- Use hreflang tags when the same page is published in multiple languages.
- Don't mix languages in a page block without `lang` attributes on the foreign-language spans.

## Don't
- Don't use `<div role="button">` when `<button>` exists.
- Don't trap users in modals without a way out.
- Don't hide content from screen readers with `display: none` and expect it to be invisible to sighted users only — sighted users won't see it either. Use `aria-hidden="true"` for visual-only decorative content, or `.sr-only` CSS for sighted-hidden / SR-shown text.
- Don't `outline: none` on focus. Replace it with something at least as visible.
- Don't auto-focus a non-essential field on page load — fights screen-reader announcement.
