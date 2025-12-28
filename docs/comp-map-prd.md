# COMP MAP OVERLAY
## Product Requirements Document + User Stories
### For Engineering Implementation

---

| Field | Value |
|-------|-------|
| **Feature ID** | C-OVERLAY-MAP |
| **Parent Bot** | C-OVERLAY (Comp Analysis Bot) |
| **Category** | Comps / Analysis / Map View |
| **Priority** | P0 - Critical Path |
| **Primary User** | Acquisition Associate (AA) |
| **UI Location** | PIQ → Comps Tab → Map View |
| **Handoff To** | Nate (CTO) |
| **Version** | v1.0 — December 28, 2024 |

---

# 1. FEATURE OVERVIEW

## 1.1 Purpose

The Comp Map Overlay provides a geographic visualization layer that helps Acquisition Associates understand spatial relationships between the subject property and comparable properties. When the IQ button is pressed, an intelligent overlay appears **below the map** providing ranked comp analysis with location-based insights.

## 1.2 Key Principle

> **This is a TRAINING OVERLAY** — Read-only explanations that guide users to manually update information. No direct data modifications.

## 1.3 Integration Point

```
+------------------------------------------+
|              MAP VIEW                     |
|   [Subject Pin] [Comp Pins by Rank]      |
|   [Tract Boundaries] [Distance Radius]   |
+------------------------------------------+
|            [ IQ Button ]                  |
+------------------------------------------+
              ↓ (on click)
+------------------------------------------+
|        COMP MAP OVERLAY PANEL            |
|   - Ranked comp list with explanations   |
|   - Geographic insights                  |
|   - KEEP/REMOVE recommendations          |
|   - "Go to [field] to update" prompts    |
+------------------------------------------+
```

---

# 2. USER STORIES

## 2.1 Primary User Story

> **US-MAP-001**: As an Acquisition Associate, I want to see my pre-filtered comps displayed on a map with visual ranking indicators so that I can quickly understand which comps are geographically most relevant to my subject property.

**Acceptance Criteria:**
- [ ] Subject property displays as distinct pin (different color/size)
- [ ] Comp pins display with visual rank indicators (color gradient or numbered)
- [ ] Clicking IQ button reveals overlay panel below map
- [ ] Overlay shows ranked list matching pin positions on map

---

## 2.2 Geographic Visualization Stories

### US-MAP-002: Distance Radius Display
> As an AA, I want to see distance radius circles around my subject property so that I can visually assess which comps fall within preferred distances.

**Acceptance Criteria:**
- [ ] Configurable radius rings displayed (0.25mi, 0.5mi, 1mi)
- [ ] Comps outside max radius visually differentiated
- [ ] Distance from subject shown on comp pin hover

---

### US-MAP-003: Tract/Subdivision Boundaries
> As an AA, I want to see tract and subdivision boundaries on the map so that I can identify which comps share the same micro-market as my subject.

**Acceptance Criteria:**
- [ ] Tract boundaries displayed as polygon overlays
- [ ] Subject's tract highlighted distinctly
- [ ] Comps within same tract visually grouped
- [ ] Tract name displayed on hover/click

**Data Sources:** MLS Subdivision → PropertyRadar legal.tract

---

### US-MAP-004: Micro-Market Barrier Visualization
> As an AA, I want to see major roads, highways, and natural barriers on the map so that I can identify pricing breaks caused by physical dividers.

**Acceptance Criteria:**
- [ ] Arterial roads highlighted when they divide subject from comps
- [ ] Visual indicator when comp is "across barrier" from subject
- [ ] Barrier impact noted in overlay explanation

**Data Sources:** Google Roads API → Distance Matrix

---

## 2.3 Pin Interaction Stories

### US-MAP-005: Comp Pin Selection
> As an AA, I want to click on any comp pin to see its detailed analysis so that I can understand why it was ranked at that position.

**Acceptance Criteria:**
- [ ] Click pin → highlights corresponding entry in overlay panel
- [ ] Pin popup shows: Address, Status, Condition, Rank Position
- [ ] "View Details" link scrolls to full explanation in overlay

---

### US-MAP-006: Subject Pin Context
> As an AA, I want to click on the subject property pin to see a summary of what the bot is comparing against so that I understand the baseline.

**Acceptance Criteria:**
- [ ] Subject pin shows: Address, Sqft, Bed/Bath, Condition Target, Key Features
- [ ] Lists which variables are being evaluated (from ABCD model)

---

### US-MAP-007: Visual Rank Encoding
> As an AA, I want comp pins to visually indicate their rank and status so that I can quickly scan the map for the best comps.

**Acceptance Criteria:**
- [ ] **Color coding by status:** Sold (Green) / Pending (Yellow) / Active (Blue)
- [ ] **Size or number by rank:** #1 largest/boldest, decreasing by rank
- [ ] **Opacity for REMOVE comps:** Reduced opacity for lower relevance/redundant
- [ ] Legend displayed on map

---

## 2.4 Overlay Panel Stories

