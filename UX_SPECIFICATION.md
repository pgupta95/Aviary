# Aviary UX Specification
**Mobile-First Design Documentation**

Version 1.0 - February 2026

---

## Table of Contents
1. [Design Philosophy](#design-philosophy)
2. [Landing Page](#landing-page)
3. [Species Detail Page](#species-detail-page)
4. [Content Generation Strategy](#content-generation-strategy)
5. [Technical Implementation Notes](#technical-implementation-notes)

---

## Design Philosophy

### Restrained Vintage
> "Simple enough to use on a phone at the beach, charming enough to linger over on a Sunday morning."

**Core Principles:**
- **Mobile-first:** Design for phone, adapt to desktop, native apps come later
- **Content is king:** Audubon illustrations carry the vintage soul, UI stays minimal
- **Minimal but warm:** Clean layouts with subtle texture and elegant typography
- **Respectful:** No dark patterns, fast load times, clear information architecture
- **Automated quality:** Claude-generated content from validated templates ensures consistency

**Visual Balance:**
- Modern, clean interface structure (no skeuomorphic gimmicks)
- Vintage soul from illustrations + serif typography
- Barely perceptible paper grain texture (5-10% opacity)
- Thoughtful details, never ornate decoration
- Everything supports the birds, nothing competes

---

## Landing Page (Migration Map)

### Purpose
**Primary Goal:** Immediately show what makes Aviary unique - live bird migration visualization.

**Core Experience:** User lands on the site and sees the Migration Map in full glory. The wow factor is instant. No boring landing page, no pitch - just beauty and discovery.

**Secondary Goals:**
- Provide search for specific birds (below the map)
- Allow personalization ("See birds near you")
- Showcase featured migrations

### Layout Philosophy

**The map IS the landing page.** 

Why hide the best feature behind a traditional landing page? Lead with the unique value immediately. Search and exploration are still accessible - just below the spectacle.

### Detailed Layout (Mobile)

#### Full-Screen Migration Map

```
┌──────────────────────────────────┐
│ AVIARY                      [≡]  │ ← Minimal header (40px)
├──────────────────────────────────┤
│                                  │
│                                  │
│     [MIGRATION MAP - GLOBAL]     │ ← FULL VIEWPORT HEIGHT
│                                  │   Spectacular default view
│                                  │   
│     [Animated migration paths]   │   Shows epic journeys:
│     [Multiple continents]        │   - Arctic Tern (pole-to-pole)
│                                  │   - Bar-tailed Godwit (Pacific)
│                                  │   - Sanderling (Americas)
│     ◉ Global view                │   - Swainson's Hawk
│                                  │   
│                                  │   5-8 paths maximum
│     Live Bird Migration          │   Color-coded by family
│     Across the World             │   Animated particles flowing
│                                  │
│    Jan ━━━━━●━━━━━━━━━━━━━ Dec   │ ← Time slider (integrated)
│                                  │   Defaults to current month
│    [▶ Play]        [Legend ≡]   │ ← Optional controls
│                                  │
│         ↓ Scroll to explore      │ ← Subtle hint (fades after 3s)
│                                  │
└──────────────────────────────────┘

Visual Treatment:
- Map fills entire viewport (100vh minus 40px header)
- Paths animate on load (staggered entrance for drama)
- Time slider integrated into map (bottom overlay)
- Text overlays have semi-transparent backgrounds
- "Scroll to explore" hint pulses gently, fades after 3 seconds
```

**Default Map State:**
- **Location:** Global view (shows all continents)
- **Month:** Current real-world month
- **Paths shown:** 5-8 most spectacular migrations active this month
  - If May: Arctic Tern, Sanderling, Warblers, etc. (northbound)
  - If September: Bar-tailed Godwit, Raptors, etc. (southbound)
  - If January: Very few paths (most birds stationary - shows clustering)
- **Hotspots:** 1-2 most critical for current month (e.g., Delaware Bay in May)

**First Impression Flow:**
```
User lands on birding.guide
  ↓
Sees full-screen animated migration map
  ↓ (3-5 seconds of pure visual experience)
Instinctively drags time slider
  ↓
Watches birds move through the year
  ↓
"Whoa, what IS this?"
  ↓
Scrolls down to learn more
```

---

#### Search & Personalization Section

```
              ↓ Scroll

┌──────────────────────────────────┐
│                                  │
│ Discover Any Bird                │ ← Section header
│                                  │   Clean, simple
│ ┌──────────────────────────┐     │
│ │ 🔍 Search by name...     │     │ ← Search bar (large)
│ └──────────────────────────┘     │   Autocomplete on type
│                                  │   Fuzzy matching
│                                  │
│            or                    │ ← Divider text
│                                  │
│ ┌──────────────────────────┐     │
│ │  See Birds Around You →  │     │ ← Personalization CTA
│ └──────────────────────────┘     │   Updates map to user location
│                                  │
└──────────────────────────────────┘

CSS:
.search-section {
  padding: 3rem 2rem;
  background: #FAF7F0;
  text-align: center;
}

.search-bar {
  font-family: 'Crimson Pro', serif;
  font-size: 1.2rem;
  padding: 1.25rem 1.5rem;
  border: 2px solid #8B7355;
  border-radius: 12px;
  background: white;
  width: 100%;
  max-width: 500px;
  margin: 0 auto;
}

.search-bar:focus {
  border-color: #2D4A3E;
  outline: none;
  box-shadow: 0 0 0 3px rgba(45, 74, 62, 0.1);
}

.personalize-button {
  font-family: 'Crimson Pro', serif;
  font-size: 1.1rem;
  padding: 1rem 2rem;
  background: #2D4A3E;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s;
}

.personalize-button:hover {
  background: #1A2F23;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(26, 47, 35, 0.2);
}
```

**Search Interaction:**
1. User types "sander" 
   → Autocomplete shows "Sanderling", "Sandhill Crane", "Sand Martin"
2. User selects "Sanderling"
   → Navigates directly to species page (no modal preview)
3. Clean, fast, familiar

**"Birds Around You" Interaction:**
1. User clicks button
   → Requests location permission (browser API)
2. If granted:
   → Map above scrolls back into view
   → Animates zoom to user's region
   → Updates paths to show local migrations
   → URL updates to /explore?location=...
3. If denied:
   → Shows manual location entry
   → "Enter your city or region"

---

#### Featured Migrations Section (Optional)

```
              ↓ Scroll

┌──────────────────────────────────┐
│ Spectacular Journeys             │ ← Section header
│                                  │
│ ┌────────────────────────────┐   │
│ │ [Sanderling illustration]  │   │ ← Card 1
│ │                            │   │
│ │ Sanderling                 │   │
│ │ 10,000 km migration        │   │   Enhanced species
│ │ Arctic → South America     │   │   Tap → Species page
│ │                            │   │
│ │ [Learn more →]             │   │
│ └────────────────────────────┘   │
│                                  │
│ ┌────────────────────────────┐   │
│ │ [Arctic Tern illustration] │   │ ← Card 2
│ │                            │   │
│ │ Arctic Tern                │   │
│ │ 44,000 km migration        │   │
│ │ Pole to pole               │   │
│ │                            │   │
│ │ [Learn more →]             │   │
│ └────────────────────────────┘   │
│                                  │
│ [2-4 total featured cards]       │
│                                  │
└──────────────────────────────────┘

CSS:
.featured-card {
  background: white;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 12px rgba(26, 47, 35, 0.08);
  margin-bottom: 1.5rem;
  transition: transform 0.3s, box-shadow 0.3s;
}

.featured-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 24px rgba(26, 47, 35, 0.15);
}

.featured-card img {
  width: 100%;
  aspect-ratio: 3/2;
  object-fit: cover;
}
```

**Featured Species Selection:**
- Rotate monthly or seasonally
- Show 3-4 enhanced species with epic migration stories
- Click card → goes to species detail page
- Mobile: Stack vertically
- Desktop: Grid layout (2 columns)

---

### User Flows

**Flow 1: Visual Discovery → Search**
```
Land on site
  ↓
See Migration Map (amazed)
  ↓
Drag time slider (explore)
  ↓
Tap a path (learn about shorebirds)
  ↓
Scroll down
  ↓
Search for specific bird (Sanderling)
  ↓
Species detail page
```

**Flow 2: Direct Search**
```
Land on site
  ↓
Scroll immediately to search
  ↓
Type bird name
  ↓
Species detail page
```

**Flow 3: Local Discovery**
```
Land on site
  ↓
See map, scroll to "Birds Around You"
  ↓
Grant location
  ↓
Map personalizes to their region
  ↓
Explore local migrations
  ↓
Scroll down for full bird list
```

---

### Desktop Adaptations

**Larger screens (>768px):**

```
┌─────────────────────────────────────────────┐
│ AVIARY                                 [≡]  │
├─────────────────────────────────────────────┤
│                                             │
│         [MIGRATION MAP - LARGER]            │ ← Still full viewport
│                                             │   More detail visible
│         [More paths visible]                │   Potentially 8-10 paths
│                                             │   (vs 5-8 on mobile)
│                                             │
│    Live Bird Migration Across the World     │
│                                             │
│    Jan ━━━━━━━●━━━━━━━━━━━━━━━━━━━ Dec     │
│                                             │
│    [▶ Play]  [Legend ≡]   ↓ Scroll          │
└─────────────────────────────────────────────┘
                 ↓ Scroll
┌─────────────────────────────────────────────┐
│                                             │
│          Discover Any Bird                  │
│                                             │
│  ┌──────────────────────────────────┐       │ ← Wider search
│  │ 🔍 Search by name...             │       │
│  └──────────────────────────────────┘       │
│                                             │
│  ┌──────────────────────────────────┐       │
│  │  See Birds Around You →          │       │
│  └──────────────────────────────────┘       │
│                                             │
└─────────────────────────────────────────────┘
                 ↓
┌─────────────────────────────────────────────┐
│ Spectacular Journeys                        │
│                                             │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐     │ ← Grid layout
│ │ Sanderl. │ │ Arctic T.│ │ Bar-tail │     │   3 columns
│ │ [card]   │ │ [card]   │ │ [card]   │     │
│ └──────────┘ └──────────┘ └──────────┘     │
└─────────────────────────────────────────────┘
```

**Key differences:**
- Map can show more paths (screen is bigger)
- Search bar wider (better for longer names)
- Featured cards in grid instead of stack
- Otherwise same experience (consistency)

---

### Performance Targets

**Critical for landing page:**
- **First Contentful Paint:** <1.5s (map base visible)
- **Time to Interactive:** <2.5s (slider draggable)
- **Smooth animations:** 60fps (map loads, then animates)
- **Initial bundle:** <150KB (code-split heavy features)

**Loading Strategy:**
1. Show map base immediately (static image or simplified SVG)
2. Load animation library (Framer Motion or CSS)
3. Animate paths in (staggered entrance)
4. Enable slider interactivity
5. Lazy load featured cards (below fold)

---

### Accessibility

**Keyboard Navigation:**
- Tab through: Search → Personalize button → Featured cards
- Time slider: Arrow keys to adjust month
- Enter on card: Navigate to species page

**Screen Reader:**
```html
<main aria-label="Migration Map">
  <section aria-label="Live bird migration visualization">
    <div role="img" aria-label="Animated map showing bird migration paths across continents">
      <!-- Map SVG -->
    </div>
    <div role="slider" aria-label="Select month" aria-valuemin="1" aria-valuemax="12" aria-valuenow="5">
      <!-- Time slider -->
    </div>
  </section>
  
  <section aria-label="Search and discovery">
    <label for="bird-search">Search for any bird species</label>
    <input id="bird-search" type="search" />
    
    <button aria-label="Find birds near your location">
      See Birds Around You
    </button>
  </section>
</main>
```

**Alternative for Screen Readers:**
- Text description of current migration activity
- "In May, 47 species are migrating through North America..."
- List of active migration groups (Shorebirds, Warblers, etc.)

---

### SEO Optimization

**Meta Tags:**
```html
<title>Aviary - Live Bird Migration Map & Field Guide</title>
<meta name="description" content="Watch bird migration happening right now across the world. Explore 11,000+ species with beautiful Audubon-style illustrations, migration maps, and bird calls. Always free.">
<meta property="og:title" content="Aviary - Live Bird Migration Map">
<meta property="og:description" content="See bird migration as it happens. Free illustrated field guide with interactive migration maps.">
<meta property="og:image" content="/images/og-migration-map.jpg">
<meta property="og:type" content="website">
```

**Structured Data:**
```json
{
  "@context": "https://schema.org",
  "@type": "WebApplication",
  "name": "Aviary",
  "description": "Interactive bird migration map and illustrated field guide",
  "applicationCategory": "EducationalApplication",
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD"
  }
}
```

---

## Species Detail Page

### Purpose
**Answer:** "Is this the bird I saw?" (ID confirmation)  
**Inform:** Interesting facts, sounds, range, migration  
**Invite:** Discover similar birds or explore local migrations

### Design Philosophy: Illustration Always Visible

**Key principle:** The illustration is not just a hero image - it's the anchor for the entire page. It should remain visible as the user scrolls through content, keeping the bird's visual presence throughout the experience.

**Approach:** Sticky illustration that starts full-screen, then shrinks and stays at top as user scrolls.

### Information Architecture

```
1. Hero Illustration (Full Viewport)
   └─ Bird name + scientific name (overlay)

2. Sticky Illustration Card (Shrinks, stays visible)
   └─ Continues to show bird as user scrolls

3. Content Cards (Float below sticky illustration)
   ├─ Facts (What Makes Them Special)
   ├─ Sounds (Audio carousel)
   ├─ Map (Where & When to Find)
   └─ Discovery (Similar Birds)
```

### Detailed Layout (Mobile)

#### 1. Hero Section (Initial State)

```
┌──────────────────────────────────┐
│ ← Back                           │  ← Minimal header (40px)
│                              [≡] │     Back button + menu
├──────────────────────────────────┤
│                                  │
│                                  │
│                                  │
│     [AUDUBON ILLUSTRATION]       │  ← Full viewport (100vh - 40px)
│                                  │     Gorgeous, breathing
│         Full screen              │     High quality
│         Beautiful                │     Generous padding
│                                  │
│                                  │
│                                  │
│                                  │
│ ┌──────────────────────────┐     │  ← Name overlay (bottom)
│ │ Sanderling               │     │     Semi-transparent bg
│ │ Calidris alba            │     │     Elegant typography
│ └──────────────────────────┘     │
│                                  │
└──────────────────────────────────┘

CSS:
.hero-illustration {
  height: calc(100vh - 40px);
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  background: #FAF7F0;
}

.hero-illustration img {
  max-width: 100%;
  max-height: 85%;
  object-fit: contain;
}

.bird-name-overlay {
  position: absolute;
  bottom: 2rem;
  left: 2rem;
  right: 2rem;
  background: rgba(250, 247, 240, 0.95);
  backdrop-filter: blur(10px);
  padding: 1.5rem;
  border-radius: 12px;
  box-shadow: 0 4px 16px rgba(26, 47, 35, 0.1);
}

.bird-common-name {
  font-family: 'Cormorant Garamond', serif;
  font-size: 2.5rem;
  font-weight: 600;
  color: #1A2F23;
  margin-bottom: 0.25rem;
}

.bird-scientific-name {
  font-family: 'Crimson Pro', serif;
  font-style: italic;
  font-size: 1.3rem;
  color: #8B7355;
}
```

---

#### 2. Sticky Illustration (Scrolled State)

```
As user scrolls down ↓

┌──────────────────────────────────┐
│ ← Sanderling                 [≡] │  ← Header with bird name
├──────────────────────────────────┤
│                                  │
│   [Illustration - Compact]       │  ← STICKY card (35-40vh)
│                                  │     Stays at top
│   Sanderling                     │     Name visible
│   Calidris alba                  │     Scrolls with page
│                                  │
└──────────────────────────────────┘
  ↑ Sticks here - doesn't scroll away ↑
─────────────────────────────────────
│                                  │
│ ┌────────────────────────────┐   │  ← Content cards
│ │ What Makes Them Special    │   │     Scroll underneath
│ │ ...                        │   │     White/cream bg
│ └────────────────────────────┘   │     Clean separation
│                                  │
│ ┌────────────────────────────┐   │
│ │ ♪ Sounds                   │   │
│ │ ...                        │   │
│ └────────────────────────────┘   │
│                                  │

CSS:
.illustration-sticky {
  position: sticky;
  top: 0;
  z-index: 10;
  background: white;
  border-bottom: 1px solid rgba(139, 115, 85, 0.2);
  transition: all 0.3s ease;
  box-shadow: 0 2px 8px rgba(26, 47, 35, 0.08);
}

/* Initial state (full screen) */
.illustration-sticky.initial {
  height: calc(100vh - 40px);
  border-bottom: none;
  box-shadow: none;
}

/* Scrolled state (compact) */
.illustration-sticky.scrolled {
  height: 35vh;
  min-height: 280px;
}

.illustration-sticky img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  padding: 1rem;
}

.sticky-bird-info {
  position: absolute;
  bottom: 1rem;
  left: 1rem;
  right: 1rem;
  text-align: center;
}

.sticky-bird-info h1 {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.5rem;
  margin-bottom: 0.25rem;
}

.sticky-bird-info .scientific {
  font-style: italic;
  color: #8B7355;
  font-size: 1rem;
}
```

**Scroll Behavior:**
```javascript
// Detect scroll and toggle states
let lastScroll = 0;

window.addEventListener('scroll', () => {
  const illustrationCard = document.querySelector('.illustration-sticky');
  const scrollY = window.scrollY;
  
  if (scrollY > 50) {
    // User has scrolled down - shrink illustration
    illustrationCard.classList.remove('initial');
    illustrationCard.classList.add('scrolled');
  } else {
    // User at top - show full illustration
    illustrationCard.classList.add('initial');
    illustrationCard.classList.remove('scrolled');
  }
  
  lastScroll = scrollY;
});
```

---

#### 3. Content Cards (Below Sticky Illustration)

---

#### Facts Section (Content Card)

**Note:** This and all following sections appear as content cards that scroll underneath the sticky illustration. The bird remains visible at the top throughout.

```
┌──────────────────────────┐
│ What Makes Them Special  │  ← Section header
│                          │    (Cormorant Garamond, 1.8rem)
│ These pale shorebirds    │  ← 2-sentence summary
│ chase waves along        │    Generated by Claude
│ beaches from fall        │    Sets the scene
│ through spring, racing   │
│ the surf in an endless   │
│ game of tag.             │
│                          │
│ Look For                 │  ← Category 1: Behavior/Field Marks
│ • Running along the      │    Bulleted list
│   surf line like a       │    Each bullet 1-2 lines max
│   wind-up toy            │
│ • Pale gray upperparts   │
│   in winter, rusty-      │
│   orange in breeding     │
│ • Stout black bill,      │
│   black legs             │
│                          │
│ Did You Know?            │  ← Category 2: Surprising Facts
│ • Migrates up to         │    Most interesting/memorable
│   10,000km twice yearly  │    details
│ • Breeds farther north   │
│   than almost any bird   │
│ • Some individuals       │
│   summer on tropical     │
│   beaches, never see     │
│   Arctic                 │
│                          │
│ Conservation             │  ← Category 3: Status (if relevant)
│ • Declining - 80%        │    Simple, factual
│   population loss since  │    Not alarmist
│   1970s                  │    Only show if noteworthy
│ • Vulnerable to beach    │
│   development and        │
│   disturbance            │
│                          │
└──────────────────────────┘

CSS:
.facts-section {
  padding: 2rem;
  background: #FAF7F0;
}

.section-header {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.8rem;
  font-weight: 600;
  color: #1A2F23;
  margin-bottom: 1.5rem;
}

.summary {
  font-family: 'Crimson Pro', serif;
  font-size: 1.1rem;
  line-height: 1.7;
  color: #2D4A3E;
  margin-bottom: 2rem;
}

.fact-category {
  margin-bottom: 2rem;
}

.fact-category h3 {
  font-family: 'Crimson Pro', serif;
  font-size: 1.2rem;
  font-weight: 600;
  color: #2D4A3E;
  margin-bottom: 0.75rem;
}

.fact-category ul {
  list-style: none;
  padding-left: 0;
}

.fact-category li {
  font-family: 'Crimson Pro', serif;
  font-size: 1rem;
  line-height: 1.6;
  color: #1A2F23;
  margin-bottom: 0.5rem;
  padding-left: 1.5rem;
  position: relative;
}

.fact-category li::before {
  content: '•';
  position: absolute;
  left: 0;
  color: #B85C38;
  font-weight: bold;
}
```

**Content Generation Template:**

```
Given raw data for [BIRD_NAME]:
- Scientific name: [...]
- Family: [...]
- Size: [...]
- Diet: [...]
- Habitat: [...]
- Range: [...]
- Migration: [...]
- Conservation status: [...]

Generate content in this exact structure:

1. SUMMARY (2 sentences, ~30-40 words total):
   - Sentence 1: Most distinctive visual or behavioral characteristic
   - Sentence 2: Where/when typically encountered
   - Tone: Naturalist, warm but factual
   - Style: Evocative but concise

2. LOOK FOR (2-4 bullets):
   - Most reliable field marks for ID
   - Distinctive behaviors
   - Seasonal plumage changes if dramatic
   - Each bullet: 10-15 words max

3. DID YOU KNOW? (2-4 bullets):
   - Migration distance/pattern if notable
   - Unusual adaptations or abilities
   - Ecological role or cultural significance
   - Each bullet: 15-20 words max

4. CONSERVATION (0-3 bullets, only if noteworthy):
   - Include ONLY if: Threatened, Endangered, or Declining
   - Current status and trend
   - Primary threats
   - Each bullet: 15-20 words max
   - Tone: Informative, not alarmist
   - If stable/common: Skip this section entirely

Constraints:
- Use active voice
- Avoid jargon (or explain it simply)
- No anthropomorphization
- Factual but engaging
- Each section stands alone (no "As mentioned above...")
```

**Example Output (Sanderling):**

```
SUMMARY:
These pale shorebirds chase waves along beaches from fall through 
spring, racing the surf in an endless game of tag. Some individuals 
travel over 10,000 kilometers twice yearly between Arctic tundra and 
tropical shores.

LOOK FOR:
• Running along the surf line like a clockwork wind-up toy
• Pale gray upperparts in winter, rusty-orange in breeding plumage
• Stout black bill and legs, plump body shape
• Often in small flocks, aggressively defending feeding territories

DID YOU KNOW?:
• Breeds farther north than almost any shorebird on Earth
• Migrates up to 10,000km from Arctic to Argentina and back
• Young birds often skip first breeding season, staying on tropical beaches
• Critical stopover at Delaware Bay in May for horseshoe crab eggs

CONSERVATION:
• Declining rapidly - 80% population loss in Americas since 1970s
• Vulnerable to beach development, disturbance, and climate change
• Depends on small number of critical staging areas during migration
```

---

#### 3. Sounds Section

```
┌──────────────────────────┐
│ ♪ Sounds                 │  ← Section header
│                          │
│ ┌────────────────────┐   │
│ │                    │   │  ← Audio card (current)
│ │  [▶  Flight Call]  │   │    Large play button
│ │                    │   │    Clean, minimal
│ │  Short, sharp 'kip'│   │    One-line description
│ │  or 'plit'         │   │
│ │                    │   │
│ └────────────────────┘   │
│                          │
│        ● ○ ○             │  ← Dot navigation
│                          │    Indicates 3 total
│                          │    Tappable to jump
│                          │
│ ← Swipe to hear more →   │  ← Hint (fades after 2s)
│                          │
└──────────────────────────┘

Swipe left:
┌──────────────────────────┐
│ ♪ Sounds                 │
│                          │
│ ┌────────────────────┐   │
│ │                    │   │  ← Now showing card 2
│ │  [▶  Alarm Call]   │   │
│ │                    │   │
│ │  Series of quick   │   │
│ │  notes when        │   │
│ │  disturbed         │   │
│ │                    │   │
│ └────────────────────┘   │
│                          │
│        ○ ● ○             │  ← Dot 2 active
│                          │
└──────────────────────────┘
```

**Interaction:**
- Swipe left/right to cycle (with wraparound: 3→1, 1→3)
- Tap dots to jump to specific sound
- Tap play button → plays audio inline (no modal)
- Pause automatically if user swipes to next
- Shows loading spinner while audio loads
- Handles errors gracefully ("Audio unavailable")

**Content:**
- 1-3 sounds per species (most have 2-3)
- Priority order: Flight call > Song > Alarm call
- One-line description per sound (10-15 words)
- Attribution to Xeno-canto at bottom (small text)

**Implementation:**
```javascript
// Swiper.js configuration
const audioCarousel = new Swiper('.audio-carousel', {
  slidesPerView: 1,
  loop: true,  // Enable wraparound
  pagination: {
    el: '.swiper-pagination',
    clickable: true,
    bulletClass: 'audio-dot',
    bulletActiveClass: 'audio-dot-active',
  },
  on: {
    slideChange: function() {
      // Pause current audio when swiping
      pauseAllAudio();
    }
  }
});
```

---

#### 4. Map Section

```
┌──────────────────────────┐
│ Where & When to Find     │  ← Section header
│                          │
│ ┌────────────────────┐   │
│ │                    │   │  ← Interactive map
│ │    [Map Widget]    │   │    Leaflet or Mapbox GL
│ │                    │   │    Shows range for current
│ │  You are here ◉    │   │    selection (month/season)
│ │                    │   │
│ │  Breeding (rust)   │   │    Legend:
│ │  Winter (blue)     │   │    - Breeding = #B85C38
│ │  Year-round (green)│   │    - Winter = #A8DADC
│ │                    │   │    - Year-round = #2D4A3E
│ └────────────────────┘   │
│                          │
│ February ────────────    │  ← Current selection
│                          │    Updates based on:
│ Jan ━━●━━━━━━━━━━ Dec    │    1. User's current date
│  │  │  │  │  │  │  │     │    2. Manual slider adjust
│  W  Sp Su  F  W          │
│                          │
│ In February, Sanderlings │  ← Contextual description
│ winter on beaches from   │    Generated dynamically
│ New England to Argentina.│    based on selected month
│ Peak migration is in     │
│ May and August.          │
│                          │
└──────────────────────────┘
```

**Map Behavior:**

**Data-Driven Granularity:**
```javascript
// Determine slider granularity from available data
const sliderConfig = {
  highFidelity: {
    // eBird has monthly data
    stops: 12,
    labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 
             'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
  },
  lowFidelity: {
    // Only seasonal data available
    stops: 4,
    labels: ['Winter', 'Spring', 'Summer', 'Fall']
  },
  resident: {
    // Year-round, no migration
    stops: 1,
    labels: ['Year-round'],
    sliderDisabled: true,
    showLifecycleNotes: true  // "Nests May-July" etc.
  }
};

// Auto-detect which to use
const mapConfig = species.hasMonthlyRangeData() 
  ? sliderConfig.highFidelity
  : species.hasMigration()
    ? sliderConfig.lowFidelity
    : sliderConfig.resident;
```

**For Migrants (Sanderling):**
- Default to current month (based on user's date)
- Slider shows 12 stops (Jan-Dec)
- Map updates smoothly as slider moves
- Breeding range (rust), winter range (blue), migration arrows
- Text updates: "In May, refueling at Delaware Bay..."

**For Residents (California Scrub-Jay):**
- Shows year-round range (forest green)
- Slider has 4 stops (seasons) OR is hidden
- Text notes lifecycle: "Nests March-July, pairs mate for life"
- Less dramatic than migrants, but still informative

**Interaction:**
- Pan/zoom enabled (pinch on mobile)
- User location marker if permission granted
- Tap legend items to toggle layers
- Smooth transitions between months (300ms ease)

**Contextual Text Template:**
```
Generate 1-2 sentences describing where to find [BIRD_NAME] in [MONTH]:

Input:
- Current month: February
- Breeding range: Canadian Arctic (June-July)
- Winter range: Coastal beaches, New England to Argentina (Aug-May)
- Migration timing: Northbound March-May, Southbound July-November
- Key stopovers: Delaware Bay (May)

Output:
"In February, Sanderlings winter on sandy beaches from New England 
to Argentina. Peak spring migration begins in March, with critical 
refueling at Delaware Bay in May."

Rules:
- First sentence: Where they are THIS month
- Second sentence: What's coming next (migration timing, breeding)
- Use present tense
- Include specific locations when relevant
- Mention critical stopovers if applicable
- Keep total under 40 words
```

---

#### 5. Discovery Exit

```
┌──────────────────────────┐
│ Similar Birds            │  ← Section header
│                          │
│ ┌──────────┐ ┌──────────┐│  ← 2 tiles
│ │  [img]   │ │  [img]   ││    Matched by habitat
│ │          │ │          ││    + present in region
│ │  Dunlin  │ │  Plover  ││
│ │  Small   │ │  Small   ││    Short descriptor
│ │  shore   │ │  shore   ││    (1 line)
│ └──────────┘ └──────────┘│
│                          │
│  ┌──────────────────┐    │  ← Main CTA
│  │ See more birds   │    │    Goes to Explore page
│  │ near you    →    │    │    (Filtered by location
│  └──────────────────┘    │     + current month)
│                          │
└──────────────────────────┘
```

**Similar Birds Logic:**
```javascript
function getSimilarBirds(currentBird, userLocation, currentMonth) {
  // Priority order for matching:
  // 1. Same habitat + currently present
  // 2. Same family
  // 3. Visually similar
  
  const candidates = allBirds.filter(bird => 
    bird.id !== currentBird.id &&
    bird.habitat === currentBird.habitat &&
    bird.isPresentIn(userLocation, currentMonth)
  );
  
  // If fewer than 2 found, expand to same family
  if (candidates.length < 2) {
    candidates.push(...allBirds.filter(bird =>
      bird.family === currentBird.family &&
      bird.isPresentIn(userLocation, currentMonth) &&
      !candidates.includes(bird)
    ));
  }
  
  // Return top 2
  return candidates.slice(0, 2);
}
```

**Tile Content:**
- Illustration thumbnail
- Common name (1 line, truncate if needed)
- One-line descriptor: habitat + size
  - "Small shorebird"
  - "Large raptor"
  - "Woodland songbird"

**CTA Behavior:**
- Tapping tiles → That bird's species page
- Tapping "See more" → Explore page (pre-filtered to user's location + current month)

---

## Content Generation Strategy

### Overview
Use Claude AI to generate consistent, high-quality content from validated templates. Start manual, validate template, then automate.

### Four-Phase Approach

#### Phase 1: Manual Baseline (20 birds)
**Goal:** Establish voice, quality bar, and content patterns

**Process:**
1. Hand-write content for 20 enhanced species
2. Focus on diverse types:
   - Common backyard (Robin, Cardinal)
   - Charismatic (Eagle, Owl)
   - Migrants (Sanderling, Warbler)
   - Residents (Scrub-Jay, Chickadee)
3. Refine until every piece feels right
4. These become gold standard examples

**Deliverable:** 20 perfect species pages + clear understanding of voice

---

#### Phase 2: Template Validation (100 birds)
**Goal:** Prove Claude can match the manual quality

**Process:**
1. Build Claude prompt template (see Facts Section above)
2. Include 3-5 gold standard examples in prompt
3. Generate content for 100 common birds
4. **Review every single one** - this is critical
5. Track failure modes:
   - Too technical/jargon-heavy?
   - Missing personality?
   - Factual errors?
   - Wrong tone?
6. Refine template based on patterns
7. Regenerate failures with improved template
8. Repeat until 95%+ quality rate

**Deliverable:** Validated template that reliably produces good content

---

#### Phase 3: Automated Scale (10,880 birds)
**Goal:** Generate content for remaining species efficiently

**Process:**
1. Batch process in groups of 500
2. Automated pipeline:
   ```
   Input: eBird data + Wikipedia summary
   ↓
   Claude API with validated template
   ↓
   Generated content (summary + 3 fact categories)
   ↓
   Automated checks (length limits, format validation)
   ↓
   Store in JSON files
   ```
3. Spot-check 1% randomly (~110 birds)
4. If spot-check quality drops below 90%, pause and refine
5. Continue until all birds have content

**Deliverable:** Complete coverage of 11,000+ species

---

#### Phase 4: Continuous Enhancement (Ongoing)
**Goal:** Progressively enhance popular species based on usage

**Process:**
1. Analytics identify most-viewed species
2. Manually enhance 3-5 per week:
   - Unique illustrations (not template)
   - Hand-written prose (not generated)
   - Multiple plumage variations
   - Detailed migration narratives
3. Promote from "basic" to "enhanced" tier
4. No pressure, no deadlines - quality over quantity

**Deliverable:** Growing library of showcase species

---

### Template Iteration Example

**Version 1 (Initial):**
```
Generate 2 sentences about [BIRD] and 3 categories of facts.
```
**Result:** Too generic, lacks personality

**Version 2 (After 10 reviews):**
```
You are a naturalist writer. Generate content about [BIRD] in warm 
but factual tone. Include:
1. Summary (2 sentences)
2. Look For (2-4 bullets)
3. Did You Know (2-4 bullets)
4. Conservation (if threatened)

Use examples: [5 gold standard species]
```
**Result:** Better, but still some jargon

**Version 3 (After 50 reviews):**
```
[Full template from Facts Section above]
+ Explicit constraints on length, tone, vocabulary
+ 5 gold standard examples
+ "Do not use these words: utilize, facilitate, individuals (use birds)..."
+ "Do use: active voice, specific details, evocative language"
```
**Result:** 95%+ match manual quality

---

### Quality Assurance

**Automated Checks:**
```javascript
function validateGeneratedContent(content) {
  const checks = {
    summaryLength: content.summary.split(' ').length >= 25 
                   && content.summary.split(' ').length <= 50,
    
    hasAllCategories: content.lookFor && content.didYouKnow,
    
    bulletCount: content.lookFor.length >= 2 
                 && content.lookFor.length <= 4,
    
    noBannedWords: !content.text.match(/utilize|facilitate|individuals/),
    
    hasSpecifics: content.text.match(/\d/) // Contains numbers
  };
  
  return Object.values(checks).every(check => check === true);
}
```

**Manual Review Checklist:**
- [ ] Facts are accurate (cross-check with eBird/Wikipedia)
- [ ] Tone is warm but not anthropomorphic
- [ ] No jargon or unexplained technical terms
- [ ] Interesting and memorable (not generic)
- [ ] Appropriate length (not too verbose)

---

## Technical Implementation Notes

### Performance Targets
- **First Contentful Paint:** < 1.5s
- **Time to Interactive:** < 2.5s
- **Largest Contentful Paint:** < 2.5s
- **Cumulative Layout Shift:** < 0.1

### Image Optimization
```javascript
// Next.js Image component with optimization
<Image
  src={`/images/species/${speciesCode}.webp`}
  alt={`${commonName} illustration`}
  width={800}
  height={1200}
  quality={85}
  priority={isHero}  // Only for hero images
  loading={isHero ? "eager" : "lazy"}
  placeholder="blur"
  blurDataURL={thumbnailBase64}
/>
```

### Data Loading Strategy
```javascript
// Static generation for all species pages
export async function generateStaticParams() {
  const species = await getAllSpecies();
  return species.map(bird => ({
    code: bird.speciesCode
  }));
}

export async function generateMetadata({ params }) {
  const bird = await getSpeciesByCode(params.code);
  return {
    title: `${bird.commonName} - Aviary`,
    description: bird.summary,
    openGraph: {
      images: [`/images/species/${params.code}.webp`]
    }
  };
}
```

### Audio Handling
```javascript
// Lazy load audio, don't fetch until play button clicked
const audioRef = useRef(null);

const handlePlay = async () => {
  if (!audioRef.current) {
    // Fetch from Xeno-canto on first play
    const audioUrl = await fetchXenoCantoRecording(recordingId);
    audioRef.current = new Audio(audioUrl);
  }
  await audioRef.current.play();
};
```

### Map Data Structure
```javascript
// GeoJSON for range polygons
{
  "speciesCode": "sander",
  "ranges": {
    "monthly": [
      {
        "month": 1,  // January
        "breeding": null,
        "winter": { type: "Polygon", coordinates: [...] },
        "yearRound": null
      },
      // ... months 2-12
    ],
    "seasonal": [
      {
        "season": "winter",
        "range": { type: "Polygon", coordinates: [...] }
      },
      // ... spring, summer, fall
    ]
  }
}
```

---

## Accessibility

### Screen Reader Support
- All images have descriptive alt text
- Audio players have ARIA labels
- Map has text alternative describing range
- Slider has aria-valuetext for current month
- Proper heading hierarchy (h1 → h2 → h3)

### Keyboard Navigation
- Tab order follows visual flow
- Slider operable with arrow keys
- Audio carousel navigable with arrows
- All interactive elements keyboard accessible
- Focus indicators clearly visible

### Color Contrast
- All text meets WCAG AA standards (4.5:1 minimum)
- Interactive elements meet AA for large text (3:1)
- Never rely solely on color to convey information

---

## Error States

### No Audio Available
```
┌────────────────────────┐
│ ♪ Sounds              │
│                        │
│ No recordings          │
│ available yet          │
│                        │
│ [Contribute to         │
│  Xeno-canto →]         │
└────────────────────────┘
```

### Location Permission Denied
```
┌────────────────────────┐
│ Enter your location    │
│ to see nearby birds    │
│                        │
│ ┌──────────────────┐   │
│ │ City or region   │   │
│ └──────────────────┘   │
│                        │
│ [Continue →]           │
└────────────────────────┘
```

### No Similar Birds Found
```
┌────────────────────────┐
│ Explore More           │
│                        │
│ ┌──────────────────┐   │
│ │ Browse all birds │   │
│ │ near you    →    │   │
│ └──────────────────┘   │
└────────────────────────┘
```

---

## Next Steps

With Landing and Species pages defined, this document continues with:
- **[Explore / Birds Around You Page](#explore-page)**
- **[About Page](#about-page)**

---

## Explore Page (Migration Map)

### Purpose
**Primary Goal:** Visualize bird migration as a living, flowing system.

**Core Question Answered:** "What birds are moving through the world RIGHT NOW?"

**Secondary Goals:**
- Show migration as temporal patterns (not just static ranges)
- Make invisible journeys visible and understandable
- Highlight critical stopover sites and migration spectacles
- Support local discovery ("What's here?") after the wow factor

### Revolutionary Approach

**Traditional tools show:** Where birds are (static heat maps, range polygons)  
**Aviary shows:** Where birds are GOING (animated migration flows, birds in motion)

This is the defining feature that makes Aviary unique.

### Design Philosophy

**AI does the heavy lifting:**
- Auto-groups 350+ migrants into 5-8 visual flows
- Infers realistic paths from breeding/winter ranges + flyways
- Filters to show only ACTIVE migrations (birds moving NOW)
- Generates contextual narratives for each month

**User does minimal work:**
- Looks at the map (immediately beautiful)
- Drags time slider (brings migration to life)
- Optionally taps paths/hotspots to learn more
- Scrolls down for traditional bird list

**No toggles, no choices, one perfect default view.**

### Information Architecture

```
1. Migration Map (Full-Screen Feature)
   ├─ Interactive world map
   ├─ 5-8 animated migration paths (auto-grouped)
   ├─ 1-3 pulsing hotspots (critical stopovers)
   ├─ Time slider (Jan-Dec)
   ├─ Auto-generated status narrative
   └─ Optional controls (Play, Legend)

2. What's Flying Through (Educational Context)
   ├─ List of active migration groups
   ├─ Top species in each group
   ├─ Route summaries
   └─ CTAs to explore families

3. All Birds Here Now (Traditional Discovery)
   ├─ Complete species list (residents + migrants)
   ├─ Grid/List toggle
   ├─ Sort options
   └─ Standard browse experience
```

### Detailed Layout (Mobile)

#### 1. Migration Map (The Feature)

#### 1. Migration Map (The Feature)

```
┌──────────────────────────────────┐
│ Migration Map               [≡]  │ ← Minimal header (40px)
├──────────────────────────────────┤
│                                  │
│                                  │
│        [INTERACTIVE MAP]         │ ← 75% of viewport
│                                  │   Edge-to-edge, immersive
│    ◉ You are here                │
│                                  │   Layers (bottom→top):
│    [5-8 animated paths]          │   1. Base map (muted)
│    [1-3 pulsing hotspots]        │   2. Heat (all birds, subtle)
│                                  │   3. Migration paths (bold)
│                                  │   4. Hotspots (pulsing)
│    Santa Barbara, CA             │   5. Your location
│    May 2026                      │
│                                  │
│    ▼ Peak Spring Migration       │ ← Status (bottom overlay)
│    47 species passing through    │   Auto-generated text
│                                  │   Semi-transparent bg
│    Jan ━━━━━●━━━━━━━━━━━━━ Dec  │ ← Time slider (integrated)
│                                  │
│    [▶ Play]         [Legend ≡]  │ ← Controls (corners)
│                                  │   Optional interactions
└──────────────────────────────────┘

Visual Details:
- Path colors: Family-based (Shorebirds=blue, Warblers=gold, etc.)
- Animation: Flowing particles moving along paths
- Glow: Paths pulse with intensity based on activity
- Hotspots: Concentric rings pulsing (Delaware Bay, etc.)
- Opacity: Varies by month (bright=active, dim=stationary)
```

**Path Visualization:**
```css
/* Each migration path */
.migration-path {
  stroke-width: 3px;
  stroke-dasharray: 10 5;  /* Dashed line */
  opacity: 0.2-1.0;  /* Varies by month */
  filter: drop-shadow(0 0 4px currentColor);  /* Glow */
  animation: glow 2s ease-in-out infinite;
}

/* Animated particles flowing along path */
.path-particle {
  r: 4px;
  fill: currentColor;
  opacity: 0.8;
  /* SVG animateMotion moves along path */
}

/* Hotspot marker */
.hotspot-marker {
  animation: pulse 2s infinite;
  filter: drop-shadow(0 0 8px rgba(74, 144, 226, 0.8));
}
```

**Time Slider Behavior:**

As user drags from January → December:

```
January (Winter):
- Paths DIM (opacity 0.2)
- Animation PAUSED
- Text: "Migration quiet - birds at winter grounds"
- Visualization: Shows clustering/concentration

March (Spring Begins):
- Paths BRIGHTEN (opacity 0.6)
- Animation SLOW
- Text: "Spring migration beginning"
- Visualization: Emerging flows

May (PEAK):
- Paths MAXIMUM BRIGHT (opacity 1.0)
- Animation FAST
- Text: "Peak spring migration - 47 species passing"
- Hotspots: Delaware Bay PULSING
- Visualization: Maximum activity

July (Breeding):
- Paths DIM (opacity 0.3)
- Animation PAUSED
- Text: "Breeding season - birds nesting"
- Visualization: Arctic concentration

September (Fall Migration):
- Paths BRIGHTEN (opacity 0.8)
- Animation FAST REVERSE (southbound)
- Text: "Fall migration - return journey"
- Visualization: Southbound flows
```

**Interaction: Tap a Path**

```
User taps blue shorebird path
  ↓
Modal slides up from bottom:

┌────────────────────────────────┐
│ ──── Shorebirds                │ ← Color bar matches path
│ Atlantic Flyway                │
│                                │
│ 23 species including:          │ ← Top 5 by abundance
│ • Sanderling                   │
│ • Dunlin                       │
│ • Red Knot                     │
│ • Ruddy Turnstone              │
│ • Semipalmated Sandpiper       │
│                                │
│ Journey                        │
│ From: South American coast     │ ← Route summary
│ To: High Arctic tundra         │
│ Distance: 5,000-10,000 km      │
│                                │
│ Critical Stopovers             │
│ • Delaware Bay (May)           │ ← Listed with timing
│ • James Bay (August)           │
│                                │
│ [See all 23 species →]         │ ← CTA to filtered list
│                      [Close]   │
└────────────────────────────────┘
```

**Interaction: Tap a Hotspot**

```
User taps pulsing Delaware Bay marker
  ↓
Modal with details:

┌────────────────────────────────┐
│ Delaware Bay                   │
│ Critical Shorebird Stopover    │
│                                │
│ Right now (May 15):            │
│ ~100,000 shorebirds            │
│                                │
│ What's happening:              │ ← Educational narrative
│ Birds are gorging on horseshoe │
│ crab eggs, doubling their body │
│ weight in 2 weeks for final    │
│ push to Arctic.                │
│                                │
│ Top species here:              │
│ • Red Knot (30,000)            │ ← Abundance numbers
│ • Sanderling (25,000)          │
│ • Ruddy Turnstone (20,000)     │
│                                │
│ [Learn more] [Close]           │
└────────────────────────────────┘
```

**Play Button:**

```
User taps [▶ Play]
  ↓
Auto-animates through year:
- Slider advances Jan → Dec (30 seconds total)
- Paths animate, hotspots pulse on cue
- Narrative updates each month
- Pauses briefly at peak months

Controls during playback:
[⏸ Pause] [⏮ Restart] [Speed: 2x ▾]
```

**Legend Toggle:**

```
[Legend ≡]
  ↓ Tap
Small overlay in corner:

┌──────────────────┐
│ Migration Flows  │
│                  │
│ ──── Shorebirds  │ ← Only active groups
│ ──── Warblers    │   (5-8 shown)
│ ──── Raptors     │
│ ──── Waterfowl   │
│                  │
│ ◉ Hotspot (peak) │ ← Symbol key
│ ○ Your location  │
│                  │
│     [Close ×]    │
└──────────────────┘

Tap any family → highlights that path
Tap again → dehighlights
```

---

#### 2. What's Flying Through (Educational Context)

```

---

## About Page

### Purpose
Share the story, mission, and community behind Aviary.

**Goals:**
- Explain what Aviary is and why it exists
- Connect with users who share the love of birds
- Invite community participation and feedback
- Build emotional connection through personal storytelling

### Structure

```
1. Hero / Opening
2. What This Is
3. Why It Exists
4. How It Works
5. Community Section
   └─ Comments / Stories
```

### Layout (Mobile)

```
┌──────────────────────────────────┐
│ ← Back                      [≡]  │
├──────────────────────────────────┤
│                                  │
│     [Beautiful illustration]     │ ← Hero image
│     (Bird in flight or           │   Full viewport
│      nature scene)               │   Sets the tone
│                                  │
│                                  │
│ About Aviary                     │ ← Title (left-bottom)
│                                  │   Like species page
└──────────────────────────────────┘
              ↓ Scroll
┌──────────────────────────────────┐
│ What This Is                     │ ← Section 1
│                                  │
│ Aviary is a free, illustrated   │   2-3 paragraphs
│ field guide to all 11,000+      │   Warm, personal
│ bird species on Earth. It       │   First person OK
│ combines the beauty of vintage  │
│ Audubon prints with modern      │
│ web technology to help you      │
│ discover, identify, and learn   │
│ about birds anywhere in the     │
│ world.                          │
│                                  │
│ Whether you're trying to        │
│ identify a bird in your         │
│ backyard, planning a birding    │
│ trip, or just curious about     │
│ the incredible journeys birds   │
│ undertake each year, this site  │
│ is here to help.                │
│                                  │
└──────────────────────────────────┘
              ↓
┌──────────────────────────────────┐
│ Why It Exists                    │ ← Section 2
│                                  │
│ [Your personal story here]       │   3-4 paragraphs
│                                  │   Authentic voice
│ This started as a personal      │   Why you care
│ project born from [your story]. │   What inspired you
│                                  │
│ I wanted to build something     │   The vision:
│ that:                           │   - Free forever
│ • Is always free, with no ads   │   - Beautiful
│ • Respects your time and        │   - Educational
│   attention                     │   - Gift to community
│ • Celebrates the beauty of      │
│   birds and nature              │
│ • Makes birding accessible      │
│   to everyone                   │
│                                  │
│ The internet has too many       │
│ things designed to capture      │
│ your attention. This is         │
│ designed to give you something  │
│ beautiful, then let you go      │
│ enjoy the birds themselves.     │
│                                  │
└──────────────────────────────────┘
              ↓
┌──────────────────────────────────┐
│ How It Works                     │ ← Section 3
│                                  │
│ All 11,000+ species have:       │   Technical overview
│ • AI-generated illustrations    │   (friendly language)
│   in Audubon's style            │
│ • Migration maps and seasonal   │
│   data from eBird               │
│ • Bird calls from Xeno-canto   │
│                                  │
│ Priority species also get:      │
│ • Hand-written naturalist       │
│   descriptions                  │
│ • Detailed migration stories    │
│ • Multiple plumage variations   │
│                                  │
│ Everything is built on public   │
│ data and open-source tools.     │
│ The illustrations are           │
│ AI-generated but manually       │
│ curated. The descriptions for   │
│ enhanced species are written    │
│ by hand.                        │
│                                  │
│ This is a work in progress,     │
│ growing slowly and carefully.   │
│                                  │
└──────────────────────────────────┘
              ↓
┌──────────────────────────────────┐
│ Support This Project             │ ← Optional section
│                                  │
│ Aviary costs about $3/month     │   Transparent costs
│ to run. If you find it useful,  │   + donation option
│ you can:                        │
│                                  │
│ [Buy me a coffee →]             │   Ko-fi / BMAC button
│                                  │
│ Or donate to bird conservation: │
│ • Audubon Society               │
│ • Cornell Lab of Ornithology    │
│ • [Local conservation org]      │
│                                  │
└──────────────────────────────────┘
              ↓
┌──────────────────────────────────┐
│ Share Your Story                 │ ← Community section
│                                  │
│ What got you into birding?       │   Invitation to share
│ How did you discover Aviary?     │
│ What do you love about birds?    │
│                                  │
│ [Leave a comment below ↓]        │
│                                  │
└──────────────────────────────────┘
              ↓
┌──────────────────────────────────┐
│ Comments                         │ ← Comments section
│                                  │
│ ┌──────────────────────────┐     │
│ │ Share your thoughts...   │     │   Simple textarea
│ │                          │     │
│ │                          │     │
│ └──────────────────────────┘     │
│                                  │
│ Name (optional): [_______]       │   Name field (optional)
│                                  │   Email NOT required
│ [Submit]                         │
│                                  │
├──────────────────────────────────┤
│                                  │
│ Sarah M. • 2 days ago            │ ← Posted comments
│ "I started birding during the    │   Simple, clean
│ pandemic. Watching birds at my   │   Minimal moderation
│ feeder became my daily ritual.   │   Display chronological
│ This site is beautiful!"         │
│                                  │
│ ──────────────────────────────    │
│                                  │
│ James K. • 1 week ago            │
│ "My grandfather taught me to     │
│ identify birds when I was six.   │
│ He'd love this."                 │
│                                  │
│ ──────────────────────────────    │
│                                  │
│ [Load more comments]             │
│                                  │
└──────────────────────────────────┘
```

---

### Content Guidelines

**Tone:**
- Personal and warm (first-person is OK)
- Honest about what this is (passion project, not a business)
- Grateful to the birding community
- Humble about limitations (work in progress)
- Enthusiastic but not overly sentimental

**"Why It Exists" - Example Structure:**

```
[Opening - personal hook]
This started when I [your birding moment]. I realized there 
wasn't a website that combined [what you wanted].

[The problem]
Most field guides are books (not searchable). Most birding apps 
are functional but not beautiful. Most websites are either 
comprehensive but ugly, or pretty but incomplete.

[The vision]
I wanted to build something that:
• Is always free and ad-free
• Treats birds (and users) with respect
• Combines beauty with utility
• Makes migration visible and understandable
• Celebrates the joy of discovery

[The approach]
This is a labor of love, built slowly in evenings and weekends. 
It's not trying to be a business or maximize engagement. It's 
just trying to be a beautiful, useful resource for anyone who 
loves birds.

[The community]
If this helps you identify a bird, plan a trip, or just brings 
a moment of beauty to your day, that's success. And if you 
want to support it, share your birding story below.
```

---

### Comments System

**Technical Implementation:**

**Option A: Third-party (easiest)**
- Use Disqus, Commento, or utterances (GitHub issues)
- Pro: Easy setup, spam filtering, moderation tools
- Con: Third-party dependency, potential privacy concerns

**Option B: Simple custom (aligned with values)**
- Store comments in database (Supabase/Postgres)
- Simple form: message + optional name
- No authentication required
- Manual moderation (approve before showing)
- Pro: Full control, privacy-friendly, aligned with project ethos
- Con: More work, need spam protection

**Recommendation:** Option B (custom) using Supabase:
```javascript
// Comment submission
async function submitComment(text, name = 'Anonymous') {
  await supabase.from('comments').insert({
    page: 'about',
    text: text,
    name: name,
    approved: false,  // Requires manual approval
    created_at: new Date()
  });
}

// Display approved comments
const { data: comments } = await supabase
  .from('comments')
  .select('*')
  .eq('page', 'about')
  .eq('approved', true)
  .order('created_at', { ascending: false });
```

**Spam Protection:**
- Require manual approval before showing
- Simple honeypot field (hidden from real users)
- Rate limiting (max 1 comment per IP per hour)
- No links allowed in comments (or require approval)

**Moderation:**
- Simple admin page to approve/reject
- Email notification on new comment
- Flag inappropriate content
- Can ban IP if needed

---

### Navigation

**Header:**
- Simple back arrow (← Back)
- Or breadcrumb: Home > About

**Footer (on About page):**
```
┌──────────────────────────────────┐
│ Aviary                           │
│                                  │
│ [Home] [About] [GitHub]          │
│                                  │
│ Built with care in 2026          │
│ Always free • No ads             │
│                                  │
└──────────────────────────────────┘
```

---

### Accessibility & SEO

**Meta Tags:**
```html
<title>About Aviary - An Illustrated Birding Field Guide</title>
<meta name="description" content="Learn about Aviary, a free illustrated field guide to 11,000+ bird species, built with care and shared as a gift to the birding community.">
<meta property="og:title" content="About Aviary">
<meta property="og:description" content="A labor of love celebrating birds, migration, and discovery.">
<meta property="og:image" content="/images/about-hero.jpg">
```

**Heading Structure:**
```
h1: About Aviary
h2: What This Is
h2: Why It Exists
h2: How It Works
h2: Support This Project
h2: Share Your Story
h3: Comments
```

**Screen Reader:**
- Proper semantic HTML throughout
- Alt text on hero image
- Skip to comments link
- Form labels for comment submission

---

*Last updated: February 14, 2026*
