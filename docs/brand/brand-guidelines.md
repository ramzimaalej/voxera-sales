---
title: Guidaro Brand Guidelines
version: 1
status: draft
updated: 2026-06-16
owner: you
---

> **DRAFT SKELETON — to be completed.** This file was scaffolded automatically when `voxera-sales` (the **Guidaro** site) was wired into the shared babysitter OS. Only the **Visual identity** section below is populated, and only from values that are directly observable in the existing site code (`src/styles/global.css`, `src/pages/index.astro`, `src/layouts/`, `src/components/`). Every other section is a **TODO** placeholder for you to fill in. Do not treat the TODO sections as authoritative until a human has written them.
>
> **Guidaro is its own brand, distinct from Voxera.** This is the canonical, *local* Guidaro brand source. Do **not** reference or import the Voxera brand (`voxera-os/docs/brand`) here — Guidaro ≠ Voxera.

---

## Brand essence

**TODO (fill in).** One paragraph: what Guidaro stands for, the single promise it makes, and the feeling the brand should leave. (Observable starting signal from the site copy: a performance-based, KI-plus-human outbound-sales specialist for the German Pflegebox & Hausnotruf market — but write the real essence intentionally, don't just restate the tagline.)

---

## Company facts

**TODO (fill in / verify).** Confirm and own these as canonical brand facts rather than copy scraped from the footer/Impressum. Observable values currently in the site:

- **Name:** Guidaro
- **Positioning line (hero badge):** "Spezialist für Pflegebox & Hausnotruf Sales"
- **Domain / contact (from footer + Impressum):** guidaro.io · sales@guidaro.io · +49 156 79752011
- **Geschäftsführer / responsible (Impressum):** Ahmed Abida
- **Registered address (Impressum):** 01 Juni Str, Sekiet Ezzit 3021, Sfax, Tunesien
- **Tax ID (Impressum):** 1949907P
- **TODO:** confirm legal entity form, founding date, team size, and any market claims (e.g. "1.500+ Anträge") you want to stand behind as brand facts.

---

## Voice & tone

The site is **German-language (lang="de"), DACH B2B, sales-consulting / Vertriebsdienstleistung** for the Pflegebox & Hausnotruf market. That register is fixed and observable; the specific brand attributes are not.

- **Language:** German (Sie-form throughout the existing copy — e.g. "Generische Callcenter kennen Ihren Markt nicht. Wir schon.").
- **Audience:** B2B — Pflegebox- und Hausnotruf-Anbieter in Deutschland (partners/operators, not end consumers).
- **Voice attributes:** **TODO (fill in)** — pick 3-5 adjectives (e.g. direct? confident? plainspoken? expert?) and define each with a do/don't example.
- **Tone by context:** **TODO (fill in)** — hero/landing vs. Leistungen vs. Kontakt vs. Impressum.
- **Sie vs. du:** **TODO (confirm)** — existing copy uses **Sie**; lock this as the rule.

---

## Visual identity

*Populated from the actual site code. This is the one section grounded in real data.*

### Color palette

CSS custom properties from `src/styles/global.css` (`:root`):

| Token | Hex | Role (observed usage) |
|---|---|---|
| `--navy` | `#0B1F3A` | Primary dark; default text color, footer & trust-bar background, headings |
| `--navy-mid` | `#1a3558` | Mid navy (defined; secondary dark) |
| `--blue` | `#1756B8` | Primary brand blue; primary buttons, links, accents, active pipeline step |
| `--blue-light` | `#2E74D4` | Blue hover state |
| `--blue-pale` | `#EBF2FF` | Pale blue; section-tag background |
| `--accent` | `#00C48C` | Accent green; badge dot, "approved" checks, hero zahl-number on dark cards |
| `--accent-dark` | `#00A073` | Darker accent green |
| `--gray-100` | `#F5F7FA` | Lightest gray; section/card backgrounds |
| `--gray-200` | `#E8ECF2` | Borders, hairlines |
| `--gray-400` | `#9AA3B0` | Muted labels |
| `--gray-700` | `#4A5568` | Body / secondary text |
| `--white` | `#FFFFFF` | Background, on-dark text |

Other observed colors not yet tokenized:
- Hero gradient: `linear-gradient(135deg, #F0F5FF 0%, #FAFBFF 60%, #F0FDF8 100%)`.
- "done" pipeline step: background `#ECFDF5`, text `#065F46`.
- Logo mark (`public/img/voxera-logo.svg`) uses blue `#1756B8` with an orange ring `#E8804A` — **note:** the orange `#E8804A` is **not** in the CSS token set. **TODO:** decide whether orange is an official brand color or just a logo detail.

### Typography

Loaded via Google Fonts in `src/styles/global.css`:

- **Headings — `--font-head`:** **Sora** (weights 300-700 loaded; used at 600/700). Applied to `h1`/`h2`/`h3`, nav/footer logo, stat numbers, trust logos.
- **Body — `--font-body`:** **DM Sans** (weights 300/400/500 + italic 300). Default `body` font; base size 16px, line-height 1.65.
- **Type scale (observed):** `h1` `clamp(36px,5vw,62px)` / 700 / letter-spacing -1.5px; `h2` `clamp(28px,3.5vw,44px)` / 700; `h3` 20px / 600; `.lead` 18px.
- **Stylistic note:** emphasis word in `h1` uses `<em>` rendered as *non-italic* in `--blue` (`h1 em { font-style: normal; color: var(--blue); }`) — i.e. emphasis is by **color**, not italics.

### Radii, shadows, spacing tokens

- `--radius: 12px`, `--radius-lg: 20px`.
- `--shadow: 0 4px 24px rgba(11,31,58,0.10)`, `--shadow-lg: 0 8px 48px rgba(11,31,58,0.14)`.
- Section padding: `96px 6vw` (desktop), `64px 5vw` (≤900px).

### Logo / mark

- **Wordmark:** "Guidaro" set in **Sora 700**, letter-spacing -0.5px (nav + footer). In nav it sits on white in `--navy`; in footer on navy in white.
- **Mark:** `public/img/voxera-logo.svg` — a blue (`#1756B8`) disc with an orange (`#E8804A`) ring and a white downward chevron. **Note:** the file is named `voxera-logo.svg` but is used with `alt="Guidaro"`; **TODO:** rename the asset and confirm the mark is the intended Guidaro logo (legacy filename).
- **Favicon:** `public/favicon.svg` is a different mark (a stylized "A" glyph that recolors for dark mode) — **does not match** the nav logo. **TODO:** reconcile favicon and logo into one consistent mark.
- **Button system (observed):** `.btn-primary` = solid `--blue`, 10px radius; `.btn-secondary` = transparent with `--gray-200` border, hover to `--blue`.

---

## Writing rules

**TODO (fill in).** Define the concrete copy rules. Observable conventions to codify or revise:

- **CTA patterns (German, observed):** "Termin buchen — 30 Min", "Partnerschaft anfragen", "Unsere Branchen", "Alle Leistungen ansehen →". **TODO:** decide canonical CTA verbs and whether the trailing "→" / "— 30 Min" patterns are house style.
- **Numbers/stats formatting (observed):** German thousands separator with a dot ("1.500+"), "Ø" for averages, "€" suffix. **TODO:** lock formatting rules.
- **Terminology:** **TODO** — preferred terms (KI-Sprachagenten, Vertriebsmitarbeiter, Pflegebox, Hausnotruf, Krankenkassen) and any to avoid.
- **Capitalization / punctuation / dash usage:** **TODO**.

---

## Anti-patterns

**TODO (fill in).** What Guidaro never does in copy or design. Examples to define: claims it can't substantiate, generic-callcenter language, English where German is expected, mismatched marks/colors, off-palette colors, italic emphasis (the system uses color emphasis instead). **TODO:** make this list real.

---

## Changelog
- 2026-06-16 v1: skeleton scaffolded from the existing site CSS/copy when voxera-sales was wired into babysitter (ADR-0015 made Voxera brand shared; Guidaro keeps its own local brand).
