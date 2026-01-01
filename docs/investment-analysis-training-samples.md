# Investment Analysis Bot — Training Samples
## Version 2.0 — BMAD iQ
### December 31, 2024

---

## Document Overview

| Field | Value |
|-------|-------|
| **Total Samples** | 15 |
| **Verdict Types Covered** | 5 |
| **Property Types** | SFR, Condo, Townhome, Duplex, Triplex, Fourplex, Mobile |
| **MLS Statuses** | NEW, ACTIVE, AGED, PENDING, BACK ON MARKET |
| **Markets** | Riverside, Los Angeles, San Bernardino, Orange County |

---

## Sample Organization

| # | Verdict | Scenario |
|---|---------|----------|
| 1 | ✅ Ideal Flip | Clean SFR, standard ROI, motivated seller |
| 2 | ✅ Ideal Flip | Townhome with agent relationship |
| 3 | ✅ Ideal Flip | New listing, strong fundamentals |
| 4 | ⚠️ Marginal Flip | Condo requiring higher ROI |
| 5 | ⚠️ Marginal Flip | Older home with multiple conditions |
| 6 | ⚠️ Marginal Flip | Property with deal killer conditions |
| 7 | ✅ Good Wholesale | Triplex outside flip box |
| 8 | ✅ Good Wholesale | Multi-family with tenant issues |
| 9 | ⚠️ Marginal Wholesale | Close to threshold, needs negotiation |
| 10 | ⚠️ Marginal Wholesale | No buyer relationship |
| 11 | ❌ Pass | Mobile home hard stop |
| 12 | ❌ Pass | Multiple hard stops |
| 13 | ❌ Pass | Price ceiling breach |
| 14 | ❌ Pass | No wholesale path |
| 15 | Mixed | Back on Market opportunity |

---

## Buy Box Reference (Operator: Pacific Residential)

**MASTER Settings:**
```
Baseline ROI: 12%
All-In %: 83%
Min Flip Profit: $40,000
Min Wholesale Fee: $10,000
% Over FHA: 20%
Max Rehab Budget: $150,000
```

**Active Counties:**
| County | FHA Limit | Max ARV | Max All-In |
|--------|-----------|---------|------------|
| Los Angeles | $970,800 | $1,164,960 | $966,917 |
| Riverside | $562,350 | $674,820 | $560,100 |
| San Bernardino | $562,350 | $674,820 | $560,100 |
| Orange | $970,800 | $1,164,960 | $966,917 |

**Property Types:**
| Type | Status | ROI |
|------|--------|-----|
| SFR | WILL_DO | 12% |
| Townhome | WILL_DO | 12% |
| Condo | WILL_DO_AT | 15% |
| PUD | WILL_DO | 12% |
| Duplex | WILL_DO_AT | 15% |
| Triplex | WILL_DO_AT | 18% |
| Fourplex | WILL_DO_AT | 18% |
| Mobile | WONT_DO | — |
| Leased Land | WONT_DO | — |

---

## Sample 1: Clean SFR — Ideal Flip

### Property Input

| Field | Value |
|-------|-------|
| Address | 7842 Magnolia Ave, Riverside CA 92504 |
| MLS Status | ACTIVE |
| DOM | 45 |
| List Price | $485,000 |
| Property Type | SFR |
| Year Built | 1992 |
| Bed/Bath | 4/2 |
| Living Sqft | 1,856 |
| Lot Sqft | 7,200 |
| ARV | $580,000 |
| Rehab Estimate | $45,000 |
| Agent ISC | 8 |
| Agent Relationship | Pacific Coast, Mike Chen (11 deals) |
| Distress Signals | NOD filed 45 days ago |
| Occupancy | Vacant 3 months |

### Market Data (Riverside)

| Field | Value |
|-------|-------|
| City - County | RIVERSIDE - RIVERSIDE |
| Market Share | 2.1% |
| Total Investors | 6,196 |
| Total Agents | 2,847 |
| PTFV (Market) | 77% |
| PTFV (Property) | 73% |
| Purchase/Resale | 478 days |
| Median Purchase | $455,000 |

### Expected Output

