# COMP.LIST BOT
## Training Samples — Bucket Classification & Ceiling Identification
### Version 1.0 — December 31, 2024

---

| Field | Value |
|-------|-------|
| **Bot ID** | C3 (List Grouping Bot) |
| **Purpose** | GPT Training Samples |
| **Sample Count** | 8 |
| **Focus** | Bucket classification + Ceiling explanation |

---

## OUTPUT STRUCTURE

Each sample shows:
1. **PIQ Context** — Subject property details
2. **Comp Classification** — How each comp is bucketed
3. **Ceiling Identification** — Which comp defines the ceiling and WHY
4. **Chip Examples** — Sample chips for key comps

---

# SAMPLE 1: CLEAN BUCKET DISTRIBUTION

## PIQ Context
```
PIQ: 456 Maple Drive
- 1,850 sqft | 4 bed / 2 bath | 1985
- 7,200 sqft lot | No pool | No ADU
- Interior street | Sunset Elementary feeder
- Standard tract home
```

## Comp Classification

### 🔵 PREMIUM (2 comps)

**789 Oak Lane** — $412,000 ($223/sqft) ⭐ CEILING COMP
- Bucket: PREMIUM
- Reasons: ["Permitted ADU (PIQ lacks)", "Pool + 8,500 sqft lot (PIQ lacks pool)"]
- Chips: `ADU` `Pool` `+1,300 sqft Lot` `Same Tract` `FLIP`

**234 Cedar Court** — $398,000 ($215/sqft)
- Bucket: PREMIUM
- Reasons: ["Cul-de-sac location (PIQ on interior street)", "Pool + spa"]
- Chips: `Pool+Spa` `Cul-de-sac` `Same Tract` `FLIP`

---

### 🟢 HIGH (3 comps)

**567 Birch Street** — $365,000 ($197/sqft)
- Bucket: HIGH
- Reasons: ["Same tract", "Within 10% sqft", "FLIP condition", "No unchangeables"]
- Chips: `Same Tract` `Similar Lot` `FLIP` `=Bed/Bath`

**890 Pine Avenue** — $358,000 ($193/sqft)
- Bucket: HIGH
- Reasons: ["Same tract", "Within 15% sqft", "GOOD condition", "Same school feeder"]
- Chips: `Same Tract` `GOOD` `-200 sqft` `Same School`

**123 Elm Way** — $352,000 ($190/sqft)
- Bucket: HIGH
- Reasons: ["Same tract", "Within 20% sqft", "FLIP condition"]
- Chips: `Same Tract` `FLIP` `Smaller Lot` `=Bed/Bath`

---

### 🟡 MID (2 comps)

**345 Walnut Drive** — $325,000 ($176/sqft)
- Bucket: MID
- Reasons: ["Different tract", "ORIGINAL condition"]
- Chips: `Different Tract` `ORIGINAL` `Similar Lot` `Dated Kitchen`

**678 Spruce Lane** — $318,000 ($172/sqft)
- Bucket: MID
- Reasons: ["ORIGINAL condition", "Backs to school (minor location drag)"]
- Chips: `Same Tract` `ORIGINAL` `Backs School` `Dated Baths`

---

### 🔴 LOW (1 comp)

**901 Ash Boulevard** — $285,000 ($154/sqft)
- Bucket: LOW
- Reasons: ["Busy street location", "FIXER condition"]
- Chips: `Busy Street` `FIXER` `Deferred Maintenance` `Same Tract`

---

## Ceiling Identification

**CEILING COMP: 789 Oak Lane ($223/sqft)**

> "789 Oak Lane ($223/sqft) is the value ceiling because it has: Permitted ADU, Pool + 8,500 sqft usable lot. PIQ at 456 Maple Drive cannot match these features through renovation. The ADU adds income potential PIQ cannot replicate without major entitlement work beyond rehab scope."

**Visual:** Red dashed line appears below 789 Oak Lane, labeled "VALUE CEILING"

---

# SAMPLE 2: NO PREMIUM COMPS (HIGH = CEILING)

## PIQ Context
```
PIQ: 123 Valley View Road
- 2,100 sqft | 4 bed / 2.5 bath | 1992
- 8,000 sqft flat lot | Pool | No ADU
- Cul-de-sac | Lincoln Elementary feeder
- Premium street position
```

## Comp Classification

### 🔵 PREMIUM (0 comps)
*No comps have unchangeable advantages over PIQ*

---

### 🟢 HIGH (4 comps) — CEILING IN THIS BUCKET

