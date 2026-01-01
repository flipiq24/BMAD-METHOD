# Investment Analysis Bot — User Stories
## Version 2.0 — BMAD iQ
### December 31, 2024

---

## Document Overview

| Field | Value |
|-------|-------|
| **Total Epics** | 7 |
| **Total User Stories** | 38 |
| **Primary User** | Acquisition Associate (AA) |
| **Secondary Users** | Acquisition Manager (AM), Operator |
| **Dependencies** | OMS, data.flipiq.com, PIQ, Command |

---

## Epic Overview

| # | Epic | Stories | Focus |
|---|------|---------|-------|
| 1 | Buy Box Builder (OMS) | 9 | 9-step intake form for operators |
| 2 | Market Data Integration | 4 | API, caching, city normalization |
| 3 | Verdict Engine | 7 | 5 verdict types, evaluation order |
| 4 | Fit Analysis | 4 | 6-category evaluation table |
| 5 | Agent Question Logic | 5 | MLS status-aware questions |
| 6 | UI/UX & Display | 5 | iQ button, panels, output format |
| 7 | Governance & Audit | 4 | Logging, permissions, overrides |

---

## Epic 1: Buy Box Builder (OMS)

**Goal:** Enable operators to define acquisition criteria through a 9-step intake form with waterfall inheritance.

---

### US-1.1: Baseline ROI & Buy Price Setup

**As an** Operator
**I want to** set my baseline ROI target and minimum profit thresholds
**So that** all calculations use my financial requirements

**Acceptance Criteria:**
- [ ] Can enter Standard Cash-on-Cash ROI Target (%)
- [ ] All-In Buy Price auto-calculates from ROI
- [ ] Can enter Min Profit per Deal ($)
- [ ] Can enter Min Wholesale Fee ($)
- [ ] Form validates numeric inputs
- [ ] Values persist on save

**ROI → All-In % Reference:**
```
10% ROI → 85% All-In
12% ROI → 83% All-In
15% ROI → 80% All-In
18% ROI → 77% All-In
20% ROI → 75% All-In
25% ROI → 70% All-In
```

---

### US-1.2: Location & Price Ceiling Configuration

**As an** Operator
**I want to** select active counties and set % over FHA limits
**So that** max ARV and max All-In calculate automatically per county

**Acceptance Criteria:**
- [ ] Can select multiple counties via checkbox
- [ ] FHA limits pre-loaded for SoCal counties
- [ ] Can enter % Over FHA Limit
- [ ] Max ARV auto-calculates: FHA × (1 + % over)
- [ ] Max All-In auto-calculates: Max ARV × All-In %
- [ ] Displays calculated values in real-time

**Example Calculation:**
```
LA County, 20% over FHA, 12% ROI:
FHA Limit: $970,800
Max ARV = $970,800 × 1.20 = $1,164,960
Max All-In = $1,164,960 × 0.83 = $966,917
```

---

### US-1.3: Property Type Configuration

**As an** Operator
**I want to** set Will Do / Will Do At X% / Won't Do for each property type
**So that** the system applies correct ROI requirements

**Acceptance Criteria:**
- [ ] Displays all property types: SFR, Townhome, Condo, PUD, Duplex, Triplex, Fourplex, Mobile, Leased Land
- [ ] Three-tier selection per type: Will Do | Will Do At ___% | Won't Do
- [ ] Custom ROI % input enabled when "Will Do At" selected
- [ ] Mobile and Leased Land default to "Won't Do"
- [ ] Selection persists on save

**Data Structure:**
```typescript
interface PropertyTypeConfig {
  type_code: 'SFR' | 'TOWNHOME' | 'CONDO' | 'PUD' | 'DUPLEX' | 'TRIPLEX' | 'FOURPLEX' | 'MOBILE' | 'LEASED_LAND';
  status: 'WILL_DO' | 'WILL_DO_AT' | 'WONT_DO';
  required_roi?: number; // Required if WILL_DO_AT
}
```

---

### US-1.4: Year Built Configuration

**As an** Operator
**I want to** set ROI requirements based on year built ranges
**So that** older properties requiring more work have higher ROI thresholds

**Acceptance Criteria:**
- [ ] Displays year ranges: 1980+, 1965-1979, 1950-1964, 1930-1949, Pre-1930
- [ ] Three-tier selection per range: Will Do | Will Do At ___% | Won't Do
- [ ] Custom ROI % input enabled when "Will Do At" selected
- [ ] Pre-1930 defaults to "Won't Do"
- [ ] Selection persists on save

