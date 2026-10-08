# Broadstairs Hedges nightly ingestion — architecture and operational reference

## Analytics, fonts and cookies (do not undo)

- This site has NO Google Analytics, NO gtag or dataLayer snippet, NO Google Fonts links and NO cookie banner or cookie-consent script. They were removed deliberately on 8 October 2026. Do not add any of them back, on any page, template, generator or sample snippet, and do not add any other third-party script, font, image host or tracker.
- Visit counting is NordAnalytics (first-party, cookieless). Every page loads `<script src="/assets/js/analytics.js?v=20261008" defer></script>` as the last element before `</body>`. That one file adds the footer links ("How we count visits", "Don't count my visits") and loads the counter. Copy the head, the footer and that script line from an existing page unchanged. Every new page must include it (redirect stubs with a meta refresh excepted).
- Fonts are self-hosted: pages link `<link rel="stylesheet" href="/assets/fonts.css">` where a Google Fonts link used to be. Copy it from an existing page.
- Audits: do NOT flag the absence of Google Analytics, a GA4 ID, gtag, consent mode or a cookie banner, and do NOT restore any of them. Older reports and notes that mention GA4 are out of date. DO flag and remove any page containing googletagmanager.com, gtag(, dataLayer, fonts.googleapis.com, fonts.gstatic.com or cookie-consent, and flag any non-redirect page missing the analytics.js script line.
- `/privacy.html` carries the approved "Website statistics" section (id `website-statistics`). Keep its wording unchanged.

**Site:** broadstairshedges.co.uk
**Pipeline shape:** SE-shape — Anthropic cloud routine (claude.ai/code scheduled agent), same shape as sandwichhedges.co.uk and sandwichelectrical.co.uk.
**Owner:** Richard (claude.ai/code account holder)
**Scaffolded:** 2026-08-12 by Jet, ring-wide rollout following the sandwichhedges.co.uk reference implementation, adapted to this site's own established conventions and its assigned structural angle (procedural-first — see `TEMPLATES/how-to.md` §0).

## What this pipeline is

An Anthropic-hosted scheduled Claude Code routine that runs nightly on
Anthropic's cloud infrastructure. Each run: discovers an uncovered
Broadstairs/Thanet hedge-care topic + a suitable UK YouTube video (or other
web source), generates a full **procedural-first** how-to article matching
`TEMPLATES/how-to.md`, commits it to this repo, and pushes to `main`. The
repo's own `.github/workflows/deploy.yml` GH Action then FTPS-deploys to
Krystal — **this repo already has working CI**, unlike a from-scratch
scaffold; no deploy-pipeline work is needed alongside this content
scaffold.

This repo had **no** content pipeline before 2026-08-12 — every existing
`how-to/*.html` article (9 of them) was hand-authored by Jet, with no
client-side category filter on the hub page (flat grid, no
`data-howto-cat` system — different from sandwichhedges).

**This file documents the pipeline shape. The routine itself (cron, prompt,
schedule) is created and owned separately on Richard's claude.ai/code
account — that setup is out of scope for this scaffolding commit.**

## How it works — end-to-end flow (per the SE-shape pattern)

### Stage 1 — trigger
- Scheduled cron on Richard's claude.ai/code account fires the routine.
- The routine spins up a fresh Claude Code session on Anthropic's infra.

### Stage 2 — topic discovery (in-session)
- Session reads the existing `how-to/` folder to identify covered vs
  uncovered topics against the category list in `config/ingestion.json`.
- Uses web search / WebFetch to find a UK hedge-care/gardening YouTube
  video (or written source) on a plausible uncovered topic — bias toward
  content with a genuine step-by-step/procedural shape, since this site's
  house style leads with numbered steps (a video that's mostly narrative
  scene-setting is a worse fit here than for other ring sites).
- Constraints: UK-based creator/source, region UK, qualifying content type
  (not shorts, not compilations). Like-count / subscriber-count used as an
  authority heuristic since the YouTube Data API isn't used (egress to
  youtube.com is often blocked from Anthropic infra — falls back to
  channel-authority heuristic via web search, same as SE and sandwichhedges).

### Stage 3 — verification
- Session confirms the source and creator/channel exist and are UK-relevant
  via web search cross-references.
- Records a note about verifiability in the run report.

### Stage 4 — article generation
- Session writes a full HTML article per `TEMPLATES/how-to.md` — the
  authoritative contract for this repo. Key shape, in order:
  1. Hero + lede.
  2. **Numbered Steps/Method section immediately after the lede** — this
     site's non-negotiable structural angle, no scene-setting before it.
  3. The WCA 1981 nesting-season `.callout` (generic class, no `--warn`
     modifier on this site) wherever timing/legality bites, placed inline
     at the relevant step, not bolted on as an afterthought.
  4. Supporting detail sections (species, local specifics, pitfalls) after
     the steps.
  5. CTA `.callout` back to `contact.html` / phone / email.
  6. `JET-RELATED-GUIDES` block (2-4 links) — see `TEMPLATES/how-to.md` §5;
     this is a new convention for this site (no existing article has one),
     established starting with the first nightly-generated article.
  7. Flat `Article` JSON-LD (not `@graph`, no `BreadcrumbList` — matches
     this site's existing per-article pattern exactly, see
     `TEMPLATES/how-to.md` §3).
  8. Sources line.
