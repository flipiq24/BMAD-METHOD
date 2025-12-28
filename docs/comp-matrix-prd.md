# Comp Matrix Value Intelligence Bot
## Product Requirements Document (PRD)
### Version 3.0 — LEAN Edition

---

**Status:** P0 – Critical Path
**Owner:** FlipIQ
**Primary User:** Acquisition Associate (AA)
**UI Location:** PIQ → Comps → Matrix View
**Dependencies:** Existing E-Value / Matrix / Map / List logic (UNCHANGED)

---

## 1. PRODUCT OVERVIEW

### 1.1 Purpose

The Comp Matrix Value Intelligence Bot is a **lean interpretation layer** that explains what the comp data means — without restating what the user can already see.

**This bot does not calculate value.**
**It does not replace pricing logic.**
**It does not restate visible data.**

It provides **interpretation, not information**.

### 1.2 Problem Statement — Why v3.0?

**v2.4 Problem:**
- 18 sections, 2,000+ words
- Restated data the user could already see
- Information overload
- Analysis paralysis

**v3.0 Solution:**
- 8 sections, focused interpretation
- What does the data MEAN?
- What is the market ceiling?
- What must be VERIFIED?

### 1.3 Product Goal

Enable Acquisition Associates to:

- Understand what the comp data means (not just what it shows)
- Know the relevancy of each data category
- See a clear E-Value outcome interpretation
- Understand market ceiling and push logic
- Know exactly what to verify before trusting the E-Value

### 1.4 Non-Goals (Hard Guardrails)

The system **MUST NOT:**

| ❌ Forbidden | Why |
|-------------|-----|
| Output a new value | Bot interprets, doesn't calculate |
| Restate visible data | User can see the matrix |
| List every comp | That's what the matrix does |
| Calculate ARV | Human decides ARV |
| Recommend an offer price | Outside scope |

---

## 2. THE ABSOLUTE RULE (Immutable)

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│  IF THE SYSTEM CANNOT PROVE UPSIDE WITH CLOSED EVIDENCE,    │
│  IT MUST BEHAVE AS IF UPSIDE DOES NOT EXIST.                │
│                                                             │
│  This rule cannot be overridden by any engineer, PM,        │
│  operator, or executive.                                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. LEAN OUTPUT STRUCTURE (8 SECTIONS)

Every response must follow this order:

```
1. PROPERTY IN QUESTION VS COMPS
2. MATRIX TABLE — ACTIVES
3. MATRIX TABLE — PENDINGS / BACKUPS
4. MATRIX TABLE — CLOSED SALES
5. E-VALUE INTERPRETATION
6. MARKET CEILING & PUSH LOGIC
7. WHAT MUST BE VERIFIED
8. CONCLUSION
```

---

## 4. SECTION SPECIFICATIONS

### 4.1 PROPERTY IN QUESTION VS COMPS

**Purpose:** Does the comp set actually represent the PIQ?

**Output Format:**
```
PROPERTY IN QUESTION VS COMPS
Relevancy: [HIGH / MID / LOW]

[2-4 sentences interpreting fit, not listing metrics]
```

**Relevancy Tags:**
| Tag | Meaning |
|-----|---------|
| **HIGH** | Material mismatch exists — average may not apply |
| **MID** | Some functional gaps but usable |
| **LOW** | Comps closely match PIQ profile — no adjustment needed |

**Note:** HIGH relevancy means the mismatch is important and needs attention. LOW relevancy means no issue exists.

**Example:**
```
PROPERTY IN QUESTION VS COMPS
Relevancy: HIGH

The PIQ is a 2-bedroom / 1-bath property, while higher-priced
comps are predominantly 3-bedroom / 2-bath homes. This represents
a fundamentally different buyer pool. The blended average includes
sales that are not comparable to the PIQ and likely overstates
its achievable value.
```

---

### 4.2 MATRIX TABLE — ACTIVES

**Purpose:** What is the competition signal from active listings?

**Output Format:**
```
MATRIX TABLE — ACTIVES
Relevancy: [HIGH / MID / LOW]

[2-4 sentences on competition signal, not listing actives]
```

**Relevancy Tags:**
| Tag | Meaning |
|-----|---------|
| **HIGH** | Active inventory creates meaningful pricing signal (pressure or scarcity) |
| **MID** | Some signal present but not dominant |
| **LOW** | Limited active competition, no significant signal |

