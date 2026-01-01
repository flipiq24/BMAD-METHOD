# COMP.LIST BOT
## Product Requirements Document + Technical Specification
**Version 2.0 (Hardened) — December 31, 2024**

---

| Field | Value |
|-------|-------|
| **Bot ID** | C3 (List Grouping Bot) |
| **Category** | Comps / Analysis |
| **Priority** | P1 — Core Feature |
| **Primary User** | Acquisition Associate (AA) |
| **UI Location** | PIQ → Comps Tab → List View |
| **Trigger Method** | iQ Button Click |
| **Upstream Dependency** | C-OVERLAY (Comp Overlay Bot) — must run first |
| **Frontend Reference** | https://html-doc-viewer--td2535.replit.app/piq/12 |
| **GitHub Repo** | https://github.com/flipiq24/v0-landing-page-recreation |
| **Handoff To** | Eric (PM) / Nate (CTO) |

---

## 0. SHARED DATA SCHEMA

All four comp bots (C-OVERLAY, Comp.Map, Comp.Matrix, Comp.List) use the SAME underlying comp data object. This ensures consistency across views.

### 0.1 Core Comp Fields (Used by ALL Bots)

| Field Name | Type | Source | Used By |
|------------|------|--------|---------|
| `comp_id` | string | MLS | All |
| `address` | string | MLS | All |
| `price` | number | MLS ClosePrice | All |
| `sqft` | number | MLS LivingArea | All |
| `price_per_sqft` | number | Calculated | All |
| `beds` | int | MLS BedroomsTotal | All |
| `baths` | float | MLS BathroomsTotal | All |
| `year_built` | int | MLS YearBuilt | All |
| `lot_sqft` | number | MLS LotSizeSquareFeet | All |
| `lat` / `lng` | float | MLS Latitude/Longitude | Map, List (distance) |
| `photo_url` | string | MLS Media | All |
| `sale_date` | date | MLS CloseDate | All |
| `days_on_market` | int | MLS DaysOnMarket | Matrix, List |
| `tract` | string | MLS Subdivision | All |
| `school_district` | string | MLS SchoolDistrict | All |

### 0.2 C-OVERLAY Output Fields (Input to Comp.List)

| Field Name | Type | Values | Used By |
|------------|------|--------|---------|
| `condition` | enum | FLIP / GOOD / ORIGINAL / FIXER | List (chips) |
| `relevance_score` | float | 0-100 (higher = more relevant) | List (sort) |
| `keep_remove` | enum | KEEP / REMOVE | List (filter) |

### 0.3 List-Specific Fields (Calculated by Comp.List)

| Field Name | Type | Calculation / Purpose |
|------------|------|----------------------|
| `bucket` | enum | PREMIUM / HIGH / MID / LOW |
| `bucket_reason` | string[] | Array of reasons explaining classification |
| `is_ceiling_comp` | bool | TRUE if this comp defines the value ceiling |
| `ceiling_reason` | string | WHY this comp is the ceiling (unchangeable feature) |
| `chips` | Chip[] | Auto-generated attribute chips |
| `distance_to_piq` | float | Haversine distance from PIQ lat/lng |

### 0.4 PIQ Reference Fields (For Comparison)

| Field Name | Type | Description |
|------------|------|-------------|
| `piq.address` | string | Subject property address |
| `piq.sqft` | number | Subject GLA (Gross Living Area) |
| `piq.beds` / `piq.baths` | int / float | Subject bed/bath count |
| `piq.lot_sqft` | number | Subject lot size |
| `piq.has_pool` | bool | Does PIQ have pool? |
| `piq.has_adu` | bool | Does PIQ have permitted ADU? |
| `piq.tract` | string | PIQ tract/subdivision |
| `piq.street_type` | enum | BUSY / INTERIOR / CUL_DE_SAC / PREMIUM |
| `piq.is_gated` | bool | Is PIQ in gated community? |
| `piq.has_view` | bool | Does PIQ have notable view? |

