# Investment Analysis Bot PRD
## Version 8.0 — Ready for Development
### December 31, 2024

---

| Field | Value |
|-------|-------|
| **Bot ID** | IAMaster |
| **Category** | Investment |
| **Priority** | P0 — Critical Path |
| **Primary User** | Acquisition Associate (AA) |
| **UI Location** | PIQ → iQ Button → Investment Analysis Panel |
| **Trigger Method** | iQ Button Click |
| **Integration Points** | OMS (Buy Box), data.flipiq.com (Market API), Command/PIQ |
| **Risk Level** | Low (no changes to existing systems) |

---

## 1. Executive Summary

### 1.1 Purpose

The Investment Analysis Bot is an **OVERLAY** that adds investment decision support to the existing iQ Property Intelligence output. It evaluates properties against the operator's Buy Box criteria and provides market intelligence to help AAs make faster, data-driven decisions.

### 1.2 Core Value Proposition

Transform a 10-minute manual property evaluation into a 30-second automated verdict with actionable agent questions.

### 1.3 System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     COMMAND / PIQ                           │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  iQ Button Click                                     │   │
│  │  ┌─────────────────┐  ┌─────────────────────────┐   │   │
│  │  │ INVESTMENT      │  │ iQ PROPERTY             │   │   │
│  │  │ ANALYSIS (NEW)  │  │ INTELLIGENCE (EXISTING) │   │   │
│  │  │ - Buy Box Verdict│  │ - DFI Summary           │   │   │
│  │  │ - Market Intel   │  │ - Property/Seller       │   │   │
│  │  │ - Fit Analysis   │  │ - Agent Intel           │   │   │
│  │  │                  │  │ - Dynamic Script        │   │   │
│  │  └────────┬─────────┘  └─────────────────────────┘   │   │
│  └───────────┼──────────────────────────────────────────┘   │
└──────────────┼──────────────────────────────────────────────┘
               │
    ┌──────────┴──────────┐
    │                     │