### US-MAP-008: Ranked Comp List Display
> As an AA, I want to see a ranked list of comps in the overlay panel that corresponds to the map pins so that I can review them systematically.

**Acceptance Criteria:**
- [ ] List ordered by relevance rank (not price)
- [ ] Each entry shows: Rank #, Address, Status, Condition, Distance
- [ ] KEEP comps displayed prominently
- [ ] REMOVE comps displayed with reason (Lower Relevance / Redundant)
- [ ] Hover on list item → highlights corresponding map pin

---

### US-MAP-009: Geographic Explanation Display
> As an AA, I want to see location-based explanations for each comp's ranking so that I understand spatial factors affecting relevance.

**Acceptance Criteria:**
- [ ] Column C (Location) variables prominently displayed:
  - Tract/Subdivision match or mismatch
  - School District alignment
  - Street Position (interior vs perimeter)
  - Busy Street / Arterial exposure
  - Micro-market barriers crossed
- [ ] Only relevant variables shown (silence = correct)
- [ ] Max 3-5 bullets per comp

---

### US-MAP-010: Manual Update Guidance
> As an AA, I want the overlay to tell me where to go to manually update information so that I can correct any data the bot may have wrong.

**Acceptance Criteria:**
- [ ] Each explanation includes actionable guidance
- [ ] Format: "To update [variable], go to [location]"
- [ ] Links/buttons to navigate to editable fields
- [ ] No direct editing within overlay (read-only)

---

### US-MAP-011: Comp Override Controls
> As an AA, I want to manually override the bot's KEEP/REMOVE recommendations so that I retain final judgment.

**Acceptance Criteria:**
- [ ] Toggle button on each comp: KEEP ↔ REMOVE
- [ ] Override visually indicated (user icon or badge)
- [ ] Override persists until user changes it
- [ ] Bot reasoning still visible after override

---

## 2.5 Filter Expansion Stories

### US-MAP-012: Expansion Recommendation
> As an AA, I want the bot to recommend filter expansions when I have fewer than 6 comps so that I know how to find more relevant comps.

**Acceptance Criteria:**
- [ ] Warning displayed when comp count < 6
- [ ] Expansion recommendations in priority order:
  1. Distance (show suggested radius increase)
  2. Year Built (show suggested range)
  3. COE 180→365 days
  4. Sqft (show suggested range)
- [ ] "Apply Expansion" button for each recommendation
- [ ] Map updates to show potential new comps (preview)

---

### US-MAP-013: Redundancy Pruning Notification
> As an AA, I want to be notified when the bot has pruned redundant comps so that I understand why some comps were deprioritized.

**Acceptance Criteria:**
- [ ] Notification when original set > 12
- [ ] Shows pruning logic applied:
  1. Farther distance duplicates removed
  2. Older sale dates removed
  3. Weaker condition alignment removed
  4. Feature mismatches removed
- [ ] "Show All Comps" toggle to reveal pruned comps

---

## 2.6 Confidence & Summary Stories

### US-MAP-014: Comp Set Strength Indicator
> As an AA, I want to see an overall assessment of my comp set strength so that I know if I need to take action.

**Acceptance Criteria:**
- [ ] Badge displayed: **STRONG** (green) / **MODERATE** (yellow) / **WEAK** (red)
- [ ] Brief reason displayed (e.g., "Limited sold comps in tract")
- [ ] Recommended action: **Proceed** or **Expand Filters**

---

# 3. MAP-SPECIFIC TECHNICAL REQUIREMENTS

## 3.1 Map Component Stack

| Component | Technology | Notes |
|-----------|------------|-------|
| Base Map | Google Maps / Mapbox | Existing PIQ integration |
| Overlay Layer | React component | Renders below map container |
| Pin Rendering | Map markers API | Custom icons for rank/status |
| Boundaries | GeoJSON polygons | Tract data from PropertyRadar |
| Distance Calc | Google Distance Matrix | For barrier detection |

## 3.2 Data Flow

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│  MLS Comp Data  │────▶│  Comp Analysis   │────▶│   Map Overlay   │
│  (Pre-filtered) │     │  Bot (GPT-4)     │     │   UI Component  │
└─────────────────┘     └──────────────────┘     └─────────────────┘
        │                       │                        │
        │                       ▼                        │
        │               ┌──────────────────┐             │
        └──────────────▶│  ABCD Variables  │◀────────────┘
                        │  - Property (A)  │
                        │  - Lot (B)       │
                        │  - Location (C)  │
                        │  - Data Src (D)  │
                        └──────────────────┘
```

## 3.3 Map Pin Schema

```typescript
interface CompMapPin {
  id: string;
  address: string;
  coordinates: {
    lat: number;
    lng: number;
  };
  status: 'SOLD' | 'PENDING' | 'ACTIVE';
  condition: 'FLIP' | 'GOOD' | 'ORIGINAL' | 'FIXER';
  rank: number;
  recommendation: 'KEEP' | 'REMOVE';
  removeReason?: 'LOWER_RELEVANCE' | 'REDUNDANT';
  distanceFromSubject: number; // miles
  isInSubjectTract: boolean;
  explanationBullets: string[]; // max 5
  userOverride?: boolean;
}

