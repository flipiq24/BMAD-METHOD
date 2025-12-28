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
- 6 sections, 200-400 words
- Interpretation only
- What does the data MEAN?
- What must be VERIFIED?

### 1.3 Product Goal

Enable Acquisition Associates to:

- Understand what the comp data means (not just what it shows)
- Know the relevancy of each data category
- See a clear outcome interpretation
- Know exactly what to verify before trusting the E-Value

### 1.4 Non-Goals (Hard Guardrails)

The system **MUST NOT:**

| ❌ Forbidden | Why |
|-------------|-----|
| Output a new value | Bot interprets, doesn't calculate |
| Restate visible data | User can see the matrix |
| List every comp | That's what the matrix does |
| Provide 18 sections of analysis | Information overload |
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

## 3. LEAN OUTPUT STRUCTURE (6 SECTIONS ONLY)

Every response must follow this order with **200-400 words total**:

```
1. PIQ VS COMPS
2. ACTIVES
3. PENDINGS / BACKUPS
4. CLOSED SALES
5. E-VALUE INTERPRETATION
6. WHAT MUST BE VERIFIED
```

---

## 4. SECTION SPECIFICATIONS

### 4.1 PIQ VS COMPS

**Purpose:** Do the comps actually represent the PIQ?

**Output Format:**
```
PIQ VS COMPS: [RELEVANCY TAG]

[1-3 sentences interpreting fit, not listing metrics]
```

**Relevancy Tags:**
| Tag | Meaning |
|-----|---------|
| **HIGH** | Comps closely match PIQ profile |
| **MID** | Some functional gaps but usable |
| **LOW** | Material mismatch — average may not apply |

**Example:**
```
PIQ VS COMPS: MID

Comps skew larger (avg 1,650 sqft vs PIQ 1,380). This pulls
the average up. PIQ may trade closer to the lower tier unless
expansion is feasible.
```

---

### 4.2 ACTIVES

**Purpose:** What is the competition signal?

**Output Format:**
```
ACTIVES: [RELEVANCY TAG]

[1-3 sentences on competition signal, not listing actives]
```

**Relevancy Tags:**
| Tag | Meaning |
|-----|---------|
| **HIGH** | Active inventory creates meaningful pricing pressure |
| **MID** | Some competition but not dominant |
| **LOW** | Limited active competition |

**Example:**
```
ACTIVES: HIGH

7 actives in PIQ's price band with avg 45 DOM. Market is absorbing
slowly at this tier. Aggressive pricing may be required for velocity.
```

---

### 4.3 PENDINGS / BACKUPS

**Purpose:** What is the directional signal?

**Output Format:**
```
PENDINGS / BACKUPS: [RELEVANCY TAG]

[1-3 sentences on directional signal, not listing pendings]
```

**Relevancy Tags:**
| Tag | Meaning |
|-----|---------|
| **HIGH** | Strong directional signal present |
| **MID** | Some signal but unconfirmed |
| **LOW** | Limited or no pending signal |

**Example:**
```
PENDINGS / BACKUPS: HIGH

3 pendings in upper tier (list $285-$295K) suggest market
accepting prices above closed average. Verify contract prices
before assuming upper range holds.
```

---

### 4.4 CLOSED SALES

**Purpose:** What is the baseline integrity?

**Output Format:**
```
CLOSED SALES: [RELEVANCY TAG]

[1-3 sentences on baseline reliability, not listing closed comps]
```

**Relevancy Tags:**
| Tag | Meaning |
|-----|---------|
| **HIGH** | Strong closed foundation, tight cluster |
| **MID** | Usable but some variance or gaps |
| **LOW** | Weak foundation — thin data or outlier-driven |

**Example:**
```
CLOSED SALES: HIGH

11 closed in 90 days with tight $178-$192/sqft range. Mean and
median align within 3%. Baseline is mathematically reliable.
```

---

### 4.5 E-VALUE INTERPRETATION

**Purpose:** What does the E-Value actually mean for this PIQ?

**Output Format:**
```
E-VALUE INTERPRETATION: [OUTCOME TAG]

[2-4 sentences of lean logic — what the E-Value means, not what it is]
```

**Outcome Tags:**
| Tag | Meaning |
|-----|---------|
| **BASELINE CONFIRMED** | E-Value is reliable for this PIQ |
| **POSSIBLE PUSH** | Evidence supports exploring upper range |
| **CAUTION** | Material risk factors present |
| **CEILING BREACH** | E-Value may exceed what market will support |

**Example:**
```
E-VALUE INTERPRETATION: POSSIBLE PUSH

Closed foundation supports baseline. Pending activity at upper
tier suggests market may accept $5-10K above baseline IF PIQ
matches condition of pending comps. Verify contract prices
before committing to upper range.
```

---

### 4.6 WHAT MUST BE VERIFIED

**Purpose:** What questions must be answered before trusting this analysis?

**Output Format:**
```
WHAT MUST BE VERIFIED:

• [Task 1]
• [Task 2]
• [Task 3]
• [Task 4]
```

**Rules:**
- Maximum 4-6 bullets
- Specific, actionable questions
- No generic "do more research" tasks
- Focus on deal-breaking unknowns

**Example:**
```
WHAT MUST BE VERIFIED:

• What did 742 Oak pending contract for vs $289K list?
• Does PIQ have any backing/busy street exposure?
• Is PIQ sqft permitted or unpermitted addition?
• Can layout support 3/2 conversion at reasonable cost?
```

---

## 5. DATA SCHEMA

