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

## Landing Page

### Purpose
**Primary Goal:** Get users to either search for a specific bird or explore birds around them.

**Secondary Goals:**
- Showcase the beauty and quality of the site
- Demonstrate the vintage aesthetic
- Set expectations (comprehensive, illustrated, migration-focused)

### Layout (Mobile)

```
┌──────────────────────────┐
│ AVIARY              [≡]  │  ← Minimal header
├──────────────────────────┤    Logo left, menu right
│                          │
│                          │
│                          │
│     [Audubon-style       │  ← Hero illustration
│      Illustration]       │    Fills viewport
│                          │    Rotates daily or seasonally
│                          │    Generous padding (2rem)
│                          │
│                          │
│ Discover Birds           │  ← Headline
│ An illustrated field     │    Serif, elegant
│ guide to the world       │
│                          │
└──────────────────────────┘
           ↓ Scroll

┌──────────────────────────┐
│ ┌────────────────────┐   │  ← Search bar
│ │ Search by name...  │   │    Large, prominent
│ └────────────────────┘   │    Autocomplete on type
│                          │
│      [Birds Around       │  ← Primary CTA
│       You →]             │    Clear, simple button
│                          │
└──────────────────────────┘
           ↓

┌──────────────────────────┐
│ Featured Birds           │  ← 3-4 showcase cards
│                          │
│ ┌────────┐  ┌────────┐  │    Grid layout
│ │ [img]  │  │ [img]  │  │    Enhanced species only
│ │ Name   │  │ Name   │  │    Rotates weekly/seasonal
│ │ Excerpt│  │ Excerpt│  │
│ └────────┘  └────────┘  │
│                          │
│ ┌────────┐  ┌────────┐  │
│ │ [img]  │  │ [img]  │  │
│ │ ...    │  │ ...    │  │
│ └────────┘  └────────┘  │
└──────────────────────────┘
           ↓

┌──────────────────────────┐
│ About • Support • GitHub │  ← Footer (minimal)
└──────────────────────────┘
```

### Interactions

**Search Bar:**
- Autofocus on desktop (not mobile - prevents keyboard popup)
- Real-time fuzzy search as user types
- Shows thumbnail + scientific name in dropdown
- Handles typos ("rob" → "American Robin", "European Robin", "Rufous-backed Robin")
- Max 8 results shown, rest accessible via "See all results"
- Enter or click → Species detail page

**Birds Around You Button:**
- Requests geolocation permission on click
- If denied → Manual location entry modal
- If granted → Smooth transition to filtered bird list (current month)
- Shows loading state while fetching data

**Featured Cards:**
- Click → Species detail page
- Gentle lift on hover (4px elevation)
- Staggered fade-in on page load (50ms delays)
- Images lazy-loaded

### Content Strategy

**Hero Illustration:**
- Rotates based on season or day of week
- Always an enhanced species with beautiful illustration
- Could tie to current season (hummingbird in summer, cardinal in winter)

**Featured Birds (3-4 cards):**
- Always enhanced species (showcase quality)
- Mix of: charismatic (eagle, owl), common (robin, cardinal), interesting migrants
- Updates monthly to keep content fresh
- Short excerpt (1 sentence) from enhanced description

---

## Species Detail Page

### Purpose
**Answer:** "Is this the bird I saw?" (ID confirmation)  
**Inform:** Interesting facts, sounds, range  
**Invite:** Discover similar birds or explore migration

### Information Architecture

```
1. Hero Illustration
   └─ Name (left-aligned bottom)

2. Facts (What Makes Them Special)
   ├─ 2-sentence summary
   ├─ Look For (behavior/field marks)
   ├─ Did You Know? (surprising facts)
   └─ Conservation (status if relevant)

3. Sounds
   └─ 1-3 audio recordings with descriptions

4. Map (Where & When to Find)
   ├─ Interactive map
   ├─ Time slider (12 months or 4 seasons)
   └─ Contextual description

5. Exit (Discovery)
   ├─ 2 similar bird tiles
   └─ "See more birds near you" CTA
```

