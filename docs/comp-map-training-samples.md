# Comp Map Overlay - Training Samples
## OpenAI Assistant Output Examples
### Aligned with PIQ UI Format (v2.0)

---

# OUTPUT FORMAT REFERENCE

Based on actual PIQ interface, the bot must generate:

1. **ABC Summary** - Three columns (PROPERTY, LOT, LOCATION)
2. **KEEP Comps** - With confidence badges and ✓ match bullets
3. **REMOVE Comps** - With confidence badges and ✗ mismatch bullets

---

# SAMPLE 1: Golf Course Community - Terra Lago

## Input Context
- **Subject:** 84100 Terra Lago Pkwy, Indio CA 92203
- **Subject Specs:** 1,876 sqft, 4bd/2ba, Pool: Yes, Built 2005
- **Subject Tract:** Terra Lago
- **Subject Features:** Golf course front, 3-car garage, Solar owned
- **Comps Provided:** 4 pre-filtered comps

---

## Bot Output

### ABC SUMMARY

| PROPERTY | LOT | LOCATION |
|----------|-----|----------|
| **Garage: 3-car attached** | **Pool: In-ground heated** | **Golf Course: Front adjacency** |
| 2 comps have 2-car — extra capacity adds appeal | 1 comp has no pool — pool changes buyer pool | Premium positioning — 2 comps are interior |
| **Solar: Owned** | **Lot Shape: Rectangular** | **Tract: Terra Lago** |
| Owned solar adds value vs leased or none | Standard usable lot, no constraints | All comps same tract — strong anchor |
| | | **HOA: Guard gated** |
| | | Community amenities included |

---

### KEEP (2 comps)

```
☐ 84303 Eremo Way                     Confidence: High         $499K  SOLD
  Single Family / 4 Br / 2 Ba / 3 cars / 2005 / 2,122 ft² / 7,841 ft² / Pool: Yes

  ✓ Best comp - Model match, same tract
  ✓ Golf course front like subject
  ✓ Flip condition - best ARV baseline
  ✓ CONDITION: Standard — 0.44 mi from subject

  ✓ WHY KEPT: Best comp - Model match, same tract
```

```
☐ 84521 Terra Lago Pkwy               Confidence: High         $650K  SOLD
  Single Family / 4 Br / 3 Ba / 2 cars / 2007 / 2,276 ft² / 8,500 ft² / Pool: No

  ✓ Same tract (Terra Lago)
  ✓ Recent flip with good condition
  ✓ Bed/bath close match (4/3 vs 4/2)
  ✓ CONDITION: Standard — 0.62 mi from subject

  ✓ WHY KEPT: Same tract (Terra Lago)
```

---

### REMOVE (2 comps)

**LOWER RELEVANCE**

```
☐ 84892 Lago Way                      Confidence: Weak         $800K  SOLD
  Single Family / 5 Br / 3 Ba / 3 cars / 2010 / 2,500 ft² / 10,000 ft² / Pool: Yes

  ✗ 5BR/3BA - Subject is 4BR/2BA, different buyer pool
  ✗ 378 sqft larger than subject
  ✗ Lot 2,159 sqft larger - affects value comparison
  ✗ CONDITION: Standard — 0.85 mi from subject

  ✗ WHY REMOVED: 5BR/3BA - Subject is 4BR/2BA, different buyer pool
```

```
☐ 84312 Avenue 43                     Confidence: Weak         $565K  SOLD
  Single Family / 4 Br / 2 Ba / 2 cars / 2004 / 2,054 ft² / 6,800 ft² / Pool: No

  ✗ NO POOL - Subject has pool
  ✗ NOT golf course location
  ✗ Interior street location
  ✗ CONDITION: Standard — 1.2 mi from subject

  ✗ WHY REMOVED: NO POOL - Subject has pool
```

---
---

# SAMPLE 2: Tract Mismatch Scenario - Eastmark

## Input Context
- **Subject:** 3456 Suburban Lane, Mesa AZ 85212
- **Subject Specs:** 1,800 sqft, 4bd/2ba, Pool: Yes, Built 2015
- **Subject Tract:** Eastmark Phase 3
- **Subject Features:** Interior lot, 2-car garage
- **Comps Provided:** 6 pre-filtered comps

---

## Bot Output

### ABC SUMMARY

