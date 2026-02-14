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

### Flow 1: Search for a Bird

```
Landing Page
   ↓
Search "Sanderling"
   ↓
Species Detail Page
```

**What you see:**
1. **Full-screen illustration** (scroll to continue)
2. **Quick facts** - 3 categories with bullets
   - Look For (field marks)
   - Did You Know? (interesting facts)
   - Conservation (if threatened)
3. **Audio player** - Swipe through 1-3 bird sounds
4. **Interactive map** - Shows where to find them, time slider to see migration
5. **Similar birds** - 2 tiles + "explore more" button

### Flow 2: Explore Birds Around You

```
Landing Page
   ↓
"Birds Around You" button
   ↓
Explore Page
```

**What you see:**
1. **Location input** - Auto-detect or manual entry
2. **Compact map** - Shows your region, bird density by color
3. **Time slider** - Drag to see different months
4. **Radius control** - 10-200 miles dropdown
5. **Habitat filter** - Beach, Forest, Grassland, Urban, Wetland
6. **Bird results** - Grid (if <30 birds) or List (if 30+)
   - Grid: Tap → go to species page
   - List: Tap → expand preview inline → "See full page"

### Flow 3: Learn About the Project

```
Any Page
   ↓
"About" link
   ↓
About Page
```

**What you see:**
1. **Personal story** - Why this exists, what inspired it
2. **How it works** - AI illustrations, public data, hand-written content
3. **Support option** - Transparent costs, donation button
4. **Community** - "Share your birding story" with comments section

---

## Page Layouts (Mobile)

### Landing Page
```
┌─────────────────────┐
│                     │
│  [Beautiful         │  ← Hero illustration (fills screen)
│   Audubon-style     │    Rotates daily/seasonally
│   illustration]     │
│                     │
│  Discover Birds     │  ← Headline (elegant serif)
│                     │
└─────────────────────┘
        ↓ Scroll
┌─────────────────────┐
│  ┌───────────────┐  │
│  │ Search by     │  │  ← Search bar (large, prominent)
│  │ name...       │  │    Autocomplete on type
│  └───────────────┘  │
│                     │
│  [Birds Around      │  ← Primary CTA (simple button)
│   You →]            │
│                     │
│  Featured Birds     │  ← 3-4 showcase cards
│  [img] [img] [img]  │    Enhanced species only
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

### AI-Generated Content (Automated)
- **Illustrations:** Generated in Audubon's style via Midjourney
  - Template poses for 10,800+ birds (batch process)
  - Unique compositions for 200 priority birds
- **Facts sections:** Claude generates from validated template
  - 2-sentence summary
  - 3 categories of bullets (behavior, facts, conservation)
  - Consistent voice across all 11,000 species

### Hand-Crafted Content (Manual)
- **Enhanced species:** 20 showcase birds to start
  - Unique illustrations with multiple plumages
  - Hand-written naturalist prose
  - Detailed migration narratives
- **About page:** Personal story and mission
- **Template validation:** Review first 100 AI generations before automation

### The Process
1. **Phase 1:** Hand-write 20 birds (establish quality bar)
2. **Phase 2:** Generate 100 with Claude, review every one (validate template)
3. **Phase 3:** Batch-generate remaining 10,880 (spot-check 1%)
4. **Phase 4:** Enhance 3-5/week based on analytics (forever)

---

## Technical Notes

**Mobile-first:** Design for phone, adapt to desktop, native apps later

**Performance targets:**
- First paint: <1.5s
- Time to interactive: <2.5s
- 60fps animations (non-negotiable)

**Stack:**
- Next.js 14 (static generation)
- Tailwind CSS (custom theme)
- Framer Motion (animations)
- Vercel (free hosting)

**Data:**
- eBird API (species taxonomy, ranges, migration)
- Wikipedia (summaries)
- Xeno-canto (bird sounds, Creative Commons)

**Budget:**
- Domain: ~$30/year
- Midjourney: $10/month (can cancel after initial batch)
- **Ongoing: $2-3/month** (everything else free)

---

## Design Questions for Feedback

### Visual
1. **Does the color palette feel right?** Too brown/sepia? Need more brightness?
2. **Restrained vintage approach:** Too minimal or just right? Should we push the vintage aesthetic harder?
3. **Typography choices:** Serif everywhere - does this feel elegant or stuffy?

### UX
4. **Landing page:** Search + "Birds Around You" button - clear enough entry points?
5. **Species page:** Name placement (left-bottom) - organic or awkward?
6. **Audio carousel:** Swipe through sounds - intuitive or confusing?
7. **Map on Explore:** Compact map (1/3 width) - useful or too small?
8. **List vs Grid:** Adaptive default based on result count - smart or should we always default to one?

### Content
9. **Facts structure:** 3 categories (Look For / Did You Know / Conservation) - too structured or helpful?
10. **About page tone:** Personal story with first-person - warm or unprofessional?
11. **Comments section:** Simple community feature or unnecessary complexity?

### Overall
12. **Does this feel cohesive?** Do all the pages work together?
13. **Is anything missing?** Features, pages, or details we haven't considered?
14. **What excites you most?** What would you want to show someone first?
15. **What concerns you?** Where do you see potential issues?

---

## Files to Review

- **UX_SPECIFICATION.md** - Complete detailed specs for all pages
- **sanderling-mockup.html** - Working visual example (open in browser)
- **README.md** - Technical overview and architecture

---

**Take your time exploring. Looking forward to your thoughts!** 🦅