```
Buy Box: ✅ Ideal Flip

✓ SFR in Riverside at standard 12% ROI
✓ ARV $580K under $675K ceiling
✓ All-In $530K under $560K max
✓ $45K rehab within $150K budget
✓ Agent ISC 8 - works with Pacific Coast, Mike Chen (11 deals)
✓ Your PTFV 73% vs Market 77% - 4 pts BELOW market
✓ NOD filed 45 days ago - seller motivated
✓ Vacant 3 months - no occupancy issues

⚠️ Active 45 DOM - not yet aged, seller may still be firm

❓ ASK: "Your seller has an NOD and the property's been vacant
3 months. What's the real number to get this closed in 10 days?"
```

### Fit Analysis

| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | Riverside County in your active list |
| Price Range | 🟢 High Match | ARV $580K under $675K ceiling |
| Property Type | 🟢 Strong Match | SFR at standard 12% ROI |
| Rehab Level | 🟢 Match | $45K cosmetic within budget |
| Deal Killers | 🟢 None | No flood/fire/title issues |
| Conditional Vars | 🟢 None | Not 55+, not 2/1, city sewer & water |

---

## Sample 2: Townhome with Agent Relationship — Ideal Flip

### Property Input

| Field | Value |
|-------|-------|
| Address | 2156 Palm Canyon Dr #B, San Bernardino CA 92405 |
| MLS Status | ACTIVE |
| DOM | 62 |
| List Price | $365,000 |
| Property Type | Townhome |
| Year Built | 1988 |
| Bed/Bath | 3/2.5 |
| Living Sqft | 1,542 |
| ARV | $445,000 |
| Rehab Estimate | $38,000 |
| Agent ISC | 7 |
| Agent Relationship | Previous deal together (1) |
| Distress Signals | Tax default $4,200 |
| Occupancy | Owner occupied |

### Expected Output

```
Buy Box: ✅ Ideal Flip

✓ Townhome in San Bernardino at standard 12% ROI
✓ ARV $445K under $675K ceiling
✓ All-In $403K under $560K max
✓ $38K rehab within budget
✓ Agent ISC 7 - previous deal together
✓ Tax default $4,200 - seller bleeding

⚠️ Owner occupied - need to confirm move-out timeline
⚠️ 62 DOM approaching aged threshold

❓ ASK: "We closed one together before. Your seller has a tax
default—what's the number to get this done before it gets worse?"
```

### Fit Analysis

| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | San Bernardino County active |
| Price Range | 🟢 High Match | ARV $445K well under ceiling |
| Property Type | 🟢 Strong Match | Townhome at standard 12% |
| Rehab Level | 🟢 Match | $38K within budget |
| Deal Killers | 🟢 None | No issues identified |
| Conditional Vars | 🟢 None | Standard configuration |

---

## Sample 3: New Listing — Ideal Flip

### Property Input

| Field | Value |
|-------|-------|
| Address | 18923 Citrus Way, Fontana CA 92335 |
| MLS Status | NEW |
| DOM | 3 |
| List Price | $425,000 |
| Property Type | SFR |
| Year Built | 2003 |
| Bed/Bath | 4/3 |
| Living Sqft | 2,105 |
| ARV | $510,000 |
| Rehab Estimate | $32,000 |
| Agent ISC | 6 |
| Distress Signals | Divorce sale |
| Occupancy | Vacant |

### Expected Output

```
Buy Box: ✅ Ideal Flip

✓ SFR in San Bernardino at standard 12% ROI
✓ ARV $510K under $675K ceiling
✓ 2003 build - no age concerns
✓ $32K light rehab within budget
✓ Divorce sale - motivated to close quickly
✓ Vacant - immediate access

⚠️ NEW listing 3 DOM - agent getting hammered with calls
⚠️ ISC 6 - agent may not prioritize investor offers

❓ ASK: "I'm not calling to lowball. I have market data showing
this area at 77% PTFV. Can we have a real conversation about
what your clients need to close this chapter?"
```

### Fit Analysis

| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | San Bernardino County active |
| Price Range | 🟢 High Match | ARV $510K well under ceiling |
| Property Type | 🟢 Strong Match | SFR at standard 12% |
| Rehab Level | 🟢 Match | $32K light cosmetic |
| Deal Killers | 🟢 None | No issues |
| Conditional Vars | 🟢 None | Standard |

---

## Sample 4: Condo Requiring Higher ROI — Marginal Flip

### Property Input

