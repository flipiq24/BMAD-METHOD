# Comp Map Overlay - Training Samples
## OpenAI Assistant Output Examples

---

# SAMPLE 1: Strong Comp Set - Same Tract

## Input Context
- **Subject:** 1234 Oak Street, Phoenix AZ 85048
- **Subject Specs:** 2,100 sqft, 4bd/2ba, Pool, Built 2005
- **Subject Tract:** Ahwatukee Foothills Unit 12
- **Target Condition:** FLIP
- **Comps Provided:** 8 pre-filtered comps

---

## Bot Output

### Comp Set Assessment

| Strength | Reason | Action |
|----------|--------|--------|
| **STRONG** | 5 of 8 comps in same tract with good status coverage | **PROCEED** |

---

### Ranked Comps

#### #1 — KEEP
**1248 Oak Street** | SOLD | 0.1 mi | FLIP

- Same tract (Ahwatukee Foothills Unit 12)
- Condition aligned: recent flip with similar renovation scope
- Pool present, matching subject
- Interior lot position, same as subject

*To verify condition details, go to Property Details → Condition Notes*

---

#### #2 — KEEP
**1301 Maple Drive** | SOLD | 0.2 mi | FLIP

- Same tract (Ahwatukee Foothills Unit 12)
- Condition aligned: sold as flip within 90 days
- Same school district (Kyrene)

*To verify renovation scope, go to Property Details → Remarks*

---

#### #3 — KEEP
**1189 Oak Street** | PENDING | 0.15 mi | GOOD

- Same tract (Ahwatukee Foothills Unit 12)
- Condition: GOOD (not FLIP, but well-maintained)
- Fills pending status coverage

*To update condition assessment, go to Property Details → Condition*

---

#### #4 — KEEP
**1422 Pine Court** | ACTIVE | 0.3 mi | FLIP

- Same tract (Ahwatukee Foothills Unit 12)
- Condition aligned: listed as renovated
- Fills active status coverage
- Corner lot (subject is interior)

---

#### #5 — KEEP
**1567 Elm Avenue** | SOLD | 0.4 mi | GOOD

- Same tract (Ahwatukee Foothills Unit 12)
- Condition: GOOD (secondary anchor)
- Pool present, matching subject

---

#### #6 — KEEP
**1890 Cedar Lane** | PENDING | 0.35 mi | FLIP

- Same tract (Ahwatukee Foothills Unit 12)
- Condition aligned: flip
- Additional pending for status depth

---

#### #7 — REMOVE (Lower Relevance)
**2234 Birch Road** | SOLD | 0.6 mi | ORIGINAL

- Different tract (Ahwatukee Foothills Unit 8)
- Condition mismatch: ORIGINAL, not aligned with FLIP target
- Crosses major subdivision boundary

*Retained in list for reference. To restore, click KEEP.*

---

#### #8 — REMOVE (Lower Relevance)
**2890 Willow Street** | ACTIVE | 0.8 mi | FIXER

- Different tract (Lakewood Unit 3)
- Condition mismatch: FIXER serves as context only
- Different school district (Tempe Union)

*Retained in list for reference. To restore, click KEEP.*

---

### Summary
6 comps retained (3 Sold, 2 Pending, 1 Active) — all within subject tract. Strong alignment for ARV analysis.

---
---

# SAMPLE 2: Moderate Comp Set - Tract Mismatches

## Input Context
- **Subject:** 5678 Desert Vista, Scottsdale AZ 85260
- **Subject Specs:** 2,800 sqft, 5bd/3ba, No Pool, Built 2010
- **Subject Tract:** DC Ranch Parcel 4.2
- **Target Condition:** FLIP
- **Comps Provided:** 10 pre-filtered comps

---

## Bot Output

### Comp Set Assessment

| Strength | Reason | Action |
|----------|--------|--------|
| **MODERATE** | Only 2 comps in subject tract; others cross HOA boundaries | **PROCEED with caution** |

---

### Ranked Comps

#### #1 — KEEP
**5702 Desert Vista** | SOLD | 0.08 mi | FLIP

