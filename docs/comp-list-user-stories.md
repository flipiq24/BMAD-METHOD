# COMP.LIST BOT
## User Stories & Epics
### For Engineering Implementation

---

| Field | Value |
|-------|-------|
| **Feature ID** | C3 (List Grouping Bot) |
| **Parent System** | PIQ Comps Module |
| **Priority** | P1 — Core Feature |
| **Primary User** | Acquisition Associate (AA) |
| **UI Location** | PIQ → Comps Tab → List View |
| **Handoff To** | Eric (PM) / Nate (CTO) |
| **PRD Reference** | comp-list-prd.md v2.0 |
| **Version** | v1.0 — December 31, 2024 |

---

# EXECUTIVE SUMMARY

The Comp.List Bot transforms a flat ranked comp list into a visually stratified view with four buckets (PREMIUM, HIGH, MID, LOW) and identifies the specific comp that defines the value ceiling.

**Core Principle:**
> The ceiling is a COMP, not a percentage. Always name the comp. Always list the unchangeables.

**What It Does:**
- Groups comps into 4 buckets based on PIQ comparison
- Identifies the SPECIFIC ceiling comp (not a calculation)
- Auto-generates attribute chips for each comp
- Explains WHY each comp is classified
- Does NOT make ARV decisions — user retains control

---

# EPIC 1: IQ BUTTON & OVERLAY ACTIVATION

## Epic Summary
Enable the iQ button to trigger bucket classification on the List view.

---

### US-LIST-001: iQ Button Activation
> As an AA, I want to click the iQ button on the List view so that I can see comps grouped into buckets with the ceiling identified.

**Acceptance Criteria:**
- [ ] iQ button visible in List view header
- [ ] Click triggers bucket classification (not auto-load)
- [ ] Loading state displayed while classification runs
- [ ] Error handling if classification fails
- [ ] Overlay appears with bucketed sections

**PRD Reference:** Section 1.4, FR14

---

### US-LIST-002: Performance Requirement
> As an AA, I want bucket classification to complete quickly so that I don't wait for analysis.

**Acceptance Criteria:**
- [ ] Classification completes within 2 seconds for up to 50 comps
- [ ] Progress indicator shown for longer operations
- [ ] Partial results displayed if timeout occurs

**PRD Reference:** NFR1

---

# EPIC 2: BUCKET CLASSIFICATION

## Epic Summary
Classify each comp into exactly one bucket (PREMIUM/HIGH/MID/LOW) using the decision sequence.

---

### US-LIST-003: Single Bucket Assignment
> As an AA, I want each comp assigned to exactly one bucket so that there's no ambiguity about classification.

**Acceptance Criteria:**
- [ ] Every comp is classified into exactly ONE bucket
- [ ] No comp appears in multiple buckets
- [ ] First match wins in classification sequence
- [ ] All KEPT comps from C-OVERLAY are classified

**PRD Reference:** Section 2.3, FR1, FR3

---

### US-LIST-004: LOW Bucket Classification
> As an AA, I want comps with significant negatives classified as LOW so that I know they're floor references only.

**Acceptance Criteria:**
- [ ] Distress sales (REO/short sale/probate/auction) → LOW
- [ ] Busy street / freeway / railroad adjacency → LOW
- [ ] FIXER condition from C-OVERLAY → LOW
- [ ] Power line adjacency → LOW
- [ ] LOW checked FIRST in decision sequence

**Classification Logic:**
```
IF is_distress_sale = true → LOW
IF street_type = 'BUSY' OR near_freeway OR near_railroad → LOW
IF condition = 'FIXER' → LOW
```

**PRD Reference:** Section 2.1 (LOW), Section 2.3 Step 1

---

### US-LIST-005: PREMIUM Bucket Classification
> As an AA, I want comps with unchangeable advantages over PIQ classified as PREMIUM so that I know they define the ceiling.

**Acceptance Criteria:**
- [ ] Comps with permitted ADU (PIQ lacks) → PREMIUM
- [ ] Comps with pool + large yard (PIQ lacks) → PREMIUM
- [ ] Comps in gated community (PIQ not gated) → PREMIUM
- [ ] Comps with notable view (PIQ lacks) → PREMIUM
- [ ] Comps on premium street (PIQ on busy street) → PREMIUM
- [ ] Comps with 50%+ bigger usable lot → PREMIUM
- [ ] PREMIUM checked AFTER LOW in decision sequence