| Field | Value |
|-------|-------|
| Address | 4521 Sunset Blvd #205, Los Angeles CA 90027 |
| MLS Status | AGED |
| DOM | 85 |
| List Price | $515,000 |
| Property Type | Condo |
| Year Built | 1978 |
| Bed/Bath | 2/2 |
| Living Sqft | 1,124 |
| ARV | $625,000 |
| Rehab Estimate | $55,000 |
| Agent ISC | 4 |
| Distress Signals | Tax default $8,200, listing expires in 5 days |
| PTFV (Market) | 69% |
| PTFV (Property) | 86.5% |

### Expected Output

```
Buy Box: ⚠️ Marginal Flip

✓ LA County - active in your Buy Box
✓ ARV $625K under $1.16M ceiling

✗ Condo requires 15% ROI (not standard 12%)
✗ 2/2 layout requires 15% ROI
✗ 1978 build requires 15% ROI (1965-1979 range)
✗ Your PTFV 86.5% vs Market 69% - 17 pts OVER market
✗ Gap: $69K over max purchase price at 15% ROI

⚠️ Aged 85 DOM - expires in 5 days
⚠️ Tax default $8,200 - seller bleeding
⚠️ Already dropped $50K (from $565K to $515K)

❓ ASK: "85 days, about to expire, tax default—what's the real
number to get this done? I can close in 10 days through First American."
```

### Fit Analysis

| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | LA County active |
| Price Range | 🟢 Match | Under ceiling |
| Property Type | 🟡 Borderline | Condo requires 15% ROI |
| Rehab Level | 🟢 Match | $55K within budget |
| Deal Killers | 🟢 None | No hard stops |
| Conditional Vars | 🟡 Borderline | 2/2 layout requires 15% |

---

## Sample 5: Older Home with Multiple Conditions — Marginal Flip

### Property Input

| Field | Value |
|-------|-------|
| Address | 892 Oak Street, Corona CA 92879 |
| MLS Status | ACTIVE |
| DOM | 54 |
| List Price | $475,000 |
| Property Type | SFR |
| Year Built | 1958 |
| Bed/Bath | 3/1.5 |
| Living Sqft | 1,320 |
| ARV | $565,000 |
| Rehab Estimate | $85,000 |
| Rehab Notes | Needs electrical panel upgrade |
| Agent ISC | 6 |
| Utilities | Septic system (no sewer) |

### Expected Output

```
Buy Box: ⚠️ Marginal Flip

✓ Riverside County - active
✓ ARV $565K under $675K ceiling

✗ 1958 build requires 18% ROI (1950-1964 range)
✗ Electrical panel upgrade requires 18% ROI
✗ Septic system requires 15% ROI
✗ Combined conditions push required ROI to 18%
✗ At 18% ROI: All-In 77% = $435K max vs $560K ask
✗ Gap: $40K over max purchase at required ROI

⚠️ $85K rehab approaching budget limit
⚠️ Multiple condition stacking increases risk

❓ ASK: "1958 with electrical work and septic—what's the
flexibility on price? I need to be at $435K to make this work."
```

### Fit Analysis

| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | Riverside County active |
| Price Range | 🟡 Borderline | Works at 18% ROI only |
| Property Type | 🟢 Strong Match | SFR |
| Rehab Level | 🟡 Borderline | $85K, electrical upgrade |
| Deal Killers | 🟢 None | No hard stops |
| Conditional Vars | 🟡 Borderline | Septic requires 15% |

---

## Sample 6: Property with Deal Killer Conditions — Marginal Flip

### Property Input

| Field | Value |
|-------|-------|
| Address | 5623 Freeway Drive, Rancho Cucamonga CA 91730 |
| MLS Status | AGED |
| DOM | 92 |
| List Price | $435,000 |
| Property Type | SFR |
| Year Built | 1985 |
| Bed/Bath | 3/2 |
| Living Sqft | 1,450 |
| ARV | $520,000 |
| Rehab Estimate | $42,000 |
| Agent ISC | 5 |
| Location Issues | Next to freeway, busy street |
| Distress Signals | Probate sale |

### Expected Output

```
Buy Box: ⚠️ Marginal Flip

✓ San Bernardino County - active
✓ ARV $520K under ceiling
✓ 1985 build - standard year range

✗ Next to freeway requires 18% ROI
✗ Busy street requires 15% ROI
✗ Combined deal killers push to 18% ROI
✗ At 18%: Max purchase $400K vs $435K ask
✗ Gap: $35K over max purchase

⚠️ 92 DOM aged - seller should be motivated
⚠️ Probate sale - estate timeline may force acceptance
⚠️ Resale limited - location concerns affect buyer pool

❓ ASK: "92 days on a freeway-adjacent property in probate—
the estate needs this closed. What's the real number?"
```