- Same tract (DC Ranch Parcel 4.2)
- Same HOA (DC Ranch Community Council)
- Condition aligned: confirmed flip via transaction history
- No pool, matching subject

*To verify flip confirmation, go to Property Details → Transaction History*

---

#### #2 — KEEP
**5544 Mirabel Club** | SOLD | 0.25 mi | FLIP

- Same tract (DC Ranch Parcel 4.2)
- Condition aligned: renovated per remarks
- Guard-gated community, same as subject

---

#### #3 — KEEP
**5890 Thompson Peak** | PENDING | 0.4 mi | GOOD

- Different tract (DC Ranch Parcel 5.1)
- Same HOA umbrella (DC Ranch)
- Condition: GOOD (not FLIP, secondary anchor)
- Fills pending status coverage

*Note: Different parcel within DC Ranch. Similar buyer pool but micro-location differs.*

---

#### #4 — KEEP
**6012 Pinnacle Vista** | ACTIVE | 0.5 mi | FLIP

- Different tract (DC Ranch Parcel 5.1)
- Same HOA umbrella (DC Ranch)
- Condition aligned: flip
- Fills active status coverage

---

#### #5 — KEEP
**6234 Blue Sky Trail** | SOLD | 0.55 mi | FLIP

- Different tract (Grayhawk Raptor)
- Different HOA (Grayhawk)
- Condition aligned: flip
- Guard-gated community, comparable tier

*Note: Grayhawk is similar market segment but different community. Use with awareness.*

---

#### #6 — KEEP
**6501 Dynamite Blvd** | PENDING | 0.6 mi | GOOD

- Different tract (Grayhawk Talon)
- Different HOA (Grayhawk)
- Condition: GOOD
- Backs to golf course (subject does not)

*Location note: Golf course adjacency may create premium not applicable to subject.*

---

#### #7 — REMOVE (Redundant)
**5920 Thompson Peak** | SOLD | 0.45 mi | FLIP

- Different tract (DC Ranch Parcel 5.1)
- Redundant: #3 and #4 already cover this tract with better recency
- Older sale date (5 months vs 2 months)

---

#### #8 — REMOVE (Redundant)
**6180 Blue Sky Trail** | SOLD | 0.52 mi | FLIP

- Different tract (Grayhawk Raptor)
- Redundant: #5 already covers this tract with closer proximity
- Similar condition and features

---

#### #9 — REMOVE (Lower Relevance)
**7100 E Carefree Hwy** | SOLD | 1.1 mi | ORIGINAL

- Different tract (Desert Mountain)
- Condition mismatch: ORIGINAL
- Different HOA tier (Desert Mountain private club)
- Crosses Scottsdale Rd barrier

---

#### #10 — REMOVE (Lower Relevance)
**4521 N Pima Road** | ACTIVE | 1.3 mi | FIXER

- Different tract (Kierland)
- Condition mismatch: FIXER
- Non-gated community (subject is guard-gated)
- Different buyer pool segment

---

### Summary
6 comps retained. Only 2 in subject tract — consider this limitation when presenting ARV. Grayhawk comps included for market context but note HOA difference.

---
---

# SAMPLE 3: Weak Comp Set - Expansion Needed

## Input Context
- **Subject:** 890 Mountain View Road, Flagstaff AZ 86001
- **Subject Specs:** 1,600 sqft, 3bd/2ba, No Pool, Built 1985
- **Subject Tract:** Flagstaff Ranch Unit 2
- **Target Condition:** FLIP
- **Comps Provided:** 4 pre-filtered comps

---

## Bot Output

### Comp Set Assessment

| Strength | Reason | Action |
|----------|--------|--------|
| **WEAK** | Only 4 comps available; no pending status coverage; limited tract data | **EXPAND FILTERS** |

---

### Expansion Recommendations

| Priority | Filter | Current | Suggested | Potential New Comps |
|----------|--------|---------|-----------|---------------------|
| 1 | Distance | 0.5 mi | 1.0 mi | +3 estimated |
| 2 | COE Days | 180 | 365 | +2 estimated |
| 3 | Year Built | 1980-1990 | 1975-1995 | +2 estimated |