**Example:**
```
MATRIX TABLE — ACTIVES
Relevancy: HIGH

There are five active listings. Three are in equal or superior
condition to the PIQ's post-rehab target and are priced between
$315,000 and $329,000. All three have been sitting for 58, 67,
and 71 days respectively, with two having taken price reductions.
The market is visibly rejecting this pricing tier.
```

---

### 4.3 MATRIX TABLE — PENDINGS / BACKUPS

**Purpose:** What is the directional signal from pending activity?

**Output Format:**
```
MATRIX TABLE — PENDINGS / BACKUPS
Relevancy: [HIGH / MID / LOW]

[2-4 sentences on directional signal, not listing pendings]
```

**Relevancy Tags:**
| Tag | Meaning |
|-----|---------|
| **HIGH** | Strong directional signal present |
| **MID** | Some signal but unconfirmed |
| **LOW** | Limited or no pending signal |

**Example:**
```
MATRIX TABLE — PENDINGS / BACKUPS
Relevancy: HIGH

There are three pendings. All three are contracting below the
closed sales average — pending average is $258/sqft versus
closed average of $281/sqft. Two of the three took 34 and 38
days to go under contract. This is a clear and consistent
downward signal across all pending activity.
```

---

### 4.4 MATRIX TABLE — CLOSED SALES

**Purpose:** What is the baseline integrity from closed transactions?

**Output Format:**
```
MATRIX TABLE — CLOSED SALES
Relevancy: [HIGH / MID / LOW]

[2-4 sentences on baseline reliability, not listing closed comps]
```

**Relevancy Tags:**
| Tag | Meaning |
|-----|---------|
| **HIGH** | Closed data provides critical signal (strong foundation OR outlier issue) |
| **MID** | Usable but some variance or gaps |
| **LOW** | Standard closed foundation, no special concerns |

**Example:**
```
MATRIX TABLE — CLOSED SALES
Relevancy: HIGH

There are seven closed sales with an average of $312/sqft.
However, one sale at $378/sqft on a 14,200 sqft lot with
permitted ADU is pulling the mean up materially. The remaining
six closed sales cluster tightly around $289/sqft. The average
is distorted by a non-transferable outlier.
```

---

### 4.5 E-VALUE INTERPRETATION

**Purpose:** What does the E-Value actually mean for this PIQ?

**Output Format:**
```
E-VALUE INTERPRETATION
Outcome: [OUTCOME TAG]

[2-4 sentences of interpretation — what the E-Value means, not what it is]
```

**Outcome Tags:**
| Tag | Meaning |
|-----|---------|
| **BASELINE CONFIRMED** | E-Value is reliable for this PIQ |
| **PUSH SUPPORTED** | Evidence supports exploring upper range |
| **BASELINE — CEILING CAPPED** | Baseline reliable but upside capped by market |
| **BASELINE + STRATEGY WARNING** | Baseline achievable but execution strategy matters |
| **CAUTION — [REASON]** | Material risk factors present |
| **CEILING BREACH — [REASON]** | E-Value may exceed what market will support |
| **VERIFY BEFORE MOVING** | Signals conflict — need confirmation |

**Caution Reasons:**
- AVERAGE OVERSTATED
- AVERAGE DISTORTED
- MARKET SOFTENING
- PRODUCT TYPE CONTAMINATION

**Ceiling Breach Reasons:**
- NO CLOSED PROOF

**Example:**
```
E-VALUE INTERPRETATION
Outcome: CAUTION — AVERAGE OVERSTATED

The E-Value of $298,670 uses the blended average, which includes
3/2 sales the PIQ cannot replicate without structural expansion.
The appropriate anchor for a 2/1 layout is approximately $241/sqft,
implying a value closer to $263,000. The E-Value may be overstated
by 12-14%.
```

---

### 4.6 MARKET CEILING & PUSH LOGIC

**Purpose:** Where is the true ceiling and what evidence supports pushing toward it?

**Output Format:**
```
MARKET CEILING & PUSH LOGIC
Relevancy: [HIGH / MID / LOW]

[2-4 sentences on ceiling definition and push evidence]
```

**Relevancy Tags:**
| Tag | Meaning |
|-----|---------|
| **HIGH** | Ceiling analysis is critical to decision |
| **MID** | Some ceiling considerations present |
| **LOW** | No significant ceiling concerns |

