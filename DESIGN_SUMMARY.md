# Aviary - Design Overview
**A beautiful, free birding field guide for the web**

---

## The Concept

**What it is:**  
A searchable database of all 11,000+ bird species with vintage Audubon-style illustrations, migration maps, and naturalist storytelling.

**Why it exists:**  
A gift to the birding community. Always free, no ads, built with care. Think museum exhibit meets modern web app.

**The feeling:**  
> "Simple enough to use on a phone at the beach, charming enough to linger over on a Sunday morning."

---

## Visual Identity

### Colors
- **Cream (#FAF7F0)** - Primary background, aged paper warmth
- **Forest (#1A2F23)** - Text and headers
- **Sepia (#8B7355)** - Borders, secondary text, vintage accents
- **Rust (#B85C38)** - Accent color, CTAs, migration paths
- **Sky Blue (#A8DADC)** - Migration ranges, water birds
- **Gold (#C9A05F)** - Enhanced species badge

### Typography
- **Headings:** Cormorant Garamond (elegant serif, museum feel)
- **Body:** Crimson Pro (readable serif)
- Both free from Google Fonts

### The Aesthetic
**Restrained Vintage** - Not skeuomorphic or ornate. Modern clean layouts where:
- Audubon-style illustrations carry the vintage soul
- Subtle paper grain texture (barely perceptible, 5-10% opacity)
- Elegant typography adds warmth
- Everything else stays minimal and gets out of the way

**Think:** Museum exhibit, not renaissance fair. Coffee table book, not antique store.

---

## User Flows (Mobile-First)

### Flow 1: Discover via Migration Map (Primary)

```
Landing Page (IS the Migration Map)
   ↓
See animated bird migration paths, drag time slider
   ↓
Scroll down to search or explore
```

**What you see:**
1. **Full-screen migration map** with animated paths (the hero experience)
2. **Time slider** (Jan-Dec) showing what's flying this month
3. **Scroll down:** Search bar + "Birds Around You" button
4. **Featured migration cards** (2-4 spectacular journeys)

### Flow 2: Search for a Bird

```
Landing Page → Search bar (below map) or search icon in header
   ↓
Search "Sanderling"
   ↓
Species Detail Page
```

**What you see:**
1. **Full-screen illustration** (scroll to continue)
2. **Family context bar** (e.g., "Sandpipers - 98 species" — tappable link)
3. **Quick facts** - 3 categories with bullets
   - Look For (field marks)
   - Did You Know? (interesting facts)
   - Conservation (if threatened)
4. **Audio player** - Swipe through 1-3 bird sounds (hidden if no recordings)
5. **Interactive map** - Shows where to find them, time slider to see migration
6. **Similar birds** - 2 tiles + "explore more" button

### Flow 3: "What's That Bird?" Quick ID

```
Any Page → Binoculars icon or "What's That Bird?" link
   ↓
3 quick questions: Size, Color, Where
   ↓
3-8 likely matches filtered by location + season
   ↓
Tap match → Species page
```

**What you see:**
1. **Size selector** - Sparrow / Robin / Crow / Goose (tap one)
2. **Color picker** - 8 common colors (tap dominant color)
3. **Where selector** - Water / Ground / Tree / Sky
4. **Results** - Filtered by your location and current month, most likely matches first

### Flow 4: Explore Birds Around You

```
Landing Page → "Birds Around You" button
   ↓
Map zooms to your location, shows local migrations
```

**What you see:**
1. **Map personalizes** to your region with local migration paths
2. **Time slider** - Drag to see different months
3. **Bird list below** - Grid or list of species in your area
4. **Habitat filter** - Beach, Forest, Grassland, Urban, Wetland

### Flow 5: Learn About the Project

```
Any Page → "About" link
   ↓
About Page
```

**What you see:**
1. **Personal story** - Why this exists, what inspired it
2. **How it works** - AI illustrations, public data, hand-written content
3. **Support option** - Transparent costs, donation button
4. **Community** - "Share your birding story" (utterances / GitHub-backed comments)

---

## Page Layouts (Mobile)

### Landing Page (Migration Map IS the Hero)
```
┌─────────────────────┐
│ AVIARY      [🔍][≡] │  ← Minimal header with persistent search icon
├─────────────────────┤
│                     │
│  [MIGRATION MAP]    │  ← Full-viewport animated migration map
│  [Animated paths]   │    THE unique feature, shown immediately
│  [Pulsing hotspots] │    5-8 flowing paths, current month
│                     │
│  Bird Migration     │  ← Overlay text (bottom)
│  This Month         │    Honest framing, not "Live"
│                     │
│  Jan ━━●━━━━━ Dec   │  ← Time slider
│  [▶ Play] [Legend]  │
│                     │
│     ↓ Scroll        │
└─────────────────────┘
        ↓ Scroll
┌─────────────────────┐
│  ┌───────────────┐  │
│  │ Search by     │  │  ← Search bar (large, prominent)
│  │ name...       │  │    Autocomplete on type
│  └───────────────┘  │
│                     │
│  [What's That       │  ← Quick ID flow
│   Bird? →]          │    Size + color + habitat filter
│                     │
│  [Birds Around      │  ← Primary CTA (simple button)
│   You →]            │
│                     │
│  Featured Journeys  │  ← 3-4 showcase cards
│  [img] [img] [img]  │    Rotates by season
└─────────────────────┘
```

### Species Detail Page (Sanderling Example)
```
┌─────────────────────┐
│ ←                   │  ← Back button
│                     │
│  [Audubon-style     │  ← Illustration (fills viewport)
│   Sanderling        │
│   illustration]     │
│                     │
│ Sanderling          │  ← Name (left-bottom, organic)
│ Calidris alba       │    Scientific name below
│ Sandpipers · 98 spp │    Family context (tappable → family page)
└─────────────────────┘
        ↓ Scroll
┌─────────────────────┐
│ What Makes Them     │  ← Section header
│ Special             │
│                     │
│ These pale shore-   │  ← 2-sentence summary
│ birds chase waves   │
│ along beaches...    │
│                     │
│ Look For            │  ← Category 1
│ • Running along     │    Behavior/field marks
│ • Pale gray winter  │
│                     │
│ Did You Know?       │  ← Category 2
│ • Migrates 10,000km │    Surprising facts
│ • Breeds in Arctic  │
│                     │
│ Conservation        │  ← Category 3 (if relevant)
│ • Declining 80%     │    Status, factual
└─────────────────────┘
        ↓
┌─────────────────────┐
│ ♪ Sounds            │
│                     │
│ [▶ Flight Call]     │  ← Audio player
│ "Short, sharp kip"  │    Swipe through 1-3
│     ● ○ ○           │    Dots show total
└─────────────────────┘
        ↓
┌─────────────────────┐
│ Where & When        │
│                     │
│ [Interactive Map]   │  ← Shows current month
│                     │    Pan/zoom enabled
│ February            │
│ Jan ━●━━━━━━━ Dec   │  ← Time slider
│                     │    (12 months or 4 seasons
│ In Feb, Sander-     │     based on data)
│ lings winter on...  │    Contextual text updates
└─────────────────────┘
        ↓
┌─────────────────────┐
│ Similar Birds       │
│                     │
│ [Dunlin] [Plover]   │  ← 2 tiles
│                     │    (habitat + region matched)
│ [See more birds     │  ← CTA to Explore
│  near you →]        │
└─────────────────────┘
```

### Explore Page
```
┌─────────────────────┐
│ Birds Around You    │
│                     │
│ ┌─────────────────┐ │
│ │ Santa Barbara   │ │  ← Location input
│ └─────────────────┘ │    Auto-detect or manual
│                     │
│ ┌────┐  47 birds    │  ← Compact map (1/3 width)
│ │Map │  in Feb      │    Bird count
│ │ ◉  │  [Expand ⤢]  │    Color = density
│ └────┘              │    Tap to go fullscreen
│                     │
│ February            │  ← Time slider (sticky)
│ Jan ━●━━━━━━━ Dec   │
│                     │
│ Within: [50mi ▾]    │  ← Radius dropdown
└─────────────────────┘
        ↓ Scroll
┌─────────────────────┐
│ Filter: ○All        │  ← Habitat chips (optional)
│ ●Beach ○Forest      │
│                     │
│ 47 birds • Feb 2026 │  ← Results header
│ [Grid ●] [List ○]   │    Toggle view
│ Sort: [Common ▾]    │    Sort dropdown
│                     │
│ ─── If Grid ───     │
│ [img] [img] [img]   │  ← 3 columns
│ Robin  Jay   Gull   │    Tap → Species page
│                     │
│ ─── If List ───     │
│ ┌──┐               │  ← Compact rows
│ │im│ American Robin │    Tap → Expand inline
│ └──┘ Common resident│
│ ─────────────       │
│ ┌────────────────┐  │  ← Expanded preview
│ │ [Larger image] │  │    Shows 1-2 sentences
│ │ Western Gull   │  │    + quick facts
│ │ Large gray...  │  │    + "See full page" CTA
│ │ [See full →]   │  │
│ └────────────────┘  │
└─────────────────────┘
```

### About Page
```
┌─────────────────────┐
│ ←                   │
│                     │
│  [Beautiful         │  ← Hero illustration
│   nature scene]     │
│                     │
│ About Aviary        │  ← Title (left-bottom)
└─────────────────────┘
        ↓ Scroll
┌─────────────────────┐
│ What This Is        │  ← Section headers
│ [2-3 paragraphs]    │    Personal, warm
│                     │
│ Why It Exists       │
│ [Your story]        │    First-person OK
│ [Your vision]       │    Why you care
│                     │
│ How It Works        │
│ [Technical overview]│    Friendly language
│                     │
│ Support             │
│ [Donation option]   │    Transparent costs
│                     │
│ Share Your Story    │
│                     │
│ ┌─────────────────┐ │  ← Comment form
│ │ Your thoughts...│ │    Simple textarea
│ └─────────────────┘ │    Name optional
│ [Submit]            │
│                     │
│ Comments            │  ← Posted comments
│ Sarah M. • 2d ago   │    Chronological
│ "I started birding  │    Manual moderation
│ during pandemic..." │
└─────────────────────┘
```

---

## Content Strategy

### Five-Tier Quality Spectrum
Every species page has at least one reason to visit. Quality is continuous, not binary.

| Tier | Count | Illustrations | Content | Key Investment |
|------|-------|--------------|---------|----------------|
| **Showcase** | 20 | Custom, hand-reviewed | Hand-written prose | The portfolio pieces |
| **Enhanced** | 200 | Custom, reviewed | AI + human editing | Every summary hand-read |
| **Curated** | 1,000 | Template, reviewed | AI + one human "hook" | One memorable specific fact per species |
| **Standard** | 5,000 | Template, spot-checked | AI from validated template | Automated quality checks |
| **Stub** | 5,000 | Family placeholder | Minimal data only | Honest "help us improve" CTA |

**The key insight:** The difference between "soulless" and "cared-for" is one specific, memorable detail. If every Curated-tier page has one human-curated "did you know" fact that's genuinely surprising, it won't feel like slop even if the rest is generated.

### Anti-Slop Guardrails
- **Ban list:** "remarkable," "epic journey," "fascinating," "feathered friend," "testament to"
- **Specificity check:** Every summary must contain a specific number AND a named location
- **Uniqueness check:** No two summaries should be interchangeable
- **The "so what" test:** Each fact should make someone want to tell a friend

### The Process
1. **Phase 1:** Hand-write 20 Showcase species (establish quality bar)
2. **Phase 2:** Generate + review 200 Enhanced (human editing pass on every one)
3. **Phase 3:** Generate 1,000 Curated with one human "hook" each (breadth of care)
4. **Phase 4:** Batch-generate 5,000 Standard (validated template, spot-check 1%)
5. **Ongoing:** Promote species up the tier ladder, 3-5/week

---

## Technical Notes

**Mobile-first:** Design for phone, adapt to desktop (with dedicated desktop layouts for species pages and migration map). Native apps later.

**Performance targets:**
- First paint: <1.5s
- Time to interactive: <2.5s
- 60fps animations (non-negotiable)

**Stack (Decisions Locked):**
- Next.js 14 (hybrid static + ISR — pre-generate 500 showcase/enhanced, ISR for rest)
- Tailwind CSS (custom theme)
- Framer Motion (animations)
- Mapbox GL JS (migration map — WebGL, custom vintage styling, free 50K loads/month)
- Pagefind (static search index — no client bundle bloat for 11K species)
- Nominatim (geocoding — free, no vendor dependency)
- Vercel (free hosting)

**Data (Tiered by Availability):**
- Tier 1 (~1,100 spp): eBird Status & Trends (weekly abundance — requires access request, raster format)
- Tier 2 (~3,000 spp): eBird API (seasonal presence, recent observations)
- Tier 3 (~7,000 spp): BirdLife/Wikipedia (coarse ranges, text descriptions)
- Audio: Xeno-canto (Creative Commons, quality-filtered B+ only, hidden when unavailable)

**Budget:**
- Domain: ~$30/year
- Midjourney: $10/month (can cancel after initial batch)
- Analytics: $9/month (Plausible, optional)
- **Ongoing: $2-12/month** (everything else free)

---

## Resolved Design Decisions

The following questions from the original design review have been resolved:

| # | Question | Decision |
|---|----------|----------|
| 1 | Landing page design | Migration map IS the hero. No traditional illustration landing page. |
| 2 | Map library | Mapbox GL JS — WebGL performance, custom vintage styling, free tier. |
| 3 | Search implementation | Pagefind — static index, no client bundle bloat. |
| 4 | Content tiers | Five-tier spectrum (Showcase → Stub), not binary. |
| 5 | Comments system | Utterances (GitHub-backed) or defer. No custom Supabase backend. |
| 6 | "Live" framing | "Migration This Month" — honest about seasonal data, not real-time. |
| 7 | Image hosting | Self-hosted with Next.js Image optimization. No Cloudinary dependency. |
| 8 | Build strategy | Hybrid ISR — pre-generate 500 enhanced, on-demand for rest. |
| 9 | Migration path types | Full pattern taxonomy: loop, altitudinal, irruptive, pelagic, austral, etc. |
| 10 | Confidence display | Visible: solid (high), dashed (medium), hidden + text (low). |

## Open Questions for Feedback

### Visual
1. **Color palette in practice:** The vintage palette looks great in mockups — does it hold up with real AI illustrations?
2. **Desktop layouts:** Side-by-side (illustration left, content right) for species page — does this feel right on 27" monitors?

### UX
3. **"What's That Bird?" quick ID flow:** 3 questions (size, color, where) — is this enough for useful filtering? Too few? Too many?
4. **Family browsing:** Is a full family page (e.g., /family/thrushes) worth building, or is a filtered species list sufficient?
5. **Seasonal landing shifts:** How aggressively should the color accents change by season?

### Content
6. **Human "hook" fact strategy:** Is writing one memorable fact per 1,000 Curated species realistic? What's the time commitment?
7. **Illustration QA pipeline:** How many birder reviewers are needed? 2-3 per batch? More?

### Migration Map
8. **Irruptive species visualization:** Range-pulse effect vs. just a text note — worth the engineering effort?
9. **Pelagic rendering:** Wave-like ocean paths vs. standard paths over water — how distinct should these be?

---

## Files to Review

- **README.md** - Technical overview, architecture, data tiers, roadmap, species schema
- **UX_SPECIFICATION.md** - Complete UX specs: all pages, flows, desktop layouts, slop detector
- **MIGRATION_MAP_SPEC.md** - Migration map: pattern types, data model, confidence, pelagic, BirdCast
- **DESIGN_REVIEW.md** - Full design review with gaps, suggestions, and rationale for all decisions