*Click "Apply Expansion" to preview additional comps on map*

---

### Ranked Comps

#### #1 — KEEP
**912 Mountain View Road** | SOLD | 0.1 mi | GOOD

- Same tract (Flagstaff Ranch Unit 2)
- Condition: GOOD (no FLIP available in tract)
- Closest comp to subject
- Similar vintage (1987)

*Note: No FLIP comps available in tract. GOOD condition used as primary anchor.*

*To update condition, go to Property Details → Condition*

---

#### #2 — KEEP
**845 Pine Cone Lane** | SOLD | 0.3 mi | ORIGINAL

- Same tract (Flagstaff Ranch Unit 2)
- Condition: ORIGINAL (data limited market)
- Retained due to tract match despite condition gap

*Note: ORIGINAL condition retained because market is data limited (<3 sold in tract within 180 days).*

---

#### #3 — KEEP
**1020 Forest Trail** | ACTIVE | 0.4 mi | GOOD

- Same tract (Flagstaff Ranch Unit 2)
- Condition: GOOD
- Only active status comp available
- Fills active coverage

---

#### #4 — KEEP
**780 Aspen Way** | SOLD | 0.5 mi | ORIGINAL

- Different tract (Flagstaff Ranch Unit 1)
- Condition: ORIGINAL
- Retained for sold status depth despite condition gap

*Location note: Unit 1 is adjacent subdivision with similar characteristics.*

---

### Status Coverage Gap

| Status | Count | Minimum | Gap |
|--------|-------|---------|-----|
| Sold | 3 | 3 | ✓ Met |
| Pending | 0 | 1 | ✗ Missing |
| Active | 1 | 1 | ✓ Met |

**Recommendation:** Expand distance filter to 1.0 mi to capture pending comps from adjacent areas.

---

### Summary
4 comps retained but set is WEAK. No FLIP condition comps available. No pending status coverage. Recommend filter expansion before ARV analysis.

---
---

# SAMPLE 4: Pruning Required - Over 12 Comps

## Input Context
- **Subject:** 3456 Suburban Lane, Mesa AZ 85212
- **Subject Specs:** 1,800 sqft, 4bd/2ba, Pool, Built 2015
- **Subject Tract:** Eastmark Phase 3
- **Target Condition:** FLIP
- **Comps Provided:** 18 pre-filtered comps

---

## Bot Output

### Comp Set Assessment

| Strength | Reason | Action |
|----------|--------|--------|
| **STRONG** | Abundant comps in tract; pruned from 18 to 11 | **PROCEED** |

---

### Pruning Applied

| Original Count | Final Count | Pruned |
|----------------|-------------|--------|
| 18 | 11 | 7 |

**Pruning Logic Applied:**
1. Removed 3 farther distance duplicates (kept closest)
2. Removed 2 older sale dates (kept most recent)
3. Removed 2 weaker condition matches (kept FLIP/GOOD over ORIGINAL)

*Click "Show All 18 Comps" to view pruned comps*

---

### Ranked Comps

#### #1 — KEEP
**3478 Suburban Lane** | SOLD | 0.05 mi | FLIP

- Same tract (Eastmark Phase 3)
- Same street as subject
- Condition aligned: confirmed flip
- Pool present, matching subject
- Sold within 45 days

---

#### #2 — KEEP
**3401 Community Drive** | SOLD | 0.1 mi | FLIP

- Same tract (Eastmark Phase 3)
- Condition aligned: flip per transaction history
- Pool present
- Interior lot, same as subject

---

#### #3 — KEEP
**3520 Festival Way** | SOLD | 0.12 mi | FLIP

- Same tract (Eastmark Phase 3)
- Condition aligned: renovated listing
- Pool present
- Sold within 60 days

---

#### #4 — KEEP
**3389 Market Street** | PENDING | 0.08 mi | FLIP

- Same tract (Eastmark Phase 3)
- Condition aligned: flip
- Pool present
- Best pending comp available

---

#### #5 — KEEP
**3445 Park Promenade** | PENDING | 0.15 mi | GOOD