### Fit Analysis

| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | San Bernardino active |
| Price Range | 🟡 Borderline | Works only at 18% ROI |
| Property Type | 🟢 Strong Match | SFR |
| Rehab Level | 🟢 Match | $42K standard |
| Deal Killers | 🟡 Borderline | Freeway + busy street |
| Conditional Vars | 🟢 None | Standard |

---

## Sample 7: Triplex Outside Flip Box — Good Wholesale

### Property Input

| Field | Value |
|-------|-------|
| Address | 1842 Central Ave, Riverside CA 92507 |
| MLS Status | AGED |
| DOM | 92 |
| List Price | $425,000 |
| Property Type | Triplex |
| Year Built | 1962 |
| Bed/Bath | 6/3 (2/1 per unit) |
| Living Sqft | 2,850 |
| ARV | $510,000 |
| Agent ISC | 7 |
| Occupancy | All 3 units vacant |
| Distress Signals | NOD filed 60 days ago |

### Expected Output

```
Buy Box: ✅ Good Wholesale

✗ Triplex - requires 18% ROI, outside standard flip Buy Box
✗ 1962 build - outside standard year range

✓ Agent motivated - 92 DOM, wants to close before expiration
✓ NOD filed 60 days ago - seller running out of time
✓ All 3 units vacant - no tenant issues
✓ 10% off list = $382K works for wholesale ($10K+ fee)

⚠️ Verify: Do you have multi-family buyer in network?
⚠️ Buyer likely wants below $400K

❓ ASK: "At $380K I can bring you a buyer this week who closes
fast. Can your seller do that number?"
```

### Fit Analysis

| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | Riverside active |
| Price Range | 🔴 Mismatch | Outside flip at any ROI |
| Property Type | 🔴 Mismatch | Triplex not in flip box |
| Rehab Level | N/A | Wholesale path |
| Deal Killers | 🟢 None | No hard stops |
| Conditional Vars | N/A | Wholesale path |

**Wholesale Calculation:**
```
List Price: $425,000
10% Discount: $382,500
Est. Wholesale Price: $380,000
Buyer Target ARV: $510,000
Buyer All-In at 83%: $423,300
Spread for Buyer: $43,300
Min Assignment Fee: $10,000 ✓
```

---

## Sample 8: Multi-Family with Tenant Issues — Good Wholesale

### Property Input

| Field | Value |
|-------|-------|
| Address | 2934 Adams Street, San Bernardino CA 92410 |
| MLS Status | ACTIVE |
| DOM | 67 |
| List Price | $485,000 |
| Property Type | Fourplex |
| Year Built | 1965 |
| Bed/Bath | 8/4 (2/1 per unit) |
| Living Sqft | 3,200 |
| ARV | $580,000 |
| Agent ISC | 6 |
| Occupancy | 3 units occupied (tenants), 1 vacant |
| Distress Signals | Code violation notice |

### Expected Output

```
Buy Box: ✅ Good Wholesale

✗ Fourplex - requires 18% ROI, outside standard flip Buy Box
✗ 1965 build - outside standard year range
✗ Occupied with tenants - requires 15% ROI
✗ Active code violations - requires 18% ROI

✓ 10% off list = $436K works for wholesale
✓ Multi-family investor buyer pool exists
✓ Cash flow property for rental buyers
✓ Code violation creates motivation

⚠️ Tenant situation adds complexity
⚠️ Verify: Do you have investor buyer for multi-family?

❓ ASK: "Fourplex with code violations and tenants—your seller
knows this limits buyers. At $435K I can bring an investor
who closes as-is in 10 days."
```

### Fit Analysis

| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | San Bernardino active |
| Price Range | 🔴 Mismatch | Outside flip box |
| Property Type | 🔴 Mismatch | Fourplex not in flip box |
| Rehab Level | N/A | Wholesale path |
| Deal Killers | 🔴 Mismatch | Tenants + code violation |
| Conditional Vars | N/A | Wholesale path |

---

## Sample 9: Close to Wholesale Threshold — Marginal Wholesale

### Property Input