### Detailed Layout (Mobile)

#### 1. Hero Section

```
┌──────────────────────────┐
│ ←                        │  ← Back button (top-left)
│                          │
│                          │
│     [Audubon-style       │  ← Illustration fills viewport
│      Illustration]       │    High-quality AI-generated
│                          │    Shows most common plumage
│                          │    Generous padding (2rem)
│                          │
│                          │
│                          │
│ Sanderling               │  ← Name (left-aligned bottom)
│ Calidris alba            │    Scientific name below
│                          │
└──────────────────────────┘

CSS:
.hero {
  position: relative;
  height: 100vh;
  padding: 2rem;
  background: #FAF7F0;
}

.bird-name {
  position: absolute;
  bottom: 2rem;
  left: 2rem;
  font-family: 'Cormorant Garamond', serif;
  font-size: 2rem;
  font-weight: 600;
  color: #1A2F23;
  line-height: 1.2;
}

.scientific-name {
  font-style: italic;
  font-size: 1.2rem;
  color: #8B7355;
  margin-top: 0.25rem;
}
```

**Design Notes:**
- Name placement: Left-bottom feels organic, less formal than top-right
- Scientific name in true italics (not slanted roman)
- Back button uses simple arrow icon, no text
- Illustration breathes with generous padding

---

#### 2. Facts Section

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

## Explore Page

### Purpose
**Primary Goal:** Help users discover birds in their location right now (or any location/time).

**Core Question Answered:** "What birds can I see here this month?"