**456 Summit Drive** — $485,000 ($231/sqft) ⭐ CEILING COMP
- Bucket: HIGH
- Reasons: ["Same tract", "Same lot size", "FLIP condition", "Pool (PIQ has pool)", "No unchangeables"]
- Chips: `Same Tract` `Pool` `FLIP` `Cul-de-sac` `=Bed/Bath`

**789 Ridge Court** — $478,000 ($228/sqft)
- Bucket: HIGH
- Reasons: ["Same tract", "Within 5% sqft", "FLIP condition"]
- Chips: `Same Tract` `Pool` `FLIP` `Premium Lot`

**234 Hillcrest Lane** — $465,000 ($221/sqft)
- Bucket: HIGH
- Reasons: ["Same tract", "GOOD condition", "Similar features"]
- Chips: `Same Tract` `Pool` `GOOD` `=Lot`

**567 Mountain View** — $458,000 ($218/sqft)
- Bucket: HIGH
- Reasons: ["Same school feeder", "Within 15% sqft", "FLIP condition"]
- Chips: `Same School` `Pool` `FLIP` `-Lot`

---

### 🟡 MID (2 comps)

**890 Foothill Drive** — $425,000 ($202/sqft)
- Bucket: MID
- Reasons: ["ORIGINAL condition", "Interior street (PIQ on cul-de-sac)"]
- Chips: `Same Tract` `Pool` `ORIGINAL` `Interior Street`

**123 Canyon Road** — $412,000 ($196/sqft)
- Bucket: MID
- Reasons: ["Different tract", "ORIGINAL condition"]
- Chips: `Different Tract` `Pool` `ORIGINAL` `Smaller Lot`

---

### 🔴 LOW (1 comp)

**345 Highway View** — $365,000 ($174/sqft)
- Bucket: LOW
- Reasons: ["Freeway adjacent", "Noise impact"]
- Chips: `Freeway` `Same Tract` `GOOD` `Pool` `⚠️ Noise`

---

## Ceiling Identification

**CEILING COMP: 456 Summit Drive ($231/sqft)**

> "No comps with unchangeable advantages found. 456 Summit Drive ($231/sqft) defines the ceiling as the best HIGH comp. PIQ at 123 Valley View Road has no structural limitations — with proper renovation, it can compete for top-of-market pricing in this micro-market."

**Visual:** Green highlighted line (not red) below 456 Summit Drive, labeled "ARV TARGET CEILING"

---

# SAMPLE 3: MULTIPLE PREMIUM COMPS (HIGHEST WINS)

## PIQ Context
```
PIQ: 789 Suburban Lane
- 1,650 sqft | 3 bed / 2 bath | 1978
- 6,500 sqft lot | No pool | No ADU
- Interior street | Jefferson Elementary
- Standard tract home
```

## Comp Classification

### 🔵 PREMIUM (4 comps)

**111 Lakeside Drive** — $525,000 ($318/sqft) ⭐ CEILING COMP
- Bucket: PREMIUM
- Reasons: ["Lake view (PIQ has no view)", "Pool + 12,000 sqft lot", "Gated community"]
- Chips: `Lake View` `Pool` `Gated` `+5,500 sqft Lot` `FLIP`

**222 Panorama Court** — $498,000 ($302/sqft)
- Bucket: PREMIUM
- Reasons: ["Panoramic hill view (PIQ has no view)", "Premium lot position"]
- Chips: `Panoramic View` `Premium Lot` `FLIP` `Same School`

**333 Golf Course Way** — $485,000 ($294/sqft)
- Bucket: PREMIUM
- Reasons: ["Golf course adjacency (PIQ lacks)", "Oversized lot"]
- Chips: `Golf Course` `+4,000 sqft Lot` `GOOD` `Pool`

**444 Pool Estate Lane** — $465,000 ($282/sqft)
- Bucket: PREMIUM
- Reasons: ["Permitted ADU (PIQ lacks)", "Pool + large yard"]
- Chips: `ADU` `Pool` `+3,500 sqft Lot` `FLIP`

---

### 🟢 HIGH (2 comps)

**555 Standard Street** — $385,000 ($233/sqft)
- Bucket: HIGH
- Reasons: ["Same tract", "Within 10% sqft", "FLIP condition", "No unchangeables"]
- Chips: `Same Tract` `FLIP` `Similar Lot` `=Bed/Bath`

**666 Typical Avenue** — $372,000 ($225/sqft)
- Bucket: HIGH
- Reasons: ["Same tract", "GOOD condition", "Same era"]
- Chips: `Same Tract` `GOOD` `=Lot` `Same School`