- Same tract (Eastmark Phase 3)
- Condition: GOOD (secondary)
- No pool (subject has pool)
- Fills pending depth

*Feature note: No pool — adjust comparison accordingly.*

---

#### #6 — KEEP
**3560 Eastmark Pkwy** | ACTIVE | 0.2 mi | FLIP

- Same tract (Eastmark Phase 3)
- Condition aligned: flip
- Pool present
- Best active comp available

---

#### #7 — KEEP
**3612 Signal Butte** | ACTIVE | 0.25 mi | FLIP

- Same tract (Eastmark Phase 3)
- Condition aligned: flip
- Pool present
- Corner lot (subject is interior)

---

#### #8 — KEEP
**3298 Gather Drive** | SOLD | 0.18 mi | GOOD

- Same tract (Eastmark Phase 3)
- Condition: GOOD (not FLIP)
- Pool present
- Provides GOOD condition baseline

---

#### #9 — KEEP
**3701 Festival Way** | PENDING | 0.22 mi | GOOD

- Same tract (Eastmark Phase 3)
- Condition: GOOD
- Pool present
- Additional pending depth

---

#### #10 — KEEP
**3180 Desert Trails** | SOLD | 0.3 mi | FLIP

- Adjacent tract (Eastmark Phase 2)
- Same HOA (Eastmark Community)
- Condition aligned: flip
- Retained for additional sold depth

---

#### #11 — KEEP
**3055 Elliot Road** | ACTIVE | 0.35 mi | GOOD

- Adjacent tract (Eastmark Phase 2)
- Same HOA (Eastmark Community)
- Condition: GOOD
- Busy street frontage (Elliot Rd arterial)

*Location note: Elliot Road frontage — traffic exposure may affect value.*

---

### Pruned Comps (Click to Expand)

| Address | Status | Distance | Reason Pruned |
|---------|--------|----------|---------------|
| 3890 Market Street | SOLD | 0.5 mi | Farther duplicate of #2 |
| 3945 Community Drive | SOLD | 0.55 mi | Farther duplicate of #3 |
| 3820 Festival Way | SOLD | 0.48 mi | Farther duplicate of #3 |
| 3678 Park Promenade | PENDING | 0.4 mi | Older date than #4, #5 |
| 3756 Gather Drive | PENDING | 0.42 mi | Older date than #9 |
| 3290 Suburban Lane | SOLD | 0.35 mi | ORIGINAL condition |
| 3412 Desert Trails | ACTIVE | 0.5 mi | ORIGINAL condition |

---

### Summary
11 comps retained (5 Sold, 3 Pending, 3 Active). Excellent tract coverage with strong FLIP alignment. 7 redundant comps pruned automatically.

---
---

# SAMPLE 5: Micro-Market Barriers Present

## Input Context
- **Subject:** 7890 West Camelback, Glendale AZ 85303
- **Subject Specs:** 1,400 sqft, 3bd/2ba, No Pool, Built 1978
- **Subject Tract:** Westgate Terrace
- **Target Condition:** FLIP
- **Comps Provided:** 9 pre-filtered comps

---

## Bot Output

### Comp Set Assessment

| Strength | Reason | Action |
|----------|--------|--------|
| **MODERATE** | Strong tract comps but several cross major arterial barriers | **PROCEED with caution** |

---

### Micro-Market Barriers Detected

| Barrier | Type | Comps Affected |
|---------|------|----------------|
| Camelback Road | Major Arterial (6 lanes) | #6, #7 |
| Grand Avenue | Diagonal Arterial | #8 |
| Glendale Ave | Major Arterial (4 lanes) | #9 |

*Comps crossing barriers may reflect different micro-market pricing.*

---

### Ranked Comps

#### #1 — KEEP
**7856 West Camelback** | SOLD | 0.08 mi | FLIP

- Same tract (Westgate Terrace)
- Same side of Camelback Road as subject
- Condition aligned: flip confirmed
- Similar vintage (1980)

*Best comp — same tract, no barriers, condition aligned.*

---

#### #2 — KEEP
**7912 West Montecito** | SOLD | 0.12 mi | FLIP

