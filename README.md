# Enchanted Saju Journal — Modular Frontend

This build is a structural refactor of the supplied Enchanted Saju Journal project. It preserves the existing Cloudflare Worker (`src/`), deterministic calculation rules, UI content, storage keys, routes, terminology and visual design while moving the large frontend monolith into focused files.

## Project structure

```text
public/
  index.html                  semantic page/form/container structure
  css/styles.css              complete visual system
  data/solar-terms.js         Swiss solar-term + equation-of-time reference data
  data/saju-data.js           stems, branches, Ten Gods and rule tables
  data/jiazi.js               canonical 60 Jiazi index
  data/glossary.js            static Learn group/query mappings
  data/sjpj-types.js          JewelSJPJ names + type art metadata
  js/state.js                 shared runtime state only
  js/storage.js               browser-storage helpers and key registry
  js/ui.js                    shared DOM/navigation/art helpers
  js/calculator.js            deterministic Four Pillars calculation engine
  js/calculator-ui.js         New Chart form + deterministic result rendering
  js/notion.js                Cloudflare/Notion client bridge
  js/readings.js              daily/natal interpretation presentation
  js/profiles.js              People, active profile and My Saju views
  js/compare.js               structural chart comparison
  js/calendar.js              Luck Calendar / annual / monthly views
  js/journal.js               journal, tags, history and patterns
  js/timeline.js              Life Timeline and events
  js/jiazi.js                 60 Jiazi reference cabinet
  js/research.js              Research Inbox/Library/import queue/provenance
  js/glossary.js              concept search, aliases and Learn views
  js/audit.js                 inspector, reference comparison and notes
  js/interfaces.js            public EnchantedSaju.modules compatibility interface
  js/app.js                   small application entry point
  assets/images/avatars/      exact existing 60 SJPJ/Jiazi art folders
  assets/images/decorative/   fairy ring, botanical, lace and divider SVGs
  assets/images/backgrounds/  paper-noise SVG extracted from the old CSS data URL
src/                            existing Cloudflare Worker/backend (preserved)
tests/                          existing backend/regression tests
```

### Frontend module model

The refactor intentionally keeps the frontend scripts as **ordered browser modules (classic external scripts)** rather than converting the whole application to ESM in the same pass. The former monolith relied on shared lexical state and many existing event-handler references. Keeping ordered files preserves that behaviour while making each feature independently inspectable and editable. Shared state/storage helpers remain migration-safe, while `js/interfaces.js` exposes an explicit `globalThis.EnchantedSaju.modules` interface for future feature work. Feature files are otherwise kept focused.

This is a migration-safety decision, not a return to a monolith: no application JavaScript remains embedded in `index.html`, and there is no giant replacement `script.js`.

## Deterministic vs interpretation

- **Deterministic calculation:** `public/js/calculator.js`, stable tables in `public/data/`.
- **Calculation presentation/audit:** `public/js/calculator-ui.js`, `public/js/audit.js`.
- **AI/source-based interpretation:** `public/js/readings.js` and backend routes in `src/`. Interpretations consume saved calculation data and do not mutate it.
- **Research provenance:** `public/js/research.js` plus existing Worker routes.

## Cloudflare / Notion boundary

`public/js/notion.js` contains only browser-safe calls to the existing `/api/*` routes and the user-entered private connection key. Notion integration tokens and other server credentials remain server-side in Cloudflare bindings/environment variables. The existing Worker entry point remains `src/index.js` via `wrangler.toml`.

## Run and deploy

```bash
npm install
npm test
npm run dev
# deploy when ready
npm run deploy
```

Wrangler serves `public/` as static assets and runs the Worker first for `/api/*`, matching the existing deployment configuration.

# WHERE TO MAKE FUTURE CHANGES

| Change | Usually send/edit |
|---|---|
| Site colours, layout, scrapbook/fairy/celestial decoration | `public/css/styles.css` |
| Page structure/forms/containers | `public/index.html` + relevant feature JS |
| Four Pillars / true solar time / day / hour / Major Luck calculations | `public/js/calculator.js` + `public/data/solar-terms.js` / `public/data/saju-data.js` |
| New Chart form/result presentation | `public/js/calculator-ui.js` |
| 60 SJPJ terminology/type metadata | `public/data/sjpj-types.js`, `public/data/jiazi.js`, `public/js/jiazi.js` |
| 60 avatar artwork | `public/assets/images/avatars/` |
| Today / generated readings / reading levels | `public/js/readings.js` |
| People / Active Profile / My Saju | `public/js/profiles.js` |
| Luck Calendar | `public/js/calendar.js` |
| Journal / patterns / history | `public/js/journal.js` |
| Life Timeline | `public/js/timeline.js` |
| Compare Charts | `public/js/compare.js` |
| Learn / searchable terminology / aliases | `public/js/glossary.js` + `public/data/glossary.js` |
| Research Library / imports / retry/cooldown / provenance | `public/js/research.js` |
| Notion/Cloudflare browser connection | `public/js/notion.js` |
| Calculation Inspector / reference testing | `public/js/audit.js` |
| Shared localStorage behaviour | `public/js/storage.js` |
| App navigation/startup | `public/js/ui.js`, `public/js/app.js` |
| Worker endpoints/server secrets | `src/index.js` and existing `src/` backend modules; never frontend JS |

## Migration note

The supplied monolithic frontend referenced a `renderPersonalNotes(...)` helper in concept/chart-note views but did not define it in the frontend or phase browser files. The modular build adds a minimal renderer for the **existing backend note shape only** so those existing note views do not throw a `ReferenceError`. No calculation or interpretation rule was added.
