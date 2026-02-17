# Aviary — Project Roadmap

> A beautiful, free birding field guide with animated migration maps.
> Built slowly, built well. This document is the north star.

---

## How to Read This Document

**Major phases** are shippable milestones — something real you can share, use, and be proud of.
**Minor phases** are one-session bodies of work — scoped to what Claude can hold in context and execute end-to-end.

Each phase includes:
- **Goal** — What it is and why it matters
- **Work** — What gets built
- **Setup** — Prerequisites and environment before starting
- **Validation** — How you know it's done right
- **Deploy Notes** — What changes in production

Sessions are intentionally small. This is a passion project. Gaps between sessions are fine — each phase is self-contained enough to resume cold.

---

## Current State

**Zero implementation code.** Four complete design documents. Vision is clear, stack is decided, first 20 species are chosen. Ready to build.

---

---

# PHASE 1 — Proof of Beauty

> **Ship goal:** A real, deployed website with one page that makes someone stop scrolling.
> The Migration Map working for 5 species. Nothing else. Just that.

---

## 1.1 — Project Scaffold

**Goal:** A running Next.js app with the right bones — fonts, colors, structure — that looks like Aviary before a single real feature exists.

**Work:**
- Init Next.js 14 app with App Router and TypeScript
- Install and configure Tailwind CSS with custom theme tokens (cream, forest, sepia, rust, sky, gold)
- Add Google Fonts: Cormorant Garamond + Crimson Pro
- Set up global CSS baseline (body background, link styles, text rendering)
- Create root layout with `<html>` font classes applied
- Create placeholder `page.tsx` with a centered "Aviary" wordmark
- Configure `next.config.js` for image optimization
- Init git, push to GitHub

**Setup:**
- Node 20+ installed (`node -v`)
- `npx create-next-app@latest aviary` with TypeScript + Tailwind selected
- Mapbox token ready (free account at mapbox.com → copy default public token)

**Validation:**
- `npm run dev` opens to a cream background with "Aviary" in Cormorant Garamond
- No console errors
- Tailwind custom colors resolve (test with a `bg-[#FAF7F0]` class)
- TypeScript compiles clean

**Deploy Notes:**
- Connect GitHub repo to Vercel on first push
- Set `NEXT_PUBLIC_MAPBOX_TOKEN` environment variable in Vercel dashboard
- Live URL confirmed working

---

## 1.2 — Migration Map Shell

**Goal:** Mapbox map fills the screen with a custom vintage base style. No data yet — just the canvas.

**Work:**
- Install `mapbox-gl` and `@types/mapbox-gl`
- Create `MigrationMap.tsx` client component
- Full-screen map (100vh, no margins) with Mapbox GL initialized
- Apply a custom Mapbox Studio style: muted terrain, cream land, no labels except country names, sepia water
- Disable scroll zoom on mobile (pinch only)
- Map attribution styled to match theme (small, sepia-colored, bottom-right)
- Landing page renders `<MigrationMap />` with a loading skeleton during hydration

**Setup:**
- Mapbox Studio account → duplicate "Monochrome" style → adjust colors to match palette → publish → copy Style URL
- `NEXT_PUBLIC_MAPBOX_TOKEN` in local `.env.local`

**Validation:**
- Map renders full-screen on desktop and mobile
- No Mapbox watermark overlap issues
- Vintage color feel — not default blue Mapbox
- Smooth initial load, no flash of default style

**Deploy Notes:**
- Style URL added as `NEXT_PUBLIC_MAPBOX_STYLE` env var in Vercel

---

## 1.3 — Time Slider + Static Migration Paths

**Goal:** 5 manually-defined migration paths animate on the map. The time slider moves them through the year. This is the emotional core of the whole project — validate it here.

**Work:**
- Create `TimeSlider.tsx` — horizontal slider Jan–Dec, draggable, snaps to weeks
- Define 5 migration path datasets as static JSON (hardcoded in `/public/data/migrations/showcase.json`):
  - Arctic Tern (pole-to-pole)
  - Bar-tailed Godwit (Alaska → New Zealand nonstop)
  - Ruby-throated Hummingbird (Gulf crossing)
  - American Golden-Plover (Atlantic ocean crossing)
  - Barn Swallow (North America ↔ South America)
