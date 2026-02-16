# Migration Map - Technical Specification
**Animated Flight Paths for Bird Migration**

Version 1.0 - February 2026

---

## Table of Contents
1. [Concept Overview](#concept-overview)
2. [Core Algorithm](#core-algorithm)
3. [Data Structures](#data-structures)
4. [Visual Design](#visual-design)
5. [Interaction Design](#interaction-design)
6. [AI-Powered Path Inference](#ai-powered-path-inference)
7. [Performance Optimization](#performance-optimization)
8. [Implementation Phases](#implementation-phases)

---

## Concept Overview

### The Vision
> "You're at the beach in May. You open Aviary. The map shows glowing migration paths of birds passing through this month. Sanderlings refueling at Delaware Bay. Warblers streaming north. Arctic Terns on their pole-to-pole circuit."

### What Makes This Unique
**Existing tools show:** Where birds are (static heat maps, range maps)
**Aviary shows:** Where birds are GOING (animated migration flows, temporal movement)

No one visualizes migration as a living, flowing system. This is the defining feature of Aviary.

### Data Honesty
The map shows what *typically* happens this month based on seasonal data from eBird Status & Trends (weekly abundance models). It is NOT real-time tracking. Frame as "Migration This Month" or "What's Flying Now" — never "Live." Birders are knowledgeable; misleading framing damages credibility instantly.

**Future real-time layer:** BirdCast (Cornell Lab) publishes nightly migration forecasts using weather radar. Integrating BirdCast data (Phase 4+) would add a genuine real-time element: "Tonight, an estimated 200 million birds are in flight across the US." This makes the "live" framing honest rather than aspirational.

### Design Principles
1. **AI does the heavy lifting** - Auto-groups, infers paths, filters to relevance
2. **Clean by default** - No toggles needed, one beautiful default view
3. **Temporal storytelling** - Time slider brings migration to life
4. **Graceful degradation** - Works globally, richer where data is better
5. **Mobile-first beauty** - Smooth 60fps animations, touch-optimized
6. **Honest about confidence** - Don't show confident-looking paths for poorly-studied species. Visible confidence markers. Honest gaps > fabricated precision.

---

## Core Algorithm

### High-Level Flow

```
User opens Migration Map
  ↓
System detects location (or shows global default)
  ↓
For current month + location:
  1. Find all migrant species present/passing
  2. Group by migration pattern (family + flyway + timing)
  3. Infer path from breeding/winter ranges + flyways
  4. Filter to ACTIVE migrations only (birds moving NOW)
  5. Limit to 5-8 groups (prevent clutter)
  6. Generate contextual narrative
  ↓
Render:
  - Map with animated paths
  - Time slider
  - Auto-generated status text
  - 1-3 pulsing hotspots (if relevant)
```

### Grouping Strategy

**Goal:** Show 350+ migrant species without visual chaos

**Method:** Intelligent clustering by:
1. **Taxonomic family** (Shorebirds, Warblers, Raptors, etc.)
2. **Flyway** (Atlantic, Pacific, Central, Mississippi)
3. **Migration timing** (spring vs fall, early vs late)

**Output:** 5-8 distinct "migration flows" each representing 5-50 species

**Example for Santa Barbara, May:**
```
Groups generated:
1. Shorebirds - Pacific Flyway (23 species)
   Exemplar: Sanderling
   Color: Blue
   Status: Northbound, active
   
2. Warblers - Pacific Flyway (18 species)
   Exemplar: Yellow Warbler
   Color: Gold
   Status: Northbound, peak now
   
3. Raptors - Pacific Flyway (5 species)
   Exemplar: Swainson's Hawk
   Color: Red
   Status: Northbound, early arrivals

... (5-8 total groups)
```

---

## Migration Pattern Types

Not all migration is north-south. The map must handle diverse patterns that each require different rendering logic.

### Pattern Taxonomy

```javascript
const MIGRATION_PATTERNS = {
  "latitudinal": {
    // Classic N-S migration. Most songbirds, shorebirds, raptors.
    // Single path geometry, reverse direction in fall.
    rendering: "animated-path",
    needsSeparateRoutes: false,
    example: "Sanderling (Arctic → South America, same corridor both ways)"
  },

  "loop": {
    // Different routes spring vs fall. Path geometry differs by season.
    // American Golden-Plover: east over Atlantic in fall, Great Plains in spring.
    rendering: "dual-animated-path",
    needsSeparateRoutes: true,  // springRoute and fallRoute are distinct geometries
    example: "American Golden-Plover, Connecticut Warbler, Swainson's Thrush"
  },

  "altitudinal": {
    // Up/down mountains, not across continents. Short distance, big habitat change.
    // Render as a localized path with altitude indicator.
    rendering: "localized-path-with-altitude",
    needsSeparateRoutes: false,
    example: "Mountain Quail (walks downhill in winter), Clark's Nutcracker"
  },

  "longitudinal": {
    // East-west movement, not north-south. Relatively rare.
    rendering: "animated-path",
    needsSeparateRoutes: false,
    example: "Evening Grosbeak, some European species"
  },

  "austral": {
    // Southern Hemisphere: birds fly SOUTH to breed, NORTH to winter.
    // Same rendering as latitudinal but narrative and season labels flip.
    rendering: "animated-path",
    needsSeparateRoutes: false,
    narrativeFlipped: true,  // "Spring" is September in Southern Hemisphere
    example: "Fork-tailed Flycatcher, Chocolate-vented Tyrant"
  },

  "irruptive": {
    // Unpredictable, driven by food supply. Some years flood south; others stay put.
    // NOT a fixed route — render as expanding/contracting range boundary.
    rendering: "range-pulse",
    needsSeparateRoutes: false,
    isUnpredictable: true,
    example: "Snowy Owl, crossbills, Pine Siskin, Bohemian Waxwing"
  },

  "nomadic": {
    // Wander based on rainfall/food. No fixed routes at all.
    // Many Australian and African species. Render as a shimmering, shifting range.
    rendering: "range-drift",
    needsSeparateRoutes: false,
    isUnpredictable: true,
    example: "Banded Stilt (Australia), many African seedeaters"
  },

  "pelagic": {
    // Seabirds that spend months/years at sea. Ranges are vast oceanic zones.
    // Render with ocean-based paths, wave-like visual treatment.
    rendering: "ocean-path",
    needsSeparateRoutes: false,
    isOceanic: true,
    example: "Sooty Shearwater (circumnavigates Pacific), albatrosses"
  },

  "sedentary": {
    // Non-migratory. Year-round range only.
    // No path to render — show static range with lifecycle notes.
    rendering: "static-range",
    needsSeparateRoutes: false,
    example: "Northern Cardinal, most tropical species"
  }
};
```

### Partial Migration

Many species are partial migrants — some populations migrate while others are year-round residents. American Robin (the first showcase species!) is a textbook example.

```javascript
const MIGRATION_VARIABILITY = {
  "obligate": {
    // All populations migrate. Safe to show path for entire range.
    rangeOverlap: false,
    narrative: "All populations migrate between breeding and winter ranges."
  },
  "partial": {
    // Some populations migrate, some stay year-round.
    // Show BOTH the resident range AND the migratory path.
    rangeOverlap: true,
    narrative: "Northern populations migrate south in fall, while birds in
               the mid-Atlantic and Southeast may stay year-round."
  },
  "facultative": {
    // Migration depends on conditions (weather, food supply).
    // Show range with note about variability.
    rangeOverlap: true,
    isConditional: true,
    narrative: "Migration varies by year — in mild winters, many stay put."
  }
};
```

**Visual treatment for partial migrants:**
- Resident portion of range: solid fill, always visible
- Migratory portion: animated path (breeding ↔ winter), overlaid on resident range
- Prevents a birder in Georgia from seeing a Robin migration path and thinking "that's wrong, Robins are here all year"

### Irruptive Species Visualization

Instead of animated paths (which imply predictable routes), irruptive species get a distinctive visualization:

```
Normal year (e.g., Snowy Owl):
┌──────────────────────────┐
│  [Arctic range - solid]  │  ← Small, concentrated range
│                          │     No paths, no animation
│  "Snowy Owls typically   │
│   stay in the Arctic"    │
└──────────────────────────┘

Irruption year:
┌──────────────────────────┐
│  [Range expands south    │  ← Range boundary pulses outward
│   with pulsing effect]   │     Like a ripple in water
│                          │     Expanding/contracting animation
│  "2026 is an irruption   │
│   year — Snowy Owls are  │
│   appearing as far south │
│   as Virginia"           │
└──────────────────────────┘
```

**Data source for irruptions:** eBird frequency data (updated regularly) can detect unusual range expansions. Alternatively, a manual editorial flag for confirmed irruption years. This turns a data gap into a feature birders would love.

### Pelagic / Ocean Species

Seabirds need different treatment than land-based migrants:

- **Base map must handle oceans as more than blank space** — bathymetric shading, current patterns
- **Paths use wave-like rendering** rather than straight land-following lines
- **Ranges are vast oceanic zones** — not polygons but probability gradients
- **Stopovers are breeding colonies** (islands, cliffs) rather than inland hotspots

```javascript
const PELAGIC_CONFIG = {
  pathStyle: "wave",          // Undulating path instead of straight
  strokeDasharray: "15 8",    // Longer dashes for ocean feel
  particleShape: "triangle",  // Arrow/wedge shape (bird-like)
  rangeRendering: "gradient", // Probability gradient, not hard boundary
  baseMapLayer: "ocean",      // Show ocean depth/currents
  stopovers: "breeding-colonies"
};
```

Affects ~300-400 species globally. Worth deciding on before building the map renderer.

---

## Data Structures

### Migration Group Schema

```javascript
{
  "groupId": "shorebirds-atlantic-spring",
  "family": "Shorebirds",
  "familyScientific": "Scolopacidae",
  "flyway": "Atlantic",
  "color": "#4A90E2",  // Blue
  "speciesCount": 23,
  "exemplarSpecies": {
    "code": "sander",
    "commonName": "Sanderling",
    "abundanceRank": 1  // Most common in group
  },
  "allSpecies": [
    "Sanderling",
    "Dunlin", 
    "Red Knot",
    "Ruddy Turnstone",
    // ... up to 23
  ],
  "migrationPattern": "latitudinal",
  // See Migration Pattern Types section. Drives rendering strategy.

  "migrationVariability": "obligate",
  // "obligate" | "partial" | "facultative"
  // Partial: show resident range AND migratory path simultaneously

  "route": {
    "type": "inferred",  // or "verified" for well-studied species
    "confidence": "high",  // high, medium, low
    "springRoute": null,  // For loop migrants: distinct spring path geometry
    "fallRoute": null,    // For loop migrants: distinct fall path geometry
    // For non-loop migrants, springRoute/fallRoute are null; use points + direction
    "points": [
      {
        "lat": -35.5,
        "lng": -57.5,
        "name": "South American Coast",
        "role": "winter",
        "months": [12, 1, 2, 3]
      },
      {
        "lat": 25.8,
        "lng": -80.2,
        "name": "Florida Coast",
        "role": "stopover",
        "months": [4],
        "isHotspot": false
      },
      {
        "lat": 39.2,
        "lng": -75.1,
        "name": "Delaware Bay",
        "role": "critical-stopover",
        "months": [5, 6],
        "isHotspot": true,
        "peakAbundance": 100000,
        "significance": "Horseshoe crab eggs - birds double body weight"
      },
      {
        "lat": 75.0,
        "lng": -95.0,
        "name": "High Arctic Tundra",
        "role": "breeding",
        "months": [6, 7, 8]
      }
    ]
  },
  "timing": {
    "northbound": {
      "start": { month: 3, location: "South America" },
      "peak": { month: 5, location: "Mid-Atlantic" },
      "end": { month: 6, location: "Arctic" }
    },
    "southbound": {
      "start": { month: 7, location: "Arctic" },
      "peak": { month: 9, location: "Atlantic Coast" },
      "end": { month: 11, location: "South America" }
    }
  },
  "narrative": {
    // Auto-generated by Claude for each month
    "5": "Peak spring migration. These shorebirds are refueling at Delaware Bay, gorging on horseshoe crab eggs to double their body weight before the final 2,000km push to Arctic breeding grounds.",
    "7": "Breeding season. Birds are nesting on Arctic tundra. Adults will begin return migration in late July.",
    "1": "Winter grounds. Concentrated along South American and southern US coasts."
  }
}
```

### Hotspot Data Schema

```javascript
{
  "hotspotId": "delaware-bay",
  "name": "Delaware Bay",
  "location": { "lat": 39.2, "lng": -75.1 },
  "type": "critical-stopover",
  "families": ["Shorebirds"],  // Which groups use this
  "peakMonths": [5, 6],
  "peakAbundance": 100000,
  "significance": "One of the most important shorebird stopovers in the Western Hemisphere. Horseshoe crab eggs provide essential fuel.",
  "species": [
    { name: "Red Knot", abundance: 30000 },
    { name: "Sanderling", abundance: 25000 },
    { name: "Ruddy Turnstone", abundance: 20000 },
    { name: "Semipalmated Sandpiper", abundance: 15000 }
  ],
  "visualization": {
    "pulseAnimation": true,
    "glowIntensity": "high",
    "labelPosition": "above"
  }
}
```

### Path Rendering Data

```javascript
{
  "pathId": "shorebirds-atlantic-spring",
  "svgPath": "M -35.5 -57.5 Q 10 -70, 25.8 -80.2 T 39.2 -75.1 Q 60 -80, 75 -95",
  // Smooth bezier curve through points
  
  "visualization": {
    "strokeColor": "#4A90E2",
    "strokeWidth": {
      "base": 3,
      "scaled": true  // Thickness varies by abundance
    },
    "opacity": {
      "1": 0.2,   // January - dim (wintering)
      "3": 0.5,   // March - starting
      "5": 1.0,   // May - peak (ACTIVE)
      "7": 0.3,   // July - breeding
      "9": 0.8,   // September - southbound
      "12": 0.2   // December - wintering
    },
    "animation": {
      "type": "flow-particles",
      "direction": {
        "3-6": "northward",   // Spring
        "7-11": "southward"   // Fall
      },
      "particleCount": 12,
      "speed": {
        "1-2": "stopped",
        "3-6": "fast",
        "7": "stopped",
        "8-11": "fast",
        "12": "stopped"
      }
    }
  }
}
```

---

## Visual Design

### Color Coding (Family-Based)

**Global consistency - same colors worldwide:**

```javascript
const FAMILY_COLORS = {
  "Shorebirds": "#4A90E2",      // Blue (water association)
  "Warblers": "#F5D547",        // Gold/Yellow (bright songbirds)
  "Raptors": "#E84A3D",         // Red (predators)
  "Waterfowl": "#45B7A0",       // Teal (ducks, geese)
  "Terns": "#7EC8E3",           // Light blue (seabirds)
  "Swallows": "#9B6FB0",        // Purple (aerial)
  "Thrushes": "#8B6F47",        // Brown (robins, etc.)
  "Sparrows": "#95A5A6"         // Gray (LBJs)
};
```

### Map Layers (Bottom to Top)

```
1. Base Map (Mapbox GL JS with custom vintage style)
   - Muted earth tones (cream land, light blue water) via Mapbox Studio
   - Simplified (no labels except major cities and water bodies)
   - Focus on coastlines, major water bodies
   - Ocean areas are NOT blank: subtle bathymetric shading for pelagic species
   - Styled to match Aviary palette (cream #FAF7F0, muted greens, sepia borders)

2. Heat Map Layer (All Birds - Subtle Background)
   - Opacity: 0.3
   - Color: Single green gradient
   - Shows: All species present (residents + migrants)
   - Purpose: Context, not focus

3. Migration Paths Layer (Foreground - THE FEATURE)
   - Opacity: 0.6-1.0 (varies by activity)
   - Colors: Family colors (8 distinct)
   - Animation: Flowing particles
   - Purpose: Primary visual

4. Hotspot Markers Layer
   - 1-3 pulsing circles
   - Only active hotspots shown
   - Labels on tap

5. User Location Marker
   - Distinct pin/circle
   - Always visible
   - "You are here" label
```

### Mobile Layout (Revised)

```
┌──────────────────────────────────┐
│ Migration Map               [≡]  │ ← Minimal header (40px)
├──────────────────────────────────┤
│                                  │
│                                  │
│        [INTERACTIVE MAP]         │ ← 75% viewport height
│                                  │   Full bleed, edge-to-edge
│    ◉ You are here                │
│                                  │   Layers:
│    [5-8 animated paths]          │   - Base map
│    [1-3 pulsing hotspots]        │   - Heat (subtle)
│                                  │   - Paths (bold)
│                                  │   - Hotspots (pulsing)
│    Santa Barbara, CA             │   - Your location
│                                  │
│    ▼ Peak Spring Migration       │ ← Status overlay (bottom)
│    47 species passing through    │   Semi-transparent bg
│                                  │   Auto-generated text
│    Jan ━━━━━●━━━━━━━━━━━━━ Dec  │ ← Time slider (overlay)
│                                  │   Integrated into map
│    [▶ Play]         [Legend ≡]  │ ← Controls (corners)
│                                  │
└──────────────────────────────────┘
              ↓ Scroll

┌──────────────────────────────────┐
│ What's Flying Through            │ ← Section header
│                                  │   Clean, simple
│ ──── Shorebirds (23 species)     │ ← Color-coded headers
│                                  │   Match map paths
│ Sanderling • Dunlin • Red Knot   │ ← Top 3 species
│                                  │
│ Arctic Coast → Delaware Bay      │ ← Route summary
│ → High Arctic                    │   Simple text
│                                  │
│ Peak: Now through June           │ ← Timing
│                                  │
│ [Explore shorebirds →]           │ ← CTA
│                                  │
│ ──── Warblers (18 species)       │
│ ... (repeat structure)           │
│                                  │
│ [5-8 total groups listed]        │
│                                  │
└──────────────────────────────────┘
              ↓

┌──────────────────────────────────┐
│ All Birds Here Now               │ ← Traditional section
│                                  │   Familiar territory
│ 127 species total in May         │
│ 47 migrating • 80 year-round     │
│                                  │
│ [Grid ●] [List ○]                │ ← Standard controls
│ Sort: [Most common ▾]            │
│                                  │
│ [Bird cards/list as before...]   │ ← Existing design
└──────────────────────────────────┘
```

### Path Visual Treatment

**SVG Implementation:**

```html
<svg class="migration-paths-layer">
  <!-- Shorebird path -->
  <path
    d="M -35.5 -57.5 Q 10 -70, 25.8 -80.2 T 39.2 -75.1 Q 60 -80, 75 -95"
    stroke="#4A90E2"
    stroke-width="3"
    stroke-dasharray="10 5"
    fill="none"
    opacity="1.0"
    class="migration-path active"
  />
  
  <!-- Animated particles along path -->
  <g class="path-particles">
    <circle r="4" fill="#4A90E2" opacity="0.8">
      <animateMotion
        dur="6s"
        repeatCount="indefinite"
        path="M -35.5 -57.5 Q 10 -70, 25.8 -80.2 T 39.2 -75.1 Q 60 -80, 75 -95"
      />
    </circle>
    <!-- 8-12 more particles at different offsets -->
  </g>
</svg>
```

**CSS Animations:**

```css
.migration-path {
  transition: opacity 0.5s ease, stroke-width 0.3s ease;
  filter: drop-shadow(0 0 4px currentColor);
}

.migration-path.active {
  opacity: 1.0;
  animation: glow 2s ease-in-out infinite;
}

.migration-path.inactive {
  opacity: 0.3;
  animation: none;
}

@keyframes glow {
  0%, 100% { filter: drop-shadow(0 0 4px currentColor); }
  50% { filter: drop-shadow(0 0 12px currentColor); }
}

.hotspot-marker {
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0%, 100% { 
    transform: scale(1); 
    opacity: 1; 
  }
  50% { 
    transform: scale(1.4); 
    opacity: 0.5; 
  }
}
```

---

## Interaction Design

### Primary Interaction: Time Slider

**Behavior as user drags:**

```javascript
// Month-by-month state changes
const MONTH_STATES = {
  1: {  // January
    pathOpacity: 0.2,
    animation: "paused",
    narrative: "Migration quiet - birds at winter grounds",
    visualization: "clustering"  // Show concentrations, not movement
  },
  3: {  // March
    pathOpacity: 0.6,
    animation: "slow",
    narrative: "Spring migration beginning - early arrivals",
    visualization: "emerging-flows"
  },
  5: {  // May
    pathOpacity: 1.0,
    animation: "fast",
    narrative: "Peak spring migration - 47 species passing through",
    visualization: "maximum-activity",
    hotspots: ["Delaware Bay", "Platte River"]  // Pulse these
  },
  7: {  // July
    pathOpacity: 0.3,
    animation: "paused",
    narrative: "Breeding season - birds nesting in Arctic",
    visualization: "clustering"
  },
  9: {  // September
    pathOpacity: 0.8,
    animation: "fast-reverse",
    narrative: "Fall migration - return journey south",
    visualization: "southbound-flows"
  },
  12: {  // December
    pathOpacity: 0.2,
    animation: "paused",
    narrative: "Winter grounds - concentrated at southern sites",
    visualization: "clustering"
  }
};

// SOUTHERN HEMISPHERE: Season labels and narratives flip.
// When user is in the Southern Hemisphere (lat < 0), the map uses
// month names instead of season names to avoid confusion.
// "September migration" not "spring migration" for austral species.
function getSeasonLabel(month, userLat) {
  if (userLat >= 0) {
    // Northern Hemisphere: standard labels
    return NORTHERN_SEASON_LABELS[month];
  }
  // Southern Hemisphere: use month names, not season names
  // "September" not "Spring" — avoids "spring migration" in September confusion
  return MONTH_NAMES[month];
}

// Austral migrants (fly south to breed) have flipped path animations:
// - Southbound in September-November (to breeding grounds)
// - Northbound in March-May (to wintering grounds)
// The animation direction reverses based on migrationPattern: "austral"

// Smooth transitions between months
function updateMapForMonth(month) {
  const state = MONTH_STATES[month];
  
  // Update path visibility
  migrationPaths.forEach(path => {
    path.style.opacity = state.pathOpacity;
    path.dataset.animation = state.animation;
  });
  
  // Update narrative text
  narrativeElement.textContent = state.narrative;
  
  // Update hotspot visibility
  updateHotspots(month);
  
  // Trigger animation state change
  updateAnimations(state.animation);
}
```

### Secondary Interaction: Tap Path

```
User taps blue shorebird path
  ↓
Modal slides up from bottom (mobile) or center (desktop):

┌────────────────────────────────┐
│ ──── Shorebirds                │ ← Color bar matches path
│ Atlantic Flyway                │
│                                │
│ 23 species including:          │
│ • Sanderling                   │ ← Top 5 by abundance
│ • Dunlin                       │
│ • Red Knot                     │
│ • Ruddy Turnstone              │
│ • Semipalmated Sandpiper       │
│                                │
│ Journey                        │
│ From: South American coast     │
│ To: High Arctic tundra         │
│ Distance: 5,000-10,000 km      │
│                                │
│ Critical Stopovers             │
│ • Delaware Bay (May)           │ ← Listed, with timing
│ • James Bay (August)           │
│                                │
│ [See all 23 species →]         │ ← CTA to filtered list
│                      [Close]   │
└────────────────────────────────┘
```

### Tertiary Interaction: Tap Hotspot

```
User taps pulsing Delaware Bay marker
  ↓
Modal with hotspot details:

┌────────────────────────────────┐
│ Delaware Bay                   │
│ Critical Shorebird Stopover    │
│                                │
│ Right now (May 15):            │
│ ~100,000 shorebirds            │
│                                │
│ What's happening:              │
│ Birds are gorging on horseshoe │
│ crab eggs, doubling their body │
│ weight in 2 weeks for final    │
│ push to Arctic.                │
│                                │
│ Top species here:              │
│ • Red Knot (30,000)            │
│ • Sanderling (25,000)          │
│ • Ruddy Turnstone (20,000)     │
│                                │
│ [Learn more] [Close]           │
└────────────────────────────────┘
```

### Play Button

```
User taps [▶ Play]
  ↓
Auto-animates through year:
- Slider advances Jan → Dec
- Takes 30 seconds total (~2.5s per month)
- Pauses briefly at peak months (May, September)
- Paths animate, hotspots pulse on cue
- Narrative updates

Controls during playback:
[⏸ Pause] [⏮ Restart] [Speed: 2x ▾]

User can still drag slider to take control
```

### Legend Toggle

```
[Legend ≡]
  ↓ Tap
Small overlay in corner:

┌──────────────────┐
│ Migration Flows  │
│                  │
│ ──── Shorebirds  │
│ ──── Warblers    │
│ ──── Raptors     │
│ ──── Waterfowl   │
│ ... (active only)│
│                  │
│ ◉ Hotspot (peak) │
│ ○ Your location  │
│                  │
│     [Close ×]    │
└──────────────────┘

Tap any family name → highlights that path
Tap again → dehighlights
```

---

## AI-Powered Path Inference

### The Challenge

**We have:** Breeding ranges + Winter ranges + Migration timing  
**We need:** Smooth, realistic flight paths between them

**Can't hand-code:** 350+ migrant species × multiple flyways = thousands of paths

**Solution:** Claude AI infers paths from data + known migration patterns

### Inference Algorithm

**Inputs:**
1. Breeding range polygon (from eBird)
2. Winter range polygon (from eBird)
3. Species family (taxonomic)
4. Known flyway corridors (Atlantic, Pacific, Central, Mississippi)
5. Documented stopover sites (from literature)

**Process:**

```javascript
async function inferMigrationPath(species, family, flyway) {
  const prompt = `
    Given this bird species:
    - Name: ${species.commonName}
    - Family: ${family}
    - Breeding range centroid: ${species.breeding.centroid}
    - Winter range centroid: ${species.winter.centroid}
    - Known flyway: ${flyway}
    
    Known facts:
    - ${family} typically use ${flyway} flyway
    - Major stopover sites in this flyway: ${flyway.stopovers}
    - Species is documented at: ${species.documentedLocations}
    
    Infer a realistic migration path as a series of 4-6 waypoints:
    - Start at winter centroid
    - Follow known flyway corridor
    - Include documented stopovers if on route
    - End at breeding centroid
    - Return as lat/lng coordinates with month of passage
    
    Confidence level: high/medium/low based on data quality
  `;
  
  const response = await claude.generatePath(prompt);
  
  return {
    path: response.waypoints,
    confidence: response.confidence,
    reasoning: response.reasoning
  };
}
```

**Output Example:**

```json
{
  "path": [
    {"lat": -34.5, "lng": -58.4, "month": 3, "name": "Argentina Coast"},
    {"lat": 10.5, "lng": -61.4, "month": 4, "name": "Venezuela Coast"},
    {"lat": 25.8, "lng": -80.2, "month": 4, "name": "Florida"},
    {"lat": 39.2, "lng": -75.1, "month": 5, "name": "Delaware Bay"},
    {"lat": 54.5, "lng": -79.8, "month": 6, "name": "James Bay"},
    {"lat": 75.0, "lng": -95.0, "month": 6, "name": "High Arctic"}
  ],
  "confidence": "high",
  "reasoning": "Route follows documented Atlantic Flyway. Delaware Bay is well-documented critical stopover for this family."
}
```

### Confidence Tracking

**We track which paths are confident vs inferred:**

```javascript
{
  "pathId": "shorebirds-atlantic",
  "confidence": "high",
  "evidenceScore": 8.5,  // Out of 10
  "basedOn": [
    "eBird abundance data (52 weeks)",
    "BirdLife range maps",
    "Published migration studies (3 papers)",
    "Citizen science observations (10,000+ records)"
  ],
  "gaps": []  // What we're missing
}

vs

{
  "pathId": "rare-tropical-flycatcher",
  "confidence": "low",
  "evidenceScore": 3.2,
  "basedOn": [
    "eBird range maps (sparse data)",
    "Family-level inference"
  ],
  "gaps": [
    "No weekly abundance data",
    "No documented stopovers",
    "Limited observations (<50 records)"
  ]
}
```

**Display strategy (confidence must be VISIBLE, not hidden):**
- **High confidence (8+):** Solid animated path, bold stroke. Full experience.
- **Medium confidence (5-7):** Dashed path, slightly transparent (opacity 0.6), note "Route inferred from family patterns" in modal. Small "i" icon on path when tapped shows data source and confidence level.
- **Low confidence (<5):** Path NOT shown on map. Species page shows text-only range description: "Migration route not well documented." Honest gaps are better than confident-looking fabrications. Added to research backlog for improvement.

**Why visible confidence matters:** A smooth, confident-looking animated path for a species where the actual route is poorly understood implies precision that doesn't exist. A birder who knows the science will see this as misleading. Dashed lines and transparency communicate "we know roughly, not exactly" — which is honest and builds trust.

### Completeness Tracking

**Internal dashboard tracks coverage:**

```javascript
{
  "totalMigrantSpecies": 3847,
  "withHighConfidencePaths": 892,  // 23%
  "withMediumConfidencePaths": 1654,  // 43%
  "withLowConfidencePaths": 801,  // 21%
  "noPaths": 500,  // 13%
  
  "coverageByRegion": {
    "North America": {
      "total": 350,
      "high": 245,  // 70%
      "medium": 89,
      "low": 16
    },
    "Europe": {
      "total": 280,
      "high": 168,  // 60%
      "medium": 87,
      "low": 25
    },
    "Africa": {
      "total": 420,
      "high": 105,  // 25% - trans-Saharan less studied
      "medium": 189,
      "low": 126
    }
    // ... other regions
  },
  
  "improvementQueue": [
    {
      "species": "Bar-tailed Godwit",
      "currentConfidence": "medium",
      "priority": "high",
      "reason": "Epic migration (11,000km non-stop), worthy of showcase",
      "needsResearch": ["Exact Pacific route", "Stopover sites"]
    }
    // ... prioritized list
  ]
}
```

**This lets us:**
- Show users what we have vs don't have
- Prioritize research/enhancement efforts
- Gradually improve coverage
- Be transparent about data quality

---

## Performance Optimization

### Challenge

Animated map with 5-8 paths, 10-30 particles, 1-3 pulsing hotspots, time slider updates — all at 60fps on mobile.

### Optimization Strategies

#### 1. Use SVG for Paths, CSS for Animations

```javascript
// Good: Hardware-accelerated CSS
.migration-path {
  transition: opacity 0.5s ease;
  will-change: opacity;  // GPU optimization hint
}

// Avoid: JavaScript animation loop
// setInterval(() => { path.style.opacity = ... }, 16);  // BAD
```

#### 2. Limit Active Elements

```javascript
const MAX_PATHS = 8;
const MAX_PARTICLES_PER_PATH = 4;
const MAX_HOTSPOTS = 3;

// Total animated elements: ~35 max
// Manageable for 60fps
```

#### 3. Lazy Rendering

```javascript
// Only render paths that are visible (opacity > 0.1)
function updateVisiblePaths(month) {
  const state = MONTH_STATES[month];
  
  allPaths.forEach(path => {
    const shouldRender = state.pathOpacity > 0.1;
    
    if (shouldRender && !path.rendered) {
      renderPath(path);  // Add to DOM
      path.rendered = true;
    } else if (!shouldRender && path.rendered) {
      removePath(path);  // Remove from DOM
      path.rendered = false;
    }
  });
}
```

#### 4. Debounce Slider

```javascript
// Don't update on every pixel drag
const debouncedUpdate = debounce((month) => {
  updateMapForMonth(month);
}, 100);  // Update max 10x per second

slider.addEventListener('input', (e) => {
  const month = parseInt(e.target.value);
  debouncedUpdate(month);
});
```

#### 5. Progressive Enhancement

```javascript
// Detect device performance
const performanceLevel = detectPerformance();

if (performanceLevel === 'high') {
  // Full experience: smooth animations, particles, glows
  enableAllAnimations();
  particlesPerPath = 4;
} else if (performanceLevel === 'medium') {
  // Reduced: simpler animations, fewer particles
  enableBasicAnimations();
  particlesPerPath = 2;
} else {
  // Low: static paths, no particles
  disableAnimations();
  particlesPerPath = 0;
}

function detectPerformance() {
  // Check hardware concurrency, memory, user agent
  const cores = navigator.hardwareConcurrency || 2;
  const isLowEnd = /Android.*Chrome\/[1-5]/i.test(navigator.userAgent);
  
  if (cores >= 4 && !isLowEnd) return 'high';
  if (cores >= 2) return 'medium';
  return 'low';
}
```

#### 6. Virtualize Off-Screen Elements

```javascript
// Don't render paths/hotspots outside viewport
function isInViewport(lat, lng) {
  const bounds = map.getBounds();
  return bounds.contains([lat, lng]);
}

// Only render visible elements
visiblePaths = allPaths.filter(p => 
  p.points.some(point => isInViewport(point.lat, point.lng))
);
```

#### 7. Use Web Workers for Heavy Computation

```javascript
// Offload path calculations to worker thread
const worker = new Worker('migration-calculator.js');

worker.postMessage({
  type: 'generatePaths',
  location: userLocation,
  month: currentMonth,
  species: allSpecies
});

worker.onmessage = (e) => {
  const { paths, hotspots, narrative } = e.data;
  renderPaths(paths);
  renderHotspots(hotspots);
  updateNarrative(narrative);
};
```

### Performance Targets

```
Desktop:
- 60fps animations (16.6ms per frame)
- Time to interactive: <2s
- Smooth slider dragging (no jank)

Mobile (iPhone 12+):
- 60fps animations
- Time to interactive: <3s
- Smooth touch interactions

Mobile (Older devices):
- 30fps acceptable (33ms per frame)
- Reduced animations
- Time to interactive: <5s
```

---

## Implementation Phases

### Phase 1: Foundation (Weeks 1-3)

**Goal:** Basic map with static paths for North America, exercising diverse migration patterns

**Tasks:**
1. Set up Mapbox GL JS with custom vintage-styled base map (Mapbox Studio)
2. Implement time slider component
3. Create 8 family color scheme
4. Build path data structure (including loop, altitudinal, pelagic variants)
5. Manually define 5-10 showcase paths representing diverse patterns:
   - Sanderling (Atlantic Flyway, classic latitudinal)
   - Arctic Tern (Global champion, pelagic)
   - Bar-tailed Godwit (Pacific record-holder)
   - Ruby-throated Hummingbird (Gulf crossing)
   - Swainson's Hawk (Americas champion)
   - American Golden-Plover (loop migration — different spring/fall routes)
6. Render SVG paths on map with confidence-based visual treatment (solid vs dashed)
7. Basic opacity changes with slider (no animation yet)
8. Southern Hemisphere month-name labels (no "spring/fall" for austral)

**Deliverable:** Map shows paths including loop and pelagic, slider changes visibility

---

### Phase 2: Animation & Interactivity (Weeks 4-5)

**Goal:** Bring paths to life

**Tasks:**
1. Implement CSS animations (glow, fade)
2. Add flowing particles along paths
3. Month-based animation states (active/inactive)
4. Tap path → modal with details
5. Auto-generated narrative for each month
6. Play button (auto-advance through year)
7. Legend overlay

**Deliverable:** Animated, interactive migration map (North America, 10 paths)

---

### Phase 3: AI Path Generation (Weeks 6-8)

**Goal:** Scale from 10 paths to 100+, with diverse migration types

**Tasks:**
1. Build Claude prompt for path inference (separate prompts for latitudinal, loop, altitudinal, pelagic)
2. Integrate eBird Status & Trends range data (Tier 1 species — raster to GeoJSON pipeline)
3. Define known flyway corridors (Atlantic, Pacific, Central, Mississippi, trans-Saharan, East Asian-Australasian)
4. Generate paths for all North American migrants (~350 species)
5. Review confidence scores — only show high/medium on map
6. Implement confidence-based visual treatment (solid vs dashed)
7. Implement grouping algorithm (reduce 100 → 8 groups per location)
8. Add irruptive species visualization (range-pulse for Snowy Owl, crossbills, etc.)
9. Add partial migration handling (resident range overlay + migratory path)
10. Test across different locations/months/hemispheres

**Deliverable:** Migration map works anywhere in North America, handles diverse patterns, honest about confidence

---

### Phase 4: Hotspots & Narratives (Weeks 9-10)

**Goal:** Add spectacle moments

**Tasks:**
1. Define 20-30 critical hotspots (Delaware Bay, Platte River, etc.)
2. Implement pulsing marker animation
3. Tap hotspot → detailed modal
4. Claude generates hotspot narratives
5. Logic to show 1-3 active hotspots per month
6. Integrate hotspots into path modals

**Deliverable:** Map highlights migration spectacles, tells richer stories

---

### Phase 5: Global Expansion (Weeks 11-12)

**Goal:** Extend beyond North America, handle hemispheric differences

**Tasks:**
1. Generate paths for European migrants (trans-Saharan routes)
2. Generate paths for East Asian-Australasian flyway
3. Generate paths for Neotropical and austral migrants (Southern Hemisphere breeding patterns)
4. Add pelagic species paths (ocean-based rendering for shearwaters, albatrosses)
5. Test degradation in data-sparse regions (tropics, Southern Hemisphere)
6. Confidence thresholds by region (higher bar for data-sparse regions)
7. Southern Hemisphere season/narrative handling (month names, reversed breeding seasons)

**Deliverable:** Global migration map, handles all regions and hemispheres honestly

---

### Phase 6: Polish & Optimization (Weeks 13-14)

**Goal:** Production-ready performance

**Tasks:**
1. Performance profiling on mobile devices
2. Implement progressive enhancement
3. Optimize animations for 60fps
4. Lazy loading, virtualization
5. Error states (no active migrations, location not found)
6. Accessibility (keyboard navigation, screen reader support)
7. Loading states, skeleton screens

**Deliverable:** Fast, smooth, production-ready

---

### Phase 7: Integration (Week 15)

**Goal:** Connect to rest of Aviary

**Tasks:**
1. Replace original Explore page with Migration Map
2. Link paths to species pages
3. "See all shorebirds" → filtered species list
4. Add to main navigation
5. Update About page to mention this feature
6. Analytics tracking (which paths get clicked, popular months)

**Deliverable:** Migration Map is live as main Explore experience

---

## Success Metrics

**Engagement:**
- Time on Migration Map page
- Slider interactions per session
- Path/hotspot tap rate

**Educational Value:**
- "Aha moments" - users discovering migration they didn't know about
- Shares on social media
- Inbound links from birding communities

**Technical:**
- 60fps animation on target devices
- <3s time to interactive
- <5% error rate (failed path rendering, etc.)

**Coverage:**
- High-confidence paths: 30% of migrants (Year 1)
- Medium-confidence: 50% (Year 1)
- Continual improvement toward 80%+ high-confidence

---

## Open Questions & Future Enhancements

### Data Quality Improvements
- Partner with migration researchers for verified routes
- Incorporate GPS tracking data where available
- Crowdsource path corrections from expert birders

### Additional Features (Future)
- **BirdCast integration (high priority):** Cornell's BirdCast publishes nightly nocturnal migration forecasts from weather radar data. Overlay: "Tonight, an estimated 200 million birds are in flight across the US." This makes the temporal framing genuinely real. Most songbirds migrate at night — a fact most people don't know. A day/night toggle showing "right now, invisible above you, millions of birds are flying" is the kind of invisible-made-visible moment that aligns perfectly with Aviary's spirit.
- **"Follow a bird" scrollytelling:** Pick one species, see its full year cycle as a scroll-driven narrative. Map pans and zooms alongside the story. "It's March. Our Sanderling — let's call her by her band number, B95 — is fattening up on a beach in Tierra del Fuego..." This is the content that gets shared and bookmarked.
- Historical migration (compare 2020 vs 2025)
- Climate change impacts (routes shifting over time)
- Rare vagrant alerts (unusual migration patterns)
- Educational mode for teachers (lesson plans, quizzes)
- **Conservation spotlight on map:** Paths for declining species with subtle visual indicator. Monthly feature: "12 species passing through your area this month are declining."

### Accessibility
- Audio descriptions of migration flows
- High-contrast mode for visibility
- Simplified static version for screen readers

---

*Last updated: February 14, 2026*
