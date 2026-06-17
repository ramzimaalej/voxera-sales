---
name: brand-conformance
description: Read-only lint of Guidaro copy (.astro pages/components, markdown, JSX/string literals) against the local Guidaro brand doc `docs/brand/brand-guidelines.md`. Use before publishing copy changes to the GUIDARO sales site, or when reviewing a copy-bearing PR. German-language site. Flags naming, tone, and CTA drift — reports issues, does not auto-fix. Note: the brand doc is currently a draft skeleton, so forbidden-words / voice rules are TBD until it is filled in.
---

# brand-conformance

Lint copy against the **Guidaro** brand guidelines.

> Guidaro is its own brand — a German-language DACH sales-consulting agency. This skill has **nothing** to do with the Voxera CRM brand. Do not import or reference any Voxera brand doc, vocabulary, or rule here.

## When to invoke

- Reviewing a copy change in `src/pages/**/*.astro` (`index`, `branchen`, `leistungen`, `ueber-uns`, `kontakt`, `impressum`) or `src/components/**/*.astro`.
- Iterating on `docs/brand/brand-guidelines.md` itself.
- Before approving a copy-bearing PR.

## Source of truth

`docs/brand/brand-guidelines.md` (local to this repo). **Read it first.** It is canonical — when the checks below conflict with the brand doc, the brand doc wins.

> **Status: draft skeleton.** The Guidaro brand doc is not yet filled in, so the brand-specific rules — forbidden words, voice attributes, approved vocabulary, italic limits, tone — are **TBD**. Until the doc is fleshed out, this skill can only enforce the stable invariants in the "Always-on checks" section below; treat everything in "Rules pending the brand doc" as advisory and **defer to the brand doc the moment it exists**. If a check here has no backing in the brand doc, flag it as `[advisory]`, not `[block]`.

## Always-on checks (stable, independent of the brand doc)

### 1. Naming — block on violation
- **Guidaro** is the brand name. It must be capitalized **Guidaro** every time (never lowercase `guidaro`, never all-caps `GUIDARO` in prose, never `Guidaro CRM` as if it were a separate product name — the CRM is "proprietäres CRM" / "unser CRM", described, not branded).
- Do **not** introduce the Voxera name or any Voxera-brand vocabulary into Guidaro copy. Flag any occurrence of "Voxera".

### 2. Language — German only — warn on violation
- This is a German-language site. All user-visible copy is in German (Deutsch). Flag stray untranslated English prose in user-facing strings.
- Established product/positioning nouns may stay as-is where that is the natural German usage (e.g. `Sales`, `Pipeline`, `Hausnotruf`, `Pflegebox`, `CRM`, `KI-Sprachagenten`). Don't flag these — flag full English sentences or marketing phrases that should be German.

### 3. Honesty — block on violation
- No fabricated metrics, fake testimonials, invented partner logos, or made-up case studies. Any number that appears as a claim (e.g. "1.500+ Anträge", "100% Performance-basiert") must be real and defensible.
- Customer-outcome aggregates are off-limits until measured. Capability/process claims are fine.

### 4. CTA consistency — warn on missing/wrong CTA
- The site's established primary CTA is **"Termin buchen"** (calendar booking, e.g. the Calendly link). Flag a page whose main call-to-action drifts from this without reason.
- `impressum` (legal notice) carries no marketing CTA — don't flag its absence there.
- Once the brand doc defines a per-page CTA matrix, **that** becomes canonical and supersedes this default.

## Rules pending the brand doc (advisory until filled in)

These are placeholders matching the *shape* of a brand lint. Each becomes enforceable only once `docs/brand/brand-guidelines.md` specifies it. Until then, surface findings as `[advisory]` and point the author at the brand doc.

- **Forbidden words / hype list** — TBD. The brand doc will list overused/empty marketing words to avoid (the German equivalents of "revolutionär", "Marktführer", "innovativ", "Lösungen", etc.). Do not block on a guessed list; wait for the doc.
- **Approved vocabulary / signature phrases** — TBD. The brand doc will name the preferred terms and any signature phrases.
- **Voice & tone attributes** — TBD. The brand doc will define Guidaro's voice (e.g. how plain-spoken, how formal `Sie` vs `du`, how technical). Until then, only flag obvious tone breaks (slang in a sales pitch, contradictory register within one page).
- **Italic / emphasis limits** — TBD. The hero currently uses `<em>` for emphasis ("Weniger Aufwand."); don't impose a hard cap until the brand doc sets one. Advisory: keep emphasis sparse.

## Workflow

1. Read each changed copy file.
2. Read `docs/brand/brand-guidelines.md`. If it is still a skeleton, note that in the report header and limit blocking findings to the always-on checks.
3. Grep-first pass for the cheap, stable checks: lowercase `guidaro`, the string `Voxera`, stray English sentences, CTA drift.
4. Read for tone only as far as the brand doc supports — don't invent rules.
5. Produce a structured report:
   ```
   Brand doc: docs/brand/brand-guidelines.md (status: draft skeleton — voice rules TBD)

   File: src/pages/index.astro
     [block]    L13  lowercase "guidaro" — must be "Guidaro"
     [block]    L40  "Voxera" referenced — wrong brand, remove
     [warn]     L21  English marketing sentence on a German page — translate
     [advisory] L55  hype word candidate — confirm against brand doc once it lists forbidden words
   ```
6. Do **not** auto-fix. This skill is read-only — it surfaces issues and lets the author decide.

## Engineering OS context

This repo uses the shared engineering OS in `voxera-os` (processes invoked by relative path `../voxera-os/.a5c/processes/<name>.js`; ADR numbers allocated from `../voxera-os/decisions/ADR-REGISTRY.md`). That is **orchestration and decision-log plumbing only** — it does not make the Guidaro site share Voxera's brand. The brand stays local here.