- Each path: array of `[lng, lat]` waypoints + `activeWeeks: [1..52]` array + color + label
- Render paths as animated Mapbox line layers — Framer Motion drives opacity based on current week
- Pulse animation on breeding/wintering endpoints when bird is "there"
- Month label displayed in bottom-left (not week number — more human)
- Auto-play on load (slow, ~4s per month cycle), pause on user interaction

**Setup:**
- Research each species' rough route waypoints (Wikipedia migration section is fine for these 5)
- Framer Motion installed (`npm install framer-motion`)

**Validation:**
- Time slider drags smoothly, no jank
- Paths appear/disappear based on season (tern not visible over North America in winter)
- Auto-play advances without stuttering
- Works on iPhone Safari (the hard one)
- Someone who doesn't know the project understands what they're looking at

**Deploy Notes:**
- Static JSON shipped with the build, no API calls
- Confirm Vercel build completes under 60s

---

## 1.4 — Map UI Layer

**Goal:** The map feels like a product, not a prototype. Search bar, minimal nav, and info panel when a path is clicked.

**Work:**
- Overlay: search bar (non-functional, placeholder text "Search a bird or place...") centered at top
- Overlay: "Aviary" wordmark top-left, links to about page (not built yet, just a route)
- Clicking a migration path opens a minimal info panel (slide up from bottom on mobile, appear right on desktop):
  - Species name (common + scientific)
  - One-line migration fact (hand-written, hardcoded for these 5)
  - "See full profile →" link (goes nowhere yet)
- Panel closes on map click-away
- Month narrative text: bottom of screen, italic, 1 sentence about what's happening this month (12 hardcoded strings per species, shown for active species)

**Setup:**
- None beyond 1.3

**Validation:**
- Panel opens/closes without layout shift
- Map doesn't jump when panel opens on mobile
- The one-sentence narratives feel evocative, not encyclopedic
- Looks good in dark mode (Tailwind `dark:` classes on overlays)

**Deploy Notes:**
- First "real" deploy worth sharing — send to 1-2 birder friends for reaction

---

---

# PHASE 2 — First Species Pages

> **Ship goal:** 20 hand-crafted species pages linked from the map. The full experience for Showcase species.

---

## 2.1 — Species Page Layout

**Goal:** The species page shell — layout, sticky illustration, typography — with no real data yet. Nail the design once, reuse for all 20.

**Work:**
- Create `/app/species/[code]/page.tsx` dynamic route
- Full-screen illustration hero (placeholder image or SVG bird silhouette)
- On scroll: illustration shrinks and sticks to top (intersection observer approach — CSS sticky with height transition, no JS scroll listeners)
- Content area beneath: family context bar, card grid
- Four content cards: "What Makes Them Special," "Sounds," "Where & When," "Similar Species"
- Card components with correct typography (Crimson Pro body, Cormorant Garamond headings)
- Mobile: cards stack. Desktop: 2-column card grid.
- Breadcrumb: Family → Species name
- Metadata: `<title>`, `<meta description>` set from species data

**Setup:**
- None. Static scaffold with placeholder content.

**Validation:**
- Sticky illustration works on iOS Safari (notoriously finicky)
- No content-layout-shift on scroll
- Readable at 320px wide (minimum mobile)
- Print stylesheet hides nav, shows full illustration

**Deploy Notes:**
- Route works but pages show placeholder content — not shareable yet

---

## 2.2 — Species Data Schema + 20 Showcase Species JSON

**Goal:** Define the JSON schema every species page will use. Populate it for 20 showcase species by hand (or light AI assist with heavy human editing).