┌───▼───┐          ┌──────▼──────┐
│  OMS  │          │ data.flipiq │
│Buy Box│          │  /markets   │
└───────┘          └─────────────┘
```

### 1.4 Output Order (Critical)

1. **Investment Analysis** (NEW) — Buy Box verdict + Market Intel + Fit Analysis
2. **iQ Property Intelligence** (EXISTING) — DFI, Property, Seller, Agent, Script

---

## 2. Success Criteria

### 2.1 User Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Time to evaluate property | < 30 seconds | Timestamp: iQ click → verdict |
| AA confidence in decisions | 90%+ | Weekly survey |
| Daily properties evaluated | 50+ per AA | Usage tracking |
| Agent question usage | 80%+ | Track if AA uses suggested question |

### 2.2 Business Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Deals closed per AA | 2/month | Transaction tracking |
| Offer-to-close ratio | 15%+ improvement | Before/after comparison |
| Time to first deal (new AA) | < 30 days | Onboarding milestone |
| Morning check-in time | < 10 minutes | Session tracking |

### 2.3 Technical Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Buy Box verdict accuracy | 95%+ | Random audit vs manual review |
| API response time | < 2 seconds | End-to-end latency |
| System uptime | 99.5%+ | Health check monitoring |
| Error rate | < 1% | Error logging |

---

## 3. Product Scope

### 3.1 MVP (Week 1)

| Feature | Description |
|---------|-------------|
| Buy Box Verdict | 5 verdict types with summary |
| Fit Analysis | 6-category evaluation table |
| Market Intelligence | 10 market metrics display |
| Agent Question | Context-aware question based on MLS status |

### 3.2 Growth (Weeks 2-4)

| Feature | Description |
|---------|-------------|
| PTFV Comparison | Property vs market PTFV variance |
| Wholesale Viability | Auto-calculate wholesale discount needed |
| Price Gap Analysis | Show $ amount over/under max purchase |
| Verdict History | Track verdicts per property over time |

### 3.3 Vision (Future)

| Feature | Description |
|---------|-------------|
| Multi-property comparison | Compare 3+ properties side-by-side |
| Market trend alerts | Notify when market PTFV shifts |
| Agent scoring integration | ISC displayed in verdict |
| Predicted days to close | ML model based on attributes |

---

## 4. User Journeys

### 4.1 Josh's First Property (New AA, Day 1)

> Josh opens his first property in PIQ. He's nervous—he doesn't know if this is a good deal. He clicks iQ.
>
> **Investment Analysis** appears first:
> - **Buy Box: ✅ Ideal Flip** — Josh immediately knows this fits
> - Summary shows ✓ SFR, ✓ Under ceiling, ✓ Agent ISC 8
> - ❓ ASK gives him the exact question to ask the agent
>
> Josh calls with confidence. He doesn't fumble.

**Requirements:** Verdict must be clear and immediate. Summary highlights WHY it fits. Agent question ready to read verbatim.

### 4.2 Maria's Morning Check-In (Experienced AA)

> Maria has 47 properties to review before 8am calls. She clicks iQ on each:
>
> - Property 1: ❌ Pass (Mobile Home) → Skip, 3 seconds
> - Property 2: ⚠️ Marginal Flip → Review fit analysis, 15 seconds
> - Property 3: ✅ Ideal Flip → Priority call, 5 seconds
>
> 47 properties reviewed in 12 minutes. Maria knows which 8 to call first.

**Requirements:** Pass verdicts instant. Marginal shows what's conditional. Ideal confirms all boxes checked.

### 4.3 David's Wholesale Pivot (Deal Rescue)

> David finds a triplex that doesn't fit flip Buy Box. Before, he'd skip it. Now:
>
> **Buy Box: ✅ Good Wholesale**
> - ✗ Triplex not in flip box
> - ✓ NOD filed, seller desperate
> - ✓ 10% off list = $382K works for wholesale
> - ❓ ASK: "At $380K I can bring you a buyer this week."
>
> David pivots to wholesale and makes $12K assignment fee.

**Requirements:** Wholesale path evaluated when flip fails. Discount threshold calculated. Agent question positions wholesale approach.

---

## 5. Buy Box Verdict Types

### 5.1 Verdict Definitions

| Verdict | When It Applies |
|---------|-----------------|
| ✅ **Ideal Flip** | Fits all Buy Box criteria at standard ROI |
| ✅ **Good Wholesale** | Out of Buy Box for flip BUT meets wholesale threshold |
| ⚠️ **Marginal Flip** | Fits only with conditional ROI adjustments |
| ⚠️ **Marginal Wholesale** | Close to wholesale threshold, needs negotiation |
| ❌ **Pass** | Does not fit Buy Box, no wholesale path |

### 5.2 Evaluation Order

1. Check for "Won't Do" hard stops → **Pass**
2. Check if fits standard ROI → **Ideal Flip**
3. Check if fits with conditional ROI → **Marginal Flip**
4. Check wholesale at 10% discount → **Good Wholesale**
5. Check wholesale with negotiation → **Marginal Wholesale**
6. Otherwise → **Pass**

### 5.3 Verdict Summary Format

Each verdict includes:
- **✓ Positives** — What fits, what's good
- **✗ Negatives** — What doesn't fit, what's concerning
- **⚠️ Context** — Important situational factors
- **❓ ASK** — The exact question to ask the agent

---

## 6. Verdict Output Examples

### ✅ Ideal Flip

```
Buy Box: ✅ Ideal Flip

✓ SFR in Riverside at standard 12% ROI
✓ ARV $580K under $675K ceiling
✓ $45K rehab within budget
✓ Agent ISC 8 - works with Pacific Coast, Mike Chen (11 deals)
✓ Your PTFV 73% vs Market 77% - 4 pts BELOW market
✓ NOD filed 45 days ago - seller motivated
✓ Vacant 3 months - no occupancy issues

⚠️ Active 45 DOM - not yet aged, seller may still be firm

❓ ASK: "Your seller has an NOD and the property's been vacant
3 months. What's the real number to get this closed in 10 days?"
```

### ⚠️ Marginal Flip

```
Buy Box: ⚠️ Marginal Flip

✓ Riverside County - active in your Buy Box
✓ Under $675K price ceiling

✗ Condo requires 15% ROI (not standard 12%)
✗ 2/2 layout requires 15% ROI
✗ Your PTFV 86.5% vs Market 69% - 17 pts OVER market
✗ Gap: $69K over max purchase price