---

### 🟡 MID (1 comp)

**777 Older Drive** — $335,000 ($203/sqft)
- Bucket: MID
- Reasons: ["ORIGINAL condition", "Dated floor plan"]
- Chips: `Same Tract` `ORIGINAL` `Dated Layout` `Similar Lot`

---

### 🔴 LOW (1 comp)

**888 Railroad Avenue** — $295,000 ($179/sqft)
- Bucket: LOW
- Reasons: ["Railroad track adjacent", "Noise and vibration impact"]
- Chips: `Railroad` `Same Tract` `GOOD` `⚠️ Vibration`

---

## Ceiling Identification

**CEILING COMP: 111 Lakeside Drive ($318/sqft)**

> "111 Lakeside Drive ($318/sqft) is the value ceiling because it has: Lake view, Pool + 12,000 sqft lot, Gated community access. PIQ at 789 Suburban Lane cannot match these features — it has no view, is not in a gated community, and has a 6,500 sqft lot with no pool. Three other PREMIUM comps also exceed PIQ's achievable value (222 Panorama, 333 Golf Course, 444 Pool Estate) but 111 Lakeside defines the highest ceiling."

---

# SAMPLE 4: ALL LOW COMPS (WARNING STATE)

## PIQ Context
```
PIQ: 555 Industrial Parkway
- 1,400 sqft | 3 bed / 1 bath | 1965
- 5,500 sqft lot | No pool | No ADU
- Near commercial zone | Mixed use area
```

## Comp Classification

### 🔵 PREMIUM (0 comps)
*None*

### 🟢 HIGH (0 comps)
*None*

### 🟡 MID (0 comps)
*None*

### 🔴 LOW (4 comps)

**111 Factory View** — $245,000 ($175/sqft)
- Bucket: LOW
- Reasons: ["Industrial adjacency", "Commercial traffic", "FIXER condition"]
- Chips: `Industrial Adjacent` `FIXER` `Commercial Zone` `Busy Street`

**222 Warehouse Way** — $238,000 ($170/sqft)
- Bucket: LOW
- Reasons: ["Backs to warehouse", "Noise impact", "ORIGINAL condition"]
- Chips: `Industrial Adjacent` `ORIGINAL` `Backs Commercial` `⚠️ Noise`

**333 Truck Route Lane** — $225,000 ($161/sqft)
- Bucket: LOW
- Reasons: ["Heavy truck traffic street", "FIXER condition"]
- Chips: `Busy Street` `FIXER` `Truck Traffic` `Deferred Maintenance`

**444 Rail Spur Court** — $218,000 ($156/sqft)
- Bucket: LOW
- Reasons: ["Railroad spur adjacent", "Industrial proximity", "REO sale"]
- Chips: `Railroad` `REO` `Industrial Adjacent` `FIXER`

---

## Ceiling Identification

**⚠️ WARNING STATE**

> "All comparable sales have significant negatives — review market carefully. No achievable comps found. PIQ at 555 Industrial Parkway is in a challenged micro-market where all recent sales involved distress, industrial adjacency, or significant location negatives. Consider expanding search radius or adjusting acquisition strategy."

**Visual:** Yellow warning banner at top, no ceiling line displayed

---

# SAMPLE 5: MIXED BUCKET WITH DISTRESS FILTERING

## PIQ Context
```
PIQ: 234 Quiet Street
- 1,750 sqft | 3 bed / 2 bath | 1988
- 7,000 sqft lot | No pool | No ADU
- Interior street | Washington Elementary
```

## Comp Classification

### 🔵 PREMIUM (1 comp)

**567 Pool Paradise** — $395,000 ($226/sqft) ⭐ CEILING COMP
- Bucket: PREMIUM
- Reasons: ["Pool + large flat yard (PIQ lacks pool)", "8,500 sqft lot"]
- Chips: `Pool` `+1,500 sqft Lot` `Same Tract` `FLIP`

---

### 🟢 HIGH (3 comps)

**890 Similar Lane** — $358,000 ($205/sqft)
- Bucket: HIGH
- Reasons: ["Same tract", "Same sqft range", "FLIP condition", "No pool (matches PIQ)"]
- Chips: `Same Tract` `FLIP` `=Lot` `=Bed/Bath` `No Pool`

