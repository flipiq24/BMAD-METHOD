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
| **Version** | v2.0 — December 28, 2024 |

---

# 1. CURRENT UI REFERENCE

## 1.1 Actual Interface Layout

Based on current PIQ implementation:

```
┌─────────────────────────────────────────────────────────────────────────┐
│ PIQ │ [Comps] │ Investment Analysis │ Agent │ Offer Terms    🟠IQ │ Notes│
├─────────────────────────────────────────────────────────────────────────┤
│ 1551-2201 sqft │ Built 941-1955 │ 1 mile radius │ List Price │ Filters │
│                                        │ [Map] Matrix List │ 3 of 3 │ ✓Finalize │
├─────────────────────────────────────────────────────────────────────────┤
│ 🗺️ Map │ Street View │ Aerial │ Draw Area │ Freehand                    │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│                         MAP VIEW                                        │
│     [$499K]              [S]                    [$650K]                  │
│                                                          [$800K]        │
│                                                                         │
├─────────────────────────────────────────────────────────────────────────┤
│  PROPERTY              │  LOT                   │  LOCATION              │
│  Garage: 3-car attached│  Pool: In-ground heated│  Golf Course: Front    │
│  Solar: Owned          │  Lot Shape: Rectangular│  Tract: Terra Lago     │
│                        │                        │  HOA: Guard gated      │
├─────────────────────────────────────────────────────────────────────────┤
│ ☐ KEEP (2 comps)                    │ ☐ REMOVE (2 comps)                 │
│    [Only Keep Selected]             │    [Only Remove Selected]          │
├─────────────────────────────────────┼────────────────────────────────────┤
│ ☐ 84303 Eremo Way  [High] $499K SOLD│ LOWER RELEVANCE                    │
│   ✓ Best comp - Model match         │ ☐ 84892 Lago Way  [Weak] $800K SOLD│
│   ✓ Golf course front like subject  │   ✗ 5BR/3BA - Subject is 4BR/2BA   │
│   ✓ WHY KEPT: Best comp, same tract │   ✗ WHY REMOVED: Different buyer   │
├─────────────────────────────────────┼────────────────────────────────────┤
│ ☐ 84521 Terra Lago  [High] $650K    │ ☐ 84312 Avenue 43 [Weak] $565K SOLD│
│   ✓ Same tract (Terra Lago)         │   ✗ NO POOL - Subject has pool     │
│   ✓ Recent flip with good condition │   ✗ NOT golf course location       │
│   ✓ WHY KEPT: Same tract            │   ✗ WHY REMOVED: NO POOL           │
└─────────────────────────────────────────────────────────────────────────┘
```

## 1.2 Key UI Components (As Built)

| Component | Location | Current Behavior |
|-----------|----------|------------------|
| **IQ Button** | Top-right header (orange) | Toggles overlay visibility |
| **Filter Bar** | Below header | Sqft, Year Built, Radius, Price filters |
| **View Tabs** | Filter bar right | Map / Matrix / List toggle |
| **Comp Counter** | Next to view tabs | "3 of 3 comps" |
| **Finalize Button** | Far right (green) | Confirms comp selection |
| **Map Sub-nav** | Below filters | Map / Street View / Aerial / Draw Area / Freehand |
| **Subject Pin** | On map | "S" marker |
| **Comp Pins** | On map | Price labels ($499K, $650K, etc.) |
| **ABC Summary** | Below map | PROPERTY / LOT / LOCATION columns |
| **KEEP Section** | Left panel below ABC | Checkboxes + comp cards |
| **REMOVE Section** | Right panel below ABC | Lower Relevance + Redundant subsections |

---

# 2. FEATURE OVERVIEW

## 2.1 Purpose

The Comp Map Overlay provides intelligent analysis of pre-filtered comparable properties, helping Acquisition Associates understand which comps are most relevant and why—while retaining full control over final selection.

## 2.2 Key Principle

> **This is a TRAINING OVERLAY** — Read-only explanations that guide users to manually update information. No direct data modifications.

## 2.3 What the IQ Button Activates

When user clicks the **IQ button** (orange, top-right):
1. ABC Summary columns populate with subject-to-comp comparisons
2. KEEP/REMOVE sections populate with ranked comp cards
3. Confidence badges appear (High/Weak)
4. Explanation bullets generate for each comp
5. WHY KEPT / WHY REMOVED summaries display