⚠️ Aged 85 DOM - expires in 5 days
⚠️ Tax default $8,200 - seller bleeding
⚠️ Already dropped $50K (from $565K to $515K)

❓ ASK: "85 days, about to expire, tax default—what's the real
number to get this done? I can close in 10 days through First American."
```

### ✅ Good Wholesale

```
Buy Box: ✅ Good Wholesale

✗ Triplex - requires 18% ROI, outside flip Buy Box
✗ 1962 build - outside standard year range

✓ Agent motivated - 92 DOM, wants to close before expiration
✓ NOD filed 60 days ago - seller running out of time
✓ All 3 units vacant - no tenant issues
✓ 10% off list = $382K works for wholesale ($10K+ fee)

⚠️ Verify: Do you have multi-family buyer in network?

❓ ASK: "At $380K I can bring you a buyer this week who closes
fast. Can your seller do that number?"
```

### ⚠️ Marginal Wholesale

```
Buy Box: ⚠️ Marginal Wholesale

✗ Fourplex - not in flip Buy Box
✗ 1958 build - outside year range
✗ Currently at 7% off list - need 10% for standard wholesale

✓ Agent works with investors (ISC 5)
✓ Probate sale - estate needs to close

⚠️ Need additional 3% discount to hit wholesale threshold
⚠️ No buyer relationship = need full 10% margin

❓ ASK: "This is a probate—what's the estate's timeline?
I need to be at $X to make this work for my buyer."
```

### ❌ Pass

```
Buy Box: ❌ Pass

✗ Mobile Home - WON'T DO in Buy Box (hard stop)
✗ 1972 build - outside year range
✗ Leased land - WON'T DO (hard stop)

⚠️ Retail agent (ISC 1) - doesn't work with investors
⚠️ No wholesale path - no mobile home buyers in network