**Unchangeable Detection Logic:**
```
IF comp.has_adu AND NOT piq.has_adu → PREMIUM
IF comp.has_pool AND comp.lot_sqft > 7000 AND NOT piq.has_pool → PREMIUM
IF comp.is_gated AND NOT piq.is_gated → PREMIUM
IF comp.has_view AND NOT piq.has_view → PREMIUM
IF comp.street_type = 'CUL_DE_SAC' AND piq.street_type != 'CUL_DE_SAC' → PREMIUM
IF comp.lot_sqft > (piq.lot_sqft * 1.5) → PREMIUM
```

**PRD Reference:** Section 2.1 (PREMIUM), Section 2.2.1, Section 2.3 Step 2

---

### US-LIST-006: HIGH Bucket Classification
> As an AA, I want comps that PIQ can realistically match classified as HIGH so that I know they're my primary ARV anchors.

**Acceptance Criteria:**
- [ ] Same tract as PIQ
- [ ] Within 20% of PIQ sqft
- [ ] Within 10 years of PIQ year built
- [ ] Same bed/bath class (±1 bed, ±0.5 bath)
- [ ] Condition = FLIP or GOOD
- [ ] NO unchangeable advantage over PIQ
- [ ] HIGH checked AFTER PREMIUM in decision sequence

**Classification Logic:**
```
IF same_tract
   AND sqft within 20% of piq.sqft
   AND year_built within 10 years of piq.year_built
   AND condition IN ('FLIP', 'GOOD')
   AND NOT has_unchangeable_piq_lacks
→ HIGH
```

**PRD Reference:** Section 2.1 (HIGH), Section 2.3 Step 3

---

### US-LIST-007: MID Bucket Classification
> As an AA, I want remaining comps classified as MID so that I have supporting market context.

**Acceptance Criteria:**
- [ ] Comps not matching LOW, PREMIUM, or HIGH criteria → MID
- [ ] Includes ORIGINAL condition comps
- [ ] Includes comps with minor location drag
- [ ] Includes comps with minor functional obsolescence
- [ ] MID is the DEFAULT bucket (Step 4)

**PRD Reference:** Section 2.1 (MID), Section 2.3 Step 4

---

### US-LIST-008: Bucket Reason Generation
> As an AA, I want to see WHY each comp was classified so that I can verify the logic.

**Acceptance Criteria:**
- [ ] Each comp has `bucket_reason` array populated
- [ ] Reasons are specific and fact-based
- [ ] Hover on comp row shows full classification reasoning
- [ ] No generic reasons like "meets criteria"

**Example Reasons:**
```
PREMIUM: ["Permitted ADU (PIQ lacks)", "Pool + 8,500 sqft lot (PIQ has no pool)"]
HIGH: ["Same tract", "Within 15% sqft", "FLIP condition", "No unchangeables"]
MID: ["Different tract", "ORIGINAL condition"]
LOW: ["Distress sale (REO)", "Busy street location"]
```

**PRD Reference:** Section 0.3, FR13

---

# EPIC 3: VALUE CEILING IDENTIFICATION

## Epic Summary
Identify the specific comp that defines the value ceiling and explain why.

---

### US-LIST-009: Ceiling Comp Selection
> As an AA, I want the ceiling comp identified by name so that I know exactly which property sets my limit.

**Acceptance Criteria:**
- [ ] If PREMIUM comps exist: highest $/sqft PREMIUM = ceiling
- [ ] If no PREMIUM comps: highest $/sqft HIGH = ceiling
- [ ] If no PREMIUM or HIGH: show warning message
- [ ] `is_ceiling_comp = true` set on ceiling comp
- [ ] Ceiling comp identified by address, not calculation

**PRD Reference:** Section 3.1, FR5

---

### US-LIST-010: Ceiling Explanation
> As an AA, I want the ceiling explained with specific unchangeable features so that I can defend my ARV.

