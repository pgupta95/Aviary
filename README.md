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

- **Live Migration Map:** THE defining feature — animated visualization of bird migration happening right now across the world (no one else does this)
- **Comprehensive Coverage:** All 11,000+ bird species worldwide, not just common ones
- **Vintage Aesthetic:** AI-generated illustrations in John James Audubon's style
- **Always Free:** No ads, no paywalls, built as a gift to the birding community
- **Modern Performance:** Static-generated, blazing fast, mobile-first
- **Two-Tier Content:** Basic data for all species, enhanced storytelling for priority birds

**What makes the Migration Map unique:**
- Shows birds IN MOTION (not just static ranges)
- 5-8 animated paths showing families traveling together
- Time slider reveals migration throughout the year
- Critical stopover sites pulse when active (Delaware Bay in May, etc.)
- AI groups 350+ migrants intelligently to avoid clutter
- Works globally, degrades gracefully in data-sparse regions

**Competitors:** Cornell's All About Birds (functional but not beautiful), Avibase (comprehensive but looks ancient), Sibley Guides (beautiful books but not a web database). 

**Nobody visualizes migration like this.** Existing tools show static range maps. Aviary shows the journey - where birds are going, when they arrive, the spectacles happening right now.

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
- **Search:** Fuse.js (client-side fuzzy search)
- **Maps:** SVG-based custom or Mapbox GL JS
- **Deployment:** Vercel (free tier)
- **Images:** Cloudinary or self-hosted optimized
- **Analytics:** Plausible or Umami (privacy-focused)

### Data Sources (All Free)
- **eBird API 2.0:** Species taxonomy, ranges, seasonal frequency
- **eBird Status & Trends:** Weekly abundance maps for migration
- **Wikipedia API:** Species summaries (attributed)
- **Macaulay Library:** Photo links
- **Xeno-canto:** Bird call recordings (Creative Commons)
- **Audubon Prints:** Public domain for AI style reference

### Build Strategy: Static Site Generation
Generate all 11,000+ species pages once at build time, serve as static HTML from CDN.