interface SubjectPin {
  id: string;
  address: string;
  coordinates: {
    lat: number;
    lng: number;
  };
  sqft: number;
  bedBath: string;
  targetCondition: string;
  tract: string;
  keyFeatures: string[];
}
```

## 3.4 Overlay Panel Schema

```typescript
interface CompMapOverlay {
  subject: SubjectPin;
  comps: CompMapPin[];
  compSetStrength: 'STRONG' | 'MODERATE' | 'WEAK';
  strengthReason: string;
  recommendedAction: 'PROCEED' | 'EXPAND_FILTERS';
  expansionSuggestions?: ExpansionSuggestion[];
  pruningApplied?: PruningNotification;
}

interface ExpansionSuggestion {
  type: 'DISTANCE' | 'YEAR_BUILT' | 'COE_DAYS' | 'SQFT';
  currentValue: string;
  suggestedValue: string;
  potentialNewComps: number;
}

interface PruningNotification {
  originalCount: number;
  prunedCount: number;
  pruningReasons: string[];
}
```

---

# 4. LOCATION VARIABLES FOR MAP (Column C Priority)

These variables are **highest priority** for the Map view overlay:

| Variable | Map Relevance | Visual Treatment |
|----------|---------------|------------------|
| **Tract/Subdivision** | CRITICAL | Boundary polygon + match indicator |
| **Distance** | CRITICAL | Radius rings + distance label |
| **School District** | HIGH | District boundary overlay option |
| **Street Position** | HIGH | Pin tooltip indicator |
| **Busy Street** | HIGH | Road highlight on map |
| **Micro-Submarket Barrier** | HIGH | Barrier line on map |
| **HOA Boundaries** | MEDIUM | Optional boundary overlay |
| **Backing/Adjacency** | MEDIUM | Adjacency indicator in popup |
| **View** | LOW | Text in overlay only |

---

# 5. ACCEPTANCE CRITERIA (QA Checklist)

## 5.1 Map Display
- [ ] Subject pin clearly distinguished from comp pins
- [ ] Comp pins show rank visually (size/number/color)
- [ ] Status color coding correct (Sold/Pending/Active)
- [ ] REMOVE comps have reduced opacity
- [ ] Distance radius rings display correctly
- [ ] Tract boundaries render from PropertyRadar data

## 5.2 IQ Button & Overlay
- [ ] IQ button visible below map
- [ ] Click reveals overlay panel (not modal/popup)
- [ ] Overlay appears below map, not blocking it
- [ ] Overlay is scrollable if content exceeds viewport

## 5.3 Interaction
- [ ] Click map pin → highlights overlay list item
- [ ] Hover overlay list item → highlights map pin
- [ ] KEEP/REMOVE toggle works and persists
- [ ] Override indicator displays correctly

## 5.4 Content
- [ ] Ranked list matches map pin positions
- [ ] Explanations use only ABCD variables
- [ ] No price-driven ranking language
- [ ] Silence when variables not relevant
- [ ] Max 3-5 bullets per comp
- [ ] Manual update guidance included

## 5.5 Comp Discipline
- [ ] Never more than 12 comps without notification
- [ ] Expansion suggested when < 6 comps
- [ ] Pruning notification when > 12 original comps
- [ ] Comp Set Strength badge displayed

---

# 6. IMPLEMENTATION PHASES

## Phase 1: Core Map Overlay
- Subject and comp pins with basic styling
- IQ button + overlay panel structure
- Ranked list display
- Basic pin interaction (click to highlight)

## Phase 2: Geographic Intelligence
- Tract boundary overlays
- Distance radius rings
- Micro-market barrier detection
- School district visualization (optional toggle)

## Phase 3: Full ABCD Integration
- Complete explanation generation
- All Column C location variables
- Data source traceability (Column D)
- Image-based condition in pin tooltips

## Phase 4: User Controls & Polish
- KEEP/REMOVE overrides
- Expansion recommendations with preview
- Pruning notifications
- Comp Set Strength assessment

---

# 7. DEPENDENCIES

| Dependency | Owner | Status |
|------------|-------|--------|
| MLS comp data feed | Existing | Available |
| PropertyRadar tract data | Existing | Available |
| Google Maps integration | Existing | Available |
| GPT-4 image analysis | Tony | Pending training spec |
| ABCD variable definitions | Tony | Pending data points |
| OpenAI assistant config | Tony | Pending output samples |

---

# 8. SIGN-OFF

| For Nate (CTO) |
|----------------|
| • React component in PIQ Comps tab |
| • Map pins with rank/status encoding |
| • Overlay panel below map (not modal) |
| • Tract boundaries via PropertyRadar GeoJSON |
| • Distance calc via Google Distance Matrix |
| • TypeScript schemas defined above |
| • Phase 1-4 implementation path |

---

*Document prepared for FlipIQ Engineering*
*Version 1.0 — December 28, 2024*