| PROPERTY | LOT | LOCATION |
|----------|-----|----------|
| **Garage: 2-car attached** | **Pool: Standard** | **Tract: Eastmark Phase 3** |
| All comps match garage capacity | 2 comps have no pool — affects buyer pool | 4 comps same tract — 2 cross to Phase 2 |
| | **Lot Size: 5,500 sqft** | **HOA: Eastmark Community** |
| | 1 comp has larger lot (7,200 sqft) | All comps same HOA umbrella |

---

### KEEP (4 comps)

```
☐ 3478 Suburban Lane                  Confidence: High         $485K  SOLD
  Single Family / 4 Br / 2 Ba / 2 cars / 2016 / 1,825 ft² / 5,400 ft² / Pool: Yes

  ✓ Same street as subject
  ✓ Same tract (Eastmark Phase 3)
  ✓ Pool present, matching subject
  ✓ Closest comp - 0.05 mi
  ✓ CONDITION: Flip — 0.05 mi from subject

  ✓ WHY KEPT: Same street, same tract, best proximity
```

```
☐ 3401 Community Drive                Confidence: High         $479K  SOLD
  Single Family / 4 Br / 2 Ba / 2 cars / 2015 / 1,790 ft² / 5,600 ft² / Pool: Yes

  ✓ Same tract (Eastmark Phase 3)
  ✓ Pool present, matching subject
  ✓ Interior lot like subject
  ✓ CONDITION: Flip — 0.1 mi from subject

  ✓ WHY KEPT: Same tract, feature aligned
```

```
☐ 3520 Festival Way                   Confidence: High         $492K  PENDING
  Single Family / 4 Br / 2 Ba / 2 cars / 2015 / 1,810 ft² / 5,500 ft² / Pool: Yes

  ✓ Same tract (Eastmark Phase 3)
  ✓ Pool present, matching subject
  ✓ Fills pending status coverage
  ✓ CONDITION: Good — 0.12 mi from subject

  ✓ WHY KEPT: Same tract, pending status needed
```

```
☐ 3180 Desert Trails                  Confidence: High         $475K  ACTIVE
  Single Family / 4 Br / 2 Ba / 2 cars / 2014 / 1,750 ft² / 5,800 ft² / Pool: Yes

  ✓ Adjacent tract (Eastmark Phase 2)
  ✓ Same HOA umbrella
  ✓ Pool present, matching subject
  ✓ Fills active status coverage
  ✓ CONDITION: Good — 0.3 mi from subject

  ✓ WHY KEPT: Same HOA, fills active status
```

---

### REMOVE (2 comps)

**LOWER RELEVANCE**

```
☐ 3890 Market Street                  Confidence: Weak         $510K  SOLD
  Single Family / 4 Br / 2 Ba / 2 cars / 2016 / 1,840 ft² / 7,200 ft² / Pool: No

  ✗ NO POOL - Subject has pool
  ✗ Lot 1,700 sqft larger than subject
  ✗ Corner lot exposure (subject is interior)
  ✗ CONDITION: Good — 0.5 mi from subject

  ✗ WHY REMOVED: NO POOL - Subject has pool
```

**REDUNDANT**

```
☐ 3945 Community Drive                Confidence: Weak         $472K  SOLD
  Single Family / 4 Br / 2 Ba / 2 cars / 2015 / 1,780 ft² / 5,500 ft² / Pool: Yes

  ✗ Same tract but farther than #2
  ✗ Older sale date (6 months vs 2 months)
  ✗ Similar features to better-ranked comp
  ✗ CONDITION: Good — 0.55 mi from subject

  ✗ WHY REMOVED: Redundant - closer comp with same features exists
```

---
---

# SAMPLE 3: Weak Comp Set - Expansion Needed

## Input Context
- **Subject:** 890 Mountain View Road, Flagstaff AZ 86001
- **Subject Specs:** 1,600 sqft, 3bd/2ba, Pool: No, Built 1985
- **Subject Tract:** Flagstaff Ranch Unit 2
- **Comps Provided:** 3 pre-filtered comps

---

## Bot Output

### ⚠️ COMP SET WARNING

| Strength | Reason | Recommendation |
|----------|--------|----------------|
| **WEAK** | Only 3 comps available — below minimum of 6 | **EXPAND FILTERS** |

**Suggested Expansions:**
1. Distance: 0.5 mi → 1.0 mi (+3 potential comps)
2. COE Days: 180 → 365 (+2 potential comps)

---

### ABC SUMMARY

| PROPERTY | LOT | LOCATION |
|----------|-----|----------|
| **Garage: 2-car attached** | **Pool: None** | **Tract: Flagstaff Ranch Unit 2** |
| All comps match | Market norm for area | 2 comps same tract — 1 adjacent |
| **Year Built: 1985** | | **School District: Flagstaff Unified** |
| Similar vintage (1980-1990) | | All comps same district |