**Work:**
- Define `/public/data/species/[code].json` schema (matches README spec):
  - `code`, `commonName`, `scientificName`, `family`, `tier`
  - `summary` (3-4 sentences, voice-edited)
  - `didYouKnow` (one specific, shareable fact)
  - `whereWhen` (text + coarse range descriptor)
  - `illustration` path reference
  - `audio` (Xeno-canto recording ID or null)
  - `similarSpecies` (array of 2-3 codes)
  - `migrationTier` (1-3, what data quality the map has)
- Write content for all 20 showcase species — start with 5 to establish voice, then do remaining 15
- Apply "slop detector" rules: no banned words, must contain a specific number and named location
- Validate each JSON file against schema (simple Node script)

**Setup:**
- Reference the 20 species list from design docs
- eBird species codes reference: https://www.birds.cornell.edu/clementschecklist (codes are the `species_code` field)
- Xeno-canto: search each species, find a B+ quality recording, copy the recording ID

**Validation:**
- All 20 JSON files pass schema validation script
- Every summary has a specific number + named location (not "across North America")
- `didYouKnow` passes the "would you tell a friend?" test — read them aloud
- No two summaries have the same sentence structure

**Deploy Notes:**
- Data files ship as static assets — no database needed

---

## 2.3 — Illustrations for 20 Showcase Species

**Goal:** Custom AI illustrations for all 20 showcase species, reviewed and approved.

**Work:**
- Develop Midjourney master prompt template: vintage natural history illustration style, white background, specific pose directions per family
- Generate 3-5 candidates per species (20 species × 4 attempts = ~80 images, ~$1.60 at $10/mo Midjourney)
- Review every image for anatomical accuracy:
  - Bill shape correct for family?
  - Leg proportions (shorebirds and waders especially)
  - Plumage pattern matches breeding male (default pose)
- Select best, export as WebP at 1200×900px
- Place in `/public/images/species/[code].webp`
- Document the approved prompt for each family in `/docs/illustration-prompts.md` (reuse in Phase 4)

**Setup:**
- Midjourney subscription active ($10/mo)
- Photopea or Preview for basic crops/whitespace adjustments (free)

**Validation:**
- Every illustration passes birder gut-check (would you recognize the species from the illustration alone?)
- Consistent warm cream/parchment background tone across all 20
- File sizes under 200KB each (Next.js Image will optimize further)

