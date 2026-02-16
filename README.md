# Aviary

**An illustrated birding field guide for the modern web**

![Status](https://img.shields.io/badge/status-in%20development-blue)
![License](https://img.shields.io/badge/license-MIT-green)

---

## Vision

Aviary is a labor of love — a free, beautiful, comprehensive birding resource that combines the aesthetic of vintage Audubon field guides with the power of modern web technology. No ads, no paywalls, no engagement tricks. Just beautiful illustrations, rich migration data, and the joy of discovering birds.

**Live Site:** birding.guide *(coming soon)*

---

## What Makes Aviary Unique

- **Migration Map:** THE defining feature — animated visualization of bird migration across the world, showing what's flying this month (no one else does this)
- **Comprehensive Coverage:** All 11,000+ bird species worldwide, not just common ones
- **Vintage Aesthetic:** AI-generated illustrations in John James Audubon's style
- **Always Free:** No ads, no paywalls, built as a gift to the birding community
- **Modern Performance:** Static-generated, blazing fast, mobile-first
- **Five-Tier Content:** Continuous quality spectrum from hand-crafted showcase pages to honest stubs — every species page has at least one reason to visit

**What makes the Migration Map unique:**
- Shows birds IN MOTION (not just static ranges)
- 5-8 animated paths showing families traveling together
- Time slider reveals migration throughout the year
- Critical stopover sites pulse when active (Delaware Bay in May, etc.)
- AI groups 350+ migrants intelligently to avoid clutter
- Handles diverse migration patterns: loop, altitudinal, irruptive, austral
- Works globally, degrades gracefully in data-sparse regions

**Competitors:** Cornell's All About Birds (functional but not beautiful), Avibase (comprehensive but looks ancient), Sibley Guides (beautiful books but not a web database). 

**Nobody visualizes migration like this.** Existing tools show static range maps. Aviary shows the journey - where birds are going, when they arrive, the spectacles happening this month.

---

## Documentation

- **[README.md](README.md)** - Project overview, vision, and technical architecture
- **[UX_SPECIFICATION.md](UX_SPECIFICATION.md)** - Complete UX design for all pages (Landing/Migration Map, Species, About)
- **[MIGRATION_MAP_SPEC.md](MIGRATION_MAP_SPEC.md)** - Deep technical specification for the Migration Map feature
- **[DESIGN_SUMMARY.md](DESIGN_SUMMARY.md)** - Brief overview for design feedback
- **Master Design Document** (Word doc) - Strategic vision and brand guidelines

---

## Core User Flows

### Primary: Discover Migration (Landing Experience)
1. User lands on birding.guide
2. **Immediately sees Migration Map** - Live animated bird migration across the world
3. Interacts with time slider to watch birds move through the year
4. Optionally taps paths/hotspots to learn about migration groups
5. **Scrolls down** to search for specific bird OR personalize to their location
6. Continues to species pages or local discovery

### Secondary: Search by Bird Name
1. User lands on Migration Map
2. Scrolls down to search bar
3. Types bird name (fuzzy search, handles typos)
4. Lands on species page with sticky illustration, description, range, migration data
5. Optional: Explore similar birds or return to Migration Map

### Tertiary: Explore Birds Around You
1. User sees Migration Map on landing
2. Scrolls to "Birds Around You" button
3. Grants location permission (or enters manually)
4. Map above updates to show local migrations + current month
5. Scrolls down to see complete bird list with filters
6. Clicks any bird → Species detail page

---

## Design Principles

### The Feeling
**Restrained Vintage** — Think museum exhibit, not ren faire.

Walking into a modern natural history museum where the space is clean and well-lit, but the Audubon prints on the walls carry centuries of history. Opening a beautifully designed coffee table book about birds. The quiet satisfaction of identifying a bird you've been wondering about. Discovery that feels like reverence, not distraction.

**Design Philosophy:**
> "Simple enough to use on a phone at the beach, charming enough to linger over on a Sunday morning."

**The Balance:**
- Modern, clean interface structure (no skeuomorphic book spreads or fake page curls)
- Vintage soul comes from illustrations + elegant typography
- Subtle warmth through minimal texture (barely perceptible paper grain)
- Thoughtful details, not ornate decoration
- Everything stays out of the way — let the birds be the stars

**Interface Principles:**
- **Minimal:** Clean layouts, generous white space, clear hierarchy
- **Stunning:** Audubon illustrations do the heavy lifting — UI supports them quietly
- **Intuitive:** Search or explore — no learning curve, no hidden features
- **Respectful:** No dark patterns, no engagement tricks — find, learn, move on
- **Warm:** Subtle texture and elegant typography add soul without decoration

### Visual Identity

**Color Palette:**
- `#FAF7F0` Cream - Primary background, aged paper
- `#1A2F23` Forest - Primary text, headers
- `#2D4A3E` Deep Green - Secondary backgrounds, cards
- `#8B7355` Sepia - Borders, secondary text, vintage accents
- `#B85C38` Rust - Accent color, CTAs, migration paths
- `#A8DADC` Sky Blue - Winter ranges, water birds
- `#C9A05F` Gold - Enhanced species badge

**Typography:**
- Display/Headings: Cormorant Garamond (elegant serif, museum feel)
- Body Text: Crimson Pro (readable serif for long-form)
- Both free from Google Fonts

**Subtle Charm Elements (Where Vintage Shows):**
- Drop cap on first paragraph of enhanced species descriptions
- Slightly looser letter-spacing on headings (classic typography: 0.02em)
- True italic scientific names (proper italics, not slanted roman)
- Very subtle paper grain texture (5-10% opacity, barely perceptible)
- Enhanced species badge: Simple wax seal icon (flat design, not gradient)
- Audio player controls: Brass-colored but minimal (no skeuomorphic radio dials)
- Migration paths: SVG stroke with slight hand-drawn wobble

**What to Avoid:**
- ❌ Heavy texture or distressed effects (no coffee stains, torn edges)
- ❌ Ornate borders or decorative filigree
- ❌ Skeuomorphic UI (no fake book bindings, leather textures, 3D embossing)
- ❌ Gimmicky page-flip animations or forced vintage interactions
- ❌ Too much brown/sepia (keep it bright with cream backgrounds)

**Animations & Interactions (Restrained):**
- Cards lift gently on hover: 4px elevation, subtle shadow increase (like picking up a photograph)
- Staggered fade-up for bird cards: 50ms delays, smooth and purposeful
- Time-of-year slider: Gradient bar with seasonal colors, smooth drag interaction
- Migration paths: Animate on with slight hand-drawn wobble (1-2 seconds, once)
- Page transitions: Simple fades with barely visible texture overlay
- **60fps non-negotiable** — smooth, never jarring, always respectful of user's attention
- **Philosophy:** Delightful but not gimmicky, supports the content, never competes with it

### Interface Philosophy
- **Minimal:** Don't overwhelm — clear search, simple navigation
- **Stunning:** Every pixel intentional, every interaction delightful
- **Intuitive:** No learning curve — search or explore, that's it
- **Respectful:** No dark patterns, no infinite scroll, no engagement tricks

---

## Technical Architecture

### Stack
- **Frontend:** Next.js 14+ (App Router), React 18+
- **Styling:** Tailwind CSS with custom theme
- **Animations:** Framer Motion
- **Search:** Pagefind (static search index, lazy-loaded chunks — no client bundle bloat for 11K species)
- **Maps:** Mapbox GL JS (WebGL-accelerated, custom vintage-styled base map via Mapbox Studio, free tier: 50K loads/month; can migrate to MapLibre GL if needed)
- **Geocoding:** Nominatim (OpenStreetMap, free, no vendor dependency)
- **Deployment:** Vercel (free tier)
- **Images:** Self-hosted with Next.js Image optimization (WebP, responsive sizing, lazy loading)
- **Analytics:** Plausible ($9/mo, privacy-first) or self-hosted Umami (free)

### Data Sources (Tiered by Availability)

**Tier 1 — Rich Data (~1,100 species, mostly Western Hemisphere):**
- **eBird Status & Trends:** Weekly abundance models, seasonal range rasters. Requires access request from Cornell Lab. Raster format (GeoTIFF, 27km resolution) — needs processing pipeline to convert to simplified GeoJSON or render as tile layers. These species get the full migration map experience.

**Tier 2 — Moderate Data (~3,000 species):**
- **eBird API 2.0:** Species taxonomy, regional frequency, recent observations. Free with API key, rate-limited (~100 req/min). Provides seasonal presence data but not weekly abundance. These species get static range maps with seasonal toggles.

**Tier 3 — Basic Data (~7,000 species):**
- **BirdLife/IUCN Range Maps:** Expert-drawn breeding/non-breeding/passage polygons. Requires data request. Coarse resolution. OR **Wikipedia API** text extraction for range descriptions. These species get text-based range descriptions and coarse maps.

**All Tiers:**
- **Xeno-canto:** Bird call recordings (Creative Commons). Coverage is uneven — excellent for NA/Europe, sparse for many tropical species. Tag `audioAvailable: boolean` per species; hide sounds section when no quality recordings exist.
- **Wikipedia API:** Species summaries (attributed). Stub articles (<50 words) for many recently-split or obscure tropical species — fall back to family-level description + field marks from eBird taxonomy.
- **Audubon Prints:** Public domain for AI style reference.
- **BirdCast (Cornell):** Live nocturnal migration forecasts for North America. Future integration for genuine real-time data.

**Important:** The `dataTier` field in the species schema drives what UI components are shown per species. Tier 1 gets the full experience; Tier 3 gets a graceful, honest minimal page.

### Build Strategy: Hybrid Static + ISR
Pre-generate showcase and enhanced species at build time; use Incremental Static Regeneration for the remaining 10,800+ species pages.

**Why hybrid:**
- Vercel free tier has a 45-minute build limit. Full static generation of 11,000+ pages will hit this.
- ISR generates pages on first visit, then caches at CDN. No build-time cost for basic species.
- Showcase species are always instant (pre-built). Basic species are fast after first visit.

**Build Tiers:**
1. **Pre-generated at build time (~500 pages):** All Showcase + Enhanced + Curated species. Always instant.
2. **ISR on first visit (~10,500 pages):** Standard and Stub species. Generated on-demand, cached globally.
3. **Revalidation:** Weekly for Tier 1 data species, monthly for Tier 2-3.

**Data Pipeline:**
1. Bulk download eBird taxonomy (single request, not per-species)
2. Batch fetch Wikipedia summaries with rate limiting and caching
3. Process eBird Status & Trends rasters for Tier 1 species (convert to simplified GeoJSON or tile layers)
4. Fetch BirdLife range polygons for Tier 2-3 species
5. Generate static JSON files per species with `dataTier` field
6. Incremental updates only — don't re-fetch unchanged data

**Offline Support (Phase 3+):**
Service worker caches visited species pages for offline re-access. Search index cached after first load. "You're offline" indicator instead of broken pages. Aligns with "phone at the beach" use case.

---

## Content Strategy

### Five-Tier Quality Spectrum

The difference between "soulless" and "cared-for" is often one specific, memorable detail. Rather than a binary enhanced/basic split with an obvious quality cliff, content quality is a continuous spectrum. Every species page should have at least one reason to visit.

**Tier 1 — Showcase (20 species):**
Everything hand-crafted. The portfolio pieces.
- Custom illustration showing characteristic behavior in habitat context
- Multiple plumage variations (breeding/non-breeding/juvenile)
- Hand-written naturalist prose (2-3 paragraphs, unique voice)
- "Follow This Journey" scrollytelling for migrants (scroll-driven narrative following one bird through a year)
- Interactive migration map with animated paths and stopovers
- Curated audio with hand-written descriptions
- Detailed conservation context with actionable links
- These pages are what you share with friends. They set the standard.

**Tier 2 — Enhanced (200 species):**
Custom illustrations, refined AI prose with human editing pass.
- Unique AI illustration (custom composition, manually reviewed for anatomical accuracy)
- AI-generated content with human editing pass — every summary hand-read
- Migration map with animated paths (for migrants)
- Curated Xeno-canto recordings (hand-selected for quality)
- Similar species comparison
- Conservation status and trends

**Tier 3 — Curated (1,000 species):**
Template illustrations but with human editorial touches.
- Template AI illustration (reviewed for accuracy)
- AI-generated content (validated by template)
- One human-curated "hook" — a genuinely interesting, specific fact written by a human that makes this page worth visiting. This is the key investment: breadth of care over depth.
- Hand-selected Xeno-canto recording (if quality recording exists)
- Seasonal range map (Tier 1 or 2 data)
- Manually verified facts

**Tier 4 — Standard (5,000 species):**
Fully automated, quality-checked.
- Template AI illustration (batch-generated, spot-checked)
- AI-generated content from validated template
- Automated quality checks (length, format, specificity)
- Range map (Tier 2 or 3 data, may be coarse)
- Audio if available (auto-selected from Xeno-canto, quality-filtered)

**Tier 5 — Stub (5,000 species):**
Minimal but honest.
- Placeholder illustration (family-representative template)
- Basic data: names, family, range description (text), conservation status
- "Help us improve this page" CTA
- Clearly presented as minimal — no pretense of completeness
- Graceful, clean page that respects the user's time

---

## AI Illustration Strategy

### Approach: Two-Track System with QA Pipeline

**Track 1: Template-Based (10,800+ species)**
- Standard poses: perched side view, front view, in flight, standing, floating
- Consistent style using `--sref` parameter with a single validated Audubon reference (same reference image across ALL batches to prevent style drift)
- Batch processing by taxonomic family (all sparrows, all warblers, etc.) — family-specific prompt templates tuned for body type accuracy
- Time per bird: 5-10 minutes
- Result: Visually consistent, beautiful coverage

**Track 2: Unique Compositions (200+ priority species)**
- Custom poses showing characteristic behavior
- Multiple illustrations for dramatic plumage changes
- Habitat context with plants and setting
- Time per bird: 20-40 minutes
- Result: Showcase-quality illustrations

### Illustration QA Pipeline

AI illustration is the aesthetic backbone of the project. A bad illustration (wrong bill shape, impossible wing position, wrong species entirely) will be noticed by birders and shared as a negative example. Quality control is non-negotiable.

**Family-Specific Prompt Library:**
Build a tested prompt template per taxonomic family that produces anatomically correct results for that body type. Test each template with 5-10 species before batch use. Document known failure modes per family (e.g., hummingbird bill length, raptor talon detail, shorebird leg proportions).

**Accuracy Review Process:**
1. Generate batch of 50-100 illustrations per family
2. Automated check: correct aspect ratio, no artifacts, correct background style
3. Birder review (2-3 people): check anatomical accuracy, species distinctiveness, pose plausibility
4. Quality rating per illustration: `approved | needs-regeneration | flagged`
5. Regenerate flagged illustrations with refined prompts
6. Only approved illustrations go live

**Common AI Art Failure Modes to Watch For:**
- Wrong number of toes (birds have 4, AI sometimes gives 3 or 5)
- Impossible wing positions (partially folded in ways that don't work anatomically)
- Bill shape/length errors (especially for families with distinctive bills)
- Eye placement wrong (too centered, too forward)
- Species confusion (generates a similar but wrong species)
- Background style inconsistency (some get botanicals, others get blank space)

### Tools
- **Primary:** Midjourney ($10/month)
- **Alternative:** Stable Diffusion (free, more technical)
- **Editing:** Canva Pro (already covered)

### Example Prompt (Template)
```
[BIRD NAME], perched side view on oak branch, scientific bird
illustration in the style of John James Audubon, watercolor on
aged cream paper, detailed feathers, minimal botanical elements,
1800s naturalist field guide --sref [STYLE-URL] --style raw --ar 2:3
```

### Example Prompt (Enhanced - Sanderling)
```
Sanderling in winter plumage running at water's edge, small shorebird,
pale gray upperparts white underparts, stout black bill, ocean wave
washing onto sand, scientific bird illustration in the style of
John James Audubon, watercolor, sandy beach background, naturalist
field guide, 1800s ornithological print --sref [STYLE-URL]
--style raw --ar 2:3
```

### Cost
- 100 species: ~$20 (2 months Midjourney)
- All 11,000 species: ~$60 (6 months at $10/mo)
- Cost per bird: ~$0.005 (vs $200-500 for human illustrator)

### Timeline to Full Coverage
- Month 1: 20 unique + 100 template species (with full QA review on all)
- Months 2-6: Batch-generate remaining ~10,900 (200-500/weekend, spot-check 10% per batch)
- Ongoing: Continue enhancing 3-5 priority species/week, regenerate any flagged illustrations

---

## Migration Visualization

### The Killer Feature
Most bird guides tell you where a bird lives. Aviary shows you **where it travels**, **when it arrives**, and the **epic journeys** it undertakes.

### Components

**Time-of-Year Slider:**
- Interactive slider (Jan-Dec) controlling map
- Gradient bar with seasonal colors
- Smooth transitions between months
- Current month displayed prominently

**Interactive Migration Map:**
- Base: Simplified world/continental map (vintage style)
- Seasonal ranges: Color-coded polygons
  - Breeding = Rust (#B85C38)
  - Winter = Sky Blue (#A8DADC)
  - Year-round = Forest (#1A2F23)
- Migration routes: Animated dashed paths
- Critical stopover sites: Pulsing markers with labels
- Smooth morphing as slider moves

**Migration Narrative:**
- Hand-written story explaining the journey (enhanced species)
- Changes contextually based on selected month
- Example: "May - Critical refueling at Delaware Bay" for Sanderling

**Migration Statistics:**
- Distance traveled (one-way and round-trip)
- Latitudinal range
- Speed/duration
- Key timing windows

### Regional Focus
- "Birds Near Me" - detects location, shows current month's birds
- Regional pages (e.g., `/northeast`, `/pacific-northwest`)
- Filter by status: year-round residents, summer breeders, winter visitors, migrants

---

## Data Structure

### Species JSON Schema
```json
{
  "speciesCode": "amerob",
  "commonName": "American Robin",
  "scientificName": "Turdus migratorius",
  "family": "Thrushes (Turdidae)",
  "familyScientific": "Turdidae",

  "contentTier": "showcase",
  // "showcase" | "enhanced" | "curated" | "standard" | "stub"
  // Drives which UI components are shown on the species page

  "dataTier": 1,
  // 1 = Full eBird Status & Trends (weekly abundance, ~1,100 spp)
  // 2 = eBird API ranges (seasonal presence, ~3,000 spp)
  // 3 = BirdLife/Wikipedia only (coarse ranges, ~7,000 spp)
  // Drives map rendering strategy and data-driven granularity

  "illustration": {
    "type": "ai-generated",
    "style": "audubon",
    "template": "perched-side-view",
    "url": "/images/species/amerob.webp",
    "plumages": ["breeding", "nonbreeding"],
    "qualityStatus": "approved",
    // "approved" | "needs-regeneration" | "placeholder"
    "attribution": "AI-generated in the style of John James Audubon"
  },

  "description": {
    "summary": "Wikipedia summary or Claude-generated...",
    "enhanced": "Hand-written naturalist prose (Tier 1-2 only)...",
    "hook": "One human-curated memorable fact (Tier 3)..."
    // The hook is the key anti-slop investment for Curated tier.
    // One genuinely specific, surprising detail written by a human.
  },

  "ranges": {
    "breeding": { /* GeoJSON polygon or null */ },
    "winter": { /* GeoJSON polygon or null */ },
    "yearRound": { /* GeoJSON polygon or null */ },
    "rangeDescription": "Fallback text when GeoJSON unavailable"
  },

  "migration": {
    "migrationPattern": "latitudinal",
    // "latitudinal" | "altitudinal" | "loop" | "longitudinal" |
    // "austral" | "irruptive" | "nomadic" | "sedentary"
    // Drives path rendering logic and narrative generation

    "migrationVariability": "partial",
    // "obligate" = all populations migrate
    // "partial" = some populations migrate, some resident
    // "facultative" = migration depends on conditions
    // Drives narrative: "Northern populations migrate; mid-Atlantic birds stay year-round"

    "migrationTiming": "nocturnal",
    // "diurnal" | "nocturnal" | "crepuscular" | "both"
    // Most songbirds migrate at night. Enriches narrative.

    "springRoute": [
      { "lat": 35, "lng": -90, "month": 3, "label": "Departs Gulf States" },
      { "lat": 45, "lng": -93, "month": 4, "label": "Arrives Great Lakes" },
      { "lat": 62, "lng": -135, "month": 5, "label": "Reaches Alaska" }
    ],
    "fallRoute": [
      // Separate route geometry for loop migrants (may differ from spring)
      // For non-loop migrants, can be null (fall route = reverse of spring)
    ],
    "timing": {
      "spring": "March-May",
      "fall": "September-November"
    },
    "narrative": {
      "march": "First robins return to northern states...",
      "may": "Breeding season begins across Canada..."
    },
    "stats": {
      "distanceKm": 3000,
      "latitudeRange": 45,
      "speed": "30-40 miles/day"
    },
    "confidence": "high"
    // "high" (8+/10) | "medium" (5-7) | "low" (<5)
    // High: solid animated paths. Medium: dashed, slightly transparent.
    // Low: not shown on map, text-only range description.
  },

  "audioAvailable": true,
  // Drives whether Sounds section is shown at all.
  // Many tropical species have 0 quality recordings — hide section entirely.
  "audio": [
    {
      "type": "song",
      "url": "xeno-canto-link",
      "qualityRating": "A",
      // Xeno-canto quality rating. Only show recordings rated B or above.
      "description": "Cheerful caroling 'cheerily cheer-up'"
    }
  ],

  "conservation": {
    "status": "Least Concern",
    "trend": "Stable",
    "threats": ["Pesticides", "Window strikes"]
  },

  "size": "medium",
  // "tiny" | "small" | "medium" | "large" | "very-large"
  // Used for "What's That Bird?" quick ID flow
  "primaryColors": ["orange", "gray", "black"],
  // Used for "What's That Bird?" quick ID flow
  "habitat": ["woodland", "suburban", "grassland"]
  // Used for filtering, similar birds, quick ID
}
```

---

## Development Roadmap

### Philosophy: Validate the Vision First

The migration map is the soul of the project. Build it first with limited data. If it doesn't create a "whoa" moment with 10 paths, it won't create one with 100. Better to discover this with 2 weeks invested than 8.

### Phase 1: Validate the Vision (Weeks 1-4)
**Build the experience before the database.**

**Migration Map (The Hero):**
- [ ] Set up Mapbox GL JS with vintage-styled base map (muted earth tones via Mapbox Studio)
- [ ] Build time slider component (Jan-Dec, current month default)
- [ ] Manually define 5-10 showcase migration paths (well-studied, iconic species):
  - Sanderling (Atlantic Flyway), Arctic Tern (global champion), Bar-tailed Godwit (Pacific record-holder), Ruby-throated Hummingbird (Gulf crossing), Swainson's Hawk (Americas), American Golden-Plover (loop migration)
- [ ] Implement SVG path rendering with CSS particle animations
- [ ] Month-based opacity/animation state changes
- [ ] Tap path → modal with group details
- [ ] Play button (auto-animate through year, 30 seconds)
- [ ] Responsive: 75% viewport on mobile, split-view on desktop

**Showcase Species Pages (20 birds):**
- [ ] Next.js project setup with Tailwind CSS + Cormorant Garamond / Crimson Pro
- [ ] Species detail page: sticky illustration, facts, audio, map, similar birds
- [ ] Family context bar below bird name (e.g., "Thrushes (Turdidae) - 45 species")
- [ ] Hand-write content for all 20 showcase species (establish voice and quality bar)
- [ ] Generate unique illustrations for 20 showcase species (with birder QA review)
- [ ] Curate Xeno-canto recordings for 20 species

**Search (Minimal):**
- [ ] Pagefind search index across 20 species
- [ ] Persistent search icon in header (accessible from migration map without scrolling)

**Deploy and Test:**
- [ ] Deploy to Vercel
- [ ] Share with 5-10 birding friends. Get honest reactions.
- [ ] Does the migration map create a "whoa" moment? Does a species page make someone linger?

**Deliverable:** A beautiful, tiny site: migration map + 20 stunning species pages. Proof of concept.

### Phase 2: Build the Foundation (Weeks 5-10)
**Scale from 20 to 500 species with data pipeline.**

**Data Pipeline:**
- [ ] eBird API integration (bulk taxonomy download, not per-species)
- [ ] eBird Status & Trends access request + raster processing pipeline (GeoTIFF → simplified GeoJSON)
- [ ] Wikipedia API script with fallback strategy (<50 words → family-level description)
- [ ] Xeno-canto integration with quality filtering (rating B+ only)
- [ ] Generate static JSON files for top 500 species (well-covered, good eBird data)
- [ ] Assign `dataTier` and `contentTier` per species

**Content Pipeline:**
- [ ] Build Claude content generation template with slop detector (see Content QA)
- [ ] Generate + review content for 200 Enhanced species (human editing pass on every one)
- [ ] Generate + spot-check content for 300 Curated species
- [ ] Write one human-curated "hook" fact for each Curated species
- [ ] Illustration: generate template illustrations for 500 species with family-specific prompts
- [ ] Birder QA review on 100-illustration batches

**Migration Map (Expanded):**
- [ ] AI path inference pipeline (Claude generates paths from eBird data + flyway knowledge)
- [ ] Scale to 100+ paths, group into 5-8 flows per location
- [ ] Add irruptive species visualization mode (expanding/contracting range pulse)
- [ ] Add loop migration support (separate spring/fall route geometries)
- [ ] Confidence-based visual treatment (solid=high, dashed=medium, hidden=low)
- [ ] Define 20-30 critical hotspots with pulsing markers and narratives

**New Features:**
- [ ] "What's That Bird?" quick ID flow (size + color + habitat → filtered matches by location/season)
- [ ] "Birds Around You" with Nominatim geocoding
- [ ] Hybrid ISR build strategy (500 pre-generated, rest on-demand)
- [ ] Add analytics (Plausible or Umami)

**Deliverable:** 500-species site with working data pipeline, expanded migration map, quick ID feature.

### Phase 3: Scale to Full Coverage (Months 3-6)
**Extend to all 11,000+ species.**

- [ ] Batch-generate template illustrations for remaining ~10,500 species (200-500/weekend, 10% QA per batch)
- [ ] Claude content generation for all Standard tier species (batches of 500, spot-check 1%)
- [ ] Stub pages for Tier 3 data species (minimal but clean, "help us improve" CTA)
- [ ] Migration map global expansion: trans-Saharan routes, East Asian-Australasian flyway, Neotropical
- [ ] Southern Hemisphere season handling (austral migration, reversed season labels)
- [ ] Regional landing pages with editorial intros (Pacific Northwest, Gulf Coast, Cape May, etc.)
- [ ] Seabird/pelagic species: ocean-based path rendering with wave-like visual treatment
- [ ] Performance profiling on mobile devices, progressive enhancement for low-end
- [ ] Service worker for offline support (cache visited pages, search index)
- [ ] Print-friendly species pages (`@media print` stylesheet)

**Deliverable:** Complete 11,000+ species coverage, global migration map, regional pages.

### Phase 4: Deepen and Polish (Ongoing, Forever)
**Slow, steady craft. No deadlines.**

- [ ] Enhance 3-5 species/week based on analytics + community interest
- [ ] Promote species up the content tier ladder (Stub → Standard → Curated → Enhanced)
- [ ] Write one human "hook" fact per species as time allows (long-term investment in breadth of care)
- [ ] BirdCast integration for genuine real-time nocturnal migration data (North America)
- [ ] "Follow This Journey" scrollytelling for more species (start with 5, add slowly)
- [ ] Conservation spotlight: monthly feature on declining species passing through user's area
- [ ] Historical migration comparison (routes shifting with climate change)
- [ ] Community features if warranted (utterances comments, birder-contributed corrections)
- [ ] Continue improving illustration quality (regenerate flagged illustrations, refine prompts)

---

## File Structure

```
aviary/
├── public/
│   ├── images/
│   │   ├── species/           # AI-generated illustrations (WebP, optimized)
│   │   ├── maps/              # Migration map assets
│   │   └── ui/                # Icons, logos, textures
│   └── data/
│       └── species/           # Static JSON files (one per species)
├── src/
│   ├── app/
│   │   ├── page.tsx           # Landing page (migration map + search)
│   │   ├── species/
│   │   │   └── [code]/
│   │   │       └── page.tsx   # Species detail page
│   │   ├── family/
│   │   │   └── [family]/
│   │   │       └── page.tsx   # Family browse page (e.g., /family/thrushes)
│   │   ├── region/
│   │   │   └── [region]/
│   │   │       └── page.tsx   # Regional page with editorial intro
│   │   ├── identify/
│   │   │   └── page.tsx       # "What's That Bird?" quick ID flow
│   │   ├── about/
│   │   │   └── page.tsx       # About page
│   │   └── layout.tsx         # Root layout
│   ├── components/
│   │   ├── BirdSearch.tsx     # Pagefind search with autocomplete
│   │   ├── BirdCard.tsx       # Species card in list views
│   │   ├── MigrationMap.tsx   # Interactive migration visualization (Mapbox GL JS)
│   │   ├── TimeSlider.tsx     # Month-by-month slider
│   │   ├── AudioPlayer.tsx    # Embedded audio player (lazy-load audio)
│   │   ├── QuickID.tsx        # "What's That Bird?" size/color/habitat filter
│   │   ├── FamilyBar.tsx      # Contextual family link below bird name
│   │   ├── Navigation.tsx     # Site header/nav with persistent search icon
│   │   └── PrintView.tsx      # Print-friendly species layout
│   ├── lib/
│   │   ├── data/
│   │   │   ├── fetch-ebird.ts      # eBird API integration (bulk + incremental)
│   │   │   ├── fetch-wikipedia.ts  # Wikipedia API with fallback strategy
│   │   │   ├── fetch-ranges.ts     # eBird Status & Trends (raster processing)
│   │   │   └── fetch-audio.ts      # Xeno-canto with quality filtering (B+ only)
│   │   ├── content/
│   │   │   ├── generate.ts         # Claude content generation pipeline
│   │   │   └── slop-detector.ts    # Quality checks: ban list, specificity, uniqueness
│   │   ├── search.ts          # Pagefind integration
│   │   └── utils.ts           # Helpers
│   └── styles/
│       ├── globals.css        # Tailwind config + custom styles
│       └── print.css          # @media print stylesheet
├── scripts/
│   ├── build-data.ts          # Generate all species JSON files (incremental)
│   ├── fetch-all.ts           # Fetch from all APIs (with caching + rate limiting)
│   ├── process-ranges.ts      # Convert eBird GeoTIFF rasters to GeoJSON
│   ├── generate-content.ts    # Batch Claude content generation with QA
│   ├── optimize-images.ts     # Image optimization pipeline
│   └── illustration-qa.ts    # Illustration quality tracking and flagging
├── tailwind.config.js         # Custom theme (colors, fonts)
├── next.config.js             # Hybrid ISR config
└── README.md                  # This file
```

---

## Landing Page Design

### Philosophy: Migration Map First
**The Migration Map IS the landing page.** No traditional landing page — lead with the unique feature immediately.

Users see animated bird migration the moment they arrive. Search and discovery are below the spectacle.

```
┌─────────────────────────────────────────┐
│ AVIARY                             [≡]  │  ← Minimal header
├─────────────────────────────────────────┤
│                                         │
│                                         │
│     [FULL-SCREEN MIGRATION MAP]         │  ← THE feature
│                                         │     Animated paths
│     [5-8 flowing migration paths]       │     Global view
│     [Pulsing hotspots]                  │     Current month
│                                         │
│     Bird Migration This Month            │  ← Simple overlay
│     Across the World                    │     Bottom of map
│                                         │
│     Jan ━━━━━●━━━━━━━━━━━━━━━━ Dec     │  ← Time slider
│     [▶ Play]          [Legend ≡]        │     Controls
│                                         │
│           ↓ Scroll to explore           │  ← Hint (fades)
└─────────────────────────────────────────┘
              ↓ Scroll
┌─────────────────────────────────────────┐
│                                         │
│         Discover Any Bird               │  ← Search section
│                                         │
│    ┌──────────────────────────────┐    │  ← Search bar
│    │  Search by bird name...      │    │     Below the map
│    └──────────────────────────────┘    │
│                                         │
│    [See Birds Around You →]             │  ← Personalizes map
│                                         │     above
│                                         │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│ Spectacular Journeys                    │  ← Featured (optional)
│ [3-4 enhanced species cards]            │
└─────────────────────────────────────────┘
```

### Key Interactions
1. **Migration Map:**
   - Animates on load (staggered entrance)
   - Time slider updates paths dynamically
   - Tap path → Modal with migration group details
   - Tap hotspot → Details about that stopover
   - Play button → Auto-animate through year

2. **Search:**
   - Scroll down to reach
   - Fuzzy autocomplete
   - Direct navigation to species page

3. **Birds Around You:**
   - Requests location
   - Scrolls back up to map
   - Zooms map to user's region
   - Shows local migrations

2. **Birds Around You button:**
   - One click → location detection (or manual entry)
   - Smooth transition to filtered grid of local birds
   - Shows current month by default
   - Time slider appears to explore other months

3. **Featured cards:**
   - 3-4 enhanced species showcasing the vision
   - Beautiful illustration + short excerpt
   - Click to see full species page
   - Rotate weekly or seasonal

---

## First 20 Showcase Species

Selected to represent diverse migration patterns, habitats, and regions — and to exercise every content type the system needs to handle.

**Backyard Favorites (6):**
- American Robin *(partial migrant — some stay, some go; tests partial migration handling)*
- Northern Cardinal *(sedentary year-round; tests resident species page)*
- Blue Jay *(partial migrant, some years irruptive)*
- Black-capped Chickadee *(resident with occasional irruptions)*
- American Goldfinch *(short-distance migrant with dramatic plumage change)*
- Mourning Dove *(partial migrant)*

**Charismatic Icons (5):**
- Bald Eagle *(partial migrant, latitudinal)*
- Great Horned Owl *(mostly sedentary)*
- Ruby-throated Hummingbird *(Gulf crossing — one of the most dramatic feats in birding)*
- Red-tailed Hawk *(partial migrant, some populations altitudinal)*
- Barn Swallow *(long-distance, nearly global distribution)*

**Migration Pattern Showcases (9):**
- Sanderling *(classic long-distance latitudinal, Atlantic Flyway)*
- Arctic Tern *(pole-to-pole, the global champion)*
- American Golden-Plover *(loop migration — different routes spring vs fall)*
- Bar-tailed Godwit *(longest non-stop flight, Pacific record-holder)*
- Mountain Quail *(altitudinal migrant — walks downhill in winter)*
- Common Loon *(lakes to ocean, habitat shift migration)*
- Snowy Owl *(irruptive — unpredictable, some years flood south)*
- Atlantic Puffin *(pelagic — spends months at sea, tests ocean rendering)*
- Sooty Shearwater *(circumnavigates the Pacific — tests global pelagic paths)*

---

## Budget & Costs

### Already Covered
- Claude subscription (development help, content writing)
- Canva Pro (image editing)
- UX/design expertise (you + wife)

### From $100 Budget
- **Domain (birding.guide):** ~$30/year
- **Midjourney (2 months):** $20
- **Analytics (optional):** $0-9/month
- **Initial spend:** $50
- **Reserve:** $50 for future needs

### Free / Low-Cost Infrastructure
- Vercel hosting: Free tier (100GB bandwidth)
- Mapbox GL JS: Free tier (50K map loads/month)
- All data APIs: Free (eBird, Wikipedia, Xeno-canto)
- Images: Self-hosted via Next.js Image optimization (no CDN dependency)
- **Ongoing monthly:** $2-3 (domain) + $9 (Plausible analytics, optional)

---

## Community Support

### How People Can Help
- **Ko-fi or Buy Me a Coffee:** Simple one-time donations
- **Suggested amounts:** $3-5 ("coffee") or $10-20 ("cover hosting")
- **Conservation option:** Donate to Audubon, Cornell Lab, or local orgs

### Transparency
- Simple "Support" page showing actual monthly costs
- Any donations beyond costs → bird conservation
- No donor perks, no exclusive content
- Everyone gets the same beautiful experience
- Optional supporters thank-you page (opt-in)

---

## Design Principles for Development

### 1. Beauty First
Aesthetics are core value, not decoration. If it doesn't look beautiful, it doesn't ship. 60fps animations non-negotiable. Colors, typography, spacing all matter deeply.

### 2. Crafted, Not Generated
Enhanced descriptions written by hand, not AI. Illustrations AI-assisted but manually curated. Each enhancement is a small act of care.

### 3. Progressive Enhancement
Site works without JavaScript. Search works without fancy features. Core functionality rock-solid, enhancements delightful.

### 4. Respect for Attention
No dark patterns, no engagement tricks, no infinite scroll. Users find what they need, learn something delightful, move on.

### 5. Mobile-First Beauty
Most people use this on phones while looking at birds. Design for that moment — glanceable, readable in sunlight, fast on slow connections.

### 6. Performance = Respect
Every byte matters. Optimize ruthlessly. Lazy load everything. Static generation means no wait times. Fast is a feature.

---

## Getting Started

### Prerequisites
- Node.js 18+
- npm or yarn
- eBird API key (free from ebird.org/api)

### Installation
```bash
# Clone the repository
git clone https://github.com/yourusername/aviary.git
cd aviary

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env.local
# Add your eBird API key to .env.local

# Fetch initial data
npm run fetch-data

# Start development server
npm run dev

# Open http://localhost:3000
```

### Build for Production
```bash
# Generate all static pages
npm run build

# Preview production build
npm run start

# Deploy to Vercel
vercel deploy
```

---

## Contributing

This is a personal passion project, but thoughtful contributions are welcome:

- **Bug reports:** Open an issue with reproduction steps
- **Feature suggestions:** Open a discussion (not all features align with vision)
- **Content:** Spot an error in species data? Let us know
- **Illustrations:** Have a better Audubon-style generation technique? Share it

**What we won't accept:**
- Features that add complexity for marginal benefit
- Anything that compromises performance or beauty
- Ads, tracking, or monetization features
- "Engagement" features (notifications, infinite scroll, etc.)

---

## License

MIT License - see LICENSE file for details

**Attribution:**
- AI-generated illustrations: "AI-generated in the style of John James Audubon"
- Data sources: eBird, Wikipedia, Macaulay Library, Xeno-canto (all attributed)
- Original content: © 2026 Aviary Project

---

## What Success Looks Like

We're not tracking growth metrics. Success is:

- ✅ A kid discovers what that blue bird in their yard is called
- ✅ A teacher uses it for a migration lesson
- ✅ Someone planning a trip learns what birds they might see
- ✅ A birder stumbles upon it and thinks "this is beautiful"
- ✅ Donations cover hosting and support conservation
- ✅ You're proud to show friends and family
- ✅ It brings you joy to work on

---

## Contact & Support

- **Website:** birding.guide *(coming soon)*
- **Support the project:** [Ko-fi link]
- **Issues:** GitHub Issues
- **Email:** hello@birding.guide

---

**This is a gift to the internet. Let's build it like one.**

*— Aviary Project, February 2026*