❓ NO CALL NEEDED - Move to next property
```

---

## 7. Fit Analysis Output

| Category | Rating | Why |
|----------|--------|-----|
| Location | Match | Riverside County in your active list |
| Price Range | High Match | ARV $580K under $675K ceiling |
| Property Type | Strong Match | SFR at standard 12% ROI |
| Rehab Level | Match | $45K cosmetic within budget |
| Deal Killers | None | No flood/fire/title issues |
| Conditional Vars | None | Not 55+, not 2/1, city sewer & water |

**Rating Scale:**
- 🟢 **Strong Match** / **High Match** / **Match** / **None** = Good
- 🟡 **Borderline** = Conditional, may require higher ROI
- 🔴 **Mismatch** = Does not fit, may block deal

---

## 8. Local Market Intelligence

| Field | Value |
|-------|-------|
| City - County | RIVERSIDE - RIVERSIDE |
| Market Share | 2.1% |
| Total Investors | 6,196 |
| Total Agents | 2,847 |
| Units/Investor | 1.4 |
| Units | 8,674 |
| Price to Future Value | 77% |
| P/M Ratio | 0.77 |
| Purchase/Resale (days) | 478 |
| Median Purchase Price | $455,000 |

---

## 9. Buy Box Builder — OMS Intake Form

**Location:** OMS.flipiq.com/buy-box-builder
**Purpose:** 9-step intake form for operators to define acquisition criteria.
**Key Principle:** Set once at MASTER level, inherit everywhere, override only where different.

### Step 1: Baseline ROI & Buy Price

**Purpose:** Define the financial foundation that drives all calculations.

| Field | Input Type | Example | Notes |
|-------|------------|---------|-------|
| Standard Cash-on-Cash ROI Target (%) | Number | 12% | Baseline return requirement |
| All-In Buy Price (% of ARV) | Auto-calc | 83% | AUTO: Derived from ROI |
| Min Profit per Deal ($) | Currency | $40,000 | Minimum flip profit |
| Min Wholesale Fee ($) | Currency | $10,000 | Minimum assignment fee |

**ROI → All-In % Reference Table:**

| ROI | All-In % | ROI | All-In % | ROI | All-In % |
|-----|----------|-----|----------|-----|----------|
| 10% | 85% | 15% | 80% | 20% | 75% |
| 12% | 83% | 18% | 77% | 25% | 70% |

---

### Step 2: Location & Price Ceiling

**Purpose:** Define geographic boundaries and maximum ARV based on FHA limits.

**FHA Loan Limits (2024 - SoCal Counties):**

| County | FHA Limit | County | FHA Limit |
|--------|-----------|--------|-----------|
| Los Angeles | $970,800 | Riverside | $562,350 |
| Orange | $970,800 | San Bernardino | $562,350 |
| San Diego | $879,750 | Ventura | $851,000 |

| Field | Input Type | Example | Notes |
|-------|------------|---------|-------|
| % Over FHA Limit | Number | 20% | How much above FHA you'll go |
| Counties Active | Multi-checkbox | LA, OC, Riverside... | Select all counties |
| Max ARV per County | Auto-calc | $1,164,960 (LA) | AUTO: FHA × (1 + % over) |
| Max All-In per County | Auto-calc | $966,917 (LA) | AUTO: Max ARV × All-In % |

**Example Calculation (LA County, 20% over FHA, 12% ROI):**
```
FHA Limit: $970,800
Max ARV = $970,800 × 1.20 = $1,164,960
Max All-In = $1,164,960 × 0.83 = $966,917
```

---

### Step 3: Property Types

**Purpose:** Define which property types you will/won't buy, with optional higher ROI.

**Three-Tier Flexibility:** ✅ Will Do | ⚠️ Will Do At X% | ❌ Won't Do

| Property Type | Will Do | Will Do At ___% | Won't Do |
|---------------|---------|-----------------|----------|
| Single Family Residence (SFR) | ☑ | - | ☐ |
| Townhome | ☑ | - | ☐ |
| Condominium | ☐ | 15% | ☐ |
| PUD (Planned Unit Dev) | ☑ | - | ☐ |
| Duplex | ☐ | 15% | ☐ |
| Triplex | ☐ | 18% | ☐ |
| Fourplex | ☐ | 18% | ☐ |
| Mobile/Manufactured | ☐ | - | ☑ |
| Leased Land | ☐ | - | ☑ |

---

### Step 4: Year Built

**Purpose:** Older homes often need electrical/plumbing work. Set age-based ROI.

| Year Built Range | Will Do | Will Do At ___% | Won't Do |
|------------------|---------|-----------------|----------|
| 1980 and newer | ☑ Standard | - | ☐ |
| 1965 - 1979 | ☐ | 15% | ☐ |
| 1950 - 1964 | ☐ | 18% | ☐ |
| 1930 - 1949 | ☐ | 20% | ☐ |
| Pre-1930 | ☐ | - | ☑ |

---

### Step 5: Property Size

**Purpose:** Set min/max boundaries for property characteristics.

| Attribute | Minimum | Maximum |
|-----------|---------|---------|
| Living Area (sqft) | ___ sqft | ___ sqft |
| Lot Size (sqft) | ___ sqft | ___ sqft |
| Bedrooms | ___ | ___ |
| Bathrooms | ___ | ___ |

---

### Step 6: Heavy Rehab Work

**Purpose:** Standard rehab assumed (paint, carpet, flooring, kitchen/bath cosmetic, windows, roof, HVAC, landscaping). Define tolerance for heavy work.

| Heavy Rehab Item | Will Do | Will Do At ___% | Won't Do |
|------------------|---------|-----------------|----------|
| Electrical rewire/panel upgrade | ☐ | 18% | ☐ |
| Plumbing repipe | ☐ | 18% | ☐ |
| Foundation repair | ☐ | 20% | ☐ |
| Structural/load-bearing walls | ☐ | 20% | ☐ |
| Pool resurface | ☐ | 15% | ☐ |
| Mold remediation | ☐ | - | ☑ |
| Asbestos abatement | ☐ | - | ☑ |
| Room additions | ☐ | - | ☑ |
| ADU construction | ☐ | - | ☑ |
| Septic system work | ☐ | 18% | ☐ |

| Field | Input |
|-------|-------|
| Max Rehab Budget ($) | $___ or ___% of ARV |

---

### Step 7: Deal Killers

**Purpose:** Items that make a property harder to sell or more risky.

| Deal Killer | Will Consider At ___% | Won't Do |
|-------------|----------------------|----------|
| Busy street / arterial road | 15% | ☐ |
| Next to commercial property | 15% | ☐ |
| Next to freeway | 18% | ☐ |
| Next to railroad | 18% | ☐ |
| Near airport / flight path | 15% | ☐ |
| Flood zone (FEMA) | - | ☑ |
| Fire zone (high risk) | - | ☑ |
| Occupied with tenants | 15% | ☐ |
| Squatters present | - | ☑ |
| Unpermitted additions | 18% | ☐ |
| Active code violations | 18% | ☐ |
| Title issues / liens | - | ☑ |
| Environmental contamination | - | ☑ |

---

### Step 8: Conditional Deals

**Purpose:** Properties with conditions that affect marketability.

| Condition | Will Do At ___% | Won't Do |
|-----------|-----------------|----------|
| Senior community (55+) | 15% | ☐ |
| 2 bedroom / 1 bathroom | 15% | ☐ |
| Septic system (no sewer) | 15% | ☐ |
| Well water (no city water) | 18% | ☐ |

---

### Step 9: Customize by Location

**Purpose:** Override MASTER settings for specific counties, cities, or ZIP codes.

**Waterfall Inheritance:** MASTER → County → City → ZIP

**Initial Question:**
```
Do you want to customize by location?
○ No (use MASTER for all)
○ Yes
```

**If Yes — Customize By:**
```
○ By County  ○ By City  ○ By ZIP Code
```

**County-Level Overrides:**

| Override Field | Input Type | Notes |
|----------------|------------|-------|
| % Over FHA Limit | Number | Override MASTER |
| Standard ROI Target | Number | Override MASTER |
| All-In % | Auto-calc | AUTO from ROI |
| Min Year Built | Dropdown | Override MASTER |
| Max Rehab Budget | Currency | Override MASTER |
| Property Types | Multi-checkbox | Override MASTER |

**City-Level Overrides:**

| Override Field | Input Type | Notes |
|----------------|------------|-------|
| Include / Exclude City | Radio | INCLUDE by default |
| Standard ROI Target | Number | Override County/MASTER |
| All-In % | Auto-calc | AUTO from ROI |
| Drill to ZIP? | Yes/No | Further customization |

**ZIP-Level Overrides:**

| Override Field | Input Type | Notes |
|----------------|------------|-------|
| Include / Exclude ZIP | Radio | INCLUDE by default |
| Standard ROI Target | Number | Override City/County/MASTER |
| All-In % | Auto-calc | AUTO from ROI |

**Inheritance Logic:**

When evaluating a property at ZIP 92262 (Palm Springs, Riverside County):
1. Check ZIP 92262 for overrides → If set, use ZIP values
2. Else check Palm Springs (City) for overrides → If set, use City values
3. Else check Riverside (County) for overrides → If set, use County values
4. Else use MASTER values

---

## 10. Buy Box Data Schema

### 10.1 Master Settings Object

| Field Name | Type | Example | Required |
|------------|------|---------|----------|
| entity_id | string | "ENT_12345" | Yes |
| baseline_roi | decimal | 0.12 | Yes |
| all_in_percent | decimal | 0.83 | Yes (calc) |
| min_flip_profit | integer | 40000 | Yes |
| min_wholesale_fee | integer | 10000 | Yes |
| pct_over_fha | decimal | 0.20 | Yes |
| max_rehab_budget | integer | 150000 | Yes |
| max_rehab_pct_arv | decimal | 0.25 | Optional |

### 10.2 Counties Array

| Field Name | Type | Example | Required |
|------------|------|---------|----------|
| county_name | string | "LOS ANGELES" | Yes |
| fha_limit | integer | 970800 | Yes |
| is_active | boolean | true | Yes |
| override_pct_over_fha | decimal | null | Optional |
| override_roi | decimal | null | Optional |
| max_arv | integer | 1164960 | Yes (calc) |
| max_all_in | integer | 966917 | Yes (calc) |

### 10.3 Property Types Array

| Field Name | Type | Example | Required |
|------------|------|---------|----------|
| type_code | string | "SFR" | Yes |
| status | enum | "WILL_DO" | Yes |
| required_roi | decimal | 0.12 | If WILL_DO_AT |

**Status enum:** `WILL_DO` | `WILL_DO_AT` | `WONT_DO`

### 10.4 Year Built Array

| Field Name | Type | Example | Required |
|------------|------|---------|----------|
| range_code | string | "1980_PLUS" | Yes |
| min_year | integer | 1980 | Yes |
| max_year | integer | null | null = present |
| status | enum | "WILL_DO" | Yes |
| required_roi | decimal | 0.12 | If WILL_DO_AT |

### 10.5 Heavy Rehab Array

| Field Name | Type | Example | Required |
|------------|------|---------|----------|
| item_code | string | "ELECTRICAL" | Yes |
| status | enum | "WILL_DO_AT" | Yes |
| required_roi | decimal | 0.18 | If applicable |

### 10.6 Deal Killers Array

| Field Name | Type | Example | Required |
|------------|------|---------|----------|
| killer_code | string | "BUSY_STREET" | Yes |
| status | enum | "WILL_CONSIDER_AT" | Yes |
| required_roi | decimal | 0.15 | If applicable |

---

## 11. MLS Status Logic

| Status | DOM | Strategy |
|--------|-----|----------|
| NEW | 0-7 | Agent hammered. Stand out with data. |
| ACTIVE | 8-69 | Seller getting realistic. Standard approach. |
| AGED | 70+ | Motivated seller. Ask: What's the real number? |
| PENDING | Δ | Follow-up Day 3, 10, 20. Contact listing agent. |
| BACK ON MKT | Δ | Deal fell. Ask why. Position as backup. |
| OFF MARKET | N/A | Price based on ROI, not asking. |

### Pending/BOM Follow-Up

| Day | Question |
|-----|----------|
| 3 | "Has your buyer wired their deposit yet?" |
| 10 | "Has your buyer removed all contingencies?" |
| 20 | "Cash buyers close in 7-14 days. Is your buyer real?" |

### Wholesale Rules

| Scenario | Min Discount |
|----------|--------------|
| No buyer relationship | 10% off list |
| Have buyer relationship | 5% off list |

---

## 12. Market Data API

**Endpoint:** `GET https://data.flipiq.com/markets?city={city}`