```typescript
interface MatrixIntelligenceOutput {
  piq_vs_comps: {
    relevancy: 'HIGH' | 'MID' | 'LOW';
    interpretation: string; // 1-3 sentences
  };

  actives: {
    relevancy: 'HIGH' | 'MID' | 'LOW';
    interpretation: string; // 1-3 sentences
  };

  pendings_backups: {
    relevancy: 'HIGH' | 'MID' | 'LOW';
    interpretation: string; // 1-3 sentences
  };

  closed_sales: {
    relevancy: 'HIGH' | 'MID' | 'LOW';
    interpretation: string; // 1-3 sentences
  };

  e_value_interpretation: {
    outcome: 'BASELINE CONFIRMED' | 'POSSIBLE PUSH' | 'CAUTION' | 'CEILING BREACH';
    interpretation: string; // 2-4 sentences
  };

  verification_tasks: string[]; // 4-6 items max
}
```

---

## 6. ENGINEERING GUARDRAILS

### 6.1 Word Count Enforcement

| Constraint | Limit |
|------------|-------|
| Total output | 200-400 words |
| Per section (1-4) | 1-3 sentences |
| E-Value section | 2-4 sentences |
| Verification tasks | 4-6 bullets max |

### 6.2 Required Constraints

| Constraint | Enforcement |
|------------|-------------|
| Output must be schema-validated | No free-form responses |
| Sections cannot be omitted | All 6 sections required |
| Sections cannot be reordered | Fixed sequence |
| No data restatement | Interpret, don't list |
| No exact dollar values in narrative | Directional language only |
| The Absolute Rule honored | Unproven upside = nonexistent |

### 6.3 Forbidden Patterns

| ❌ Do NOT | ✅ Instead |
|-----------|-----------|
| "The comps are: 123 Main, 456 Oak..." | "Comps skew larger than PIQ..." |
| "E-Value is $267,500" | "Baseline is supported by closed evidence" |
| "There are 7 actives at $X, $Y, $Z..." | "Active inventory creates pricing pressure" |
| "Pending at 742 Oak listed at $289K" | "Pending activity suggests upper tier acceptance" |

---

## 7. ACCEPTANCE CRITERIA (QA)

- [ ] Total output is 200-400 words
- [ ] All 6 sections present in order
- [ ] Each section has relevancy/outcome tag
- [ ] No data restatement (interpretation only)
- [ ] No exact dollar values in narrative
- [ ] Verification tasks are specific and actionable
- [ ] Maximum 4-6 verification tasks
- [ ] The Absolute Rule honored
- [ ] CEILING BREACH used when E-Value exceeds closed evidence

---

## 8. SAMPLE OUTPUT

```
PIQ VS COMPS: MID

Comps skew slightly larger (avg 1,580 sqft vs PIQ 1,420).
Most closed in same tract and school district. Functional
match is acceptable but size gap may limit upper range.

ACTIVES: HIGH

Heavy active inventory (9 listings) in PIQ's target range.
Average DOM at 52 days signals slow absorption. Price
resistance is present at upper tier.

PENDINGS / BACKUPS: MID

2 pendings in mid-tier provide directional support. No
pending evidence at upper range. Cannot confirm market
acceptance above baseline without contract verification.

CLOSED SALES: HIGH

8 closed in 90 days form tight cluster. Mean/median gap
under 4%. One high outlier (pool, premium lot) should not
pull expectations — not transferable to PIQ.

E-VALUE INTERPRETATION: BASELINE CONFIRMED

E-Value is mathematically supported by closed cluster.
Upper range lacks closed proof — pending activity is
insufficient to justify aggressive positioning. Heavy
active inventory reinforces baseline-conservative stance.

WHAT MUST BE VERIFIED:

• What did 892 Elm pending contract for vs $274K list?
• Does PIQ have any location negatives (backing, busy street)?
• Why is 1847 Pine still active at 78 DOM?
• Is the high-outlier pool comp pulling the average unfairly?
```

**Word count: 218 words**

---

## 9. v2.4 → v3.0 MIGRATION NOTES

| v2.4 Element | v3.0 Treatment |
|--------------|----------------|
| Context Header | REMOVED — user knows why they're here |
| 18 sections | CONSOLIDATED to 6 |
| Confidence Score | REPLACED with Relevancy Tags |
| Signal Strength | MERGED into interpretation |
| Outlier Classification | ONLY mentioned if relevant |
| Market Regime Declaration | FOLDED into Actives interpretation |
| Market Saturation | FOLDED into Actives interpretation |
| Competitive Alternatives | REMOVED — that's Map overlay job |
| Status Bucket Intelligence | SPLIT into Actives + Pendings |
| Transferability Test | ONLY mentioned when needed |
| Deal Viability Gate | EXPRESSED through Outcome Tags |
| Capital at Risk | EXPRESSED through Outcome Tags |
| Escalation Check | REMOVED — human judgment |
| Final Stance (🟢🟡🔴) | REPLACED with Outcome Tags |
| Bot Limitation Flag | REMOVED — unnecessary |
| Human Override Protocol | REMOVED — system-level, not output |
| Post-Close Learning | REMOVED — system-level, not output |

---

## 10. ONE-LINE ENGINEERING SUMMARY

Build a lean interpretation layer that explains what comp data means in 200-400 words across 6 sections: PIQ fit, active competition, pending direction, closed foundation, E-Value outcome, and verification tasks — without restating visible data or providing false precision.

---

*Document prepared for FlipIQ Engineering*
*Version 3.0 (LEAN) — December 28, 2024*