---

### US-1.5: Property Size Boundaries

**As an** Operator
**I want to** set min/max boundaries for property characteristics
**So that** the system filters properties by size requirements

**Acceptance Criteria:**
- [ ] Can set min/max Living Area (sqft)
- [ ] Can set min/max Lot Size (sqft)
- [ ] Can set min/max Bedrooms
- [ ] Can set min/max Bathrooms
- [ ] Validation: min cannot exceed max
- [ ] Empty values treated as "no limit"

---

### US-1.6: Heavy Rehab Work Configuration

**As an** Operator
**I want to** define tolerance for heavy rehab items
**So that** properties requiring major work are evaluated correctly

**Acceptance Criteria:**
- [ ] Displays items: Electrical, Plumbing, Foundation, Structural, Pool, Mold, Asbestos, Additions, ADU, Septic
- [ ] Three-tier selection: Will Do | Will Do At ___% | Won't Do
- [ ] Can set Max Rehab Budget ($ or % of ARV)
- [ ] Mold, Asbestos, Room Additions, ADU default to "Won't Do"
- [ ] Selection persists on save

---

### US-1.7: Deal Killers Configuration

**As an** Operator
**I want to** identify property attributes that kill or complicate deals
**So that** AAs know which properties to avoid or require higher ROI

**Acceptance Criteria:**
- [ ] Displays items: Busy street, Commercial adjacent, Freeway, Railroad, Airport, Flood zone, Fire zone, Tenants, Squatters, Unpermitted, Code violations, Title issues, Environmental
- [ ] Two-tier selection: Will Consider At ___% | Won't Do
- [ ] Flood zone, Fire zone, Squatters, Title issues, Environmental default to "Won't Do"
- [ ] Selection persists on save

---

### US-1.8: Conditional Deals Configuration

**As an** Operator
**I want to** set ROI requirements for properties with specific conditions
**So that** AAs understand how these factors affect deal viability

**Acceptance Criteria:**
- [ ] Displays conditions: 55+ community, 2bed/1bath, Septic system, Well water
- [ ] Two-tier selection: Will Do At ___% | Won't Do
- [ ] Custom ROI % input for each condition
- [ ] Selection persists on save

---

### US-1.9: Location Override Configuration

**As an** Operator
**I want to** override MASTER settings at County, City, or ZIP level
**So that** I can customize criteria for specific markets

**Acceptance Criteria:**
- [ ] Initial question: "Customize by location? Yes/No"
- [ ] If Yes, can select: By County | By City | By ZIP Code
- [ ] County-level: Override % over FHA, ROI, Min Year, Max Rehab, Property Types
- [ ] City-level: Include/Exclude, Override ROI, Drill to ZIP option
- [ ] ZIP-level: Include/Exclude, Override ROI
- [ ] Waterfall inheritance applies: MASTER → County → City → ZIP
- [ ] UI shows inheritance chain for clarity

**Inheritance Example:**
```
Property at ZIP 92262 (Palm Springs, Riverside):
1. Check ZIP 92262 overrides → If set, use ZIP values
2. Else check Palm Springs overrides → If set, use City values
3. Else check Riverside overrides → If set, use County values
4. Else use MASTER values
```

---

## Epic 2: Market Data Integration

**Goal:** Provide real-time market intelligence for property evaluation and comparison.

---

### US-2.1: Market Data API Endpoint

**As a** System
**I want to** expose a /markets endpoint with city-level data
**So that** the Investment Analysis Bot can fetch market intelligence

**Acceptance Criteria:**
- [ ] Endpoint: GET https://data.flipiq.com/markets?city={city}
- [ ] Returns 10 required fields (see data structure)
- [ ] Response time < 500ms
- [ ] Handles city name variations (case-insensitive)
- [ ] Returns 404 for unknown cities

**Data Structure:**
```typescript
interface MarketData {
  city: string;           // PALM SPRINGS
  county: string;         // RIVERSIDE
  market_share: number;   // 0.0104
  total_investors: number; // 3294
  total_agents: number;   // 1063
  units_per_investor: number; // 1.4
  units: number;          // 8674
  ptfv: number;           // 0.691
  pm_ratio: number;       // 0.77
  avg_flip_days: number;  // 518
  median_purchase: number; // 615000
}
```

---

### US-2.2: City Data Loading

**As a** System
**I want to** pre-load market data for 413 SoCal cities
**So that** API responses are fast and consistent

