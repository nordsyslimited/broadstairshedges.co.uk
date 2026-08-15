# how-to article contract: broadstairshedges.co.uk

This is the authoritative rulebook for every `how-to/*.html` article on this
site, hand-authored or nightly-generated. Read this before writing or editing
any guide. It is derived from the 9 hand-authored articles already live,
match their established conventions exactly unless this file says otherwise.

## 0. This site's structural angle: procedural-first

**broadstairshedges.co.uk is the procedural-first ring site.** Every hedge
ring sister domain (sandwichhedges, canterburyhedges, dealhedges,
doverhedges, margatehedges, ramsgatehedges, thanethedges, winghamhedges)
runs its own distinct macro-structure so Google doesn't see 9 near-identical
article sets across sister domains. This site's assigned angle:

> **Lead with a numbered Steps/Method section, near the top, before any long
> scene-setting.** The reader wants "just tell me what to do" first, context
> second.

Concretely, every new article on this site follows this skeleton:

1. Hero: eyebrow (`.meta` line), `<h1>`, lede paragraph.
2. **A numbered `<h2>Steps</h2>` or `<h2>Method</h2>` section, immediately
   after the lede (no scene-setting section before it).** Use an ordered
   list (`<ol>`) or numbered `<h3>` sub-steps ("Step 1: ...", "Step 2:
   ..."), 4-8 steps. This is the load-bearing difference from
   sandwichhedges' month-by-month / mixed-format house style: **do not**
   open with a "why this matters" or "the situation in [town]" section
   before the steps. Steps come first, every time, no exceptions.
3. The WCA 1981 nesting-season legal callout (`.callout`, see §2), placed
   at the point in the steps where timing/legality actually bites, not
   bolted on as an afterthought at the end. If the topic has no timing
   dimension at all (rare, e.g. a pure "how to choose a species" piece),
   the callout can be omitted, but check first: most hedge-care procedural
   topics touch cutting, and cutting always touches nesting law.
4. After the steps: supporting detail sections as needed (species notes,
   local specifics, what-can-go-wrong): this is where scene-setting and
   colour belong, *after* the reader already has the steps.
5. A CTA `.callout` box back to `contact.html` / phone / email.
6. Related guides: see §5.
7. `Article` JSON-LD (see §3).
8. Sources line (`.muted`, small print) at the foot of `.prose`, matching
   the existing articles' citation-list convention.

Existing hand-authored articles (calendar, herring-gull, high-hedges, etc.)
predate this rule and are NOT retrofitted as part of routine content
ingestion: they stay as-is. The procedural-first rule applies to every
**new** article shipped from this scaffold onward.

## 1. Page shell: match exactly

Copy the `<head>` block structure from any existing article
(`how-to/when-to-cut-hedges-broadstairs-calendar.html` is a clean reference):

- `<meta charset>`, viewport, `og:image` (1200x630, `/assets/img/og-default.png`
  unless a more specific image exists).
- Unique `<title>`: pattern: `{Specific Headline}: {Angle} | Broadstairs
  Hedges & Tree Services`.
- Unique `<meta name="description">`: 1-2 sentences, specific, not
  boilerplate.
- `<link rel="canonical">` to the full article URL.
- `<meta name="robots" content="index,follow">` (add
  `,max-image-preview:large` if the article has real photography, matching
  the calendar article).
- Google Fonts preconnect + `Libre Caslon Text` (display/headings) +
  `Source Sans 3` (body) stylesheet link.
- `<link rel="stylesheet" href="/assets/css/styles.css">`.
- The inline SVG favicon data URI (navy `#1e2f45` background, gold
  `#b58a44` hedge-arc mark): copy verbatim from any existing page, do not
  regenerate.
- GA4: `gtag.js?id=G-MXMY0WM04D`, standard config snippet: copy verbatim.
- `<meta property="og:type" content="article">`, `og:title`, `og:description`,
  `og:url`.
- `theme-color` is **not currently set per-page** on this site (checked:
  absent from article `<head>`s; only appears via CSS var `#1e2f45`
  elsewhere). Do not add a `<meta name="theme-color">` tag as part of
  routine content work; that is a site-wide change, out of scope for a
  content-ingestion run.

Body shell: topbar (phone/WhatsApp/email + pensioner-discount pill), header
nav (Home / Services / Areas / Guides / Recent jobs / About / Contact),
`<article class="guide"><div class="prose">`, footer (`.foot-grid` with
4 columns: business blurb, Services, Local guides, Contact). Copy these
blocks verbatim from an existing article and only change the active nav
state (`class="active"` stays on the Guides `<a>`, not per-article).

## 2. The WCA 1981 nesting-season callout

This site's established legal baseline (from the calendar and herring-gull
articles, cite these facts consistently, do not soften or vary them):