**Example:**
```
MARKET CEILING & PUSH LOGIC
Relevancy: HIGH

The ceiling is not defined by what sellers are asking or what
formulas produce — it is defined by what buyers are currently
refusing to pay. Three comparable actives sitting at 58-71 DOM
with price reductions establish a visible rejection point.
Pricing the PIQ into that zone introduces significant absorption risk.
```

---

### 4.7 WHAT MUST BE VERIFIED

**Purpose:** What questions must be answered before trusting this analysis?

**Output Format:**
```
WHAT MUST BE VERIFIED
• [Task 1]
• [Task 2]
• [Task 3]
• [Task 4]

[Optional: 1 sentence summary of verification priority]
```

**Rules:**
- 3-5 bullets typically
- Specific, actionable questions
- No generic "do more research" tasks
- Focus on deal-breaking unknowns

**Example:**
```
WHAT MUST BE VERIFIED
• Confirm no feasible layout expansion (can the PIQ be converted to 3/2?)
• Get contractor assessment on bedroom/bath addition feasibility and cost
• Confirm buyer resistance to 2/1 layouts at higher prices via agent feedback
• Recalculate acquisition basis using 2/1 cluster anchor (~$241/sqft)

If layout expansion is not feasible, the deal must work at the
2/1 price level or it does not work.
```

---

### 4.8 CONCLUSION

**Purpose:** Summary statement tying everything together.

**Output Format:**
```
CONCLUSION

[2-4 sentences summarizing the key finding and recommended stance]
```

**Example:**
```
CONCLUSION

The layout mismatch introduces real downward risk. The blended
average masks a structural disadvantage that buyers will recognize
immediately. The PIQ should anchor to the 2/1 cluster, and the
baseline should be treated as a ceiling rather than a starting point.
```

---

## 5. DATA SCHEMA

```typescript
interface MatrixIntelligenceOutput {
  piq_vs_comps: {
    relevancy: 'HIGH' | 'MID' | 'LOW';
    interpretation: string; // 2-4 sentences
  };

  actives: {
    relevancy: 'HIGH' | 'MID' | 'LOW';
    interpretation: string; // 2-4 sentences
  };

  pendings_backups: {
    relevancy: 'HIGH' | 'MID' | 'LOW';
    interpretation: string; // 2-4 sentences
  };

  closed_sales: {
    relevancy: 'HIGH' | 'MID' | 'LOW';
    interpretation: string; // 2-4 sentences
  };

  e_value_interpretation: {
    outcome: OutcomeTag;
    interpretation: string; // 2-4 sentences
  };

  market_ceiling: {
    relevancy: 'HIGH' | 'MID' | 'LOW';
    interpretation: string; // 2-4 sentences
  };

  verification_tasks: string[]; // 3-5 items
  verification_summary?: string; // optional summary

  conclusion: string; // 2-4 sentences
}

type OutcomeTag =
  | 'BASELINE CONFIRMED'
  | 'PUSH SUPPORTED'
  | 'BASELINE — CEILING CAPPED'
  | 'BASELINE + STRATEGY WARNING'
  | 'CAUTION — AVERAGE OVERSTATED'
  | 'CAUTION — AVERAGE DISTORTED'
  | 'CAUTION — MARKET SOFTENING'
  | 'CAUTION — PRODUCT TYPE CONTAMINATION'
  | 'CEILING BREACH — NO CLOSED PROOF'
  | 'VERIFY BEFORE MOVING';
```

---

## 6. ENGINEERING GUARDRAILS

### 6.1 Required Constraints

| Constraint | Enforcement |
|------------|-------------|
| Output must be schema-validated | No free-form responses |
| Sections cannot be omitted | All 8 sections required |
| Sections cannot be reordered | Fixed sequence |
| No data restatement | Interpret, don't list |
| No exact dollar values in narrative | Directional language preferred |
| The Absolute Rule honored | Unproven upside = nonexistent |

### 6.2 Forbidden Patterns

| ❌ Do NOT | ✅ Instead |
|-----------|-----------|
| "The comps are: 123 Main, 456 Oak..." | "There are seven closed sales forming a baseline..." |
| Only stating E-Value number | Interpret what the E-Value means |
| Listing individual actives with prices | "Three actives sitting at 58-71 DOM with price reductions" |
| Generic verification tasks | Specific actionable questions |