---

## 1. PRODUCT OVERVIEW

### 1.1 Problem Statement

After C-OVERLAY ranks comps by relevance, AAs still lack visual clarity on WHERE a comp sits in the market hierarchy. A flat list of ranked comps doesn't communicate: Which comp DEFINES the ceiling PIQ cannot exceed? Which comps PIQ can realistically match? Which comps represent the floor? This creates ARV justification confusion and inconsistent offer pricing.

### 1.2 Product Goal

Transform the flat ranked comp list into a visually stratified view that:

- Groups comps into four buckets: PREMIUM, HIGH, MID, LOW
- Identifies the SPECIFIC COMP that defines the value ceiling (not a percentage)
- Auto-generates attribute chips for each comp
- Explains WHY each comp is classified with fact-driven reasoning
- Educates the user without making decisions for them

### 1.3 Core Context: Rehab Flipping

> ⚠️ **CRITICAL UNDERSTANDING:** FlipIQ serves rehab flippers, NOT developers.

| CAN Change (Rehab) | CANNOT Change (Unchangeable) |
|--------------------|------------------------------|
| Interior finishes | Lot size |
| Kitchens, baths | Location |
| Flooring, paint | Street position |
| Landscaping | Gated status |
| | School district |
| | View |

**The UNCHANGEABLE features define the ceiling.** If a comp has something PIQ physically cannot achieve through renovation, that comp becomes a ceiling reference.

### 1.4 User Story

> **AS AN** Acquisition Associate reviewing comps,
> **I WANT** comps grouped into buckets with the ceiling comp clearly identified and explained,
> **SO THAT** I can instantly understand which comp sets my ARV ceiling, WHY (what unchangeable feature), and defend my offer price.

### 1.5 Data Flow Architecture

Four bots use the SAME data but produce DIFFERENT outputs:

| Sequence | Bot | Output | Purpose |
|----------|-----|--------|---------|
| 1 | C-OVERLAY | Ranked List | Ranks comps by relevance, KEEP/REMOVE logic |
| 2a | Comp.Map | Colored Pins | Geographic visualization, Top 3 overlay |
| 2b | Comp.Matrix | Data Grid | Feeds valuation calculations |
| 2c | **Comp.List (THIS)** | Bucketed Groups | Visual stratification + ceiling identification |

> 🚫 **CRITICAL:** Do NOT mix features between bots. Top 3 Comps = Comp.Map ONLY.

---

## 2. BUCKET CLASSIFICATION LOGIC

### 2.1 Bucket Definitions

#### 🔵 PREMIUM — Ceiling Comps (Unchangeable Advantages)

**Definition:** Comps with UNCHANGEABLE features that PIQ CANNOT achieve through renovation.

**Unchangeable Features (triggers PREMIUM):**
- Permitted ADU — PIQ does not have one and adding one is beyond rehab scope
- Bigger usable lot — PIQ's lot cannot be expanded
- Pool + large yard — PIQ has no pool or inadequate yard for pool
- Premium street / cul-de-sac — PIQ is on busy street or inferior position
- View (lake/panoramic) — PIQ lacks this view
- Waterfront / golf course / park adjacency — PIQ lacks this adjacency
- Gated community — PIQ is not in gated community
- Superior school feeder — PIQ is in different/inferior district

**Governance:** PREMIUM comps define the ceiling. The BEST PREMIUM comp (by $/sqft) with unchangeables PIQ lacks IS the ceiling.

---

#### 🟢 HIGH — Primary ARV Anchors (Achievable)

**Definition:** Comps that PIQ CAN realistically match after rehab. No unchangeable advantage over PIQ.

**Qualifying Criteria (ALL must be true):**
- Same property type, within 20% of PIQ sqft, within 10 years of PIQ year built
- Same bed/bath class (±1 bed, ±0.5 bath)
- Flip-level or updated condition (FLIP or GOOD from C-OVERLAY)
- NO unchangeable advantage over PIQ
- Same micro-market (tract / school feeder)