**123 Match Court** — $352,000 ($201/sqft)
- Bucket: HIGH
- Reasons: ["Same tract", "Within 10% sqft", "GOOD condition"]
- Chips: `Same Tract` `GOOD` `Similar Lot` `Same School`

**456 Comparable Way** — $345,000 ($197/sqft)
- Bucket: HIGH
- Reasons: ["Same school feeder", "FLIP condition", "Same era"]
- Chips: `Same School` `FLIP` `-Lot` `=Bed/Bath`

---

### 🟡 MID (2 comps)

**789 Dated Drive** — $318,000 ($182/sqft)
- Bucket: MID
- Reasons: ["ORIGINAL condition", "Dated kitchen and baths"]
- Chips: `Same Tract` `ORIGINAL` `Dated Kitchen` `Dated Baths`

**234 Older Avenue** — $305,000 ($174/sqft)
- Bucket: MID
- Reasons: ["Different tract", "ORIGINAL condition", "Smaller lot"]
- Chips: `Different Tract` `ORIGINAL` `-Lot` `Older Era`

---

### 🔴 LOW (3 comps)

**567 Foreclosure Lane** — $275,000 ($157/sqft)
- Bucket: LOW
- Reasons: ["REO sale (bank-owned)", "Distress pricing"]
- Chips: `REO` `Same Tract` `FIXER` `Bank Owned`

**890 Short Sale Court** — $268,000 ($153/sqft)
- Bucket: LOW
- Reasons: ["Short sale", "Distress pricing", "Deferred maintenance"]
- Chips: `Short Sale` `Same Tract` `FIXER` `Distress`

**123 Busy Boulevard** — $285,000 ($163/sqft)
- Bucket: LOW
- Reasons: ["Busy street (major arterial)", "Traffic noise"]
- Chips: `Busy Street` `Same Tract` `GOOD` `⚠️ Traffic Noise`

---

## Ceiling Identification

**CEILING COMP: 567 Pool Paradise ($226/sqft)**

> "567 Pool Paradise ($226/sqft) is the value ceiling because it has: Pool + 8,500 sqft flat usable lot. PIQ at 234 Quiet Street has no pool and a 7,000 sqft lot. Adding a pool is possible but lot constraints may limit design options. Note: Three LOW comps (foreclosure, short sale, busy street) were excluded from ceiling analysis as they represent distressed or impaired sales."

---

# SAMPLE 6: ADU PREMIUM DRIVING CEILING

## PIQ Context
```
PIQ: 678 Standard Avenue
- 1,600 sqft | 3 bed / 2 bath | 1982
- 7,500 sqft lot | No pool | No ADU
- Interior street | Lincoln Elementary
```

## Comp Classification

### 🔵 PREMIUM (2 comps)

**901 ADU Estate** — $485,000 ($303/sqft) ⭐ CEILING COMP
- Bucket: PREMIUM
- Reasons: ["Permitted ADU (PIQ lacks)", "ADU generates rental income", "650 sqft detached unit"]
- Chips: `Permitted ADU` `650 sqft Unit` `Same Tract` `FLIP` `+Income`

**234 Guest House Lane** — $458,000 ($286/sqft)
- Bucket: PREMIUM
- Reasons: ["Permitted ADU (PIQ lacks)", "400 sqft attached unit"]
- Chips: `Permitted ADU` `400 sqft Unit` `Same Tract` `GOOD` `+Income`

---

### 🟢 HIGH (3 comps)

**567 No ADU Street** — $365,000 ($228/sqft)
- Bucket: HIGH
- Reasons: ["Same tract", "Same sqft", "FLIP condition", "No ADU (matches PIQ)"]
- Chips: `Same Tract` `FLIP` `=Lot` `=Bed/Bath` `No ADU`

**890 Similar Court** — $358,000 ($224/sqft)
- Bucket: HIGH
- Reasons: ["Same tract", "GOOD condition", "No unchangeables"]
- Chips: `Same Tract` `GOOD` `Similar Lot` `Same Era`

**123 Match Way** — $352,000 ($220/sqft)
- Bucket: HIGH
- Reasons: ["Same school feeder", "FLIP condition"]
- Chips: `Same School` `FLIP` `-Lot` `=Bed/Bath`

---

### 🟡 MID (1 comp)

**456 Older Drive** — $325,000 ($203/sqft)
- Bucket: MID
- Reasons: ["ORIGINAL condition", "Dated throughout"]
- Chips: `Same Tract` `ORIGINAL` `Dated` `Similar Lot`

---

### 🔴 LOW (1 comp)