**Acceptance Criteria:**
- [ ] `ceiling_reason` populated with specific unchangeables
- [ ] Template: "[Address] ($XXX/sqft) is the ceiling because it has: [list unchangeables]. PIQ cannot match these through renovation."
- [ ] NO percentage calculations in explanation
- [ ] NO P90/P75 references

**Good Example:**
```
"789 Oak Lane ($425/sqft) is the value ceiling because it has:
Permitted ADU, Pool + 8,500 sqft usable lot, Cul-de-sac location.
PIQ at 123 Main St cannot match these features through renovation."
```

**Bad Example:**
```
"Value ceiling is $425/sqft based on P90 calculation of HIGH comps."
```

**PRD Reference:** Section 3.3, FR6

---

### US-LIST-011: Ceiling Visual Indicator
> As an AA, I want the ceiling comp visually highlighted so that I can spot it immediately.

**Acceptance Criteria:**
- [ ] Ceiling comp row has ⭐ star icon
- [ ] Ceiling comp row has highlighted background
- [ ] Value Ceiling Line drawn directly below ceiling comp
- [ ] Line is red dashed, labeled "VALUE CEILING"
- [ ] Hover on line shows full ceiling explanation

**PRD Reference:** Section 3.4, FR7, FR8

---

### US-LIST-012: No PREMIUM Ceiling Handling
> As an AA, I want to know when no comps have unchangeables so that I understand PIQ has no ceiling constraints.

**Acceptance Criteria:**
- [ ] When 0 PREMIUM comps: ceiling = best HIGH comp
- [ ] Explanation: "No comps with unchangeable advantages found. Best HIGH comp defines ceiling."
- [ ] Different visual treatment (no red line, different messaging)

**PRD Reference:** Section 3.1, Section 9

---

# EPIC 4: CHIP/TAG GENERATION

## Epic Summary
Auto-generate attribute chips for each comp based on data analysis.

---

### US-LIST-013: Chip Generation
> As an AA, I want chips auto-generated for each comp so that I can quickly scan key attributes.

**Acceptance Criteria:**
- [ ] Chips generated from MLS data + image analysis + remarks
- [ ] Every comp has at least one chip
- [ ] Maximum 6 chips displayed per row
- [ ] Overflow shows "+ N more" indicator
- [ ] Chips use 18 official categories ONLY

**PRD Reference:** Section 4, FR9, FR10

---

### US-LIST-014: Chip Categories
> As an AA, I want chips to use consistent categories so that I can learn the system.

**Acceptance Criteria:**
- [ ] Only these 18 categories allowed:
  1. Additions
  2. Bed / Bath
  3. Busy Street
  4. Check Notes
  5. Design
  6. Freeway
  7. Garage
  8. Guest / ADU
  9. Location
  10. Lot
  11. Lot Usable Area
  12. Obsolescence Adjacent
  13. Parking
  14. Pool
  15. Power Lines
  16. Railroad Tracks
  17. View
  18. Zoning
- [ ] No custom/ad-hoc chip categories

**PRD Reference:** Section 4.1, AC5

---

### US-LIST-015: Chip Comparison Indicators
> As an AA, I want chips to show how comps compare to PIQ so that I understand relative positioning.

**Acceptance Criteria:**
- [ ] +/=/- indicators for comparison chips
- [ ] "+1 Bed" means comp has 1 more bedroom than PIQ
- [ ] "-1 Bath" means comp has 1 fewer bathroom than PIQ
- [ ] "Bigger Lot" / "Smaller Lot" / "Similar Lot" vs PIQ

**PRD Reference:** Section 4.2

---

### US-LIST-016: Chip Hover Explanations
> As an AA, I want to hover on chips for explanation so that I understand why they were assigned.

**Acceptance Criteria:**
- [ ] Every chip has hover tooltip
- [ ] Tooltip explains WHY chip was assigned
- [ ] Tooltip renders within 100ms
- [ ] No scrolling required to read tooltip

**Example:**
```
Chip: "ADU"
Hover: "Permitted ADU detected via PropertyRadar records.
       Adds approximately $50-100K to value depending on size/finish."
```

**PRD Reference:** Section 4.2, FR11, NFR2