**Governance:** If NO PREMIUM comps exist, the best HIGH comp becomes the ceiling. HIGH comps are the primary ARV anchors.

---

#### 🟡 MID — Supportive Comps

**Definition:** Near-match comps with notable but manageable deltas.

**Qualifying Criteria (one or more):**
- Older finishes (ORIGINAL condition)
- Smaller or slightly inferior lot
- Minor location drag (some traffic noise, backs school)
- Minor obsolescence (dated floor plan)

**Governance:** MID comps show market floor and mid-range. They support but do not drive ARV.

---

#### 🔴 LOW — Floor Comps

**Definition:** Comps with significant negatives. Context only.

**Qualifying Criteria (one or more):**
- Distress sale (REO / short sale / probate / auction)
- Busy street / freeway / railroad / power lines
- Poor or unusable lot
- Bad condition (FIXER)

**Governance:** LOW comps define the bottom. They NEVER drive ARV.

---

### 2.2 PIQ Comparison Matrix

For each comp, calculate these comparison flags against PIQ:

| Comparison Field | Logic | Result |
|------------------|-------|--------|
| `same_tract` | comp.tract == piq.tract | bool |
| `same_school_feeder` | comp.school_district == piq.school_district | bool |
| `same_bed_class` | abs(comp.beds - piq.beds) <= 1 | bool |
| `same_bath_class` | abs(comp.baths - piq.baths) <= 0.5 | bool |
| `same_gla_bin` | comp.sqft within 20% of piq.sqft | bool |
| `same_era` | abs(comp.year_built - piq.year_built) <= 10 | bool |
| `has_unchangeable_piq_lacks` | See 2.2.1 below | bool |

#### 2.2.1 Unchangeable Feature Detection

A comp has an unchangeable PIQ lacks if ANY of these are true:

| Unchangeable | Detection Logic |
|--------------|-----------------|
| Permitted ADU | `comp.has_adu == true AND piq.has_adu == false` |
| Pool + Large Yard | `comp.has_pool AND comp.lot_sqft > 7000 AND (NOT piq.has_pool)` |
| Gated Community | `comp.is_gated == true AND piq.is_gated == false` |
| Notable View | `comp.has_view == true AND piq.has_view == false` |
| Premium Street | `comp.street_type == 'CUL_DE_SAC' AND piq.street_type != 'CUL_DE_SAC'` |
| Bigger Usable Lot | `comp.lot_sqft > (piq.lot_sqft * 1.5) AND comp.lot_usable_pct > 80` |
| Better Location | `comp.street_type == 'INTERIOR' AND piq.street_type == 'BUSY'` |

---

### 2.3 Classification Decision Sequence

**Process each comp in this EXACT sequence (first match wins):**

**Step 1:** Check for LOW disqualifiers
- Is distress sale? → LOW
- Is on busy street / freeway / railroad? → LOW
- Is condition = FIXER? → LOW

**Step 2:** Check for PREMIUM unchangeables
- Does comp have unchangeable PIQ lacks? → PREMIUM

**Step 3:** Check for HIGH qualification
- Is same_tract AND same_gla_bin AND same_era AND condition = FLIP/GOOD? → HIGH

**Step 4:** Everything else → MID

> 🚫 **RULE:** A comp can only be in ONE bucket. First match wins.

---

## 3. VALUE CEILING LOGIC

> ⚠️ **CRITICAL: THE CEILING IS A COMP, NOT A PERCENTAGE**

The value ceiling is determined by identifying the SPECIFIC COMP that has unchangeable features PIQ cannot match. It is NOT a P90 calculation. It is fact-driven:

*"This comp has an ADU, bigger lot, and pool. PIQ does not. This comp IS the ceiling."*

### 3.1 Ceiling Determination Logic