**Acceptance Criteria:**
- [ ] 413 cities loaded into database
- [ ] Data includes all 10 market metrics
- [ ] Data source documented
- [ ] Initial load script available
- [ ] Data refresh mechanism defined

---

### US-2.3: City Name Normalization

**As a** System
**I want to** normalize city name variations
**So that** lookups succeed regardless of formatting

**Acceptance Criteria:**
- [ ] Case-insensitive matching
- [ ] Handles common variations (e.g., "LA" → "LOS ANGELES")
- [ ] Strips extra spaces
- [ ] Returns standardized city name in response
- [ ] Logs unmatched queries for review

---

### US-2.4: Market Data Caching

**As a** System
**I want to** cache market data with 24-hour TTL
**So that** API performance is optimized and stale data is flagged

**Acceptance Criteria:**
- [ ] Cache TTL: 24 hours
- [ ] Cache invalidation on data refresh
- [ ] Fallback to cached data if source unavailable
- [ ] Display "Stale Data" indicator if cache > 24 hours
- [ ] Log cache hit/miss ratio

---

## Epic 3: Verdict Engine

**Goal:** Evaluate properties against Buy Box criteria and produce accurate verdicts.

---

### US-3.1: Hard Stop Detection

**As a** System
**I want to** immediately identify "Won't Do" hard stops
**So that** Pass verdict is returned without further evaluation

**Acceptance Criteria:**
- [ ] Checks Property Type against Won't Do list
- [ ] Checks Year Built against Won't Do list
- [ ] Checks Deal Killers against Won't Do list
- [ ] Checks Heavy Rehab against Won't Do list
- [ ] Returns Pass verdict with list of hard stops
- [ ] Stops evaluation after first hard stop found

**Output Example:**
```
Buy Box: ❌ Pass
✗ Mobile Home - WON'T DO in Buy Box (hard stop)
✗ Leased land - WON'T DO (hard stop)
```

---

### US-3.2: Ideal Flip Evaluation

**As a** System
**I want to** evaluate if property fits all Buy Box criteria at standard ROI
**So that** Ideal Flip verdict confirms all boxes checked

**Acceptance Criteria:**
- [ ] All fit categories return Match or Strong Match
- [ ] Property Type status = WILL_DO
- [ ] Year Built status = WILL_DO
- [ ] No Deal Killers triggered
- [ ] ARV under Max ARV ceiling
- [ ] All-In under Max All-In
- [ ] Rehab within budget
- [ ] Returns ✅ Ideal Flip verdict

---

### US-3.3: Marginal Flip Evaluation

**As a** System
**I want to** evaluate if property fits with conditional ROI adjustments
**So that** Marginal Flip verdict shows what requires higher ROI

**Acceptance Criteria:**
- [ ] At least one condition triggers WILL_DO_AT
- [ ] Applies highest required ROI from all triggers
- [ ] Re-calculates All-In % based on new ROI
- [ ] Lists all conditions requiring higher ROI
- [ ] Returns ⚠️ Marginal Flip verdict with conditions

**Output Example:**
```
Buy Box: ⚠️ Marginal Flip
✗ Condo requires 15% ROI (not standard 12%)
✗ 2/2 layout requires 15% ROI
```

---

### US-3.4: Good Wholesale Evaluation

**As a** System
**I want to** evaluate wholesale viability when flip fails
**So that** AAs can pivot to wholesale path

**Acceptance Criteria:**
- [ ] Triggers when flip evaluation fails
- [ ] Calculates 10% discount from list price
- [ ] Checks if discounted price meets wholesale threshold
- [ ] Verifies Min Wholesale Fee ($10K+) achievable
- [ ] Returns ✅ Good Wholesale verdict with discount calculation

**Output Example:**
```
Buy Box: ✅ Good Wholesale
✗ Triplex - requires 18% ROI, outside flip Buy Box
✓ 10% off list = $382K works for wholesale ($10K+ fee)
```

---

### US-3.5: Marginal Wholesale Evaluation

**As a** System
**I want to** identify properties close to wholesale threshold
**So that** AAs know negotiation needed

**Acceptance Criteria:**
- [ ] Property fails flip AND standard wholesale
- [ ] Calculates gap to reach wholesale threshold
- [ ] Shows additional discount needed
- [ ] Returns ⚠️ Marginal Wholesale verdict

**Output Example:**
```
Buy Box: ⚠️ Marginal Wholesale
✗ Currently at 7% off list - need 10% for standard wholesale
⚠️ Need additional 3% discount to hit wholesale threshold
```

---

### US-3.6: Pass Verdict Generation

