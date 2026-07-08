# broadstairshedges.co.uk

Local hedge-trimming site for Broadstairs and St Peter's. Part of a 5-site Kent ring (Canterbury / Margate / Broadstairs / Thanet / Deal).

## Stack
Plain static HTML. No framework. No build step.

- `index.html` — homepage
- `services/` — 4 service pages (cutting, reduction, removal, planting)
- `areas/` — coverage index across the 9 CT10 neighbourhoods
- `how-to/` — hand-authored locally-grounded guide articles (organic-draw layer)
- `jobs/` — JSON-driven recent-jobs feed (`jobs.json` is the source of truth; append new entries to the top)
- `assets/css/styles.css` — chalk-white / deep-navy / brass palette + Libre Caslon Text serif typography
- `assets/partials.html` — reusable HTML fragments (header / footer / topbar) for hand-copying into new pages

## Identity
- **Palette:** chalk-white (`#f6f2ea`), warmer cream (`#fbf7ef`), deep navy (`#1e2f45`), brass (`#b58a44`), sea-slate (`#3e5766`).
- **Typography:** Libre Caslon Text (headlines, brand), Source Sans 3 (body, UI).
- **Tone:** literary, considered, quietly established. Dickensian heritage + Folk Week + Blue-Flag Viking Bay + North Foreland lighthouse.

## Deploy
Static HTML deploy — addon domain on Krystal 3dbee cPanel, FTPS via UAPI-provisioned account. Contact form uses `contact-submit.php` -> Resend API.

## GA4
GA4 property ID placeholder: `G-BROADSTAIRS-PLACEHOLDER`. Swap in the real ID post-launch.

## Content sources
Broadstairs research brief — see project handoff (chalk geology, Thanet Coast SSSI, three CAs, coastal species matrix, herring-gull nesting law, neighbourhoods, seasonality).
