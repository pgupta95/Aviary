# Aviary Design Review

**Reviewer Notes - Pre-Build Assessment**
February 2026

---

## Overall Impression

The vision is clear, the aesthetic sensibility is strong, and the migration map concept is genuinely novel. The docs demonstrate real thought about what makes this different from Cornell's All About Birds or Avibase. The "restrained vintage" philosophy is the right instinct - it prevents the project from becoming either a sterile database or a kitschy novelty.

This review focuses on gaps that could cause real problems during build, decisions that need to be locked before writing code, and enhancements that strengthen the core idea without scope creep.

---

## 1. Ornithological & Migration Knowledge Gaps

These are areas where the docs reveal simplifications about bird biology that will affect data modeling, map accuracy, and content credibility with birders.

### 1.1 Migration is Not Just North-South

The docs model migration exclusively as latitudinal (north-south, breeding grounds to wintering grounds). Real migration is more varied:

- **Altitudinal migration**: Many species move up/down mountains seasonally, not across continents. Mountain Quail in the Sierra Nevada walk downhill in winter. White-crowned Sparrow populations in the Rockies descend 2,000m. These species have meaningful seasonal movements that the current lat/lng path model won't capture.
- **Loop migration**: Many species take different routes in spring vs fall. American Golden-Plover flies east over the Atlantic in fall but comes back through the Great Plains in spring. The current data structure assumes one path with direction reversal - it needs to support two distinct route geometries.
- **Longitudinal migration**: Evening Grosbeak and some European species move east-west, not north-south. The time slider narrative ("northbound spring / southbound fall") breaks for these.
- **Austral migration**: In the Southern Hemisphere, migration goes *south* to breed and *north* to winter. Fork-tailed Flycatcher, Chocolate-vented Tyrant, and others. The docs' language and assumptions are Northern-Hemisphere-centric.

**Suggestion**: Add a `migrationPattern` enum to the species schema: `latitudinal | altitudinal | loop | longitudinal | austral | irruptive | nomadic | sedentary`. This drives both path rendering logic and narrative generation. For loop migrants, the route schema needs `springRoute` and `fallRoute` as distinct geometries, not just a direction reversal on the same path.

### 1.2 Irruptive and Nomadic Species Are Missing Entirely

The docs don't account for species whose movements are unpredictable and driven by food supply rather than season:

- **Irruptive species**: Snowy Owl, crossbills, Pine Siskin, Bohemian Waxwing, Red-breasted Nuthatch. Some years they flood south in huge numbers; other years they stay put. This is some of the most exciting birding content imaginable ("Snowy Owl irruption year!") and the map could highlight it.
- **Nomadic species**: Many Australian and African species wander based on rainfall and seed crops. They don't have fixed routes at all.

**Suggestion**: Rather than ignoring these, add a special irruptive/nomadic visualization mode. Instead of animated paths, show an expanding/contracting range boundary with a "pulse" effect during irruption years. Tag species as `irruptive` so the narrative template can say "This species stays in the boreal forest most years, but periodically erupts southward when spruce cone crops fail - and 2026 is shaping up to be one of those years." This turns a data gap into a feature that birders would genuinely love. The challenge is data freshness - irruptions happen in real-time, and you'd need either eBird frequency data updated more than weekly, or a manual editorial flag.

### 1.3 Partial Migration Is Not Addressed

American Robin - your first enhanced species - is itself a partial migrant. Some robin populations migrate; others are year-round residents. This is true for many species: Dark-eyed Junco, Song Sparrow, Red-tailed Hawk, European Robin. The current schema treats migration as a species-level boolean, but it's actually a population-level spectrum.

**Suggestion**: Add a `migrationVariability` field: `obligate | partial | facultative`. For partial migrants, the narrative should acknowledge this: "Northern populations migrate south in fall, while birds in the mid-Atlantic may stay year-round." The map could show the resident range as a stable overlay while animating only the migratory portion. This prevents a birder in Georgia from seeing an animated Robin migration path and thinking "that's wrong, Robins are here all year" - which would immediately damage credibility.

