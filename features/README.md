# Features

Feature work for the **Guidaro** site, folder-per-feature (the workspace standard for code repos).
The lifecycle below keeps this tree from becoming a bag of files.

## Layout

| Path | What |
|---|---|
| `_template/` | The spec bundle to copy for a new feature (`spec.md`; add `status.json` when a run starts). |
| `FEAT-NNN-slug/` | An **active** feature (German-site landing pages, contact flow, etc.). |
| `_archive/FEAT-NNN-slug/` | A **DONE** feature, harvested + slimmed to `spec.md` + `outcome.md`. |

## Lifecycle (anti-rot)

1. Copy `_template/` → `features/FEAT-NNN-slug/`; write `spec.md` (acceptance criteria, scope). Keep copy German.
2. Build via `implement-feature` / fix via `fix-bug` (from `../voxera-os/.a5c/processes/`).
3. **On DONE, harvest:** run the shared anti-rot process so durable knowledge moves UP and the folder is archived slimmed:
   ```sh
   /babysitter:call --process ../voxera-os/.a5c/processes/harvest-feature.js#process -- feature features/FEAT-NNN-slug
   ```
   Patterns → `engineering-os/patterns/`, lessons → `engineering-os/learning/lessons-log.md`, decisions → a PROPOSED ADR in `decisions/`; the folder moves to `_archive/` slimmed.

**Rule:** a DONE feature does not stay in the active tree unharvested.
