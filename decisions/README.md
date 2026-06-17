# Guidaro-site decision log (ADRs)

This is the decision log for the **Guidaro** sales-consulting site (`guidaro-sales`). ADRs here record **engineering and marketing** choices for this site: framework and rendering approach, hosting (Cloudflare Pages / wrangler), build and CI tooling, page structure and information architecture, content/SEO strategy, analytics, forms and lead capture, accessibility policy. The decision lives next to the Astro code and content it constrains.

Guidaro is a **separate brand** — a German-language DACH sales-consulting agency — not the Voxera CRM. Its brand lives locally at [`../docs/brand/brand-guidelines.md`](../docs/brand/brand-guidelines.md). Do not reference the Voxera brand here.

**Voxera company strategy, GTM, vision, and workspace-governance decisions do not apply to this site** — they govern the Voxera product, not this separate-brand marketing site. This repo only records ADRs about building and running the Guidaro site.

## Numbering — global + permanent

ADR numbers are **global, unique, and permanent across the whole workspace.** Don't restart numbering per repo; allocate only from the `voxera-os` registry below.

To claim the next number, take the highest ADR number from the **workspace registry** and add 1, then add the new ADR's row to that registry in the same change:

- Workspace registry / number allocator: [`../../voxera-os/decisions/ADR-REGISTRY.md`](../../voxera-os/decisions/ADR-REGISTRY.md).

The registry — not a filesystem glob — is the source of truth for which numbers are taken (some ADRs live in repos this site can't read). Numbers are never reused, never renumbered — even when an ADR is superseded or moves repos.

## ADRs in this repo

| ADR | Title | Status |
|---|---|---|
| _(none yet)_ | | |

## Conventions

- Copy [`_template.md`](./_template.md), set the globally-next number from the registry, name the file `ADR-NNNN-<slug>.md`.
- Add the new ADR's row to both the table above and the workspace registry in the same change.
- Don't delete superseded ADRs — set `status: superseded` and link `superseded_by`.
- `affects:` lists repo-relative paths this decision touches (`src/...`, `astro.config.mjs`, `wrangler.jsonc`, `docs/...`).
- This is a standard, non-confidential marketing-site repo. Keep ADR bodies free of anything that isn't safe to share.