### 1.4 Nocturnal Migration Is a Missed Opportunity

Most songbirds migrate at night. This is a fascinating fact that most people don't know, and it's well-documented by weather radar data (BirdCast from Cornell). The docs don't mention diurnal vs nocturnal migration at all.

**Suggestion**: Consider integrating BirdCast data (https://birdcast.info) or at minimum tagging species with `migrationTiming: diurnal | nocturnal | crepuscular`. For the migration map, a subtle day/night toggle or overlay could show "Tonight, an estimated 200 million birds are in flight across the US." This is the kind of invisible-made-visible moment that aligns perfectly with the project's spirit. BirdCast publishes live migration forecasts and has an API - this could be a Phase 4+ enhancement that makes the "live" framing genuinely real rather than aspirational.

### 1.5 Seabird and Pelagic Gaps

The initial enhanced species list includes Atlantic Puffin, but the docs don't address how to handle pelagic species. Albatrosses, petrels, shearwaters, and jaegers spend months or years at open ocean. Their "ranges" are vast oceanic zones, not land-based polygons. Sooty Shearwater circumnavigates the entire Pacific Ocean annually.

**Suggestion**: For pelagic species, use ocean-based path rendering with different visual treatment (perhaps wave-like paths rather than land-following lines). The narrative should acknowledge the oceanic context. This affects maybe 300-400 species globally and is worth deciding on before building the map renderer, since the base map needs to handle ocean areas as more than blank space.

### 1.6 The "Live" Framing Needs Honest Language

The docs repeatedly use "live," "right now," and "happening now" to describe the migration map. But the actual data is seasonal averages from eBird Status & Trends - it shows what *typically* happens in May, not what's happening *this* May. A birder who sees "Live Bird Migration" and then realizes it's showing the same data in May 2026 as it would in May 2027 will feel misled.

**Suggestion**: Frame it as "Migration This Month" or "What's Flying Now" rather than "Live." The data *is* real and current-month-relevant, it's just not real-time. Alternatively, you could make it genuinely live by incorporating eBird's recent observations API (last 30 days of sightings for a region), which would add real temporal freshness. This is a small language change but a critical credibility decision. The birding community is knowledgeable and will notice.

---

## 2. Data Source Gaps and Risks

### 2.1 eBird Status & Trends Is Not Free

The docs list eBird Status & Trends as a free data source, but this is inaccurate:

- **eBird API 2.0** (taxonomy, recent observations, hotspots): Free with API key. Rate-limited to ~100 requests/minute.
- **eBird Status & Trends** (weekly abundance models, range maps): Requires a separate access request through Cornell Lab. The data products (abundance maps, range boundaries) are available for download but have **specific usage restrictions** and require attribution to specific papers. The raster abundance data is not simple GeoJSON polygons - it's 27km resolution raster grids in GeoTIFF format.
- **Coverage**: Status & Trends models exist for ~1,100 species (mostly Western Hemisphere). That's 10% of global species. For the other 90%, you don't have weekly abundance data.

**Suggestion**: Audit exactly which data products you can access and what their format is *before* building the data pipeline. The README's build process ("Fetch seasonal range data from eBird Status & Trends") won't work as described for most species. You'll likely need a tiered data strategy:
- **Tier 1** (~1,100 species): Full eBird Status & Trends weekly abundance. Rich migration data.
- **Tier 2** (~3,000 species): eBird API range maps (static polygons, not temporal). Basic seasonal presence.
- **Tier 3** (~7,000 species): BirdLife International range maps (breeding/non-breeding/passage) or Wikipedia text extraction. Coarse presence data only.

This changes the architecture meaningfully. Document the data tier per species in the schema so the UI can gracefully adapt (rich map for Tier 1, simple range overlay for Tier 2, text-only for Tier 3).

### 2.2 GeoJSON Range Polygons Don't Exist in the Form Described

The species schema shows `ranges.breeding`, `ranges.winter`, and `ranges.yearRound` as GeoJSON polygons. But the actual sources provide:
- **eBird Status & Trends**: Raster grids (GeoTIFF), not vector polygons. Need processing to convert to GeoJSON.
- **BirdLife/IUCN**: Provide range polygons, but they're expert-drawn, coarse resolution, and behind a data request process.
- **eBird API**: Provides *observation locations* and *regional frequency*, not range polygons.

**Suggestion**: Decide whether to: (a) convert eBird raster data to simplified GeoJSON polygons via a processing pipeline (using something like `geojson-vt` or GDAL), or (b) use BirdLife polygons (apply for access), or (c) render eBird abundance rasters directly as tile layers. Option (c) is actually easiest and would look beautiful - abundance shown as a heat gradient rather than hard polygon boundaries. This is an architectural decision that affects both the build pipeline and the map rendering approach.

### 2.3 eBird API Rate Limits Will Bottleneck Build

Fetching data for 11,000+ species from the eBird API with a rate limit of ~100 requests/minute means:
- Basic taxonomy: ~1 request (bulk download available)
- Recent observations per species per region: Could be thousands of requests
- Regional frequency data: Hundreds of requests per species

A naive "fetch all" script will hit rate limits within minutes.

**Suggestion**: Design the data pipeline with caching, incremental updates, and bulk download in mind. eBird provides bulk taxonomy downloads and regional data exports. Use those instead of per-species API calls where possible. The build process should be incremental (only re-fetch changed data), not a full rebuild every time.

### 2.4 Xeno-canto and Wikipedia Coverage Is Uneven

- **Xeno-canto**: Has excellent coverage for European and North American birds, but many tropical species have 0 recordings. Some species have hundreds of recordings of varying quality. You need a curation strategy, not just "link to Xeno-canto."
- **Wikipedia**: Many species (especially recently split ones, or obscure tropical birds) have stub articles with 1-2 sentences. The "fetch Wikipedia summary" step will return useless content for thousands of species.

**Suggestion**: Add fallback strategies: if Wikipedia summary is <50 words, fall back to family-level description + species-specific field marks from eBird taxonomy. For Xeno-canto, define minimum quality criteria (recording quality rating, background noise level) and prefer curated selections over raw API results. Tag species with `audioAvailable: boolean` in the schema so the UI can hide the sounds section entirely rather than showing empty states for thousands of species.

### 2.5 Macaulay Library Photo Strategy Is Undefined

The README mentions "Links to Macaulay Library photos" but never specifies how. The Macaulay Library API has usage restrictions, and deep-linking to specific assets may not be permitted without a formal data use agreement.

**Suggestion**: Either formalize a plan for Macaulay integration (apply for API access, understand terms) or drop it from the initial scope. The AI illustrations are the hero content; linking to external photo libraries is a nice-to-have that shouldn't block the build.

---

## 3. Technical Architecture Decisions to Lock

### 3.1 Map Library: SVG vs Mapbox GL JS vs Leaflet

The README says "SVG-based custom or Mapbox GL JS" as if this is a minor detail. It's actually the most consequential technical decision for the migration map feature:

- **Custom SVG**: Full visual control, matches vintage aesthetic perfectly, no dependency. But: building pan/zoom/touch from scratch is significant work, no tile-based base map, projection math is manual, performance at global scale needs careful engineering.
- **Mapbox GL JS**: Professional-grade, WebGL-accelerated, beautiful base map styling, supports GeoJSON natively. But: requires API key with usage-based pricing (free tier: 50K map loads/month), adds vendor dependency, base map may fight the vintage aesthetic.
- **Leaflet + OpenStreetMap**: Free, lightweight, large plugin ecosystem. But: canvas-based (not WebGL), fewer styling options for base map, animation performance may be limiting.

**Suggestion**: Go with **Mapbox GL JS** for the initial build. The free tier is generous (50K loads/month is a lot for a passion project), the base map can be styled to match the vintage palette (Mapbox Studio supports custom styles with muted earth tones), and the WebGL rendering will handle animation performance far better than SVG at scale. The vintage soul comes from the *paths and illustrations*, not the base map. If the project outgrows Mapbox's free tier, you can migrate to MapLibre GL (the open-source fork) with minimal code changes.

### 3.2 Comments System Introduces Backend Dependency

The UX spec recommends a custom Supabase comments system. This contradicts the core architecture of "no backend to maintain" and introduces:
- A Supabase project to manage
- Database tables to create and maintain
- Spam moderation burden (this is real - even small sites get bot comments)
- A new failure mode (Supabase outage = broken page)

**Suggestion**: For the initial build, use **utterances** (GitHub Issues-backed comments). It's free, requires no backend, handles spam via GitHub authentication, and aligns with the open-source spirit. It does require commenters to have GitHub accounts, which limits participation - but the About page comments section is a nice-to-have, not a core feature. If community engagement grows, migrate to a custom system in Phase 4+.

### 3.3 Static Generation of 11,000+ Pages on Vercel Free Tier

Vercel's free tier has a **45-minute build time limit** and **100MB function size limit**. Generating 11,000+ static pages with images, JSON data, and SEO metadata will push these limits. Next.js ISR (Incremental Static Regeneration) can help, but the initial build is still a concern.

**Suggestion**: Plan for this upfront. Options:
- **Output Export** (`next export`): Generates pure static HTML. No server functions needed, works well with CDN hosting. But no ISR or dynamic routes at runtime.
- **ISR with on-demand revalidation**: Pages are generated on first visit, then cached. Reduces build time to near-zero but requires a serverless function.
- **Hybrid**: Pre-generate the 200 enhanced species at build time, use ISR for the remaining 10,800. This keeps builds fast while still serving all pages.

The hybrid approach is likely best. Test build times early with a subset of 1,000 pages to validate.

### 3.4 Fuse.js for 11,000+ Species

Client-side fuzzy search with Fuse.js means loading the entire species index into the browser. For 11,000 species with names, codes, and families, that's likely 500KB-1MB of JSON.

**Suggestion**: This is probably fine for the MVP - compress the index (gzip should reduce it to ~100KB) and lazy-load it on search focus. But measure it. If the index exceeds 200KB gzipped, consider:
- **Pagefind** (static search index, designed for static sites): Generates a search index at build time with lazy-loaded chunks. Very fast, very small.
- **Algolia free tier**: 10K records, 10K searches/month. Professional search quality, no client bundle.

Pagefind in particular would be an excellent fit for this project.

### 3.5 Image Hosting and Build Strategy

The docs mention both Cloudinary and self-hosted as options, without deciding. For 11,000+ illustrations:
- **Self-hosted on Vercel**: Free bandwidth (100GB), but images need to be committed to the repo or fetched at build time. Next.js Image component handles optimization.
- **Cloudinary free tier**: 25GB storage, 25GB bandwidth. Enough for launch but will be exceeded with traffic.
- **R2 / Backblaze B2**: Effectively free for static assets at this scale. No optimization pipeline built in.

**Suggestion**: Self-host images in the repo or a separate asset bucket, use **Next.js Image component** for optimization (automatic WebP, responsive sizing, lazy loading). This avoids any CDN dependency and keeps the "no ongoing costs" promise honest. For 11,000 images at ~100KB average (optimized WebP), total storage is ~1.1GB - manageable for a Git LFS repo or a simple bucket. Avoid Cloudinary unless you need its transformation features.

---

## 4. UX Flow Gaps

### 4.1 Landing Page Contradiction Between Documents

The README and UX_SPECIFICATION say the landing page IS the migration map (full-screen, no hero illustration). But the DESIGN_SUMMARY shows a traditional landing page with a hero illustration, search bar, and "Birds Around You" button - no migration map at all.

**Suggestion**: Lock this decision. The migration map as landing page is the stronger choice - it's the unique differentiator and creates immediate visual impact. The DESIGN_SUMMARY should be updated to match. If the migration map isn't ready for Phase 1 launch, use a stunning illustration as a *temporary* hero with a "Migration Map coming soon" teaser, but design the architecture around the map being the eventual landing experience.

### 4.2 No Offline or Poor-Connectivity Handling

The design philosophy says "simple enough to use on a phone at the beach." Beaches often have poor cell signal. The docs don't address offline or degraded connectivity at all.

**Suggestion**: Since the site is statically generated, it's naturally CDN-friendly. Add a **service worker** for basic offline support:
- Cache visited species pages for offline re-access
- Cache the search index after first load
- Show a "you're offline" indicator rather than a broken page
- The migration map obviously won't work offline, but viewed species pages could

This is a Phase 3+ enhancement but worth noting in the architecture now so it doesn't require a rewrite.

### 4.3 Desktop Experience Is Underspecified

The UX spec briefly mentions ">768px" adaptations but the desktop experience is treated as "same but wider." For a project about beauty and intentionality, the desktop layout deserves more thought:

- Species page: The sticky illustration at 35vh on a 27" monitor is a lot of screen real estate consumed by one image. Consider a **side-by-side layout** on desktop: illustration fixed on the left (40% width), content scrolling on the right (60%).
- Migration map: On desktop, the map could show more context. Consider a **split view**: map on the left, "What's Flying Through" panel on the right, eliminating the need to scroll to see educational content.
- Featured cards: Three columns of cards with full-width illustrations would be stunning on desktop.

**Suggestion**: Create desktop-specific wireframes for the three main pages. The mobile experience is well-defined; give the desktop layout the same care.

### 4.4 Species Page Navigation Is a Dead End

Once a user reaches a species page, their options are: back button, 2 similar birds, or "see more birds near you." There's no:
- Family browsing ("all thrushes")
- Taxonomic context ("this is in the Thrush family - here are its relatives")
- Regional browsing ("other birds in California")
- Seasonal browsing ("other birds migrating right now")

**Suggestion**: Add a **thin contextual bar** below the bird name: `Thrushes (Turdidae) • 45 species in this family`. Make "Thrushes" a link to a family page or filtered species list. This costs almost nothing in UI complexity but creates a natural browsing path that field guide users expect. It also prevents the "dead end" feeling where users bounce after viewing one species.

### 4.5 Search Discoverability from Migration Map

If the migration map is the full-viewport landing experience, first-time users need to understand that search exists below the fold. The current design has a "Scroll to explore" hint that fades after 3 seconds.

**Suggestion**: Add a **persistent, minimal search icon** in the header bar (next to the hamburger menu). Tapping it either scrolls to the search section or opens an inline search overlay. Many users will arrive wanting to look up a specific bird; making them scroll past a full-screen map is a friction point for that use case.

### 4.6 The "Birds Around You" Flow Has Edge Cases

The docs describe location permission grant/deny but don't address:
- User is in a location with extremely sparse eBird data (rural Africa, central Asia). What do they see? "0 birds" is a bad experience.
- User is in the Southern Hemisphere where seasons are reversed. The narrative ("spring migration") is Northern-Hemisphere-specific.
- User denies location, enters a city name. How does city-to-coordinates geocoding work? This needs a geocoding service (Mapbox, Nominatim, etc.).

**Suggestion**: For sparse-data regions, fall back to the nearest well-covered area with a note: "Showing birds near [nearest covered region]. Data coverage in your area is limited." For Southern Hemisphere, season labels should flip or use month names instead of "spring/fall." For geocoding, Nominatim (OpenStreetMap's free geocoder) is sufficient and avoids another paid dependency.

---

## 5. Anti-AI-Slop Decision Points

These are the decisions that determine whether Aviary feels like a craft project or a generated product. This is the most important section.

### 5.1 The Quality Cliff Between Enhanced and Basic Species

The two-tier model creates an obvious quality boundary. A user who views the stunning Sanderling page with hand-written prose, then clicks to a basic species with a template illustration and Claude-generated bullets, will immediately feel the difference. Multiply this by 10,800 basic species and 200 enhanced ones, and 98% of the site feels "lesser."

**Suggestion**: Instead of a binary enhanced/basic split, create a **continuous quality spectrum** with 4-5 tiers:
1. **Showcase** (20 species): Everything hand-crafted. The portfolio pieces.
2. **Enhanced** (200 species): Custom illustrations, refined AI prose with human editing pass.
3. **Curated** (1,000 species): Template illustrations but with hand-selected Xeno-canto recordings, manually verified facts, and a one-line editorial "hook" (one genuinely interesting specific fact, written by a human, that makes this species page worth visiting).
4. **Standard** (5,000 species): Template illustrations, AI-generated content, quality-checked.
5. **Stub** (5,000 species): Minimal data, placeholder illustration, "help us improve this page" CTA.

The key insight: the difference between "soulless" and "cared-for" is often *one specific, memorable detail*. If every Standard-tier page has one human-curated "did you know" fact that's genuinely surprising, it won't feel like slop even if the rest is generated. Invest editorial effort in breadth (one great fact per species) rather than depth (20 perfect pages).

### 5.2 Claude-Generated Content Needs Adversarial QA

The content template is good, but the docs underestimate how AI-generated nature writing tends toward a specific kind of generic warmth. "These remarkable birds undertake an epic journey" is the AI equivalent of stock photography. Birders will recognize it instantly.

**Suggestion**: Build a **"slop detector"** into the content pipeline. Specific rules:
- **Ban list**: "remarkable," "incredible," "epic journey," "fascinating," "thriving," "testament to," "nature's," "feathered friend," "avian," "winged." These are AI nature-writing crutches.
- **Specificity check**: Every summary must contain at least one *number* and one *named location*. "Migrates long distances" fails. "Flies 11,000 km from Alaska to New Zealand" passes.
- **Uniqueness check**: No two species summaries should share more than 30% of their words. AI tends to produce structurally identical outputs.
- **The "so what" test**: Each "Did You Know" bullet should make someone want to tell a friend. "Is a migratory bird" fails. "Can sleep with one eye open while flying over the ocean" passes.

Include 3-5 examples of *bad* outputs in the prompt (with explanations of why they're bad), not just good ones. Anti-examples are more effective than examples for preventing slop.

### 5.3 AI Illustration Consistency at Scale

Midjourney is excellent for individual illustrations but produces inconsistent results at batch scale even with `--sref`. Common issues:
- Proportions drift (birds too fat, too thin, wrong bill shape)
- Background style varies (some get detailed botanicals, others get blank space)
- Anatomical errors (wrong number of toes, impossible wing positions, eyes in wrong location)
- Species confusion (AI generates a similar but wrong species)

**Suggestion**: Build a **species-specific prompt library** with validated reference images. For each taxonomic family, create a proven prompt template that has been tested and produces anatomically correct results for that body type. Tag each illustration with a quality rating and flag any that need regeneration. Consider having 2-3 birders review a batch of 100 illustrations for accuracy before scaling to 11,000. An anatomically wrong illustration will be noticed by birders and shared as a negative example - it's worse than having no illustration. Also explore the option of a single consistent style reference across ALL illustrations from the start rather than batch-by-batch, where style drift can accumulate.

### 5.4 Migration Narratives Need Grounding

The AI-generated month-by-month narratives are a great concept, but they need to be grounded in real, cite-able information rather than plausible-sounding generalizations.

**Suggestion**: For each narrative, require the generation prompt to include:
- A specific location name (not "coastal areas" but "Delaware Bay" or "Copper River Delta")
- A specific number (distance, abundance, timing)
- A specific ecological detail (what they eat, why they stop there)

Create a narrative review checklist: "Could a birder fact-check this paragraph? If every claim is too vague to verify, it's slop."

### 5.5 Don't Let the Map Data Drive False Precision

The AI-inferred migration paths present a credibility risk. Showing a smooth, confident-looking animated path for a species where the actual route is poorly understood implies precision that doesn't exist. A birder who knows the science will see this as misleading.

**Suggestion**: Make confidence visible in the visualization. High-confidence paths: solid, bold animated lines. Medium-confidence: dashed, slightly transparent. And rather than showing paths for poorly-studied species, show a simple breeding/winter range toggle with a note: "Migration route not well documented." Honest gaps are better than confident-looking fabrications. Consider adding a small "i" icon on paths that, when tapped, shows the data source and confidence level.

---

## 6. Enhancements That Build on the Core Idea

### 6.1 "What's That Bird?" Quick ID Flow

The docs focus on search-by-name, but a huge use case is: "I see a bird right now and don't know what it is." A simple **guided ID flow** would be extremely valuable:

1. User taps "What's that bird?" (or a small binoculars icon)
2. Three quick questions: Size (sparrow / robin / crow / goose), Color (dominant color picker), Habitat (water / ground / tree / sky)
3. Returns 3-8 likely matches filtered by location and season
4. User taps the match to confirm

This doesn't require any AI or image recognition - just a filtered query against existing data (size, color, habitat, range, season). It's the second most common birding use case after "tell me about [bird name]" and would differentiate Aviary from pure databases.

### 6.2 Seasonal Landing Experience

The docs mention rotating featured cards seasonally, but the entire landing experience could subtly shift with the seasons:

- **Color accent shift**: The migration map's dominant path colors change by season (spring: warm golds and greens; fall: ambers and rusts; winter: cool blues and grays). The accent color in the header/UI could subtly echo this.
- **Featured content**: "This week's migration highlight" - one specific species, one specific location, one specific thing happening right now. Hand-curated weekly (or whenever you can), not auto-generated.
- **Time slider default**: Already noted in the docs but worth emphasizing - always default to the current real-world month. Never show January in July.

### 6.3 "Follow This Journey" Single-Species Migration Story

The docs mention this as a future enhancement, but it's worth elevating to a core concept for enhanced species. Instead of just showing a species' range on a map, tell the story of one hypothetical individual:

> *"It's March. Our Sanderling - let's call her by her band number, B95 - is fattening up on a beach in Tierra del Fuego. In two weeks, she'll begin a 10,000 km journey north. She's done this 19 times before."*

This could be a scrollytelling page (scroll-driven animation) for the 20 showcase species. One long scroll that follows the bird through a year, with the map panning and zooming alongside the narrative. This is the kind of content that gets shared and bookmarked - it's what elevates Aviary from "database" to "experience."

### 6.4 Regional Landing Pages with Personality

The file structure includes `/region/[region]/page.tsx` but the docs don't describe these pages. Regional pages could be powerful for SEO and discovery:

- `/pacific-northwest` - "148 species, 37 migrating this month"
- `/gulf-coast` - "Spring fallout season: trans-Gulf migrants arriving daily"
- `/cape-may` - "September hawk watch: thousands of raptors overhead"

Each regional page could have a brief editorial intro about what makes birding special in that region, plus the local migration map view. These pages are excellent SEO targets ("birds in Pacific Northwest") and create natural entry points beyond the homepage.

### 6.5 Conservation Status as a Quiet Thread

The docs treat conservation as a per-species data point, but it could be woven more meaningfully through the experience:

- On the migration map, paths for declining species could have a subtle visual indicator (slightly more transparent, or a small warning icon in the modal).
- A "conservation spotlight" section on the landing page: "12 species passing through your area this month are declining. Here's what threatens them."
- On species pages, conservation-relevant species could link to actionable resources (local Audubon chapters, habitat preservation organizations).

This keeps the tone "informative, not alarmist" (as the docs specify) while making the data meaningfully present. Birders care deeply about conservation - surfacing it tastefully builds trust and purpose.

### 6.6 Print / Save View for Species Pages

Birders often want to bring information into the field. A print-friendly view or "save as PDF" option for species pages would be genuinely useful. The vintage aesthetic would actually look beautiful in print.

**Suggestion**: Add a `@media print` stylesheet that: hides nav/footer/interactive elements, shows illustration at full resolution, lays out facts in a clean single-column format, includes the scientific name and a QR code back to the digital page.

---

## 7. Missed Decision Points Summary

These need answers before writing code:

| Decision | Options | Recommendation |
|----------|---------|----------------|
| Map library | Custom SVG / Mapbox GL JS / Leaflet | Mapbox GL JS (free tier, WebGL perf, custom styling) |
| Range data format | GeoJSON polygons / Raster tiles / Hybrid | Hybrid: simplified GeoJSON for well-covered species, fallback to coarse ranges |
| Search implementation | Fuse.js / Pagefind / Algolia | Pagefind (static, no bundle cost, designed for static sites) |
| Comments system | Custom Supabase / Utterances / None at launch | Utterances or defer entirely |
| Image hosting | Self-hosted / Cloudinary / R2 bucket | Self-hosted with Next.js Image optimization |
| Build strategy | Full static export / ISR / Hybrid | Hybrid: pre-generate enhanced, ISR for basic |
| Landing page design | Migration map hero / Illustration hero | Migration map (resolve the document contradiction) |
| "Live" data framing | Seasonal averages / Real-time eBird / Hybrid | Seasonal averages with honest labeling; real-time as future enhancement |
| Migration path types | Single reversible path / Spring+fall routes / Pattern-specific | Pattern-specific (support loop, altitudinal, etc.) |
| Content quality tiers | Binary (enhanced/basic) / Continuous spectrum | Continuous 4-5 tier spectrum |
| AI illustration QA | Trust Midjourney output / Expert review pipeline | Review pipeline with ornithological accuracy checks |
| Geocoding service | Mapbox Geocoding / Nominatim / None | Nominatim (free, no dependency) |
| Analytics | Plausible / Umami / None at launch | Plausible (hosted, privacy-first, $9/mo) or self-hosted Umami (free) |

---

## 8. Suggested Priority Reordering

The current roadmap is logical but front-loads the hardest technical risk (data pipeline for 11,000 species) before validating the most important experience (the migration map). Consider reordering:

**Revised Phase 1 (Validate the Vision):**
- Build the migration map with 5-10 hand-crafted paths
- Build 20 showcase species pages (hand-crafted content + illustrations)
- Build search across those 20 species only
- Deploy. Share with birding friends. Get reactions.

**Revised Phase 2 (Build the Foundation):**
- Data pipeline for top 500 species (well-covered, good eBird data)
- Template illustration generation for those 500
- Claude content generation pipeline with QA
- Search across 500 species

**Revised Phase 3 (Scale):**
- Extend to full 11,000 species
- Regional landing pages
- "Birds Around You" with location

**Why**: The migration map is the "soul" of the project. Building it first, even with limited data, validates whether the experience is as compelling as the docs describe. If the map doesn't create a "whoa" moment with 10 paths, it won't create one with 100. Better to discover this with 2 weeks of work invested than 8.

---

## 9. Final Notes

The strongest aspects of this design:
- **Clear aesthetic vision** that avoids both blandness and kitsch
- **Migration map as hero** - genuinely novel in the birding space
- **Honest, respectful ethos** - no dark patterns, no engagement tricks, always free
- **Realistic budget** that doesn't depend on outside funding

The biggest risk is not technical - it's the gap between the hand-crafted showcase experience (20 beautiful species) and the generated mass experience (10,800 template species). Bridging that gap with consistent quality, honest data presentation, and at least one memorable human touch per species is what will make this feel like a gift rather than a generator.

Build the map first. Make 20 species pages perfect. Then scale with care.

---

*Review completed February 14, 2026*