---

### KEEP (3 comps)

```
☐ 912 Mountain View Road              Confidence: High         $425K  SOLD
  Single Family / 3 Br / 2 Ba / 2 cars / 1987 / 1,650 ft² / 8,000 ft² / Pool: No

  ✓ Same tract (Flagstaff Ranch Unit 2)
  ✓ Same street as subject
  ✓ Similar vintage (1987 vs 1985)
  ✓ No pool, matching subject
  ✓ CONDITION: Good — 0.1 mi from subject

  ✓ WHY KEPT: Best comp - same street, same tract
```

```
☐ 845 Pine Cone Lane                  Confidence: High         $410K  SOLD
  Single Family / 3 Br / 2 Ba / 2 cars / 1983 / 1,580 ft² / 7,500 ft² / Pool: No

  ✓ Same tract (Flagstaff Ranch Unit 2)
  ✓ Similar sqft (1,580 vs 1,600)
  ✓ CONDITION: Original — 0.3 mi from subject

  ✓ WHY KEPT: Same tract, data limited market
```

```
☐ 1020 Forest Trail                   Confidence: High         $440K  ACTIVE
  Single Family / 3 Br / 2 Ba / 2 cars / 1988 / 1,700 ft² / 8,500 ft² / Pool: No

  ✓ Same tract (Flagstaff Ranch Unit 2)
  ✓ Only active status available
  ✓ Similar vintage
  ✓ CONDITION: Good — 0.4 mi from subject

  ✓ WHY KEPT: Fills active status coverage
```

---

### REMOVE (0 comps)

*No comps removed — set is below minimum. All available comps retained.*

---

### STATUS COVERAGE

| Status | Count | Minimum | Status |
|--------|-------|---------|--------|
| Sold | 2 | 3 | ⚠️ Below minimum |
| Pending | 0 | 1 | ❌ Missing |
| Active | 1 | 1 | ✓ Met |

---
---

# SAMPLE 4: Micro-Market Barrier Detection

## Input Context
- **Subject:** 7890 West Camelback, Glendale AZ 85303
- **Subject Specs:** 1,400 sqft, 3bd/2ba, Pool: No, Built 1978
- **Subject Tract:** Westgate Terrace
- **Comps Provided:** 5 pre-filtered comps

---

## Bot Output

### ABC SUMMARY

| PROPERTY | LOT | LOCATION |
|----------|-----|----------|
| **Garage: 2-car attached** | **Pool: None** | **Tract: Westgate Terrace** |
| 1 comp has 1-car — capacity difference | Market norm for area | 3 comps same tract — 2 cross barriers |
| **Year Built: 1978** | **Lot Size: 6,500 sqft** | **⚠️ BARRIER: Camelback Road** |
| Range 1975-1985 across comps | Standard for tract | 2 comps north of 6-lane arterial |

---

### KEEP (3 comps)

```
☐ 7856 West Camelback                 Confidence: High         $345K  SOLD
  Single Family / 3 Br / 2 Ba / 2 cars / 1980 / 1,450 ft² / 6,200 ft² / Pool: No

  ✓ Same tract (Westgate Terrace)
  ✓ Same side of Camelback Road
  ✓ No barrier crossing
  ✓ CONDITION: Flip — 0.08 mi from subject

  ✓ WHY KEPT: Best comp - same tract, no barriers
```

```
☐ 7912 West Montecito                 Confidence: High         $338K  SOLD
  Single Family / 3 Br / 2 Ba / 2 cars / 1978 / 1,380 ft² / 6,500 ft² / Pool: No

  ✓ Same tract (Westgate Terrace)
  ✓ Same side of Camelback Road
  ✓ Same vintage as subject
  ✓ CONDITION: Good — 0.12 mi from subject

  ✓ WHY KEPT: Same tract, same side of barrier
```

```
☐ 7780 West Palmaire                  Confidence: High         $352K  PENDING
  Single Family / 3 Br / 2 Ba / 2 cars / 1982 / 1,480 ft² / 6,800 ft² / Pool: No

  ✓ Same tract (Westgate Terrace)
  ✓ Same side of Camelback Road
  ✓ Fills pending status
  ✓ CONDITION: Good — 0.15 mi from subject

  ✓ WHY KEPT: Same tract, fills pending status
```

---

### REMOVE (2 comps)

**LOWER RELEVANCE**