- Same tract (Westgate Terrace)
- Same side of Camelback Road
- Condition aligned: flip
- Interior street (quieter than subject)

---

#### #3 — KEEP
**7780 West Palmaire** | PENDING | 0.15 mi | GOOD

- Same tract (Westgate Terrace)
- Same side of Camelback Road
- Condition: GOOD (not FLIP)
- Fills pending status

---

#### #4 — KEEP
**7945 West Montecito** | ACTIVE | 0.18 mi | FLIP

- Same tract (Westgate Terrace)
- Same side of Camelback Road
- Condition aligned: flip
- Fills active status

---

#### #5 — KEEP
**8010 West Palmaire** | SOLD | 0.2 mi | GOOD

- Same tract (Westgate Terrace)
- Same side of Camelback Road
- Condition: GOOD
- Provides secondary anchor

---

#### #6 — KEEP (with note)
**7750 West Bethany Home** | SOLD | 0.35 mi | FLIP

- Different tract (Bethany Estates)
- **CROSSES BARRIER: North of Camelback Road**
- Condition aligned: flip
- Different school zone (Washington Elementary vs Pendergast)

*Barrier note: This comp is north of Camelback Rd (6-lane arterial). Micro-market pricing may differ. Use with awareness.*

*To adjust school district, go to Property Details → Location*

---

#### #7 — KEEP (with note)
**7680 West Georgia** | PENDING | 0.4 mi | FLIP

- Different tract (Georgia Heights)
- **CROSSES BARRIER: North of Camelback Road**
- Condition aligned: flip
- Retained for pending depth

*Barrier note: North of Camelback Rd. Consider proximity discount when comparing.*

---

#### #8 — REMOVE (Lower Relevance)
**6890 North 79th Ave** | SOLD | 0.6 mi | FLIP

- Different tract (Grand Terrace)
- **CROSSES BARRIER: West of Grand Avenue**
- Condition aligned but location significantly different
- Grand Avenue creates strong pricing break

*Grand Avenue barrier typically creates 5-10% pricing differential in this area.*

---

#### #9 — REMOVE (Lower Relevance)
**8234 West Glendale Ave** | ACTIVE | 0.7 mi | ORIGINAL

- Different tract (Glendale Heights)
- **CROSSES BARRIER: North of Glendale Avenue**
- Condition mismatch: ORIGINAL
- Busy street frontage (Glendale Ave)
- Different school district

---

### Map Legend
- 🔴 Subject Property
- 🟢 KEEP — Same side of barriers
- 🟡 KEEP — Crosses barrier (use with note)
- ⚪ REMOVE — Crosses major barrier + other issues

---

### Summary
7 comps retained (3 Sold, 2 Pending, 2 Active). 5 comps are on same side of all barriers (strongest). 2 comps cross Camelback Rd — use with micro-market awareness. Grand Avenue and Glendale Ave barriers exclude 2 comps.

---
---

# TRAINING NOTES FOR ASSISTANT

## Key Behaviors to Learn

1. **Ranking is by RELEVANCE, never price** — Notice samples never mention $/sqft as ranking factor

2. **Silence = Correct** — Variables only mentioned when relevant (e.g., pool only mentioned when it differs)

3. **KEEP vs REMOVE reasons are distinct:**
   - Lower Relevance = structural/location mismatch
   - Redundant = better-aligned comp exists

4. **Tract matching is highest priority** — Same tract comps always rank higher

5. **Barriers create notes, not automatic removal** — Comps crossing barriers can be KEEP with warning

6. **Condition hierarchy:** FLIP → GOOD → ORIGINAL → FIXER

7. **Status coverage matters:** Always try to maintain Sold + Pending + Active representation

8. **Manual update guidance included** — Each sample shows "To update X, go to Y" language

9. **Comp Set Strength drives action:**
   - STRONG → PROCEED
   - MODERATE → PROCEED with caution
   - WEAK → EXPAND FILTERS

10. **Pruning is deterministic** — When >12 comps, pruning order is: Distance → Date → Condition → Features

---

*Training Samples v1.0 — December 28, 2024*
*For OpenAI Assistant Configuration*