**789 Probate Court** — $298,000 ($186/sqft)
- Bucket: LOW
- Reasons: ["Probate sale", "Estate condition", "Distress pricing"]
- Chips: `Probate` `FIXER` `Estate Sale` `Same Tract`

---

## Ceiling Identification

**CEILING COMP: 901 ADU Estate ($303/sqft)**

> "901 ADU Estate ($303/sqft) is the value ceiling because it has: Permitted 650 sqft detached ADU generating rental income. PIQ at 678 Standard Avenue cannot add a permitted ADU through standard rehab scope — ADU entitlement requires city permits, utility connections, and construction timeline beyond typical flip parameters. The ADU premium of approximately $75-120K over comparable non-ADU homes reflects income potential PIQ cannot capture."

---

# SAMPLE 7: GATED COMMUNITY PREMIUM

## PIQ Context
```
PIQ: 345 Open Tract Lane
- 2,200 sqft | 4 bed / 3 bath | 1995
- 8,000 sqft lot | Pool | No ADU
- Interior street | NOT gated
- Roosevelt Elementary
```

## Comp Classification

### 🔵 PREMIUM (2 comps)

**123 Gated Estates Drive** — $565,000 ($257/sqft) ⭐ CEILING COMP
- Bucket: PREMIUM
- Reasons: ["Gated community (PIQ not gated)", "Guard gate + HOA amenities", "Premium security"]
- Chips: `Gated` `Guard Gate` `Pool` `HOA Amenities` `FLIP`

**456 Private Community Court** — $545,000 ($248/sqft)
- Bucket: PREMIUM
- Reasons: ["Gated community (PIQ not gated)", "Community pool + tennis"]
- Chips: `Gated` `Community Pool` `Tennis` `GOOD` `=Lot`

---

### 🟢 HIGH (3 comps)

**789 Open Street** — $485,000 ($220/sqft)
- Bucket: HIGH
- Reasons: ["Same tract (not gated)", "Pool", "FLIP condition", "No unchangeables"]
- Chips: `Same Tract` `Pool` `FLIP` `=Lot` `Not Gated`

**234 Similar Lane** — $478,000 ($217/sqft)
- Bucket: HIGH
- Reasons: ["Same school feeder", "Pool", "GOOD condition"]
- Chips: `Same School` `Pool` `GOOD` `Similar Lot`

**567 Match Court** — $465,000 ($211/sqft)
- Bucket: HIGH
- Reasons: ["Same tract", "Pool", "FLIP condition"]
- Chips: `Same Tract` `Pool` `FLIP` `-Lot`

---

### 🟡 MID (2 comps)

**890 Dated Drive** — $425,000 ($193/sqft)
- Bucket: MID
- Reasons: ["ORIGINAL condition", "No pool", "Same tract"]
- Chips: `Same Tract` `ORIGINAL` `No Pool` `Dated`

**123 Older Way** — $412,000 ($187/sqft)
- Bucket: MID
- Reasons: ["Different tract", "ORIGINAL condition"]
- Chips: `Different Tract` `ORIGINAL` `Pool` `-Lot`

---

### 🔴 LOW (1 comp)

**456 Arterial Boulevard** — $385,000 ($175/sqft)
- Bucket: LOW
- Reasons: ["Busy arterial street", "Traffic noise", "Commercial adjacency"]
- Chips: `Busy Street` `Traffic` `Commercial Adjacent` `Pool`

---

## Ceiling Identification

**CEILING COMP: 123 Gated Estates Drive ($257/sqft)**

> "123 Gated Estates Drive ($257/sqft) is the value ceiling because it has: Gated community with guard gate and HOA amenities (pool, clubhouse, tennis). PIQ at 345 Open Tract Lane is not in a gated community and cannot become gated — community status is an unchangeable feature tied to the entire subdivision, not individual properties. The gated premium of approximately $80K reflects perceived security and exclusivity."

---

# SAMPLE 8: VIEW PREMIUM DEFINING CEILING

## PIQ Context
```
PIQ: 567 Flatland Street
- 1,900 sqft | 4 bed / 2 bath | 1990
- 7,000 sqft lot | No pool | No ADU
- Interior street | No view
- Madison Elementary
```

## Comp Classification

### 🔵 PREMIUM (3 comps)

**789 Hilltop Vista** — $545,000 ($287/sqft) ⭐ CEILING COMP
- Bucket: PREMIUM
- Reasons: ["Panoramic city view (PIQ has no view)", "Hillside lot with unobstructed sightlines"]
- Chips: `Panoramic View` `City Lights` `Hillside` `FLIP` `Premium Lot`