| Scenario | Ceiling Determination |
|----------|----------------------|
| PREMIUM comps exist | The HIGHEST $/sqft PREMIUM comp that has unchangeables PIQ lacks = CEILING COMP |
| No PREMIUM comps | The HIGHEST $/sqft HIGH comp = CEILING COMP (nothing holds PIQ back) |
| No PREMIUM or HIGH | Show warning: "Unable to determine ceiling — insufficient comparable data" |

### 3.2 Ceiling Comp Identification

When a ceiling comp is identified, the system must:

1. Set `is_ceiling_comp = true` on that comp
2. Populate `ceiling_reason` with specific unchangeables
3. Display visual indicator (star icon) on that comp row
4. Draw the VALUE CEILING LINE directly below that comp

### 3.3 Ceiling Explanation Template

The ceiling explanation must be fact-driven, not calculated:

**✅ GOOD Example:**
> "789 Oak Lane ($425/sqft) is the value ceiling because it has: Permitted ADU, Pool + 8,500 sqft usable lot, Cul-de-sac location. PIQ at 123 Main St cannot match these features through renovation."

**❌ BAD Example:**
> ~~"Value ceiling is $425/sqft based on P90 calculation of HIGH comps."~~

**RULE:** Always name the comp. Always list the unchangeables. Never cite a math formula.

### 3.4 Visual Behavior

- RED DASHED LINE appears between ceiling comp and the next bucket down
- Line is labeled: "VALUE CEILING" with ceiling comp address
- Ceiling comp row has ⭐ icon and highlighted background
- Hover on line shows full ceiling explanation

---

## 4. CHIP/TAG GENERATION SYSTEM

Chips are auto-generated based on MLS data, remarks parsing, and image analysis.

### 4.1 Official Chip Categories (18 Total)

| Chip Category | Values / Examples | Primary Data Source |
|---------------|-------------------|---------------------|
| **Additions** | +, =, - (comp has more/same/less than PIQ) | MLS + Comparison |
| **Bed / Bath** | +1 Bed, -1 Bath, Same | MLS + PIQ Compare |
| **Busy Street** | Yes/No indicator | Google Roads API |
| **Check Notes** | ⚠️ Yellow flag for manual review | Conflict Detection |
| **Design** | High Flip, Standard, Original, Fixer | C-OVERLAY + Images |
| **Freeway** | Proximity indicator | Google Places API |
| **Garage** | 2-car, 3-car, Converted, None | MLS GarageSpaces |
| **Guest / ADU** | Permitted ADU, Unpermitted, Guest House | Remarks + PropertyRadar |
| **Location** | Same Tract, Different Tract, Gated, Premium | MLS + PropertyRadar |
| **Lot** | Bigger, Smaller, Similar (vs PIQ) | MLS LotSize + Compare |
| **Lot Usable Area** | Flat, Hillside, Partially Usable | PropertyRadar + Images |
| **Obsolescence Adjacent** | Functional obsolescence indicator | Images + Remarks |
| **Parking** | RV Parking, Extra Driveway, Limited | MLS + Images |
| **Pool** | Pool, No Pool, Pool + Spa, Luxury Pool | MLS PoolPrivateYN |
| **Power Lines** | Adjacent/Visible indicator | Images + Mapping |
| **Railroad Tracks** | Proximity indicator | Google Places API |
| **View** | None, Peek, Partial, Panoramic | MLS View + Images |
| **Zoning** | Same Zone, Different Zone, Horse Property | PropertyRadar |

### 4.2 Chip Display Rules

- Each chip has hover text explaining WHY it was assigned
- Maximum 6 chips displayed per comp row; "+ N more" for overflow
- Chips showing DIFFERENCE from PIQ use +/=/- indicators
- Priority order: Design → Location → Lot → Pool → ADU → Others

---

## 5. SORT ORDER LOGIC

### 5.1 Primary Sort: By Bucket (Fixed Order)

Sections appear in this fixed order, top to bottom:

| Order | Section | Color | Purpose |
|-------|---------|-------|---------|
| 1 | PREMIUM | Purple | Ceiling comps (unchangeables PIQ lacks) |
| — | VALUE CEILING LINE | Red Dashed | Drawn below ceiling comp |
| 2 | HIGH | Green | ARV anchors (achievable) |
| 3 | MID | Yellow | Supporting comps |
| 4 | LOW | Red | Floor comps |

### 5.2 Secondary Sort: Within Each Bucket

| Bucket | Sort Field | Direction | Rationale |
|--------|------------|-----------|-----------|
| PREMIUM | `price_per_sqft` | DESC | Highest ceiling comp first |
| HIGH | `relevance_score` | DESC | Most relevant ARV anchors up |
| MID | `price_per_sqft` | DESC | Mid-range ceiling first |
| LOW | `sale_date` | DESC | Most recent floor comps up |

### 5.3 Tertiary Sort (Tie-Breaker)

`distance_to_piq ASC` — Closer comps rank higher within same $/sqft

---

## 6. FUNCTIONAL REQUIREMENTS

### 6.1 Bucket Classification

- **FR1:** System SHALL classify each comp into exactly ONE bucket (PREMIUM/HIGH/MID/LOW)
- **FR2:** System SHALL apply classification decision sequence (LOW→PREMIUM→HIGH→MID)
- **FR3:** System SHALL prevent duplicate bucket assignments (first match wins)
- **FR4:** System SHALL compare each comp against PIQ using the comparison matrix

### 6.2 Value Ceiling

- **FR5:** System SHALL identify the specific comp that defines the value ceiling
- **FR6:** System SHALL explain ceiling using unchangeable features, NOT percentages
- **FR7:** System SHALL display ceiling comp with visual indicator (star + highlight)
- **FR8:** System SHALL draw value ceiling line directly below ceiling comp

### 6.3 Chip Generation

- **FR9:** System SHALL auto-generate chips from MLS data + image analysis
- **FR10:** System SHALL display maximum 6 chips per comp row with overflow indicator
- **FR11:** System SHALL provide hover explanation for each chip

### 6.4 User Interface

- **FR12:** System SHALL display comps in collapsible bucket sections
- **FR13:** System SHALL provide hover AI explanation for every comp
- **FR14:** System SHALL trigger classification only on iQ button click (not auto-load)

---

## 7. NON-FUNCTIONAL REQUIREMENTS

### 7.1 Performance

- **NFR1:** Bucket classification SHALL complete within 2 seconds for up to 50 comps
- **NFR2:** Chip hover tooltips SHALL render within 100ms

### 7.2 Reliability

- **NFR3:** System SHALL handle missing MLS fields gracefully (no crashes)
- **NFR4:** System SHALL log all data conflicts to Column D

### 7.3 Usability

- **NFR5:** Color contrast SHALL meet WCAG AA standards
- **NFR6:** Hover explanations SHALL be readable without scrolling

---

## 8. ACCEPTANCE CRITERIA

- **AC1:** Every comp is classified into exactly ONE bucket
- **AC2:** Ceiling comp is identified by name, not by percentage calculation
- **AC3:** Ceiling explanation lists specific unchangeable features
- **AC4:** Every comp has at least one chip assigned
- **AC5:** Chips use the 18 official categories only
- **AC6:** Value Ceiling line appears between PREMIUM and HIGH sections
- **AC7:** Hover explanation appears for every comp with classification reasoning
- **AC8:** Bucket sections are collapsible
- **AC9:** Sort order matches specification (bucket → $/sqft or relevance → distance)
- **AC10:** System does NOT make ARV decisions — user retains full control
- **AC11:** No advisory language in any explanation
- **AC12:** Feature does NOT appear until iQ button is clicked

---

## 9. EDGE CASES & ERROR HANDLING