```
☐ 7750 West Bethany Home              Confidence: Weak         $365K  SOLD
  Single Family / 3 Br / 2 Ba / 2 cars / 1979 / 1,420 ft² / 6,400 ft² / Pool: No

  ✗ CROSSES BARRIER: North of Camelback Road (6-lane arterial)
  ✗ Different tract (Bethany Estates)
  ✗ Different school zone (Washington vs Pendergast)
  ✗ CONDITION: Flip — 0.35 mi from subject

  ✗ WHY REMOVED: Crosses Camelback Road barrier - different micro-market
```

```
☐ 7680 West Georgia                   Confidence: Weak         $355K  ACTIVE
  Single Family / 3 Br / 2 Ba / 1 car / 1976 / 1,350 ft² / 6,000 ft² / Pool: No

  ✗ CROSSES BARRIER: North of Camelback Road
  ✗ Different tract (Georgia Heights)
  ✗ 1-car garage (subject has 2-car)
  ✗ CONDITION: Original — 0.4 mi from subject

  ✗ WHY REMOVED: Crosses barrier + garage capacity mismatch
```

---
---

# SAMPLE 5: High-Volume Pruning Required

## Input Context
- **Subject:** 5678 Desert Vista, Scottsdale AZ 85260
- **Subject Specs:** 2,800 sqft, 5bd/3ba, Pool: Yes, Built 2010
- **Subject Tract:** DC Ranch Parcel 4.2
- **Subject Features:** Guard-gated, golf community
- **Comps Provided:** 15 pre-filtered comps

---

## Bot Output

### PRUNING APPLIED

| Original | Final | Pruned | Reason |
|----------|-------|--------|--------|
| 15 | 9 | 6 | Redundancy + Lower Relevance |

**Pruning Logic:**
- 3 removed: Farther distance duplicates
- 2 removed: Older sale dates (>6 months)
- 1 removed: Different HOA tier

---

### ABC SUMMARY

| PROPERTY | LOT | LOCATION |
|----------|-----|----------|
| **Garage: 3-car attached** | **Pool: Heated with spa** | **Tract: DC Ranch Parcel 4.2** |
| All comps have 3-car | 2 comps pool-only (no spa) | 6 comps same parcel — 3 adjacent parcels |
| **Stories: 2-story** | **Lot Size: 12,000 sqft** | **HOA: DC Ranch (Guard-gated)** |
| 2 comps are single-story | Range 10K-15K across comps | All comps same HOA umbrella |

---

### KEEP (6 comps)

```
☐ 5702 Desert Vista                   Confidence: High         $1.25M  SOLD
  Single Family / 5 Br / 3.5 Ba / 3 cars / 2011 / 2,850 ft² / 11,500 ft² / Pool: Yes

  ✓ Same tract (DC Ranch Parcel 4.2)
  ✓ Same street as subject
  ✓ Bed/bath match (5/3.5 vs 5/3)
  ✓ Pool with spa like subject
  ✓ CONDITION: Flip — 0.08 mi from subject

  ✓ WHY KEPT: Best comp - same street, same tract, feature aligned
```

```
☐ 5544 Mirabel Club                   Confidence: High         $1.18M  SOLD
  Single Family / 5 Br / 3 Ba / 3 cars / 2009 / 2,720 ft² / 12,200 ft² / Pool: Yes

  ✓ Same tract (DC Ranch Parcel 4.2)
  ✓ Guard-gated like subject
  ✓ Pool present
  ✓ CONDITION: Flip — 0.25 mi from subject

  ✓ WHY KEPT: Same tract, condition aligned
```

```
☐ 5890 Thompson Peak                  Confidence: High         $1.22M  PENDING
  Single Family / 5 Br / 3 Ba / 3 cars / 2010 / 2,780 ft² / 11,800 ft² / Pool: Yes

  ✓ Adjacent tract (DC Ranch Parcel 5.1)
  ✓ Same HOA umbrella
  ✓ Fills pending status
  ✓ CONDITION: Good — 0.4 mi from subject

  ✓ WHY KEPT: Same HOA, fills pending status
```

```
☐ 6012 Pinnacle Vista                 Confidence: High         $1.35M  ACTIVE
  Single Family / 5 Br / 4 Ba / 3 cars / 2012 / 2,950 ft² / 13,000 ft² / Pool: Yes

  ✓ Adjacent tract (DC Ranch Parcel 5.1)
  ✓ Same HOA umbrella
  ✓ Fills active status
  ✓ CONDITION: Flip — 0.5 mi from subject

  ✓ WHY KEPT: Same HOA, fills active status
```