**234 Canyon Overlook** — $525,000 ($276/sqft)
- Bucket: PREMIUM
- Reasons: ["Canyon view (PIQ has no view)", "Protected viewshed"]
- Chips: `Canyon View` `Protected View` `GOOD` `+Lot`

**567 Peek-a-View Lane** — $485,000 ($255/sqft)
- Bucket: PREMIUM
- Reasons: ["Partial mountain view (PIQ has no view)"]
- Chips: `Partial View` `Mountain` `Same Tract` `FLIP`

---

### 🟢 HIGH (2 comps)

**890 Flat Street** — $398,000 ($209/sqft)
- Bucket: HIGH
- Reasons: ["Same tract", "No view (matches PIQ)", "FLIP condition"]
- Chips: `Same Tract` `No View` `FLIP` `=Lot` `=Bed/Bath`

**123 Standard Court** — $385,000 ($203/sqft)
- Bucket: HIGH
- Reasons: ["Same school feeder", "No view", "GOOD condition"]
- Chips: `Same School` `No View` `GOOD` `Similar Lot`

---

### 🟡 MID (2 comps)

**456 Older Drive** — $352,000 ($185/sqft)
- Bucket: MID
- Reasons: ["ORIGINAL condition", "No view", "Dated kitchen"]
- Chips: `Same Tract` `ORIGINAL` `No View` `Dated Kitchen`

**789 Different Tract** — $345,000 ($182/sqft)
- Bucket: MID
- Reasons: ["Different tract", "ORIGINAL condition"]
- Chips: `Different Tract` `ORIGINAL` `No View` `-Lot`

---

### 🔴 LOW (1 comp)

**234 Power Line Lane** — $315,000 ($166/sqft)
- Bucket: LOW
- Reasons: ["Power line easement through property", "Visual and perceived health impact"]
- Chips: `Power Lines` `Easement` `Same Tract` `GOOD`

---

## Ceiling Identification

**CEILING COMP: 789 Hilltop Vista ($287/sqft)**

> "789 Hilltop Vista ($287/sqft) is the value ceiling because it has: Panoramic city view with unobstructed hillside sightlines. PIQ at 567 Flatland Street has no view and cannot gain one — view is entirely determined by lot position and topography, which cannot be changed through renovation. Two other view comps (234 Canyon Overlook, 567 Peek-a-View) also exceed PIQ's achievable value due to view premiums. The view premium ranges from $85-145K in this market."

---

# SCENARIO COVERAGE MATRIX

| Sample | Scenario | Ceiling Source | Key Teaching Point |
|--------|----------|----------------|-------------------|
| 1 | Clean distribution | PREMIUM (ADU + Pool) | Standard 4-bucket classification |
| 2 | No PREMIUM comps | HIGH (best achievable) | PIQ has no constraints |
| 3 | Multiple PREMIUM | Highest $/sqft PREMIUM | Select best ceiling from multiple |
| 4 | All LOW comps | Warning state | Market quality warning |
| 5 | Mixed with distress | PREMIUM (Pool) | Filter distress from ceiling analysis |
| 6 | ADU premium | PREMIUM (ADU) | ADU beyond rehab scope |
| 7 | Gated premium | PREMIUM (Gated) | Community feature unchangeable |
| 8 | View premium | PREMIUM (View) | Topography unchangeable |

---

# TRAINING NOTES FOR GPT

## Key Behaviors to Reinforce

1. **Ceiling = COMP, not calculation** — Always name the specific comp. Never cite P90/P75.

2. **Unchangeables define PREMIUM** — ADU, pool+lot, gated, view, location = things rehab can't change.

3. **Decision sequence matters** — LOW first, then PREMIUM, then HIGH, then MID.

4. **One bucket per comp** — First match wins. No dual classification.

5. **Distress = LOW always** — REO, short sale, probate, auction = floor comps only.

6. **No advisory language** — Report facts, don't advise. "This comp has X" not "You should consider X"

7. **Chips explain quickly** — Visual tags that summarize comp attributes at a glance.

8. **HIGH = achievable target** — If no PREMIUM, best HIGH becomes ceiling (PIQ has no constraints).

9. **Explain WHY** — Every classification needs reasons. Every ceiling needs unchangeables listed.

10. **Warnings when needed** — All LOW, no HIGH, insufficient data = explicit user warnings.

---

*Document prepared for FlipIQ Engineering*
*Training Samples v1.0 — December 31, 2024*