**As a** System
**I want to** generate Pass verdict with clear reasons
**So that** AAs understand why property doesn't fit

**Acceptance Criteria:**
- [ ] Lists all hard stops with (hard stop) label
- [ ] Lists all negative factors with ✗
- [ ] Includes relevant warnings with ⚠️
- [ ] Displays: ❓ NO CALL NEEDED - Move to next property
- [ ] Does not block AA from making call if desired

---

### US-3.7: Verdict Evaluation Order

**As a** System
**I want to** evaluate verdicts in correct priority order
**So that** the most favorable applicable verdict is returned

**Acceptance Criteria:**
- [ ] Order: 1) Hard Stops → Pass
- [ ] Order: 2) Standard ROI fit → Ideal Flip
- [ ] Order: 3) Conditional ROI fit → Marginal Flip
- [ ] Order: 4) 10% wholesale → Good Wholesale
- [ ] Order: 5) Negotiated wholesale → Marginal Wholesale
- [ ] Order: 6) Otherwise → Pass
- [ ] Only one verdict returned per evaluation

---

## Epic 4: Fit Analysis

**Goal:** Provide detailed 6-category evaluation explaining why property does/doesn't fit.

---

### US-4.1: Location Fit Evaluation

**As an** AA
**I want to** see if property location is in my active Buy Box
**So that** I know if county/city/ZIP is approved

**Acceptance Criteria:**
- [ ] Displays county, city, ZIP
- [ ] Shows if county is active
- [ ] Shows if city/ZIP has overrides
- [ ] Rating: Match | Mismatch
- [ ] One-line explanation

---

### US-4.2: Price Range Fit Evaluation

**As an** AA
**I want to** see if ARV is within ceiling
**So that** I know if price works for my Buy Box

**Acceptance Criteria:**
- [ ] Displays property ARV
- [ ] Displays Max ARV ceiling for county
- [ ] Shows gap (over/under ceiling)
- [ ] Rating: High Match | Match | Borderline | Mismatch
- [ ] One-line explanation

---

### US-4.3: Property Type Fit Evaluation

**As an** AA
**I want to** see property type status and required ROI
**So that** I know the ROI requirement

**Acceptance Criteria:**
- [ ] Displays property type
- [ ] Shows status: Will Do | Will Do At X% | Won't Do
- [ ] Shows required ROI if conditional
- [ ] Rating: Strong Match | Match | Borderline | Mismatch
- [ ] One-line explanation

---

### US-4.4: Fit Analysis Table Generation

**As a** System
**I want to** generate 6-category fit analysis table
**So that** AAs see complete evaluation at a glance

**Acceptance Criteria:**
- [ ] Categories: Location, Price Range, Property Type, Rehab Level, Deal Killers, Conditional Vars
- [ ] Each row: Category | Rating | Why
- [ ] Ratings use emoji: 🟢 🟡 🔴
- [ ] One-line explanations are specific to property
- [ ] Table displays in consistent format

**Output Format:**
```
| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | Riverside County in your active list |
| Price Range | 🟢 High Match | ARV $580K under $675K ceiling |
| Property Type | 🟢 Strong Match | SFR at standard 12% ROI |
| Rehab Level | 🟢 Match | $45K cosmetic within budget |
| Deal Killers | 🟢 None | No flood/fire/title issues |
| Conditional Vars | 🟢 None | Not 55+, not 2/1, city sewer & water |
```

---

## Epic 5: Agent Question Logic

**Goal:** Generate context-aware questions based on MLS status and property situation.

---

### US-5.1: MLS Status Detection

**As a** System
**I want to** detect MLS status and DOM
**So that** agent question is tailored to property situation

**Acceptance Criteria:**
- [ ] Detects: NEW (0-7 DOM), ACTIVE (8-69), AGED (70+)
- [ ] Detects: PENDING, BACK ON MARKET, OFF MARKET
- [ ] Calculates days since status change
- [ ] Passes status to question generator

---

### US-5.2: New Listing Question Generation

**As an** AA
**I want to** receive a question for new listings (0-7 DOM)
**So that** I stand out from other callers

**Acceptance Criteria:**
- [ ] Recognizes agent is getting hammered
- [ ] Question focuses on standing out with data
- [ ] Positions AA as different from other investors
- [ ] Includes specific property data points

---

### US-5.3: Aged Listing Question Generation

**As an** AA
**I want to** receive a question for aged listings (70+ DOM)
**So that** I can ask about "the real number"