**Why:**
- Blazing fast (no database queries, no server rendering)
- Virtually free to host (Vercel's free tier: 100GB bandwidth)
- Resilient (works even if APIs go down)
- Simple (no backend to maintain)

**Build Process:**
1. Weekly automated data fetch from eBird API
2. Fetch Wikipedia summaries for all species
3. Fetch seasonal range data from eBird Status & Trends
4. Generate static JSON files for each species
5. Next.js builds 11,000+ static pages (`getStaticProps` + `generateStaticPaths`)
6. Deploy to Vercel edge network globally
7. Build time: ~30-45 minutes

---

## Content Strategy

### Two-Tier Model

**Basic Coverage (All 11,000+ Species):**
- Common and scientific names
- Wikipedia summary (attributed)
- Range maps and seasonal presence
- Template-based AI illustration (Audubon style)
- Links to Macaulay Library photos
- Links to Xeno-canto recordings
- Comprehensive and functional from day one

**Enhanced Coverage (200+ Priority Species):**
- Unique AI-generated illustration (custom composition)
- Multiple plumage variations (breeding/non-breeding)
- 2-3 paragraphs original naturalist prose
- Interactive migration map with animated paths
- Time-of-year slider showing monthly presence
- Migration narrative with stopover sites
- Migration statistics (distance, timing, routes)
- Embedded audio player with curated recordings
- Similar species comparison
- Conservation status and trends

---

## AI Illustration Strategy

### Approach: Two-Track System

**Track 1: Template-Based (10,800+ species)**
- Standard poses: perched side view, front view, in flight, standing, floating
- Consistent style using `--sref` parameter with Audubon reference
- Batch processing by family (all sparrows, all warblers)
- Time per bird: 5-10 minutes
- Result: Visually consistent, beautiful coverage

**Track 2: Unique Compositions (200+ priority species)**
- Custom poses showing characteristic behavior
- Multiple illustrations for dramatic plumage changes
- Habitat context with plants and setting
- Time per bird: 20-40 minutes
- Result: Showcase-quality illustrations

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
- Month 1: 20 unique + 100 template species
- Months 2-6: Batch-generate remaining ~10,900 (200-500/weekend)
- Ongoing: Continue enhancing 3-5 priority species/week

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
  "enhanced": true,
  
  "illustration": {
    "type": "ai-generated",
    "style": "audubon",
    "template": "perched-side-view",
    "url": "/images/species/amerob.webp",
    "plumages": ["breeding", "nonbreeding"],
    "attribution": "AI-generated in the style of John James Audubon"
  },
  
  "description": {
    "basic": "Wikipedia summary...",
    "enhanced": "Hand-written naturalist prose..."
  },
  
  "ranges": {
    "breeding": { /* GeoJSON polygon */ },
    "winter": { /* GeoJSON polygon */ },
    "yearRound": { /* GeoJSON polygon */ }
  },
  
  "migration": {
    "type": "long-distance",
    "pattern": "northbound-spring-southbound-fall",
    "routes": [
      { "lat": 45, "lng": -93, "month": 4, "label": "Arrives Great Lakes" },
      { "lat": 62, "lng": -135, "month": 5, "label": "Reaches Alaska" }
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
    }
  },
  
  "audio": [
    {
      "type": "song",
      "url": "xeno-canto-link",
      "description": "Cheerful caroling 'cheerily cheer-up'"
    }
  ],
  
  "conservation": {
    "status": "Least Concern",
    "trend": "Stable",
    "threats": ["Pesticides", "Window strikes"]
  }
}
```

---

## Development Roadmap

### Phase 1: Foundation (Weeks 1-4)
**Data Pipeline:**
- [ ] eBird API integration (fetch complete taxonomy)
- [ ] Wikipedia API script (species summaries)
- [ ] eBird Status & Trends integration (seasonal ranges)
- [ ] Image pipeline setup (Macaulay Library links)
- [ ] Generate static JSON files for all 11,000+ species

**Core Application:**
- [ ] Next.js project setup with Tailwind CSS
- [ ] Implement static page generation for all species
- [ ] Build client-side fuzzy search (Fuse.js)
- [ ] Location detection and filtering
- [ ] Time-of-year slider with seasonal filtering
- [ ] Apply complete visual design (colors, typography, animations)

**Deliverable:** Functional MVP with all 11,000+ species, beautiful interface, basic content

### Phase 2: Enhanced Content & Illustrations (Weeks 5-8)
- [ ] Midjourney setup + style reference library
- [ ] Generate template illustrations for 100 common species
- [ ] Generate unique illustrations for 20 priority enhanced species
- [ ] Write original naturalist descriptions for 20 birds
- [ ] Build migration map visualization component
- [ ] Add analytics (Plausible or Umami)
- [ ] Implement enhanced species features (audio, multiple plumages, conservation alerts)

**Deliverable:** Live site with 120 illustrated species including 20 fully enhanced showcase birds

### Phase 3: Full Coverage (Months 2-6)
- [ ] Batch-generate template illustrations for remaining ~10,900 species
- [ ] Process 200-500 species per weekend session
- [ ] Continue enhancing 3-5 priority species per week

**Deliverable:** Complete illustrated coverage of all bird species

### Phase 4: Ongoing Enhancement (Forever)
- [ ] Enhance 3-5 species/week based on analytics + interest + user requests
- [ ] Add features organically (better audio, similar species comparisons)
- [ ] No deadlines, no pressure — just steady care

---

## File Structure

```
aviary/
├── public/
│   ├── images/
│   │   ├── species/           # AI-generated illustrations
│   │   ├── maps/              # Migration map assets
│   │   └── ui/                # Icons, logos, textures
│   └── data/
│       └── species/           # Static JSON files (one per species)
├── src/
│   ├── app/
│   │   ├── page.tsx           # Landing page (search + explore)
│   │   ├── species/
│   │   │   └── [code]/
│   │   │       └── page.tsx   # Species detail page
│   │   ├── region/
│   │   │   └── [region]/
│   │   │       └── page.tsx   # Regional bird list
│   │   └── layout.tsx         # Root layout
│   ├── components/
│   │   ├── BirdSearch.tsx     # Smart search with autocomplete
│   │   ├── BirdCard.tsx       # Species card in list views
│   │   ├── MigrationMap.tsx   # Interactive migration visualization
│   │   ├── TimeSlider.tsx     # Month-by-month slider
│   │   ├── AudioPlayer.tsx    # Embedded audio player
│   │   └── Navigation.tsx     # Site header/nav
│   ├── lib/
│   │   ├── data/
│   │   │   ├── fetch-ebird.ts      # eBird API integration
│   │   │   ├── fetch-wikipedia.ts  # Wikipedia API integration
│   │   │   └── fetch-ranges.ts     # eBird Status & Trends
│   │   ├── search.ts          # Fuse.js search logic
│   │   └── utils.ts           # Helpers
│   └── styles/
│       └── globals.css        # Tailwind config + custom styles
├── scripts/
│   ├── build-data.ts          # Generate all species JSON files
│   ├── fetch-all.ts           # Fetch from all APIs
│   └── optimize-images.ts     # Image optimization pipeline
├── tailwind.config.js         # Custom theme (colors, fonts)
├── next.config.js             # Static export config
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
│     Live Bird Migration                 │  ← Simple overlay
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

## First 20 Enhanced Species

**Backyard Favorites (8):**
- American Robin
- Northern Cardinal
- Blue Jay
- Black-capped Chickadee
- American Goldfinch
- Mourning Dove
- Song Sparrow
- White-breasted Nuthatch

**Charismatic Icons (6):**
- Bald Eagle
- Great Horned Owl
- Ruby-throated Hummingbird
- Red-tailed Hawk
- Barn Swallow
- Downy Woodpecker

**Migration Showcases (6):**
- Sanderling (long-distance coastal)
- Common Loon (lakes to ocean)
- California Scrub-Jay (resident)
- Carolina Wren (expanding range)
- Greater Roadrunner (desert specialist)
- Atlantic Puffin (seabird)

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

### Free Infrastructure
- Vercel hosting: Free tier (100GB bandwidth)
- Cloudinary: Free tier (25GB storage + bandwidth)
- All APIs: Free
- **Ongoing monthly:** $2-3 (just domain)

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