| Field | Type | Example |
|-------|------|---------|
| city | string | PALM SPRINGS |
| county | string | RIVERSIDE |
| market_share | decimal | 0.0104 |
| total_investors | int | 3294 |
| total_agents | int | 1063 |
| ptfv | decimal | 0.691 |
| avg_flip_days | int | 518 |
| median_purchase | int | 615000 |

---

## 13. Functional Requirements

### 13.1 Buy Box Evaluation

| ID | Requirement |
|----|-------------|
| FR1 | AA can view Buy Box verdict when clicking iQ button |
| FR2 | System can calculate verdict based on 6 fit categories |
| FR3 | System can apply conditional ROI adjustments |
| FR4 | System can detect "Won't Do" hard stops |
| FR5 | System can identify wholesale viability when flip fails |

### 13.2 Verdict Output

| ID | Requirement |
|----|-------------|
| FR6 | System can generate verdict summary with positives (✓) |
| FR7 | System can generate verdict summary with negatives (✗) |
| FR8 | System can generate context-aware warnings (⚠️) |
| FR9 | System can generate context-aware agent question (❓ ASK) |
| FR10 | AA can view fit analysis with one-line explanations |

### 13.3 Market Intelligence

| ID | Requirement |
|----|-------------|
| FR11 | System can fetch market data by city |
| FR12 | AA can view 10 market metrics in table format |
| FR13 | System can compare property PTFV to market PTFV |
| FR14 | System can display PTFV variance |