**Acceptance Criteria:**
- [ ] References DOM in question
- [ ] Asks about seller's real number
- [ ] Mentions cash buyer advantage
- [ ] Includes expiration if approaching

**Example:**
```
❓ ASK: "85 days, about to expire, tax default—what's the real
number to get this done? I can close in 10 days through First American."
```

---

### US-5.4: Pending Status Question Generation

**As an** AA
**I want to** receive follow-up questions for pending properties
**So that** I can position as backup buyer

**Acceptance Criteria:**
- [ ] Day 3: "Has your buyer wired their deposit yet?"
- [ ] Day 10: "Has your buyer removed all contingencies?"
- [ ] Day 20: "Cash buyers close in 7-14 days. Is your buyer real?"
- [ ] Question varies by days since pending status

---

### US-5.5: Wholesale Question Generation

**As an** AA
**I want to** receive a wholesale-positioned question
**So that** I can approach with investor buyer offer

**Acceptance Criteria:**
- [ ] Includes specific price point
- [ ] Mentions "bring you a buyer"
- [ ] Emphasizes speed ("this week")
- [ ] Tailored to property situation

**Example:**
```
❓ ASK: "At $380K I can bring you a buyer this week who closes
fast. Can your seller do that number?"
```

---

## Epic 6: UI/UX & Display

**Goal:** Deliver Investment Analysis output in the correct format and position.

---

### US-6.1: iQ Button Integration

**As an** AA
**I want to** click the iQ button and see Investment Analysis first
**So that** verdict is immediately visible

**Acceptance Criteria:**
- [ ] Investment Analysis appears ABOVE iQ Property Intelligence
- [ ] Panel loads in < 2 seconds
- [ ] Loading indicator shown during fetch
- [ ] Error state handled gracefully

---

### US-6.2: Verdict Summary Display

**As an** AA
**I want to** see verdict with visual indicators
**So that** I can quickly understand the decision

**Acceptance Criteria:**
- [ ] Verdict type displayed with emoji: ✅ ⚠️ ❌
- [ ] Positives listed with ✓
- [ ] Negatives listed with ✗
- [ ] Context listed with ⚠️
- [ ] Agent question displayed with ❓ ASK

---

### US-6.3: Market Intelligence Panel

**As an** AA
**I want to** see 10 market metrics in table format
**So that** I understand the local market context

**Acceptance Criteria:**
- [ ] Displays all 10 fields from Market API
- [ ] Table format with Field | Value columns
- [ ] PTFV comparison highlighted if above/below market
- [ ] Stale data indicator if cache > 24 hours

---

### US-6.4: Fit Analysis Panel

**As an** AA
**I want to** see 6-category fit analysis table
**So that** I understand why property does/doesn't fit

**Acceptance Criteria:**
- [ ] Displays all 6 categories
- [ ] Color-coded ratings (green/yellow/red)
- [ ] One-line explanations are visible
- [ ] Table is scannable in < 5 seconds

---

### US-6.5: Mobile Responsive Display

**As an** AA
**I want to** view Investment Analysis on mobile devices
**So that** I can evaluate properties on the go

**Acceptance Criteria:**
- [ ] Verdict visible without scrolling
- [ ] Tables stack vertically on mobile
- [ ] Touch targets are 44px minimum
- [ ] Agent question is copy-able

---

## Epic 7: Governance & Audit

**Goal:** Ensure proper access control, logging, and override management.

---

### US-7.1: Buy Box Access Control

**As a** System
**I want to** restrict Buy Box editing to Operators only
**So that** AAs cannot modify acquisition criteria

**Acceptance Criteria:**
- [ ] Operators can create/edit/delete Buy Box
- [ ] AMs can view Buy Box (read-only)
- [ ] AAs can view Buy Box (read-only)
- [ ] Changes require Operator authentication
- [ ] UI hides edit controls for non-Operators

---

### US-7.2: Verdict Override with Approval

**As an** AA
**I want to** override Pass verdict with AM approval
**So that** I can pursue deals with a valid reason

**Acceptance Criteria:**
- [ ] AA can request override
- [ ] Override requires reason text
- [ ] AM receives notification for approval
- [ ] Approved override logged with AM name
- [ ] Override reason attached to verdict

---

### US-7.3: Verdict Audit Logging

**As a** System
**I want to** log all verdicts immutably
**So that** audit trail exists for performance tracking

**Acceptance Criteria:**
- [ ] Log: property_id, verdict_type, timestamp, aa_id
- [ ] Log: fit_analysis results
- [ ] Log: market_data snapshot
- [ ] Log: override if applicable
- [ ] Logs cannot be deleted or modified

