# Agent / Contributor Notes — broadstairshedges.co.uk

Ground rules for any future AI assistant or human contributor working on
this site. This site is one of 9 sister domains in the NordSys "hedge ring"
(broadstairs / canterbury / deal / dover / margate / ramsgate / thanet /
wingham / sandwich hedges) — same business model and layout conventions,
different towns, deliberately different content angles per site (see
`TEMPLATES/how-to.md` §0).

## Stack

- Plain HTML, one shared `assets/css/styles.css`, one shared
  `assets/js/main.js`. **No build step.**
- Deployed via GitHub Actions FTPS to Krystal shared hosting — **this repo
  already has a working CI pipeline**, `.github/workflows/deploy.yml`,
  triggered on push to `main`. Uploads everything except `.git*`,
  `node_modules`, `.DS_Store`, `README.md`, `assets/partials.html` to
  `p125.lon.krystal.io` via FTPS (explicit AUTH TLS, port 21) using GitHub
  repo secrets `FTP_USER` / `FTP_PASS`. **No manual FTPS deploy needed** for
  routine content changes on `main` — push and the Action ships it.
  - Local FTP credential reference (for manual/emergency use only, e.g. CI
    outage): `~/.claude/secrets/broadstairshedges-ftp.env` — host
    `p125.lon.krystal.io`, user `deploy@broadstairshedges.co.uk`, port 21,
    remote dir `/`. Do not paste the password into any chat channel.
- Contact form: `contact-submit.php` (server-side PHP, not FormSubmit.co —
  different from the sandwichhedges reference site). Don't change the
  contact pipeline as part of content-ingestion work.

## Branding

- Palette: navy `#1e2f45`, gold accent `#b58a44` / `#c9a961`, sand/linen
  neutrals. Favicon: inline SVG data URI, navy circle with a gold hedge-arc
  mark — copy verbatim, don't regenerate.
- Fonts: Libre Caslon Text (display/headings) + Source Sans 3 (body), both
  via Google Fonts.
- Business identity: "Broadstairs Hedges & Tree Services", run by "Richard
  Lim" (first-person sole-operator persona used throughout site copy —
  about.html, article bylines). Tagline: "The considered clip. A proper
  job, or you pay nothing." Topbar line: "Considered work above Viking
  Bay." 5%-off-for-pensioners pill in the topbar.
- Contact: phone `07763 100 477` (`tel:+447763100477`), WhatsApp same
  number with a pre-filled `wa.me` link, email `hello@broadstairshedges.co.uk`.
  Area: Broadstairs, St Peter's, the Thanet clifftop bays (Stone Bay, Joss
  Bay, Kingsgate, North Foreland), CT10.
- Sister sites in the ring share layout DNA but each has its own palette
  and voice — do not clone another ring site's look here.

## Writing style

- UK English (colour, organisation, whilst, metres). Precise local detail
  earns its place: the three conservation areas (Broadstairs CA / St
  Peter's CA / Reading Street CA), s.211 six-week notice, Thanet Coast
  SSSI / SPA / Ramsar boundary, Margate Chalk, herring-gull Red List
  status, Dickens Festival, August bank holiday, holiday-let seasonality.
- First-person working-contractor voice ("I cut hedges", "I quote fixed
  price") — not a faceless brand voice, not "we".
- **No em-dashes.** Commas, full stops, colons or parentheses instead.
  Hyphens in compound words and en-dashes in ranges are fine.
- No corporate filler: utilise, leverage, seamlessly, best-in-class,
  synergy, robust, cutting-edge.
- Dates as "22 April 2026" or "late August".
- Lean on specific, sourced numbers (fees, hectares, dates, rainfall
  figures) — that precision is this site's credibility signal, see any
  existing `how-to/` article for the pattern.

## SEO and AI search baked in

Every page must carry:

- Unique `<title>`, `<meta name="description">`, `<link rel="canonical">`.
- Open Graph (`og:image` 1200x630, `og:type`, `og:title`, `og:description`,
  `og:url`) + the standard GA4 snippet (`G-MXMY0WM04D`).
- `<html lang="en-GB">`.
- `<meta name="robots" content="index,follow">` (add
  `,max-image-preview:large` where the page has real photography).
- JSON-LD: hub pages use the `@graph` pattern (`CollectionPage` +
  `BreadcrumbList`); individual article and content pages use a flat
  single-object schema (`Article` for how-to guides) — see
  `TEMPLATES/how-to.md` §3 for the exact article pattern, do not mix the
  two conventions on one page type.
- `sitemap.xml` updated whenever a page is added or removed — no build
  step does this automatically, it's a hand-maintained (or
  ingestion-routine-maintained) flat file.
- `robots.txt` — check it allows the standard AI-crawler set (GPTBot,
  ChatGPT-User, PerplexityBot, ClaudeBot, Google-Extended, CCBot,
  Applebot, Bingbot) before assuming it needs a fix; don't touch it as
  part of routine content work without checking current state first.

## How-to pattern

See `TEMPLATES/how-to.md` for the full article contract, including this
site's assigned ring-wide structural angle: **procedural-first** — every
new how-to article leads with a numbered Steps/Method section near the
top, before any scene-setting. This is what differentiates this site's
article set from the 8 sister ring domains for SEO purposes; don't drift
back toward a scene-setting-first or pure-narrative structure on new
articles.

## What not to do

- No frameworks (React, Vue, Tailwind, Next, etc.).
- No build step. No npm dependencies.
- No tracking scripts beyond the existing GA4 tag without asking first.
- No third-party chat widgets.
- Do not clone the visual style or article structure of a sister ring
  site (sandwichhedges, canterburyhedges, etc.) — each site's
  differentiated structural angle is a deliberate SEO decision, not an
  oversight to "fix" into consistency.
- Do not add author bylines beyond the existing "Richard Lim" persona
  already in use across the site.
- Do not silently upgrade article-level JSON-LD to the `@graph`/
  `BreadcrumbList` pattern as a side effect of a content-ingestion run —
  that's a deliberate, site-wide template change, tracked as a known gap
  in `TEMPLATES/how-to.md` §3, not something to fix article-by-article.
- Do not invent a `data-howto-cat` filter system for this site — it
  doesn't have one (unlike sandwichhedges), and the hub is a flat grid by
  design.

## Content pipeline

See `INGESTION.md` for the nightly content-ingestion pipeline shape,
`config/ingestion.json` for topic/category config, and
`TEMPLATES/how-to.md` for the article contract the routine must follow.

## Ownership

- **Template contract, site content, PRs:** Jet (site fleet lane).
- **Routine config (prompt, cron, model), if/when scheduled:** Richard,
  set up separately from this repo, same shape as sandwichhedges.
- **Infra (deploy pipeline, hosting, DNS):** Webster.
- **Owner-of-record:** Richard / NordSys.