- **Wildlife and Countryside Act 1981, section 1**: offence to intentionally
  kill, injure or take a wild bird, or to intentionally damage/destroy an
  active nest, or disturb a Schedule 1 species. Penalty: unlimited fine per
  offence, potential prison for aggravated offences (the calendar article
  cites "up to six months... and an unlimited fine per nest" for a
  layperson-facing framing; both are used on this site, pick whichever
  reads more naturally in context, don't invent new figures).
- **Practical caution window: 1 March to 31 August.** The herring-gull
  article extends this to 30 September specifically for cliff-edge/gull
  contexts (late broods). Use 1 March-31 August as the default; only extend
  to 30 September when the article is specifically about clifftop/gull
  topics.
- Herring gull: UK Red-Listed since BoCC5 (December 2021); general licences
  for nest disturbance withdrawn; individual Natural England licence now
  required. Only cite this when the topic is genuinely gull/clifftop-related
  Do not bolt it onto an inland or non-timing article.

**Markup:** this site uses one generic callout class, `article.guide
.callout` (CSS at `assets/css/styles.css` ~line 261): there is no
`--warn` or safety-specific modifier class here (unlike sandwichhedges'
`.article-callout--warn`). Use:

```html
<div class="callout">
  <h4>{Short question or statement framing the legal point}</h4>
  <p style="margin: 0;">{2-4 sentences: what the law says, what it means for
  this specific task, what's actually still permitted.}</p>
</div>
```

Match the existing tone: direct, specific, cites the Act and the date
window plainly, never hedges into vague "check local regulations" language.

## 3. JSON-LD: match the established per-article pattern exactly

Every existing article uses a **single flat `Article` object**, not the
`@graph` pattern (the hub page `how-to/index.html` uses `@graph` with
`CollectionPage` + `BreadcrumbList`: that is the hub's own pattern, not the
article pattern; do not copy it onto article pages):

```html
<script type="application/ld+json">
{"@context":"https://schema.org","@type":"Article","headline":"{headline}","description":"{meta description text}","author":{"@type":"Person","name":"Richard Lim"},"publisher":{"@type":"Organization","name":"Broadstairs Hedges & Tree Services"},"datePublished":"{YYYY-MM-DD}","inLanguage":"en-GB","mainEntityOfPage":"{full canonical URL}"}
</script>
```

No `BreadcrumbList` and no `FAQPage` on article pages currently. This is a
known gap versus the hub page and versus sandwichhedges' richer per-article
`@graph`, flagged here as a future enhancement, **not** something to
silently fix inside a routine content-ingestion run. If Richard or Jet
later decides to upgrade article-level schema, that's a deliberate,
site-wide template change applied to all 9+N articles at once, not
something one nightly run should introduce inconsistently.

## 4. Voice and copy rules

- UK English throughout (colour, organisation, metres). Kent/Thanet
  place-specificity earns its place: Broadstairs, St Peter's, Reading
  Street, Stone Bay, Joss Bay, Kingsgate, North Foreland, Viking Bay, the
  three conservation areas (Broadstairs CA / St Peter's CA / Reading
  Street CA), Thanet District Council, Margate Chalk.
- First-person working-contractor voice ("I cut hedges", "I quote fixed
  price"): the existing articles and about.html are written as Richard
  Lim, sole operator. Keep that voice; do not switch to "we" or a faceless
  brand voice.
- **No em-dashes.** Commas, full stops, colons or parentheses instead.
  Hyphens in compound words and en-dashes in ranges (e.g. "1 March-31
  August") are fine, and match existing article usage.
- No corporate filler: utilise, leverage, seamlessly, best-in-class,
  synergy, robust, cutting-edge, "elevate your outdoor space".
- Specific numbers over vague claims: exact fees (£650 TDC complaint fee,
  £20,000-per-tree conservation fine), exact hectares/dates for
  designations, exact rainfall/sunshine figures where relevant. This site
  leans on precise, sourced detail as its credibility signal.
- Dates as "22 April 2026" or "late August", matching existing usage.
- Sources line at the foot of every article (`<p class="muted"
  style="font-size: 0.9rem; margin-top: 3rem;">Sources: ...</p>`): list
  the Acts, designations and reports actually cited in the piece.

## 5. Related guides

This site does **not** currently have a `JET-RELATED-GUIDES` marker block
inside article bodies (checked all 9, none present); interlinking
currently happens only via the footer's static "Local guides" column
(3 hardcoded links, same on every page) and the hub grid.

For nightly-generated articles, add a simple related-guides block **inside
the article body**, after the last content section and before the closing
CTA callout, using this marker convention so future runs can find and
update it (this establishes the pattern going forward; do not attempt to
retrofit the 9 existing hand-authored articles as part of a content run):

```html
<!-- BEGIN JET-RELATED-GUIDES -->
<div class="callout">
  <h4>Related guides</h4>
  <ul style="margin: 0.5rem 0 0;">
    <li><a href="/how-to/{slug}.html">{Title}</a></li>
    <li><a href="/how-to/{slug}.html">{Title}</a></li>
  </ul>
</div>
<!-- END JET-RELATED-GUIDES -->
```

2-4 genuinely relevant links (topic or geographic overlap), pulled from
existing `how-to/` articles. When a new article ships, check whether any
existing `JET-RELATED-GUIDES` block (once any exist) should link to it too,
and add it if genuinely relevant. Cap at 4 links per block, drop the
least-relevant existing link if adding a new one would exceed that.

## 6. Hub page and sitemap updates

- Add a new `.card` to the `grid-2` grid in `how-to/index.html` (see
  existing cards for the pattern: `.section-eyebrow` category label,
  `<h3><a class="card-link">`, one-sentence description, "Read the guide"
  link). This site has **no `data-howto-cat` filter system** (unlike
  sandwichhedges): cards are a flat grid, no client-side filtering to keep
  in sync.
- Update `how-to/index.html`'s `@graph` JSON-LD only if the hub's own
  metadata changes (it normally won't for a routine article addition,
  the `CollectionPage`/`BreadcrumbList` block describes the hub itself,
  not its contents).
- Add the new URL to `sitemap.xml` with `priority>0.7` matching the other
  how-to entries, positioned alongside the other `/how-to/` URLs.
- Update the footer's "Local guides" 3-link list on the new article's own
  page and, optionally, on 1-2 highly relevant existing pages if the new
  article is a clearly better fit than what's currently linked, but this
  is a nice-to-have, not required every run (the footer list is
  site-static by design, don't attempt to rotate it wholesale).

## 7. Category taxonomy (for topic-gap tracking, not filter UI)

No UI filter exists, but the nightly finder should still track coverage
against a working category list so it doesn't produce near-duplicate
topics. See `config/ingestion.json` → `ingestion.categories` for the
current list. Existing 9 articles roughly map to: seasonality/timing,
wildlife & the law, conservation-area rules, coastal species/planting,
neighbour law, clifftop reduction, holiday-let kerb-appeal. New topics
should fill a genuine gap in that list, expressed with a procedural
"how do I actually do X" framing given this site's structural angle.