---

### US-7.4: Buy Box Change Logging

**As a** System
**I want to** log all Buy Box changes
**So that** we know who changed what, when

**Acceptance Criteria:**
- [ ] Log: field_changed, old_value, new_value
- [ ] Log: changed_by, timestamp
- [ ] Log: change_reason (optional)
- [ ] Changes visible in audit UI
- [ ] Logs immutable

---

## Data Schemas

### Buy Box Master Schema

```typescript
interface BuyBoxMaster {
  entity_id: string;
  baseline_roi: number;         // 0.12
  all_in_percent: number;       // 0.83 (auto-calc)
  min_flip_profit: number;      // 40000
  min_wholesale_fee: number;    // 10000
  pct_over_fha: number;         // 0.20
  max_rehab_budget: number;     // 150000
  max_rehab_pct_arv?: number;   // 0.25

  counties: CountyConfig[];
  property_types: PropertyTypeConfig[];
  year_built: YearBuiltConfig[];
  heavy_rehab: HeavyRehabConfig[];
  deal_killers: DealKillerConfig[];
  conditional_deals: ConditionalDealConfig[];

  created_at: Date;
  updated_at: Date;
  updated_by: string;
}

interface CountyConfig {
  county_name: string;          // "LOS ANGELES"
  fha_limit: number;            // 970800
  is_active: boolean;
  override_pct_over_fha?: number;
  override_roi?: number;
  max_arv: number;              // auto-calc
  max_all_in: number;           // auto-calc
  cities?: CityConfig[];
}

interface CityConfig {
  city_name: string;
  is_included: boolean;         // default true
  override_roi?: number;
  zips?: ZIPConfig[];
}

interface ZIPConfig {
  zip_code: string;
  is_included: boolean;         // default true
  override_roi?: number;
}
```

### Verdict Output Schema

```typescript
interface VerdictOutput {
  verdict_type: 'IDEAL_FLIP' | 'GOOD_WHOLESALE' | 'MARGINAL_FLIP' | 'MARGINAL_WHOLESALE' | 'PASS';
  verdict_emoji: '✅' | '⚠️' | '❌';
  positives: string[];          // Items with ✓
  negatives: string[];          // Items with ✗
  context: string[];            // Items with ⚠️
  agent_question: string;       // ❓ ASK text

  fit_analysis: FitAnalysis;
  market_data: MarketData;

  evaluation_timestamp: Date;
  evaluated_by: string;
  override?: OverrideInfo;
}

interface FitAnalysis {
  location: FitCategory;
  price_range: FitCategory;
  property_type: FitCategory;
  rehab_level: FitCategory;
  deal_killers: FitCategory;
  conditional_vars: FitCategory;
}

interface FitCategory {
  rating: 'Strong Match' | 'High Match' | 'Match' | 'Borderline' | 'Mismatch' | 'None';
  rating_emoji: '🟢' | '🟡' | '🔴';
  explanation: string;
}

interface OverrideInfo {
  requested_by: string;
  approved_by: string;
  reason: string;
  approved_at: Date;
}
```

---

## Story Dependencies

```
US-1.1 → US-1.2 (ROI needed for All-In calc)
US-1.2 → US-1.9 (Counties needed for location overrides)
US-2.1 → US-2.2 → US-2.3 → US-2.4 (API pipeline)
US-3.1 → US-3.7 (Hard stops first in evaluation)
US-5.1 → US-5.2, US-5.3, US-5.4, US-5.5 (Status needed for questions)
US-6.1 → US-6.2, US-6.3, US-6.4 (Button triggers panels)
US-7.1 → US-7.2, US-7.3, US-7.4 (Access control first)
```

---

## Acceptance Testing Checklist

- [ ] 5 verdict types display correctly for representative properties
- [ ] Buy Box 9-step form saves/loads all configurations
- [ ] ROI → All-In % auto-calculates correctly
- [ ] FHA limits → Max ARV → Max All-In chain works
- [ ] 3-tier status applies correct ROI requirements
- [ ] Location waterfall inheritance works at all levels
- [ ] Market data displays 10 fields from API
- [ ] PTFV comparison shows variance correctly
- [ ] Agent question varies by MLS status
- [ ] Fit Analysis shows 6 categories with ratings
- [ ] All verdicts logged for audit
- [ ] Response time < 2 seconds

---

**Document Version:** 2.0
**Last Updated:** December 31, 2024
**Status:** Ready for Development