**Secondary Goals:**
- Enable temporal exploration (what's here in other months?)
- Support spatial exploration (what's in other regions?)
- Filter by habitat for focused discovery

### User Entry Points

**From Landing Page:**
- Click "Birds Around You" button
- Auto-detects location OR prompts for manual entry
- Defaults to current month

**From Species Page:**
- Click "See more birds near you" CTA
- Inherits user's location (if already set)
- Defaults to current month
- **No habitat pre-filtering** - shows all birds in area

**From Direct URL:**
- `/explore` with optional params
- Restores previous state if returning via back button

### Information Architecture

```
1. Location & Controls
   ├─ Location input (auto-detect or manual)
   ├─ Compact map (context + density visualization)
   ├─ Time slider (month selection)
   └─ Radius control (search area)

2. Filtering & Display Options
   ├─ Habitat filter (optional chips)
   ├─ View toggle (Grid/List)
   └─ Sort dropdown

3. Results
   ├─ Bird count header
   ├─ Grid or List of birds (adaptive default)
   └─ Pagination (load more or infinite scroll)
```

### Detailed Layout (Mobile)

#### 1. Header & Location

```
┌──────────────────────────────────┐
│ Birds Around You            [≡]  │ ← Page header
├──────────────────────────────────┤
│                                  │
│ ┌──────────────────────────┐     │ ← Location input
│ │ 📍 Santa Barbara, CA  [✕]│     │   - Auto-detected on first load
│ └──────────────────────────┘     │   - Or manual entry with autocomplete
│                                  │   - Clear (✕) to change location
│                                  │
└──────────────────────────────────┘

CSS:
.location-input {
  font-family: 'Crimson Pro', serif;
  font-size: 1.1rem;
  padding: 1rem 1.5rem;
  border: 2px solid #8B7355;
  border-radius: 8px;
  background: white;
  color: #1A2F23;
}

.location-input::placeholder {
  color: #8B7355;
  opacity: 0.7;
}
```

**Location Detection Logic:**
```javascript
async function detectLocation() {
  try {
    // Try browser geolocation first
    const position = await getCurrentPosition();
    const location = await reverseGeocode(
      position.coords.latitude, 
      position.coords.longitude
    );
    return location; // "Santa Barbara, CA"
  } catch (error) {
    // Permission denied or not available
    // Show manual entry prompt
    return null;
  }
}
```

**Manual Entry:**
- Uses Google Places Autocomplete (or similar)
- Suggests cities, regions, landmarks
- Handles "Santa Barbara", "Santa Barbara CA", "93101"
- Biases results to user's general region

**Empty State (Location Not Detected):**
```
┌──────────────────────────────────┐
│ Birds Around You                 │
│                                  │
│ 👇 Where are you birding?        │
│                                  │
│ ┌──────────────────────────┐     │
│ │ Enter city or region...  │     │
│ └──────────────────────────┘     │
│                                  │
│ Or tap map to select location    │
│                                  │
│ Popular regions:                 │
│ [California] [Texas] [Florida]   │
│                                  │
└──────────────────────────────────┘
```

---

#### 2. Map & Context

```
┌──────────────────────────────────┐
│ ┌────────────┐  47 birds         │ ← Compact map (1/3 width)
│ │    Map     │  in February      │   Shows selected region
│ │            │                   │   Color intensity = density
│ │     ◉      │  [Expand full ⤢]  │   Darker = more species
│ │            │                   │
│ └────────────┘                   │
│                                  │
│ Legend: Darker = More species    │ ← Simple legend
└──────────────────────────────────┘

CSS:
.map-container {
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 1rem;
  padding: 1.5rem;
  background: white;
  border-radius: 8px;
  margin: 1rem 0;
}

.map-widget {
  aspect-ratio: 1;
  border: 2px solid #8B7355;
  border-radius: 4px;
  position: relative;
}

.bird-count {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.5rem;
  font-weight: 600;
  color: #1A2F23;
}
```

**Map Behavior:**
- Shows user's selected region (zoomed appropriately)
- User location marked with ◉
- Radius circle shown (subtle outline)
- Color gradient overlay:
  - Light green: 1-20 species
  - Medium green: 21-50 species
  - Dark green: 51+ species
- Pan/zoom enabled
- Tap "Expand full ⤢" → Full-screen map view

**Full-Screen Map (Expanded):**
```
┌──────────────────────────────────┐
│ ← Back to results                │
├──────────────────────────────────┤
│                                  │
│                                  │
│        [Full Screen Map]         │ ← Interactive
│                                  │   Pan anywhere
│              ◉                   │   Zoom in/out
│         You are here             │
│                                  │
│     Tap any region to            │
│     explore birds there          │
│                                  │
│                                  │
└──────────────────────────────────┘
      Jan ━━━●━━━━━━━━━━━━ Dec     ← Time slider (sticky)
```

**When user taps different region:**
```
┌─────────────────────────┐
│ Northern California     │ ← Modal overlay
│ 83 birds in February    │
│                         │
│ Top 3:                  │
│ • American Robin        │
│ • Dark-eyed Junco       │
│ • Red-tailed Hawk       │
│                         │
│ [Switch to this region] │
│              [Cancel]   │
└─────────────────────────┘
```

---

#### 3. Time & Radius Controls

```
┌──────────────────────────────────┐
│ February ──────────────────       │ ← Time slider
│ Jan ━━━●━━━━━━━━━━━━━━━━━ Dec   │   Defaults to current month
│                                  │   Sticky on scroll
│                                  │
│ Within: [50 miles ▾]             │ ← Radius dropdown
│                                  │   Smart default based on
│                                  │   population density
└──────────────────────────────────┘

CSS:
/* Make time controls sticky on scroll */
.time-controls {
  position: sticky;
  top: 0;
  background: #FAF7F0;
  padding: 1rem 1.5rem;
  z-index: 10;
  border-bottom: 1px solid rgba(139, 115, 85, 0.2);
}

/* On scroll down, add subtle shadow */
.time-controls.scrolled {
  box-shadow: 0 2px 8px rgba(26, 47, 35, 0.08);
}
```

**Time Slider:**
- Identical to Species page implementation
- 12 stops (one per month)
- Current month highlighted
- Updates results dynamically (with brief loading state)
- Label shows full month name

**Radius Control:**
```
Within: [50 miles ▾]
   ↓ Click
┌──────────────────┐
│ 10 miles         │
│ 25 miles         │
│ 50 miles      ✓  │
│ 100 miles        │
│ 200 miles        │
└──────────────────┘
```

**Smart Defaults:**
```javascript
function getDefaultRadius(location) {
  const density = getPopulationDensity(location);
  
  if (density > 5000) return 10;   // Dense urban
  if (density > 1000) return 25;   // Urban/suburban
  if (density > 100) return 50;    // Suburban/rural
  if (density > 10) return 100;    // Rural
  return 200;                      // Remote/wilderness
}
```

---

#### 4. Filtering & Display Options

```
┌──────────────────────────────────┐
│ Filter by habitat:               │ ← Optional filter
│ ○ All  ○ Beach  ○ Forest         │   Chip-style toggles
│ ○ Grassland  ○ Urban  ○ Wetland  │   Not pre-selected
│                                  │
│ 47 birds • February 2026         │ ← Results header
│ [Grid ●] [List ○]                │   View toggle
│ Sort: [Most common ▾]            │   Sort dropdown
└──────────────────────────────────┘

CSS:
.filter-chips {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  padding: 1rem 1.5rem;
  background: rgba(45, 74, 62, 0.03);
}

.filter-chip {
  padding: 0.5rem 1rem;
  border: 2px solid #8B7355;
  border-radius: 20px;
  background: white;
  font-family: 'Crimson Pro', serif;
  font-size: 0.95rem;
  color: #2D4A3E;
  cursor: pointer;
  transition: all 0.2s;
}

.filter-chip.active {
  background: #2D4A3E;
  color: white;
  border-color: #2D4A3E;
}
```

**Habitat Options:**
- All (default - no filter)
- Beach / Coastal
- Forest / Woodland
- Grassland / Prairie
- Urban / Suburban
- Wetland / Marsh

**Sort Options:**
```
Sort by: [Most common ▾]
   ↓
┌────────────────────┐
│ Most common     ✓  │ ← Default (uses eBird frequency data)
│ Alphabetical       │
│ Size (largest)     │
│ Size (smallest)    │
└────────────────────┘
```

---

#### 5. Results Display

**Adaptive Default:**
```javascript
// Auto-select view based on result count
const defaultView = birdCount < 30 ? 'grid' : 'list';
```

**Grid View (< 30 birds or user preference):**
```
┌──────────────────────────────────┐
│ ┌─────────┐ ┌─────────┐ ┌───────┐│
│ │  [img]  │ │  [img]  │ │ [img] ││ ← 3 columns on mobile
│ │         │ │         │ │       ││   2 columns on narrow
│ │ American│ │  Blue   │ │Western││   4-5 on tablet/desktop
│ │  Robin  │ │   Jay   │ │ Gull  ││
│ └─────────┘ └─────────┘ └───────┘│
│                                  │
│ ┌─────────┐ ┌─────────┐ ┌───────┐│
│ │  [img]  │ │  [img]  │ │ [img] ││
│ │ Common  │ │ Song    │ │Downy  ││
│ │Yellowth.│ │Sparrow  │ │Woodp. ││
│ └─────────┘ └─────────┘ └───────┘│
│                                  │
└──────────────────────────────────┘

Interaction:
- Tap card → Species page (direct navigation)
- Gentle lift on tap (4px elevation)
- Image lazy-loaded as scrolled into view
```

**List View (30+ birds or user preference):**
```
┌──────────────────────────────────┐
│ ┌────┐                           │
│ │img │ American Robin            │ ← Compact row (collapsed)
│ └────┘ Common year-round         │   Tap to expand
│ ──────────────────────────────    │
│                                  │
│ ┌────┐                           │
│ │img │ Blue Jay                  │
│ └────┘ Common resident           │
│ ──────────────────────────────    │
│                                  │
│ ┌──────────────────────────────┐ │
│ │ [Larger illustration]        │ │ ← Expanded state
│ │                              │ │   Shows preview
│ │ Western Gull                 │ │
│ │                              │ │
│ │ Large gray-backed gull       │ │ ← 1-2 sentence teaser
│ │ common along Pacific coast.  │ │
│ │                              │ │
│ │ • Abundant coastal resident  │ │ ← Quick facts (2-3)
│ │ • Often seen at beaches      │ │
│ │ • Size: 22-26 inches         │ │
│ │                              │ │
│ │ [See full page →]            │ │ ← CTA to species page
│ └──────────────────────────────┘ │
│                                  │
│ ┌────┐                           │
│ │img │ Sanderling                │
│ └────┘ Winter visitor            │
│ ──────────────────────────────    │
└──────────────────────────────────┘

Interaction:
- Tap collapsed row → Expands inline
- Tap "See full page" → Species page
- Tap elsewhere → Collapse current, expand new
- Only one expanded at a time
```

**Pagination:**
```
Load first 30 birds initially

Then either:

Option A (Explicit):
┌──────────────────┐
│  [Load more →]   │ ← Button to load next 30
└──────────────────┘

Option B (Infinite scroll):
- Automatically load next 30 as user approaches bottom
- Show subtle loading indicator
- Continue until all results shown
```

**My recommendation:** Option A (explicit button) for first version. Less ambiguous, gives user control.

---

### State Management

**URL Parameters:**
Preserve state for back navigation and shareability:

```
/explore?location=santa-barbara-ca&lat=34.4208&lng=-119.6982&month=2&radius=50&habitat=beach&view=list&sort=common

Parameters:
- location: URL-safe location name
- lat, lng: Coordinates (for precise location)
- month: 1-12
- radius: 10, 25, 50, 100, 200
- habitat: all, beach, forest, grassland, urban, wetland
- view: grid, list
- sort: common, alpha, size-large, size-small
```

**State Restoration:**
```javascript
// On page load
const params = new URLSearchParams(window.location.search);
const state = {
  location: params.get('location') || await detectLocation(),
  month: parseInt(params.get('month')) || getCurrentMonth(),
  radius: parseInt(params.get('radius')) || getDefaultRadius(),
  habitat: params.get('habitat') || 'all',
  view: params.get('view') || getAdaptiveView(),
  sort: params.get('sort') || 'common'
};

// Update URL when state changes (without page reload)
function updateURL(newState) {
  const params = new URLSearchParams(newState);
  window.history.replaceState({}, '', `/explore?${params}`);
}
```

**Benefits:**
- ✅ Back button works (returns to exact state)
- ✅ URLs are shareable ("Check out birds near me!")
- ✅ Refresh preserves selections
- ✅ Deep linking supported

---

### Error States

**No Results:**
```
┌──────────────────────────────────┐
│ No birds found                   │
│                                  │
│ This combination of filters      │
│ didn't match any birds.          │
│                                  │
│ Try:                             │
│ • Expanding your radius          │
│ • Adjusting the month            │
│ • Removing habitat filter        │
│                                  │
│ [Show all birds in California →] │
└──────────────────────────────────┘
```

**Location Not Found:**
```
┌──────────────────────────────────┐
│ Location not recognized          │
│                                  │
│ Please try:                      │
│ • A nearby city name             │
│ • State or region                │
│ • ZIP code                       │
│                                  │
│ Or tap map to select location    │
└──────────────────────────────────┘
```

**Network Error:**
```
┌──────────────────────────────────┐
│ Unable to load bird data         │
│                                  │
│ Please check your connection     │
│ and try again.                   │
│                                  │
│ [Retry]                          │
└──────────────────────────────────┘
```

---

### Performance Optimizations

**Initial Load:**
- Detect location in background while showing UI skeleton
- Load first 30 birds immediately
- Lazy load images as they scroll into view
- Defer map initialization until visible

**Filtering/Sorting:**
```javascript
// Debounce filter changes to avoid excessive re-renders
const debouncedFilter = useDe bounce((filters) => {
  updateResults(filters);
}, 300);

// Optimistic UI - show loading state, update immediately
function handleFilterChange(newFilter) {
  setLoading(true);
  debouncedFilter(newFilter);
}
```

**Virtual Scrolling:**
If list exceeds 200 birds, implement virtual scrolling:
- Only render visible items + buffer
- Dramatically improves scroll performance
- Use `react-window` or `react-virtualized`

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