---

# 3. USER STORIES

## 3.1 Primary User Story

> **US-MAP-001**: As an Acquisition Associate, I want to see my pre-filtered comps analyzed and sorted into KEEP/REMOVE categories with clear explanations so that I can quickly understand which comps are most relevant to my subject property.

**Acceptance Criteria:**
- [x] Subject property displays as distinct "S" pin
- [x] Comp pins display with price labels
- [x] IQ button reveals analysis overlay below map
- [x] Comps sorted into KEEP and REMOVE sections
- [x] Each comp shows confidence indicator (High/Weak)

---

## 3.2 ABC Summary Stories

### US-MAP-002: Property Summary (Column A)
> As an AA, I want to see a summary of key property differences so that I understand structural variations across comps.

**Current Implementation:**
- Garage: Type and capacity comparison
- Solar: Owned vs Leased vs None
- Shows delta from subject ("2 comps have 2-car — extra capacity adds appeal")

**Acceptance Criteria:**
- [x] Only shows variables that differ across comps
- [x] Explains relevance of difference
- [ ] Add: Stories/Layout comparison
- [ ] Add: ADU/Guest House detection

---

### US-MAP-003: Lot Summary (Column B)
> As an AA, I want to see a summary of lot differences so that I understand usability variations.

**Current Implementation:**
- Pool: Type and presence ("1 comp has no pool — pool changes buyer pool")
- Lot Shape: Standard vs irregular

**Acceptance Criteria:**
- [x] Pool comparison working
- [x] Lot shape comparison working
- [ ] Add: Lot size delta when significant
- [ ] Add: RV/Boat parking detection

---

### US-MAP-004: Location Summary (Column C)
> As an AA, I want to see a summary of location factors so that I understand micro-market positioning.

**Current Implementation:**
- Golf Course: Adjacency type ("Front adjacency — Premium positioning")
- Tract: Name and match status ("Terra Lago — All comps same tract")
- HOA: Type ("Guard gated — Community amenities included")

**Acceptance Criteria:**
- [x] Golf course adjacency working
- [x] Tract matching working
- [x] HOA type displayed
- [ ] Add: School district comparison
- [ ] Add: Busy street / arterial detection
- [ ] Add: Micro-market barrier warnings

---

## 3.3 KEEP Section Stories

### US-MAP-005: KEEP Comp Cards
> As an AA, I want to see detailed explanations for why each comp is recommended to keep so that I can validate the bot's reasoning.

**Current Implementation:**
```
☐ 84303 Eremo Way    Confidence: High    $499K  SOLD
   Single Family / 4 Br / 2 Ba / 3 cars / 2005 / 2,122 ft² / 7,841 ft² / Pool: Yes
   ✓ Best comp - Model match, same tract
   ✓ Golf course front like subject
   ✓ Flip condition - best ARV baseline
   ✓ CONDITION: Standard — 0.44 mi from subject
   ✓ WHY KEPT: Best comp - Model match, same tract
```

**Acceptance Criteria:**
- [x] Checkbox for selection
- [x] Address + Confidence badge + Price + Status
- [x] Property specs summary line
- [x] Green checkmarks (✓) for positive matches
- [x] WHY KEPT summary in green
- [x] Distance from subject displayed
- [ ] Add: Click to highlight pin on map
- [ ] Add: "Only Keep Selected" batch action

---

### US-MAP-006: Confidence Indicator
> As an AA, I want to see a confidence level for each comp so that I know how strongly the bot recommends it.

**Current Implementation:**
- **High** (green badge): Strong alignment with subject
- **Weak** (orange badge): Lower alignment, use with caution

**Acceptance Criteria:**
- [x] High/Weak badges displayed
- [ ] Add: Tooltip explaining confidence factors
- [ ] Consider: "Moderate" middle tier

---

## 3.4 REMOVE Section Stories

### US-MAP-007: REMOVE Comp Cards - Lower Relevance
> As an AA, I want to see why comps are marked for removal due to lower relevance so that I understand the mismatch.

**Current Implementation:**
```
LOWER RELEVANCE
☐ 84892 Lago Way    Confidence: Weak    $800K  SOLD
   Single Family / 5 Br / 3 Ba / 3 cars / 2010 / 2,500 ft² / 10,000 ft² / Pool: Yes
   ✗ 5BR/3BA - Subject is 4BR/2BA, different buyer pool
   ✗ 378 sqft larger than subject
   ✗ Lot 2,159 sqft larger - affects value comparison
   ✗ CONDITION: Standard — 0.85 mi from subject
   ✗ WHY REMOVED: 5BR/3BA - Subject is 4BR/2BA, different buyer pool
```