### 13.4 Buy Box Builder (OMS)

| ID | Requirement |
|----|-------------|
| FR15 | Operator can create Buy Box through 9-step form |
| FR16 | System can auto-calculate All-In % from ROI |
| FR17 | System can auto-calculate Max ARV from FHA limits |
| FR18 | System can auto-calculate Max All-In from Max ARV |
| FR19 | Operator can set 3-tier status per item |
| FR20 | Operator can override MASTER at County level |
| FR21 | Operator can override County at City level |
| FR22 | Operator can override City at ZIP level |
| FR23 | System can apply waterfall inheritance |

### 13.5 MLS Status Logic

| ID | Requirement |
|----|-------------|
| FR24 | System can detect MLS status |
| FR25 | System can adjust agent question based on MLS status |
| FR26 | System can flag DOM milestones (0-7, 8-69, 70+) |
| FR27 | System can recommend follow-up cadence |

---

## 14. Non-Functional Requirements

### 14.1 Performance

| ID | Requirement |
|----|-------------|
| NFR1 | iQ button response < 2 seconds |
| NFR2 | Market API response < 500ms |
| NFR3 | Buy Box evaluation < 200ms |
| NFR4 | Support 100 concurrent AA users |

### 14.2 Availability

| ID | Requirement |
|----|-------------|
| NFR5 | 99.5% uptime during business hours |
| NFR6 | Graceful degradation if Market API unavailable |
| NFR7 | Cached Buy Box available if OMS unreachable |

