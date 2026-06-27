# canterburyhedges.co.uk

Local hedge-trimming site for Canterbury and surrounding villages. First of a 5-site Kent ring (Canterbury / Margate / Broadstairs / Thanet / Deal).

## Stack
Plain static HTML. No framework. No build step.

- `index.html` — homepage
- `services/` — 4 service pages (cutting, reduction, removal, planting)
- `areas/` — coverage map + per-neighbourhood landing pages
- `how-to/` — 15-25 hand-authored locally-grounded guide articles (organic-draw layer)
- `jobs/` — JSON-driven recent-jobs feed (`jobs.json` is the source of truth; append new entries to the top)
- `assets/css/styles.css` — Cathedral-stone palette + Cormorant Garamond serif typography
- `assets/partials.html` — reusable HTML fragments (header / footer / topbar) for hand-copying into new pages

## Identity
- **Palette:** Cathedral stone (`#d4c4a8`), cream (`#f5efe2`), deep moss (`#1d2a1f`), rust accent (`#8b4a2b`).
- **Typography:** Cormorant Garamond (headlines, brand), Inter (body, UI).
- **Tone:** authoritative, heritage-aware, warm. Lean into the city's age and gravitas.

## Deploy
TBC — addon domain in 3dbee cPanel, FTPS via UAPI-provisioned account.

## Content sources
Local research brief at `../ClaudeAgent/claudejet/scratchpad/hedge-research/canterbury.md` (chalk geology, CA rules, species, Blean woods/dormice, neighbourhoods, seasonality).