**Acceptance Criteria:**
- [x] "LOWER RELEVANCE" subsection header
- [x] Red X marks (✗) for mismatches
- [x] WHY REMOVED summary in red
- [x] Specific mismatch explanations
- [ ] Add: "Restore to KEEP" action
- [ ] Add: Click to highlight pin on map

---

### US-MAP-008: REMOVE Comp Cards - Feature Mismatch
> As an AA, I want to see when comps are removed due to specific feature mismatches so that I understand the specific issue.

**Current Implementation:**
```
☐ 84312 Avenue 43    Confidence: Weak    $565K  SOLD
   ✗ NO POOL - Subject has pool
   ✗ NOT golf course location
   ✗ Interior street location
   ✗ WHY REMOVED: NO POOL - Subject has pool
```

**Acceptance Criteria:**
- [x] Feature mismatches clearly stated
- [x] Subject comparison included ("Subject has pool")
- [ ] Add: Redundant subsection (separate from Lower Relevance)

---

## 3.5 Map Interaction Stories

### US-MAP-009: Pin-to-Card Linking
> As an AA, I want to click a map pin and see the corresponding comp card highlighted so that I can connect geographic position to details.

**Acceptance Criteria:**
- [ ] Click pin → scroll to and highlight comp card
- [ ] Click comp card → highlight pin on map
- [ ] Hover preview on pin with address and key stats

---

### US-MAP-010: Price vs Rank Display
> As an AA, I want map pins to show price but understand this doesn't indicate rank so that I don't confuse price with relevance.

**Current Implementation:**
- Pins show price labels ($499K, $650K, $800K)

**Consideration:**
- Price display is useful for context
- Rank is shown in KEEP/REMOVE order, not on map
- May want option to toggle pin display: Price / Rank # / Status

---

## 3.6 Batch Action Stories

### US-MAP-011: Bulk Selection Actions
> As an AA, I want to select multiple comps and apply bulk actions so that I can quickly finalize my comp set.

**Current Implementation:**
- Checkboxes on each comp card
- "Only Keep Selected" button
- "Only Remove Selected" button

**Acceptance Criteria:**
- [x] Individual checkboxes working
- [x] Bulk action buttons present
- [ ] Add: "Select All KEEP" shortcut
- [ ] Add: "Move to KEEP" for selected REMOVE comps

---

### US-MAP-012: Finalize Workflow
> As an AA, I want to finalize my comp selection and proceed to ARV analysis so that I can complete my evaluation.

**Current Implementation:**
- Green "✓ Finalize" button in header

**Acceptance Criteria:**
- [x] Finalize button visible
- [ ] Confirmation of final comp count
- [ ] Validation: Warn if < 6 comps finalized
- [ ] Transition to next workflow step

---

# 4. TECHNICAL SPECIFICATIONS

## 4.1 Data Schema (Updated for Current UI)

```typescript
interface CompCard {
  id: string;
  address: string;
  price: number;
  status: 'SOLD' | 'PENDING' | 'ACTIVE';
  confidence: 'High' | 'Weak';
  recommendation: 'KEEP' | 'REMOVE';
  removeCategory?: 'LOWER_RELEVANCE' | 'REDUNDANT';

  // Property specs line
  propertyType: string;
  beds: number;
  baths: number;
  cars: number;
  yearBuilt: number;
  sqft: number;
  lotSqft: number;
  hasPool: boolean;

  // Analysis results
  matchBullets: string[];      // ✓ items for KEEP
  mismatchBullets: string[];   // ✗ items for REMOVE
  whyKept?: string;
  whyRemoved?: string;
  condition: string;
  distanceFromSubject: number; // miles

  // Map data
  coordinates: { lat: number; lng: number };
  isSelected: boolean;
}

interface ABCSummary {
  property: SummaryItem[];  // Column A
  lot: SummaryItem[];       // Column B
  location: SummaryItem[];  // Column C
}

interface SummaryItem {
  label: string;      // e.g., "Garage: 3-car attached"
  explanation: string; // e.g., "2 comps have 2-car — extra capacity adds appeal"
}

interface CompMapOverlay {
  subject: SubjectProperty;
  abcSummary: ABCSummary;
  keepComps: CompCard[];
  removeComps: CompCard[];
  totalComps: number;
  selectedCount: number;
}
```