### 14.3 Security

| ID | Requirement |
|----|-------------|
| NFR8 | Buy Box data isolated per Entity/Operator |
| NFR9 | API authentication required |
| NFR10 | Audit logs immutable |

---

## 15. Governance Rules

1. **Buy Box is operator-controlled** — AAs cannot modify
2. **Verdict is advisory** — AAs can override with AM approval
3. **All verdicts logged** — For audit and performance tracking
4. **Market data refreshed daily** — Stale data flagged
5. **Pass verdict doesn't block** — AA can call if they have a reason
6. **Changes logged** — Who changed what, when

---

## 16. Development Plan

> **Note:** Timelines reflect BMAD Method + Claude Code (AI-assisted development)

### Epic 1: Buy Box Builder (OMS) — 6 hrs (1 day)

| Task | Hours |
|------|-------|
| 1.1 9-step intake form UI | 1.5 |
| 1.2 ROI/All-In auto-calc | 0.5 |
| 1.3 FHA lookup + Max ARV/All-In | 0.5 |
| 1.4 Property Type & Year matrices | 0.5 |
| 1.5 Size, Rehab, Killers, Conditional | 0.5 |
| 1.6 Location overrides (waterfall) | 1 |
| 1.7 Backend API: Save/Load | 0.75 |
| 1.8 Database schema | 0.5 |
| 1.9 Validation + errors | 0.25 |

### Epic 2: Market Data API — 2 hrs (0.25 day)

| Task | Hours |
|------|-------|
| 2.1 /markets endpoint | 0.5 |
| 2.2 Load 413 cities | 0.5 |
| 2.3 City normalization | 0.5 |
| 2.4 24-hour cache | 0.25 |
| 2.5 Fallback handling | 0.25 |

### Epic 3: Investment Analysis Bot — 4 hrs (0.5 day)

| Task | Hours |
|------|-------|
| 3.1 iQ button + panel UI | 0.5 |
| 3.2 Fetch PIQ data | 0.25 |
| 3.3 Fetch Buy Box | 0.25 |
| 3.4 Fetch Market Data | 0.25 |
| 3.5 Buy Box comparison engine | 1 |
| 3.6 Verdict + summary generation | 0.75 |
| 3.7 Agent question logic | 0.5 |
| 3.8 Fit analysis output | 0.25 |
| 3.9 Error handling | 0.25 |

### Timeline Summary

| Phase | Duration | Deliverable |
|-------|----------|-------------|
| Epic 1: Buy Box Builder | 1 day | OMS form live |
| Epic 2: Market API | 0.25 day | API live |
| Epic 3: Investment Bot | 0.5 day | Bot live in Command |
| Integration Testing | 0.25 day | E2E verified |
| **TOTAL** | **2 days** | **Full system deployed** |

---

## 17. Acceptance Criteria

- [ ] AC1: 5 verdict types display correctly
- [ ] AC2: Buy Box 9-step form saves/loads
- [ ] AC3: ROI → All-In % auto-calculates
- [ ] AC4: FHA limits → Max ARV → Max All-In chain works
- [ ] AC5: 3-tier status (Will Do / At X% / Won't Do) applies correctly
- [ ] AC6: Location waterfall inheritance works (MASTER → County → City → ZIP)
- [ ] AC7: Market data displays 10 fields
- [ ] AC8: PTFV comparison shows variance
- [ ] AC9: Agent question varies by MLS status
- [ ] AC10: Fit Analysis shows 6 categories with ratings
- [ ] AC11: Verdict logged for audit
- [ ] AC12: < 2 second response time

---

**Document prepared for FlipIQ Engineering**
**Version 8.0 — December 31, 2024**