**Deploy Notes:**
- Images committed to repo (they're static assets, reasonable size)

---

## 2.4 — Wire Species Pages to Data + Audio

**Goal:** 20 species pages show real content, real illustrations, real audio.

**Work:**
- Update `page.tsx` to read `/public/data/species/[code].json` at build time (`import` the JSON directly — static, no fetch)
- Render all content fields into the card layout from 2.1
- Create `AudioPlayer.tsx` — minimal player: play/pause button + waveform bar (CSS-animated fake waveform is fine, no library)
- Fetch Xeno-canto audio by recording ID: `https://xeno-canto.org/[id]/download` (direct link, no API key needed for linking)
- If no audio, card shows "No recording available" with honest explanation
- Generate static params for all 20 species codes (`generateStaticParams`)
- Add species to Pagefind search index (`npm install pagefind` and run at build time)

**Setup:**
- Pagefind: follow their Next.js integration guide (runs as a post-build script)

**Validation:**
- All 20 pages load under 1s (Lighthouse)
- Audio plays without page reload (no full navigation)
- Pagefind finds "Arctic Tern" when typed in search bar
- Each page has correct `<title>` and `<meta description>` for SEO

**Deploy Notes:**
- First shareable species pages
- Add `generateMetadata` for Open Graph (species name + illustration as og:image)

---

## 2.5 — Link Map Paths to Species Pages

**Goal:** Clicking a migration path on the map leads to a real species page. The loop is complete.

**Work:**
- Update info panel (from 1.4) to show illustration thumbnail + real summary snippet
- "See full profile →" now routes to `/species/[code]`
- Add "← Back to Map" link on species pages (with browser history awareness)
- Landing page: below the map, add a "Featured Migrations" section — 5 cards (one per path), each with illustration thumbnail, species name, one-line hook
- Featured section scrolls into view on mobile when user swipes map up

**Setup:**
- None beyond previous phases

**Validation:**
- Full user journey: land → see map → click path → read info panel → open species page → back to map
- Journey feels seamless, not like jumping between different apps
- "Featured Migrations" section loads without layout shift

**Deploy Notes:**
- This is Phase 2's "ship" moment — 20 real species, real map, real experience
- Share the link. Get reactions.

---

---

# PHASE 3 — Data Pipeline

> **Ship goal:** 500 species pages generated automatically from real data. Search works. "Birds Around You" works.

---

## 3.1 — eBird API Integration

**Goal:** A script that fetches species taxonomy and basic range data from eBird and writes it to JSON files.

**Work:**
- eBird API key (free, instant at ebird.org/api/keygen)
- Script: `scripts/fetch-ebird-taxonomy.ts` — fetches all species, writes `species-list.json`
- Script: `scripts/fetch-ebird-region.ts` — for a given eBird region code, fetches species checklist by month
- Map eBird species codes to our JSON schema's `code` field
- Handle rate limiting (eBird: 500 requests/day free tier) with `p-limit` and per-run resumption
- Output: enriched species records with `seasonalPresence: { jan: boolean, feb: boolean, ... }` for each of ~10K species

**Setup:**
- eBird API key in `.env.local` as `EBIRD_API_KEY`
- `npm install p-limit` for concurrency limiting

**Validation:**
- Script runs to completion without errors on first 100 species
- Output JSON matches schema
- Re-running is idempotent (doesn't re-fetch already-fetched species)

**Deploy Notes:**
- Scripts never run on Vercel — local/CI only
- Output JSON files committed to repo

---

## 3.2 — AI Content Generation Pipeline

**Goal:** Claude generates validated species summaries for 500 species at Standard tier quality.

**Work:**
- Script: `scripts/generate-content.ts`
- Claude API key set up (Anthropic console)
- Prompt template from design doc — include 5 hand-written showcase examples as few-shot demonstrations
- For each species: call Claude with taxonomy + seasonal presence data → get summary + didYouKnow
- Automated "slop detector" check on output:
  - Regex scan for banned words (fail if found)
  - Check for at least one number in summary (fail if missing)
  - Retry once on failure, then flag for manual review
- Write approved content to `species/[code].json`
- Log flagged species to `flagged-content.json` for manual cleanup

**Setup:**
- `ANTHROPIC_API_KEY` in `.env.local`
- `npm install @anthropic-ai/sdk`

**Validation:**
- Run on 50 species, manually read every output
- Banned word rate: 0%
- Specific number present: 95%+
- Flagged rate under 10%
- After calibration, run remaining 450

**Deploy Notes:**
- Content generation: local only, one-time (or on-demand for new species)
- Committed output to repo

---

## 3.3 — Batch Illustration Generation

**Goal:** AI illustrations for 480 non-showcase species (Standard/Curated tier).

**Work:**
- Script: `scripts/generate-illustrations.sh` — reads species list, calls Midjourney API (or manual batching via Discord bot)
- Use approved family-specific prompt templates from `docs/illustration-prompts.md` (built in 2.3)
- Batch by family (do all warblers together, all shorebirds together) — consistent style within family
- Automated image size check (reject if under 50KB — likely generation failure)
- Manual spot-check: review 1-in-20 images for obvious errors
- Failures and obvious errors go to `flagged-illustrations.json` — use family placeholder temporarily

**Setup:**
- Midjourney active, `--sref` parameter set to approved reference image from Phase 2

**Validation:**
- 95%+ pass automated size check
- Spot-check catches no species misidentification (bill/silhouette wrong for family)
- All 480 images at consistent dimensions and background tone

**Deploy Notes:**
- Images committed to repo across multiple commits (keep PRs under 100 files)

---

## 3.4 — "Birds Around You" Feature

**Goal:** User clicks location button → map zooms to their region → shows birds present this month there.

**Work:**
- Browser Geolocation API to get coordinates
- Nominatim reverse geocode: coordinates → human-readable region name
- eBird API: `GET /v2/data/obs/{regionCode}/recent` for the matched region code
- Filter to species with pages in our dataset (cross-reference species-list.json)
- Highlight 3-5 "of note" birds this month (migrant arrivals, rarities, seasonal peaks — determined by `seasonalPresence` data)
- Map pans/zooms to region, highlights relevant migration paths
- Below map: "Right now near [City], [State]" section with species thumbnails

**Setup:**
- Nominatim has a usage policy: include `User-Agent` header with project name + contact
- Handle geolocation denial gracefully (prompt to type a location instead)

**Validation:**
- Works in New York, Tokyo, Cape Town (test three continents)
- Geolocation denial shows a clean fallback, no errors
- Species shown match what eBird shows for that region this month (manual spot-check)
- Response feels fast (under 2s from click to map update)

**Deploy Notes:**
- Geolocation is client-side only — no server involvement
- eBird API call proxied through a Next.js API route (hides API key from client)

---

## 3.5 — Search (Pagefind at Scale)

**Goal:** Search across all 500 species, fast, with no server.

**Work:**
- Pagefind post-build script indexes all 500 static species pages
- Search bar on landing page now fully functional
- Results show: species name, family, one-line description snippet, thumbnail
- Keyboard navigation through results (arrow keys, Enter to select)
- Search works for common name, scientific name, and family name
- "No results" state with a suggestion (check spelling, or "Browse all families")

**Setup:**
- Pagefind: add `data-pagefind-body` attribute to species page content area
- Add Pagefind UI script to layout (they provide a pre-built web component)

**Validation:**
- "warbler" returns all warblers, not just American Yellow Warbler
- Scientific name search works (Setophaga petechia)
- Search result thumbnails load without layout shift
- Works on first keypress (no perceptible delay for 500 species index)

**Deploy Notes:**
- Pagefind index is generated at build time and served as static files
- No Algolia, no Elasticsearch, no cost

---

---

# PHASE 4 — Scale to Global Coverage

> **Ship goal:** 3,000+ species with working pages. The guide feels genuinely comprehensive.

---

## 4.1 — eBird Status & Trends Access + Raster Pipeline

**Goal:** Richer migration data for Tier 1 species (~1,100 species with weekly abundance models).

**Work:**
- Apply for eBird Status & Trends data access (Cornell Lab — free, requires brief project description)
- Download weekly abundance rasters for top 100 species (start small)
- Script: `scripts/process-rasters.ts` — converts GeoTIFF to simplified weekly presence polygons (GeoJSON)
- Store simplified GeoJSON per species per week (not raw rasters — too large)
- Update migration map to use GeoJSON polygons for Tier 1 species instead of hand-drawn paths
- Confidence shading: more opaque where abundance model is stronger

**Setup:**
- Cornell eBird S&T access approval (apply early — may take 1-2 weeks)
- `npm install gdal-async` or shell out to `ogr2ogr` for raster conversion

**Validation:**
- Raster data for American Robin shows expected pattern (winter south, summer north, migration in between)
- Simplified GeoJSON files under 100KB per species per week
- Map renders Tier 1 species noticeably more accurately than hand-drawn paths

**Deploy Notes:**
- GeoJSON files committed to repo (not raw rasters)
- Raster processing: local/CI only

---

## 4.2 — AI Path Inference Pipeline

**Goal:** Migrate from manual paths to AI-inferred migration paths for Tier 2/3 species.

**Work:**
- Script: `scripts/infer-migration-paths.ts`
- For each Tier 2 species: send Claude the breeding range, wintering range, flyway region, family, and `seasonalPresence` data
- Claude returns: array of `[lng, lat]` waypoints + `peakWeeks` + `pathType` (latitudinal/loop/irruptive/etc)
- Validate output (is the path geographically sane? does it cross continents without a flyway?)
- Write inferred paths to `species/[code].json` under `migration.inferredPath`
- Flag suspicious paths (e.g., path goes over open ocean for a non-pelagic species) for human review

**Setup:**
- Prompt includes the 5 hand-defined showcase paths as examples of correct format
- Confidence field: Claude rates its own path confidence (high/medium/low) based on data quality

**Validation:**
- Run on 50 species, compare output to known flyway maps
- No paths cross impossible geography
- Loop migrations correctly show different spring/fall routes
- Low confidence paths render as dashed on the map

**Deploy Notes:**
- Inferred paths stored in species JSON (no runtime AI calls)

---

## 4.3 — Family Browse + Regional Pages

**Goal:** Users can explore by family or by region, not just search by name.

**Work:**
- `/app/family/[family]/page.tsx` — lists all species in a family with thumbnails, common name, brief hook fact
- `/app/region/[region]/page.tsx` — regional species list (e.g., "Birds of the Pacific Northwest")
- 10 hand-selected regions to start (not all of eBird's thousands of codes)
- Family pages generated statically from species-list.json
- Sidebar on species pages: "More in this family" with 5 thumbnails
- Navigation: landing page gets a "Browse" section below the map — entry point to families

**Setup:**
- Define 10 initial regions with friendly names and eBird region codes in `regions.json`

**Validation:**
- Every family page loads under 500ms
- "Browse Warblers" shows all warbler species in our dataset
- Regional pages show only species we have pages for (no broken links)

**Deploy Notes:**
- All statically generated at build time

---

## 4.4 — "What's That Bird?" Quick ID

**Goal:** A simple identification helper. Not a computer vision classifier — a guided question flow.

**Work:**
- `/app/identify/page.tsx`
- Step-by-step questions: Size (sparrow/robin/crow/hawk) → Dominant color → Bill shape → Habitat → Region
- Each answer filters the species list
- After 3-4 answers: show top 5-8 candidates with illustrations and distinguishing field marks
- "See full profile" links to species pages
- No ML, no camera — pure filter logic
- State managed in URL params so results are shareable (`/identify?size=robin&color=red&bill=thick`)

**Setup:**
- Define question flow and filter logic against species-list.json fields
- Add `fieldmarks: { size, primaryColor, billShape, habitat }` to species schema (populate for all 500 current species)

**Validation:**
- "Robin-sized, red breast, forest" returns American Robin as top result
- Results page loads under 300ms (client-side filter, no network)
- Shareable URL loads correct filtered results

**Deploy Notes:**
- Pure client-side — no server, no cost

---

## 4.5 — Global Coverage Expansion to 3,000 Species

**Goal:** Expand beyond North American focus to Europe, Asia, Africa, Australia.

**Work:**
- Run content generation pipeline (from 3.2) for 2,500 additional species
- Run illustration generation (from 3.3) in family batches
- Expand "Birds Around You" to return results for non-North American coordinates
- Migration map: add 5 global migration flows (trans-Saharan, East Asian-Australasian Flyway, African-Eurasian, Neotropical, Australasian)
- Handle Southern Hemisphere seasons: swap month labels ("Spring" in September for AU/NZ/SA)
- Global navigation: map defaults to hemisphere based on visitor's rough timezone/locale

**Setup:**
- Southern Hemisphere season logic: `const season = lat < 0 ? southernHemisphereSeason(month) : northernHemisphereSeason(month)`

**Validation:**
- "Birds Around You" for Sydney, Australia returns Australian species
- Migration map shows trans-Saharan migrants moving correctly (south in Sept, not north)
- Species pages for non-North American birds have content quality comparable to NA species

**Deploy Notes:**
- Build time will increase significantly — monitor Vercel build limits
- Enable ISR for non-showcase species pages (revalidate weekly) to keep build fast

---

---

# PHASE 5 — Polish & Depth

> **Ship goal:** The site you'd be genuinely proud to put your name on. Details and delight.

---

## 5.1 — Performance Audit + Offline Support

**Goal:** Site scores 95+ on Lighthouse. Works without internet for previously visited pages.

**Work:**
- Lighthouse audit → identify top 5 performance issues → fix each
- Service worker with Workbox: cache species pages and illustrations on visit
- Offline fallback page ("You're offline. Here are the birds you've visited recently.")
- Image optimization audit: confirm all illustrations are served as WebP, have correct `sizes` attributes
- Font loading optimization: `font-display: swap`, preload critical fonts

**Setup:**
- `npm install workbox-webpack-plugin`
- Test offline mode in Chrome DevTools Network tab → Offline

**Validation:**
- Lighthouse: Performance 95+, Accessibility 100, Best Practices 100, SEO 100
- Previously visited species page loads instantly offline
- No layout shift (CLS = 0)

**Deploy Notes:**
- Service worker registered via `next-pwa` or custom Workbox config

---

## 5.2 — "Follow This Journey" — Scrollytelling for 3 Showcase Species

**Goal:** Scroll-driven narrative for the 3 most spectacular migration stories.

**Work:**
- Three candidates: Arctic Tern, Bar-tailed Godwit, Sooty Shearwater
- Each gets a `/species/[code]/journey` page
- Scroll drives the narrative: as user scrolls, map pans along the route, text cards appear
- Implementation: IntersectionObserver on text sections → map flyTo each waypoint in sequence
- Text: poetic but specific (written by hand — this is Showcase tier content)
- Mobile: map above fold, text below, scrolling text triggers map updates
- Desktop: map fixed right, text scrolls left

**Setup:**
- No new dependencies — IntersectionObserver is native
- Mapbox `flyTo` with easing for smooth transitions

**Validation:**
- Scrolling feels intentional, not janky — animation easing matches scroll speed
- Each text card has content worth lingering on (read them aloud — if you wouldn't tell someone this, rewrite it)
- Works on iPhone (the hard one) without frame drops

**Deploy Notes:**
- Linked from each species page as "Follow the Journey"
- These pages are shareable standalone — they should feel complete even without the species page

---

## 5.3 — Print-Friendly Field Cards

**Goal:** Any species page can be printed as a beautiful single-page field card.

**Work:**
- `@media print` CSS: hide nav, map, audio player
- Print layout: illustration (1/3 page top), key field marks and range as clean text, Aviary URL footer
- Custom print typography: slightly larger font, high-contrast (black on white)
- "Print Field Card" button on each species page (just triggers `window.print()`)
- Test with 5 different species to confirm layout holds

**Setup:**
- No new dependencies

**Validation:**
- Print preview in Chrome shows no clipping, no orphaned headers
- Illustration prints at full width without pixelation (ensure full-res image used for print)
- Someone could take this printout birding and find it useful

**Deploy Notes:**
- No deployment changes — CSS only

---

## 5.4 — Analytics + Continuous Improvement Loop

**Goal:** Understand what users care about without tracking them.

**Work:**
- Self-hosted Umami on a cheap VPS ($6/mo on DigitalOcean, or use Umami Cloud free tier)
- Track: page views by species (which species get the most love?), search queries (what are people looking for that we don't have?), "Birds Around You" usage by region
- Build a simple admin page (`/admin`, password-protected via `ADMIN_PASSWORD` env var) that shows:
  - Top 20 species by views
  - Top 10 search queries with no results
  - Illustration flagged count
- Weekly ritual: check admin page → pick 1-3 Standard tier species to upgrade → write better content
- Document the upgrade loop: view analytics → choose species → edit JSON → commit → deploy

**Setup:**
- Umami: follow their self-hosted or cloud setup guide
- One tracking script added to root layout

**Validation:**
- Pageview data appears in Umami within 24h of deploy
- Admin page loads (no public access — 401 if wrong password)
- Zero personal data collected (IP anonymized, no fingerprinting)

**Deploy Notes:**
- `UMAMI_SCRIPT_URL` and `UMAMI_WEBSITE_ID` env vars in Vercel

---

## 5.5 — About Page + Credits

**Goal:** The humans (and birds) behind this project get proper acknowledgment.

**Work:**
- `/app/about/page.tsx`
- Content: why this exists (the gift philosophy), how content is made (honest about AI + human curation), data sources with links and proper attribution, illustration credits
- Design: same typography and cards as species pages, not a wall of text
- Credits section: eBird (Cornell Lab), Xeno-canto, Mapbox, Nominatim, all open-source dependencies
- A note inviting birders to flag errors (email or GitHub Issues link)
- One photo or illustration that represents the project's spirit

**Setup:**
- None

**Validation:**
- Data source attributions are accurate and link correctly
- AI illustration disclosure is honest and specific
- Someone reading this page would trust the project more, not less

**Deploy Notes:**
- Link in nav (already placeholder from Phase 1)

---

---

# PHASE 6 — Community & Future

> **Ongoing, not a sprint.** Things to do when the project feels finished but you want to keep going.

---

## 6.1 — Community Comments (Utterances)

One-session work. Utterances wires GitHub Issues as comments — zero infra, free forever. Add it to species pages below the content cards. Birders can share sightings and lore. You moderate via GitHub Issues.

---

## 6.2 — BirdCast Integration

When BirdCast opens their API (or via screen scraping their public nocturnal migration forecasts), add a "Tonight's Migration" layer to the map — real-time radar-based migration forecasts. This is aspirational and depends on data access.

---

## 6.3 — Illustration Quality Upgrades

Ongoing ritual: each week, pick 3-5 Standard tier illustrations that got high views but low-quality images. Regenerate with better prompts. Document what improved. Gradually the visual quality lifts across the whole site.

---

## 6.4 — Historical Migration Comparison

A "Then vs. Now" view for select species showing range shift over the last 30 years (using historical eBird data). Requires significant data work. Good for species with well-documented climate-driven changes (e.g., American Robin wintering range expansion).

---

---

# Reference

## Tech Stack (Decided)
| Layer | Choice |
|-------|--------|
| Framework | Next.js 14, App Router, TypeScript |
| Styling | Tailwind CSS + custom tokens |
| Animation | Framer Motion |
| Maps | Mapbox GL JS |
| Search | Pagefind (static, no server) |
| Fonts | Cormorant Garamond + Crimson Pro (Google) |
| Hosting | Vercel (free tier) |
| Images | Next.js Image Optimization |
| Analytics | Umami (self-hosted or cloud free) |
| AI Content | Anthropic Claude API |
| AI Illustrations | Midjourney |

## Color Tokens
| Name | Hex | Use |
|------|-----|-----|
| Cream | `#FAF7F0` | Primary background |
| Forest | `#1A2F23` | Text, headers |
| Deep Green | `#2D4A3E` | Secondary backgrounds |
| Sepia | `#8B7355` | Borders, secondary text |
| Rust | `#B85C38` | Accent, CTAs, migration paths |
| Sky | `#A8DADC` | Winter ranges |
| Gold | `#C9A05F` | Enhanced tier badge |

## First 20 Showcase Species
American Robin, Northern Cardinal, Blue Jay, Black-capped Chickadee, American Goldfinch, Mourning Dove, Bald Eagle, Great Horned Owl, Ruby-throated Hummingbird, Red-tailed Hawk, Barn Swallow, Sanderling, Arctic Tern, American Golden-Plover, Bar-tailed Godwit, Mountain Quail, Common Loon, Snowy Owl, Atlantic Puffin, Sooty Shearwater

## eBird Species Codes (First 5 for Map)
| Species | eBird Code |
|---------|-----------|
| Arctic Tern | arcter |
| Bar-tailed Godwit | batgod |
| Ruby-throated Hummingbird | rthhum |
| American Golden-Plover | amgplo |
| Barn Swallow | barswa |

## Monthly Milestones (Rough Estimate)
| Milestone | Phase | Status |
|-----------|-------|--------|
| Live map with 5 paths | 1.3 | Not started |
| 20 species pages | 2.5 | Not started |
| 500 species live | 3.5 | Not started |
| 3,000 species live | 4.5 | Not started |
| Full polish + offline | 5.1 | Not started |

---

*This document updates as phases complete. When a phase ships, mark it. When a phase teaches you something unexpected, update the next phase's setup and validation sections.*