| Scenario | Behavior |
|----------|----------|
| < 3 comps total | Show warning: "Insufficient comps for reliable classification" |
| 0 PREMIUM comps | Ceiling = best HIGH comp. Explanation: "No comps with unchangeables found" |
| 0 HIGH comps | Show alert: "No achievable comps found — expand search" |
| All comps = LOW | Show alert: "All comps have significant negatives — review market" |
| PIQ data incomplete | Flag affected comparisons with ⚠️ "Check Notes" chip |
| Image confidence < 60% | Add "Low Photo Confidence" chip |
| Multiple ceiling candidates | Select highest $/sqft. List others in hover explanation. |

---

## 10. GLOSSARY

| Term | Definition |
|------|------------|
| **PIQ** | Property In Question — the subject property being valued |
| **ARV** | After Repair Value — estimated value post-renovation |
| **Unchangeable** | Feature that cannot be added/modified through rehab (ADU, view, location) |
| **Ceiling Comp** | The specific comp that defines the maximum value PIQ can achieve |
| **Micro-market** | Specific tract/school feeder/neighborhood boundary |
| **GLA** | Gross Living Area — heated/cooled square footage |
| **C-OVERLAY** | Upstream bot that ranks comps by relevance before List processes them |

---

## 11. DEVELOPMENT PLAN

> **Note:** Timelines reflect BMAD Method + Claude Code (AI-assisted development)

### Epic 1: Bucket Classification Engine — 3 hrs

| Task | Hours |
|------|-------|
| 1.1 PIQ comparison matrix logic | 0.75 |
| 1.2 Unchangeable feature detection | 0.5 |
| 1.3 Classification decision sequence | 0.75 |
| 1.4 Bucket assignment (PREMIUM/HIGH/MID/LOW) | 0.5 |
| 1.5 Ceiling comp identification | 0.5 |

### Epic 2: Chip Generation System — 2 hrs

| Task | Hours |
|------|-------|
| 2.1 18 chip category mapping | 0.75 |
| 2.2 MLS data extraction | 0.5 |
| 2.3 Chip display logic (max 6 + overflow) | 0.5 |
| 2.4 Hover explanation generator | 0.25 |

### Epic 3: UI Components — 2 hrs

| Task | Hours |
|------|-------|
| 3.1 Bucketed list view | 0.5 |
| 3.2 Value ceiling line + star indicator | 0.5 |
| 3.3 Collapsible sections | 0.5 |
| 3.4 Sort order implementation | 0.5 |

### Epic 4: Testing & Edge Cases — 1 hr

| Task | Hours |
|------|-------|
| 4.1 Edge case handling (< 3 comps, no PREMIUM, etc.) | 0.5 |
| 4.2 Acceptance criteria validation | 0.5 |

### Timeline Summary

| Phase | Hours | Deliverable |
|-------|-------|-------------|
| Epic 1: Classification Engine | 3 hrs | Bucket logic complete |
| Epic 2: Chip Generation | 2 hrs | 18 chips working |
| Epic 3: UI Components | 2 hrs | List view live |
| Epic 4: Testing | 1 hr | All AC pass |
| **TOTAL** | **8 hrs (~1 day)** | **Bot live in List view** |

---

## HANDOFF SUMMARY

### For Eric (PM)

| Item | Detail |
|------|--------|
| Problem | Flat comp list lacks ceiling identification |
| Success | Ceiling comp identified by name + reason |
| Key Insight | Ceiling = COMP, not percentage |
| Priority | P1 Core Feature |
| QA | 12 acceptance criteria defined |
| **Timeline** | **8 hrs (~1 day) with BMAD + Claude Code** |

### For Nate (CTO)

| Item | Detail |
|------|--------|
| Frontend | React — see GitHub repo |
| APIs | MLS, PropertyRadar, Google, C-OVERLAY |
| Data | Shared schema in Section 0 |
| Chips | 18 official categories in Section 4 |
| Sort Logic | Section 5 (bucket → $/sqft → distance) |
| **Timeline** | **8 hrs (~1 day) with BMAD + Claude Code** |

---

**Document prepared by FlipIQ Technical Team**
**Version 2.0 (Hardened) — December 31, 2024**