```
☐ 5680 Dynamite Blvd                  Confidence: High         $1.15M  SOLD
  Single Family / 5 Br / 3 Ba / 3 cars / 2008 / 2,650 ft² / 10,500 ft² / Pool: Yes

  ✓ Same tract (DC Ranch Parcel 4.2)
  ✓ Provides sold depth
  ✓ CONDITION: Good — 0.35 mi from subject

  ✓ WHY KEPT: Same tract, sold status depth
```

```
☐ 5820 Silverleaf                     Confidence: High         $1.28M  SOLD
  Single Family / 5 Br / 3.5 Ba / 3 cars / 2011 / 2,820 ft² / 12,000 ft² / Pool: Yes

  ✓ Same tract (DC Ranch Parcel 4.2)
  ✓ Most recent sale (45 days)
  ✓ CONDITION: Flip — 0.3 mi from subject

  ✓ WHY KEPT: Same tract, most recent sale
```

---

### REMOVE (3 comps)

**LOWER RELEVANCE**

```
☐ 7100 E Carefree Hwy                 Confidence: Weak         $1.45M  SOLD
  Single Family / 5 Br / 4 Ba / 3 cars / 2015 / 3,200 ft² / 18,000 ft² / Pool: Yes

  ✗ Different HOA tier (Desert Mountain private club)
  ✗ 400 sqft larger than subject
  ✗ Lot 6,000 sqft larger
  ✗ CONDITION: Flip — 1.1 mi from subject

  ✗ WHY REMOVED: Different HOA tier - not comparable community
```

```
☐ 6234 Blue Sky Trail                 Confidence: Weak         $1.08M  SOLD
  Single Family / 4 Br / 3 Ba / 3 cars / 2008 / 2,450 ft² / 10,000 ft² / Pool: Yes

  ✗ Different HOA (Grayhawk)
  ✗ 4BR - Subject is 5BR, different buyer pool
  ✗ 350 sqft smaller than subject
  ✗ CONDITION: Good — 0.9 mi from subject

  ✗ WHY REMOVED: Different HOA + bedroom count mismatch
```

**REDUNDANT**

```
☐ 5920 Thompson Peak                  Confidence: Weak         $1.19M  SOLD
  Single Family / 5 Br / 3 Ba / 3 cars / 2010 / 2,750 ft² / 11,500 ft² / Pool: Yes

  ✗ Same tract as #3 but farther
  ✗ Older sale date (5 months vs 2 months)
  ✗ Similar features to better-ranked comp
  ✗ CONDITION: Good — 0.45 mi from subject

  ✗ WHY REMOVED: Redundant - #3 covers this tract with better recency
```

---

### Pruned Comps (Collapsed)

*Click "Show All 15 Comps" to reveal 6 additional pruned comps*

| Address | Reason |
|---------|--------|
| 5850 Desert Vista | Farther duplicate of #1 |
| 5990 Mirabel Club | Farther duplicate of #2 |
| 6100 Silverleaf | Older sale date (8 months) |
| 6250 Thompson Peak | Farther duplicate of #3 |
| 5780 Dynamite Blvd | Older sale date (7 months) |
| 6400 Pinnacle Vista | Farther duplicate of #4 |

---
---

# TRAINING RULES SUMMARY

## Output Format Requirements

1. **ABC Summary** must have three columns: PROPERTY, LOT, LOCATION
2. **Each column item** needs: Label + Explanation with comp comparison
3. **KEEP comps** use green ✓ checkmarks
4. **REMOVE comps** use red ✗ marks
5. **Every comp** needs: Address, Confidence badge, Price, Status, Property specs line
6. **WHY KEPT / WHY REMOVED** must be a single clear sentence

## Content Rules

1. **Silence = Correct** - Only show variables that differ or are notable
2. **Never mention price as ranking factor**
3. **Tract matching is highest priority**
4. **Condition hierarchy**: FLIP > GOOD > ORIGINAL > FIXER
5. **Status coverage** matters: aim for Sold + Pending + Active
6. **Barriers create warnings**, not automatic removal
7. **Redundant ≠ Lower Relevance** - keep these categories separate

## Confidence Levels

| Level | When to Use |
|-------|-------------|
| **High** | Same tract + condition aligned + feature matched |
| **Weak** | Tract mismatch OR condition gap OR feature mismatch |

---

*Training Samples v2.0 — December 28, 2024*
*Aligned with actual PIQ UI format*
*For OpenAI Assistant Configuration*