---

### US-LIST-017: Check Notes Chip
> As an AA, I want a "Check Notes" chip when data conflicts exist so that I know to manually verify.

**Acceptance Criteria:**
- [ ] ⚠️ Yellow "Check Notes" chip when:
  - PIQ data is incomplete
  - MLS data conflicts with PropertyRadar
  - Image confidence < 60%
- [ ] Hover explains what to check

**PRD Reference:** Section 4.1, Section 9

---

# EPIC 5: SORT ORDER & DISPLAY

## Epic Summary
Display comps in correct sort order with collapsible bucket sections.

---

### US-LIST-018: Bucket Section Order
> As an AA, I want buckets displayed in fixed order so that I can scan from ceiling to floor.

**Acceptance Criteria:**
- [ ] Display order: PREMIUM → HIGH → MID → LOW
- [ ] Value Ceiling Line between PREMIUM and HIGH
- [ ] Color coding: Purple (PREMIUM), Green (HIGH), Yellow (MID), Red (LOW)
- [ ] Bucket headers show count (e.g., "HIGH (4 comps)")

**PRD Reference:** Section 5.1, AC6

---

### US-LIST-019: Within-Bucket Sort
> As an AA, I want comps sorted within each bucket so that most relevant appear first.

**Acceptance Criteria:**
- [ ] PREMIUM: sorted by $/sqft DESC (highest first)
- [ ] HIGH: sorted by relevance_score DESC
- [ ] MID: sorted by $/sqft DESC
- [ ] LOW: sorted by sale_date DESC (most recent first)
- [ ] Tie-breaker: distance_to_piq ASC

**PRD Reference:** Section 5.2, Section 5.3, AC9

---

### US-LIST-020: Collapsible Sections
> As an AA, I want to collapse bucket sections so that I can focus on specific tiers.

**Acceptance Criteria:**
- [ ] Each bucket section has collapse/expand toggle
- [ ] Collapsed state shows header + count only
- [ ] User preference persists during session
- [ ] All sections expanded by default

**PRD Reference:** FR12, AC8

---

# EPIC 6: HOVER EXPLANATIONS

## Epic Summary
Provide AI-generated explanations for every comp on hover.

---

### US-LIST-021: Comp Hover Explanation
> As an AA, I want to hover on any comp for full explanation so that I understand classification reasoning.

**Acceptance Criteria:**
- [ ] Hover triggers explanation panel
- [ ] Panel shows:
  - Bucket assignment + reasons
  - Key attributes vs PIQ
  - Chip explanations
  - Ceiling status (if applicable)
- [ ] No advisory language ("you should", "recommended")
- [ ] Readable without scrolling

**PRD Reference:** FR13, AC7, AC11

---

### US-LIST-022: Ceiling Comp Hover
> As an AA, I want extra detail when hovering on ceiling comp so that I fully understand the constraint.

**Acceptance Criteria:**
- [ ] Ceiling comp hover shows:
  - Full unchangeable list
  - Comparison to PIQ
  - Why these features can't be added
- [ ] If multiple ceiling candidates existed, list others
- [ ] Clear statement: "[Address] defines the ceiling"

**PRD Reference:** Section 3.3, Section 9

---

# EPIC 7: EDGE CASES & ERROR HANDLING

## Epic Summary
Handle edge cases gracefully with appropriate warnings.

---

### US-LIST-023: Insufficient Comps Warning
> As an AA, I want a warning when there aren't enough comps so that I know to expand my search.

**Acceptance Criteria:**
- [ ] < 3 comps total: "Insufficient comps for reliable classification"
- [ ] 0 HIGH comps: "No achievable comps found — expand search"
- [ ] All comps = LOW: "All comps have significant negatives — review market"
- [ ] Warnings are prominent but don't block display

**PRD Reference:** Section 9

---

### US-LIST-024: Missing Data Handling
> As an AA, I want the system to handle missing MLS fields gracefully so that it doesn't crash.

**Acceptance Criteria:**
- [ ] Missing fields don't cause errors
- [ ] Missing data flagged with "Check Notes" chip
- [ ] Comparisons using missing fields are skipped
- [ ] Log missing fields for data quality tracking

