# Enchanted Saju Journal — Future AI File Map

Send only the smallest relevant set whenever possible:

| What you want changed | Files to send first |
|---|---|
| Colours, typography, decoration, responsive layout | `public/css/styles.css` |
| Page/form/container structure | `public/index.html` + the relevant feature JS |
| Four Pillars, solar-time, day/hour, Major Luck calculations | `public/js/calculator.js`, `public/data/saju-data.js`, and if boundary-related `public/data/solar-terms.js` |
| New Chart form or chart-result rendering | `public/js/calculator-ui.js` |
| Today / natal or daily readings / reading levels | `public/js/readings.js` |
| People, profiles, Active Profile, My Saju | `public/js/profiles.js` |
| Luck Calendar | `public/js/calendar.js` |
| Journal, tags, history, patterns | `public/js/journal.js` |
| Life Timeline | `public/js/timeline.js` |
| Compare Charts | `public/js/compare.js` |
| Research Library/import/retry/provenance | `public/js/research.js` |
| Learn/glossary/aliases | `public/js/glossary.js`, `public/data/glossary.js` |
| JewelSJPJ/SJPJ type names and metadata | `public/data/sjpj-types.js`, `public/data/jiazi.js`, `public/js/jiazi.js` |
| Exact 60 avatar art | the relevant folder(s) under `public/assets/images/avatars/` |
| Cloudflare ↔ Notion browser calls | `public/js/notion.js` |
| Browser persistence keys/helpers | `public/js/storage.js` |
| Navigation/shared UI | `public/js/ui.js`, `public/js/app.js` |
| Calculation Inspector/reference comparison | `public/js/audit.js` |
| Server routes, Notion credentials/bindings, backend behavior | relevant `src/` file(s) and `wrangler.toml`; do **not** move secrets into `public/` |

For broad changes that cross features, also include `public/js/interfaces.js` so the AI can see the public module boundaries.