| Field | Value |
|-------|-------|
| Address | 7756 Mountain View Dr, Fontana CA 92336 |
| MLS Status | ACTIVE |
| DOM | 48 |
| List Price | $465,000 |
| Property Type | Triplex |
| Year Built | 1970 |
| Bed/Bath | 5/3 |
| Living Sqft | 2,400 |
| ARV | $540,000 |
| Agent ISC | 5 |
| Distress Signals | Probate sale |

### Expected Output

```
Buy Box: ⚠️ Marginal Wholesale

✗ Triplex - not in flip Buy Box
✗ 1970 build - outside year range
✗ Currently 0% off list - need 10% discount

✓ Agent works with investors (ISC 5)
✓ Probate sale - estate needs to close

⚠️ Need 10% ($46.5K) discount to hit standard wholesale
⚠️ 48 DOM not yet aged - seller may still be firm
⚠️ No buyer relationship = need full 10% margin

❓ ASK: "This is a probate—what's the estate's timeline?
I need to be at $418K to make this work for my buyer."
```

### Fit Analysis

| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | San Bernardino active |
| Price Range | 🔴 Mismatch | Outside flip box |
| Property Type | 🔴 Mismatch | Triplex |
| Rehab Level | N/A | Wholesale path |
| Deal Killers | 🟢 None | No hard stops |
| Conditional Vars | N/A | Wholesale path |

**Wholesale Gap Calculation:**
```
List Price: $465,000
Current Discount: $0 (0%)
Required Discount (10%): $46,500
Target Price: $418,500
Gap to Close: $46,500
```

---

## Sample 10: No Buyer Relationship — Marginal Wholesale

### Property Input

| Field | Value |
|-------|-------|
| Address | 3421 Industrial Blvd, Ontario CA 91761 |
| MLS Status | AGED |
| DOM | 105 |
| List Price | $525,000 |
| Property Type | Duplex |
| Year Built | 1968 |
| Bed/Bath | 4/2 |
| Living Sqft | 1,800 |
| ARV | $615,000 |
| Agent ISC | 4 |
| Current Discount | 5% off original ($552K) |
| Distress Signals | Pre-foreclosure |

### Expected Output

```
Buy Box: ⚠️ Marginal Wholesale

✗ Duplex - requires 15% ROI, marginal for flip
✗ 1968 build - requires 15% ROI
✗ Currently at 5% off original - need 10% for standard wholesale

✓ 105 DOM aged - seller motivated
✓ Pre-foreclosure - timeline pressure
✓ Agent ISC 4 - retail but may accept investor

⚠️ No established buyer relationship for this property type
⚠️ Need additional 5% discount to hit wholesale threshold
⚠️ Full 10% margin required without buyer relationship

❓ ASK: "105 days in pre-foreclosure—what's the number to
get this done in the next 10 days? I can be at $472K."
```

### Fit Analysis

| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | San Bernardino active |
| Price Range | 🟡 Borderline | Marginal at 15% flip |
| Property Type | 🟡 Borderline | Duplex at 15% |
| Rehab Level | N/A | Wholesale path |
| Deal Killers | 🟢 None | No hard stops |
| Conditional Vars | 🟢 None | Standard |

**Wholesale Calculation:**
```
Original List: $552,000
Current List: $525,000 (5% off)
Required (10% off original): $496,800
Required (10% off current): $472,500
Target Price: $472,500
Additional Discount Needed: $52,500 (10% of current)
```

---

## Sample 11: Mobile Home Hard Stop — Pass

### Property Input

| Field | Value |
|-------|-------|
| Address | 1823 Desert Rose Lane #45, Palm Desert CA 92260 |
| MLS Status | ACTIVE |
| DOM | 34 |
| List Price | $125,000 |
| Property Type | Mobile/Manufactured |
| Year Built | 1972 |
| Bed/Bath | 2/2 |
| Living Sqft | 980 |
| Land Status | Leased land |
| Agent ISC | 3 |

### Expected Output

```
Buy Box: ❌ Pass

✗ Mobile Home - WON'T DO in Buy Box (hard stop)
✗ 1972 build - outside year range (hard stop)
✗ Leased land - WON'T DO (hard stop)

⚠️ Retail agent (ISC 3) - doesn't work with investors
⚠️ No wholesale path - no mobile home buyers in network
⚠️ Leased land eliminates most financing options

❓ NO CALL NEEDED - Move to next property
```

### Fit Analysis

| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | Riverside active |
| Price Range | N/A | Hard stop |
| Property Type | 🔴 Hard Stop | Mobile home WON'T DO |
| Rehab Level | N/A | Hard stop |
| Deal Killers | 🔴 Hard Stop | Leased land WON'T DO |
| Conditional Vars | N/A | Hard stop |

---

## Sample 12: Multiple Hard Stops — Pass

### Property Input

| Field | Value |
|-------|-------|
| Address | 9234 Riverside Dr, Norco CA 92860 |
| MLS Status | ACTIVE |
| DOM | 28 |
| List Price | $385,000 |
| Property Type | SFR |
| Year Built | 1952 |
| Bed/Bath | 3/1 |
| Living Sqft | 1,180 |
| Lot Sqft | 43,560 (1 acre) |
| Agent ISC | 5 |
| Zone | Flood zone (FEMA) |
| Title Status | Active liens |
| Utilities | Well water |

### Expected Output

```
Buy Box: ❌ Pass

✗ Flood zone (FEMA) - WON'T DO (hard stop)
✗ Title issues / liens - WON'T DO (hard stop)
✗ 1952 build - requires 18% ROI even if no hard stops

⚠️ Well water requires 18% ROI (moot due to hard stops)
⚠️ 3/1 configuration limits buyer pool
⚠️ Flood insurance costs eliminate most buyers

❓ NO CALL NEEDED - Move to next property
```

### Fit Analysis

| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | Riverside active |
| Price Range | N/A | Hard stop |
| Property Type | 🟢 Match | SFR |
| Rehab Level | N/A | Hard stop |
| Deal Killers | 🔴 Hard Stop | Flood zone + liens |
| Conditional Vars | N/A | Hard stop |

---

## Sample 13: Price Ceiling Breach — Pass

### Property Input

| Field | Value |
|-------|-------|
| Address | 4521 Harbor View, Newport Beach CA 92660 |
| MLS Status | ACTIVE |
| DOM | 45 |
| List Price | $1,450,000 |
| Property Type | SFR |
| Year Built | 2005 |
| Bed/Bath | 4/3.5 |
| Living Sqft | 2,800 |
| ARV | $1,750,000 |
| Agent ISC | 7 |
| Distress Signals | Divorce sale |

### Expected Output

```
Buy Box: ❌ Pass

✗ ARV $1.75M exceeds Orange County ceiling $1.16M
✗ $585K over max ARV - significant breach
✗ At 12% ROI: Max All-In $967K vs $1.45M ask
✗ Gap: $483K over max purchase price

⚠️ Divorce sale typically motivated but price too high
⚠️ No wholesale path at this price point
⚠️ Luxury market has different dynamics

❓ NO CALL NEEDED - Move to next property
```

### Fit Analysis

| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | Orange County active |
| Price Range | 🔴 Ceiling Breach | ARV $585K over max |
| Property Type | 🟢 Strong Match | SFR |
| Rehab Level | N/A | Ceiling breach |
| Deal Killers | 🟢 None | No hard stops |
| Conditional Vars | 🟢 None | Standard |

---

## Sample 14: No Wholesale Path — Pass

### Property Input

| Field | Value |
|-------|-------|
| Address | 892 Senior Lane, Hemet CA 92545 |
| MLS Status | ACTIVE |
| DOM | 67 |
| List Price | $285,000 |
| Property Type | SFR |
| Year Built | 1985 |
| Bed/Bath | 2/2 |
| Living Sqft | 1,150 |
| Community | 55+ Senior Community |
| ARV | $340,000 |
| Agent ISC | 2 |

### Expected Output

```
Buy Box: ❌ Pass

✗ 55+ Senior Community - requires 15% ROI
✗ 2 bedroom / 2 bathroom - requires 15% ROI
✗ Combined conditions push ROI to 15%
✗ At 15%: Max All-In $272K vs $285K ask
✗ Gap: $13K over max purchase

⚠️ Retail agent (ISC 2) - won't prioritize investor
⚠️ 55+ limits buyer pool significantly
⚠️ No wholesale path - limited buyer network for 55+

❓ NO CALL NEEDED - Move to next property
```

### Fit Analysis

| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | Riverside active |
| Price Range | 🔴 Mismatch | Over max at required ROI |
| Property Type | 🟢 Match | SFR |
| Rehab Level | 🟢 Match | Standard cosmetic |
| Deal Killers | 🟢 None | No hard stops |
| Conditional Vars | 🔴 Mismatch | 55+ and 2/2 stack to 15% |