**PRD Reference:** NFR3, Section 9

---

# TECHNICAL SPECIFICATIONS

## Data Schema

```typescript
interface CompListOutput {
  comps: BucketedComp[];
  ceiling_comp: {
    comp_id: string;
    address: string;
    price_per_sqft: number;
    unchangeables: string[];
    explanation: string;
  } | null;
  warnings: string[];
  bucket_counts: {
    premium: number;
    high: number;
    mid: number;
    low: number;
  };
}

interface BucketedComp {
  // Core fields from C-OVERLAY
  comp_id: string;
  address: string;
  price: number;
  sqft: number;
  price_per_sqft: number;
  beds: number;
  baths: number;
  year_built: number;
  lot_sqft: number;
  condition: 'FLIP' | 'GOOD' | 'ORIGINAL' | 'FIXER';
  relevance_score: number;

  // List-specific fields
  bucket: 'PREMIUM' | 'HIGH' | 'MID' | 'LOW';
  bucket_reason: string[];
  is_ceiling_comp: boolean;
  ceiling_reason?: string;
  chips: Chip[];
  distance_to_piq: number;

  // PIQ comparison flags
  same_tract: boolean;
  same_school_feeder: boolean;
  same_bed_class: boolean;
  same_bath_class: boolean;
  same_gla_bin: boolean;
  same_era: boolean;
  has_unchangeable_piq_lacks: boolean;
  unchangeables_list?: string[];
}

interface Chip {
  category: ChipCategory;
  value: string;
  indicator?: '+' | '=' | '-';
  hover_text: string;
}

type ChipCategory =
  | 'Additions' | 'Bed / Bath' | 'Busy Street' | 'Check Notes'
  | 'Design' | 'Freeway' | 'Garage' | 'Guest / ADU'
  | 'Location' | 'Lot' | 'Lot Usable Area' | 'Obsolescence Adjacent'
  | 'Parking' | 'Pool' | 'Power Lines' | 'Railroad Tracks'
  | 'View' | 'Zoning';
```

---

# ACCEPTANCE CRITERIA CHECKLIST

## Classification
- [ ] AC1: Every comp is classified into exactly ONE bucket
- [ ] AC2: Ceiling comp is identified by name, not by percentage calculation
- [ ] AC3: Ceiling explanation lists specific unchangeable features

## Chips
- [ ] AC4: Every comp has at least one chip assigned
- [ ] AC5: Chips use the 18 official categories only

## Display
- [ ] AC6: Value Ceiling line appears between PREMIUM and HIGH sections
- [ ] AC7: Hover explanation appears for every comp with classification reasoning
- [ ] AC8: Bucket sections are collapsible
- [ ] AC9: Sort order matches specification (bucket → $/sqft or relevance → distance)

## Governance
- [ ] AC10: System does NOT make ARV decisions — user retains full control
- [ ] AC11: No advisory language in any explanation
- [ ] AC12: Feature does NOT appear until iQ button is clicked

---

# IMPLEMENTATION PHASES

## Phase 1: Core Classification
- Bucket classification logic (LOW→PREMIUM→HIGH→MID)
- PIQ comparison matrix
- Unchangeable feature detection

## Phase 2: Ceiling Logic
- Ceiling comp selection
- Ceiling explanation generation
- Visual indicators (star, line)

## Phase 3: Chip System
- 18 chip categories
- Auto-generation from MLS + images
- Hover tooltips

## Phase 4: UI/UX
- Bucket sections with colors
- Collapse/expand functionality
- Sort order implementation
- Hover explanation panels

---

# SIGN-OFF

| For Eric (PM) |
|---------------|
| • 24 user stories across 7 epics |
| • Problem: flat list lacks ceiling ID |
| • Success: ceiling comp named + explained |
| • Key insight: ceiling = COMP, not percentage |

| For Nate (CTO) |
|----------------|
| • TypeScript schema defined |
| • PRD v2.0 reference linked |
| • 12 acceptance criteria |
| • 4-phase implementation path |
| • 18 chip categories specified |

---

*Document prepared for FlipIQ Engineering*
*Version 1.0 — December 31, 2024*