---

## 7. ACCEPTANCE CRITERIA (QA)

- [ ] All 8 sections present in order
- [ ] Each section 1-4 has relevancy tag (HIGH/MID/LOW)
- [ ] E-VALUE INTERPRETATION has outcome tag
- [ ] MARKET CEILING has relevancy tag
- [ ] WHAT MUST BE VERIFIED has 3-5 specific tasks
- [ ] CONCLUSION provides clear summary
- [ ] No data restatement (interpretation only)
- [ ] The Absolute Rule honored
- [ ] CEILING BREACH used when E-Value exceeds closed evidence

---

## 8. SAMPLE OUTPUT (Full Format)

```
PROPERTY IN QUESTION VS COMPS
Relevancy: LOW

The PIQ aligns structurally with the comp set. Layout and size
fall within the dominant buyer pool, and higher-priced comps
share the same functional profile. There is no buyer-pool
mismatch that would require discounting or adjustment.

MATRIX TABLE — ACTIVES
Relevancy: LOW

There are three active listings. All are priced consistently
with closed data and represent standard market positioning.
None introduce ceiling pressure or suggest buyer resistance
at the E-Value level.

MATRIX TABLE — PENDINGS / BACKUPS
Relevancy: LOW

There is one pending sale at a price consistent with the closed
average. It does not introduce directional movement in either
direction. No backup activity exists to suggest competitive bidding.

MATRIX TABLE — CLOSED SALES
Relevancy: HIGH

There are nine closed sales forming a tight, reliable baseline
around $267/sqft. Mean and median are within 2% of each other.
All sales represent similar condition and no single comp is
distorting the average. This establishes a dependable anchor.

E-VALUE INTERPRETATION
Outcome: BASELINE CONFIRMED

The E-Value of $301,910 accurately reflects current market
conditions for properties like the PIQ. The closed cluster is
tight, the PIQ aligns structurally, and no forward signals
suggest movement above or below the established range.

MARKET CEILING & PUSH LOGIC
Relevancy: LOW

The highest credible sold price defines a soft ceiling at
$312,000. However, no pending activity or scarcity signal
supports reaching for it. The E-Value sits appropriately
within the proven range without requiring verification of upside.

WHAT MUST BE VERIFIED
• Confirm no undisclosed PIQ negatives (busy street, backing issues, deferred maintenance)
• Confirm PIQ condition aligns with rehab scope assumptions
• Verify the PIQ does not have unique disadvantages versus the closed set

If verification confirms alignment, the E-Value can be trusted
as the working basis.

CONCLUSION

The dataset is clean and internally consistent. Closed sales
establish a reliable baseline, and no contradicting signals
exist in pending or active data. The E-Value represents market
reality without adjustment.
```

---

## 9. SCENARIO COVERAGE

The bot must handle these 10 core scenarios:

| # | Scenario | Typical Outcome |
|---|----------|-----------------|
| 1 | Clean Baseline | BASELINE CONFIRMED |
| 2 | Bed/Bath Mismatch | CAUTION — AVERAGE OVERSTATED |
| 3 | Outlier Distortion | CAUTION — AVERAGE DISTORTED |
| 4 | Actives Capping Value | BASELINE — CEILING CAPPED |
| 5 | Size Ceiling Breach | CEILING BREACH — NO CLOSED PROOF |
| 6 | Shifting Market | CAUTION — MARKET SOFTENING |
| 7 | Scarcity Upside | PUSH SUPPORTED |
| 8 | Over-Improvement Risk | BASELINE + STRATEGY WARNING |
| 9 | New Construction Contamination | CAUTION — PRODUCT TYPE CONTAMINATION |
| 10 | Mixed Signals | VERIFY BEFORE MOVING |

See `comp-matrix-training-samples.md` for full examples of each scenario.

---

## 10. ONE-LINE ENGINEERING SUMMARY

Build a lean interpretation layer that explains what comp data means across 8 sections: PIQ fit, actives, pendings, closed sales, E-Value interpretation, market ceiling logic, verification tasks, and conclusion — without restating visible data or providing false precision.

---

*Document prepared for FlipIQ Engineering*
*Version 3.0 (LEAN) — December 28, 2024*