- Rotates specific procedural sub-format within the "steps-first" umbrella
  (e.g. straight numbered list vs numbered `<h3>` sub-steps vs
  decision-checklist-then-steps) so articles don't read as
  fingerprintably identical to each other, while keeping the ring-wide
  differentiator (procedural-first) constant across all of this site's
  articles.

### Stage 5 — site updates
- Session updates `how-to/index.html` — adds a new `.card` to the flat
  `grid-2` grid (no filter-tab bookkeeping needed, unlike sandwichhedges).
- Adds/updates `JET-RELATED-GUIDES` blocks on 2-4 existing or
  previously-generated articles genuinely relevant to the new one, once
  that convention has at least one article to link to.
- Adds the new URL to `sitemap.xml` (`priority` 0.7, alongside the other
  `/how-to/` entries).
- Footer's static "Local guides" 3-link list is site-wide and not rotated
  automatically — leave it alone unless a new article is a clearly better
  fit than a currently-linked one (rare, judgement call, not required
  every run).

### Stage 6 — self-audit report
- Session writes `reports/YYYY-MM-DD-content.md` documenting: URLs
  shipped, sources used, creator/authority verification, topic-gap check
  evidence, Broadstairs/Thanet-specificity notes, confirmation the
  procedural-first structure was followed, files modified.

### Stage 7 — commit + push
- Session commits with a `Nightly: add N how-to articles (D Month YYYY)`
  message body listing the shipped articles.
- Includes a `Claude-Session:` trailer for provenance (per SE convention).
- Pushes to `main`.

### Stage 8 — deploy
- Repo's `.github/workflows/deploy.yml` fires on push, FTPS-uploads
  changed files to Krystal via `SamKirkland/FTP-Deploy-Action@v4.3.5`. No
  changes needed to this workflow as part of content scaffolding.

## Morning audit (second scheduled run)

A second nightly/morning routine audits the full site: broken links, dead
video embeds, SEO completeness (title/description/canonical/OG/JSON-LD
present on every page), procedural-first structural compliance on how-to
articles (steps section present and positioned near the top, not buried),
and general relevance. Writes `reports/YYYY-MM-DD-audit.md`.

## Configuration surface

Most config lives inside the routine's prompt on Richard's claude.ai/code
account (opaque from this repo), except the two disciplines every
SE-shape routine adopts:

| What | Where |
|---|---|
| Discovery + verification prompt | Routine prompt on claude.ai/code (opaque from repo) |
| Article template rules (incl. procedural-first angle) | `TEMPLATES/how-to.md` — routine should read at run start |
| Category list, cadence, schedule | `config/ingestion.json` |
| Site palette + brand voice | `AGENTS.md` + rendered examples in `how-to/` |
| Deploy target | `.github/workflows/deploy.yml` (already working) + FTPS repo secrets |
| Cron schedule | claude.ai/code routine schedule |

## Cost model

- **Claude usage:** counts against Richard's claude.ai/code allowance.
- **Web search / WebFetch:** included in claude.ai/code.
- **YouTube:** used only via WebFetch (no Data API key). Egress-proxy 403
  is common — routine falls back to channel-authority heuristic.
- **FTPS / hosting:** covered by Krystal LiteSpeed hosting plan.

No YouTube Data API quota to babysit. No local infrastructure dependency.

## Failure modes and resilience

| Symptom | Root cause | Resilience |
|---|---|---|
| YouTube egress blocked (HTTP 403) | Anthropic infra egress policy on youtube.com | Routine falls back to channel-authority heuristic via web search — documented in every run report |
| Fewer than 3 articles shipped | Third topic candidate not verifiable within session | Routine ships what it can verify, reports the shortfall |
| Anthropic infra outage | Anthropic side | Manual re-run via routine dashboard, or wait for next scheduled run |
| New article drifts to scene-setting-first structure | Model defaults to narrative opening instead of following §0 | Morning audit checks steps-section position; fix is a structural edit, flag in report if it recurs |
| New article breaks hub grid layout | Malformed `.card` markup copy-paste | Morning audit checks rendered hub page |

## Ownership

- **Routine config (prompt, cron, model):** Richard (his claude.ai/code account)
- **Template contract file:** Jet (site fleet lane) — PRs land in this repo
- **Site content review / audits:** Jet
- **Infra (deploy pipeline, hosting):** Webster
- **Owner-of-record for the site:** Richard

## Related files

- Site repo: `E:/Ai/Codex/broadstairshedges.co.uk/`
- Template contract: `TEMPLATES/how-to.md`
- Config: `config/ingestion.json`
- Ring reference implementation (do not copy verbatim — different
  structural angle): `E:/Ai/Codex/sandwichhedges.co.uk/INGESTION.md`,
  `E:/Ai/Codex/sandwichhedges.co.uk/TEMPLATES/how-to.md`
- SE-shape origin reference: `E:/Ai/Codex/sandwichelectrical.co.uk/INGESTION.md`
- Nightly self-audits (once live): `reports/YYYY-MM-DD-content.md` /
  `reports/YYYY-MM-DD-audit.md` in this repo