---

## Sample 15: Back on Market Opportunity — Mixed

### Property Input

| Field | Value |
|-------|-------|
| Address | 6723 Eucalyptus Ave, Chino CA 91710 |
| MLS Status | BACK ON MARKET |
| Original DOM | 34 |
| Days Pending | 18 |
| List Price | $465,000 |
| Property Type | SFR |
| Year Built | 1994 |
| Bed/Bath | 4/2 |
| Living Sqft | 1,890 |
| ARV | $545,000 |
| Rehab Estimate | $40,000 |
| Agent ISC | 6 |
| Fall-Through Reason | Unknown |

### Expected Output

```
Buy Box: ✅ Ideal Flip

✓ SFR in San Bernardino at standard 12% ROI
✓ ARV $545K under $675K ceiling
✓ All-In $505K under $560K max
✓ $40K rehab within budget
✓ 1994 build - standard year range

⚠️ BACK ON MARKET - previous deal fell through
⚠️ Unknown why deal failed - must investigate
⚠️ Agent frustrated - opportunity to position as reliable
⚠️ Seller likely more motivated after failed deal

❓ ASK: "Your deal fell through—what happened? I close with
First American in 10 days, no contingencies. What does your
seller need to make sure this one sticks?"
```

### Fit Analysis

| Category | Rating | Why |
|----------|--------|-----|
| Location | 🟢 Match | San Bernardino active |
| Price Range | 🟢 High Match | ARV under ceiling |
| Property Type | 🟢 Strong Match | SFR at 12% |
| Rehab Level | 🟢 Match | $40K standard |
| Deal Killers | 🟢 None | No issues |
| Conditional Vars | 🟢 None | Standard |

**Back on Market Strategy:**
```
Day 0: Ask why deal failed
Day 1: Position as cash, no contingencies
Day 3: Follow up with formal offer
Key: Seller burned once, wants certainty
```

---

## Edge Cases Reference

### Multiple Conditions Stacking

When multiple conditions each require higher ROI:
- Use the HIGHEST required ROI (not sum)
- List all conditions in output
- Example: Condo (15%) + 2/2 (15%) + 1970 build (15%) = 15% ROI required

### Location Override Applied

When property is in overridden City or ZIP:
- Show inheritance chain in output
- Display: "Using ZIP 92262 override: 15% ROI"
- Note if override makes property viable vs MASTER settings

### Wholesale with Buyer Relationship

| Scenario | Min Discount |
|----------|--------------|
| No buyer relationship | 10% off list |
| Have buyer relationship | 5% off list |

If 5% discount works with existing buyer:
- Change verdict from ⚠️ Marginal Wholesale to ✅ Good Wholesale
- Note: "Existing buyer relationship allows 5% margin"

---

## Output Format Reference

### Verdict Summary Structure

```
Buy Box: [EMOJI] [VERDICT TYPE]

✓ [Positive items - what fits, what's good]
✓ ...

✗ [Negative items - what doesn't fit, concerns]
✗ ...

⚠️ [Context - situational factors, warnings]
⚠️ ...

❓ ASK: "[Context-aware agent question]"
```

### Fit Analysis Table Structure

```
| Category | Rating | Why |
|----------|--------|-----|
| Location | [EMOJI] [RATING] | [One-line explanation] |
| Price Range | [EMOJI] [RATING] | [One-line explanation] |
| Property Type | [EMOJI] [RATING] | [One-line explanation] |
| Rehab Level | [EMOJI] [RATING] | [One-line explanation] |
| Deal Killers | [EMOJI] [RATING] | [One-line explanation] |
| Conditional Vars | [EMOJI] [RATING] | [One-line explanation] |
```

### Rating Scale

| Rating | Emoji | Meaning |
|--------|-------|---------|
| Strong Match | 🟢 | Exceeds requirements |
| High Match | 🟢 | Well within requirements |
| Match | 🟢 | Meets requirements |
| None | 🟢 | No issues (for killers/conditions) |
| Borderline | 🟡 | Conditional, requires higher ROI |
| Mismatch | 🔴 | Does not fit |
| Hard Stop | 🔴 | Won't Do - blocks deal |
| Ceiling Breach | 🔴 | Over max ARV |

---

**Document Version:** 2.0
**Last Updated:** December 31, 2024
**Status:** Ready for Training