## 4.2 API Response Structure

```json
{
  "subject": {
    "address": "123 Main St",
    "beds": 4,
    "baths": 2,
    "sqft": 1876,
    "pool": true,
    "tract": "Terra Lago",
    "golfCourse": "front"
  },
  "abcSummary": {
    "property": [
      { "label": "Garage: 3-car attached", "explanation": "2 comps have 2-car — extra capacity adds appeal" },
      { "label": "Solar: Owned", "explanation": "Owned solar adds value vs leased or none" }
    ],
    "lot": [
      { "label": "Pool: In-ground heated", "explanation": "1 comp has no pool — pool changes buyer pool" },
      { "label": "Lot Shape: Rectangular", "explanation": "Standard usable lot, no constraints" }
    ],
    "location": [
      { "label": "Golf Course: Front adjacency", "explanation": "Premium positioning — 2 comps are interior" },
      { "label": "Tract: Terra Lago", "explanation": "All comps same tract — strong anchor" },
      { "label": "HOA: Guard gated", "explanation": "Community amenities included" }
    ]
  },
  "keepComps": [...],
  "removeComps": [...]
}
```

---

# 5. ACCEPTANCE CRITERIA (QA Checklist)

## 5.1 IQ Button Activation
- [x] IQ button visible in header (orange)
- [x] Click toggles overlay visibility
- [ ] Loading state while analysis runs
- [ ] Error handling if analysis fails

## 5.2 ABC Summary Display
- [x] Three columns: PROPERTY, LOT, LOCATION
- [x] Only relevant variables displayed
- [x] Explanations include comp comparisons
- [ ] Silence when no differences exist

## 5.3 KEEP Section
- [x] Comp count displayed "(2 comps)"
- [x] Confidence badges (High/Weak)
- [x] Green checkmarks for matches
- [x] WHY KEPT summary
- [x] Distance from subject
- [ ] Checkbox selection working
- [ ] "Only Keep Selected" action

## 5.4 REMOVE Section
- [x] Comp count displayed "(2 comps)"
- [x] "LOWER RELEVANCE" subsection
- [x] Red X marks for mismatches
- [x] WHY REMOVED summary
- [ ] "REDUNDANT" subsection (when applicable)
- [ ] Checkbox selection working
- [ ] "Only Remove Selected" action

## 5.5 Map Integration
- [x] Subject pin with "S" marker
- [x] Comp pins with price labels
- [ ] Pin-to-card linking (click interaction)
- [ ] Visual differentiation for KEEP vs REMOVE pins

## 5.6 Finalize Flow
- [x] Finalize button visible
- [ ] Comp count validation
- [ ] Confirmation dialog
- [ ] Proceed to next step

---

# 6. ENHANCEMENTS BACKLOG

## 6.1 Phase 2 - Map Enhancements
- [ ] Tract boundary polygons overlay
- [ ] Distance radius rings (0.25mi, 0.5mi, 1mi)
- [ ] School district boundaries (toggle)
- [ ] Micro-market barrier highlighting

## 6.2 Phase 2 - Interaction Enhancements
- [ ] Bidirectional pin ↔ card highlighting
- [ ] Pin hover previews
- [ ] Drag-and-drop KEEP ↔ REMOVE
- [ ] Comp card expand/collapse

## 6.3 Phase 2 - Analysis Enhancements
- [ ] Comp Set Strength badge (Strong/Moderate/Weak)
- [ ] Filter expansion recommendations when < 6 comps
- [ ] Redundancy pruning notifications when > 12 comps
- [ ] Image-based condition confidence

---

# 7. SIGN-OFF

| For Nate (CTO) |
|----------------|
| • Current UI structure documented above |
| • TypeScript schemas match actual data |
| • QA checklist with current vs TODO items |
| • Phase 2 backlog prioritized |
| • ABC Summary follows ABCD model from parent PRD |
| • KEEP/REMOVE logic matches comp discipline rules |

---

*Document prepared for FlipIQ Engineering*
*Version 2.0 — December 28, 2024*
*Updated with actual UI reference from PIQ screenshot*
