# COMP MATRIX VALUE INTELLIGENCE BOT
## User Stories & Epics
### For Engineering Implementation — v3.0 LEAN Edition

---

| Field | Value |
|-------|-------|
| **Feature ID** | C-MATRIX-VI |
| **Parent System** | PIQ Comps Module |
| **Priority** | P0 - Critical Path |
| **Primary User** | Acquisition Associate (AA) |
| **UI Location** | PIQ → Comps Tab → Matrix View |
| **Handoff To** | Nate (CTO) |
| **PRD Reference** | comp-matrix-prd.md v3.0 LEAN |
| **Training Samples** | comp-matrix-training-samples.md |
| **Version** | v2.0 — December 28, 2024 |

---

# EXECUTIVE SUMMARY

The Comp Matrix Value Intelligence Bot is a **lean interpretation layer** — not an AVM, not a pricing engine.

**v3.0 LEAN Principle:**
> Interpretation, not information. 8 sections. Focus on what the data MEANS.

**What It Does:**
- Interprets what comp data means (does NOT restate visible data)
- Provides relevancy tags for each data category
- Delivers clear E-Value outcome interpretation
- Analyzes market ceiling and push logic
- Generates specific verification tasks

**8 Required Output Sections:**
1. PROPERTY IN QUESTION VS COMPS
2. MATRIX TABLE — ACTIVES
3. MATRIX TABLE — PENDINGS / BACKUPS
4. MATRIX TABLE — CLOSED SALES
5. E-VALUE INTERPRETATION
6. MARKET CEILING & PUSH LOGIC
7. WHAT MUST BE VERIFIED
8. CONCLUSION

---

# EPIC 1: IQ BUTTON & OVERLAY ACTIVATION

## Epic Summary
Enable the IQ button to activate the lean Value Intelligence overlay on the Matrix view.

---

### US-MTX-001: IQ Button Activation
> As an AA, I want to click the IQ button on the Matrix view so that I can see the Value Intelligence interpretation overlay.

**Acceptance Criteria:**
- [ ] IQ button visible in Matrix view header (matches Map view placement)
- [ ] Click toggles overlay visibility
- [ ] Loading state displayed while analysis runs
- [ ] Error handling if analysis fails
- [ ] Overlay appears below matrix content (not modal)

**PRD Reference:** Section 3 - LEAN Output Structure

---

### US-MTX-002: 8-Section Output Display
> As an AA, I want to see all 8 sections in fixed order so that I get consistent, scannable output.

**Acceptance Criteria:**
- [ ] All 8 sections rendered in fixed order:
  1. PROPERTY IN QUESTION VS COMPS
  2. MATRIX TABLE — ACTIVES
  3. MATRIX TABLE — PENDINGS / BACKUPS
  4. MATRIX TABLE — CLOSED SALES
  5. E-VALUE INTERPRETATION
  6. MARKET CEILING & PUSH LOGIC
  7. WHAT MUST BE VERIFIED
  8. CONCLUSION
- [ ] Sections cannot be omitted
- [ ] Sections cannot be reordered

**PRD Reference:** Section 3

---

# EPIC 2: PROPERTY IN QUESTION VS COMPS

## Epic Summary
Display PIQ fit interpretation with relevancy tag.

---

### US-MTX-003: PIQ VS COMPS Section Display
> As an AA, I want to see a relevancy tag for PIQ vs Comps fit so that I know if the average applies to my PIQ.

**Acceptance Criteria:**
- [ ] Section header: "PROPERTY IN QUESTION VS COMPS"
- [ ] Relevancy line: "Relevancy: [HIGH / MID / LOW]"
- [ ] Tag definitions:
  - HIGH: Material mismatch exists — average may not apply
  - MID: Some functional gaps but usable
  - LOW: Comps closely match PIQ profile — no adjustment needed
- [ ] 2-4 sentences of interpretation (not data listing)

**PRD Reference:** Section 4.1

---

### US-MTX-004: PIQ Mismatch Detection
> As an AA, I want interpretation of structural mismatches (bed/bath, size, age) so that I understand buyer pool risk.

**Acceptance Criteria:**
- [ ] Detects bed/bath count mismatches
- [ ] Detects square footage mismatches
- [ ] Detects year built / product type differences
- [ ] Explains buyer pool implications, not just metric differences

**Example Scenarios:**
- 2/1 PIQ vs 3/2 comps → HIGH relevancy, average overstated
- Size mismatch creating ceiling breach → HIGH relevancy
- New construction contaminating resale comps → HIGH relevancy

**PRD Reference:** Section 4.1, Training Samples 2, 5, 9

---

# EPIC 3: MATRIX TABLE — ACTIVES

## Epic Summary
Display active competition interpretation with relevancy tag.

---

### US-MTX-005: ACTIVES Section Display
> As an AA, I want to see a relevancy tag for active inventory so that I know if competition is a pricing factor.

**Acceptance Criteria:**
- [ ] Section header: "MATRIX TABLE — ACTIVES"
- [ ] Relevancy line: "Relevancy: [HIGH / MID / LOW]"
- [ ] Tag definitions:
  - HIGH: Active inventory creates meaningful pricing signal
  - MID: Some signal present but not dominant
  - LOW: Limited active competition, no significant signal
- [ ] 2-4 sentences of interpretation

**PRD Reference:** Section 4.2

---

### US-MTX-006: Active Ceiling Detection
> As an AA, I want to know when stale actives are capping the market so that I don't price into rejection.

**Acceptance Criteria:**
- [ ] Detects actives with high DOM (50+ days)
- [ ] Identifies price reduction patterns
- [ ] Explains market rejection signal
- [ ] Links to ceiling interpretation in section 6

**Example Pattern:**
```
Three actives sitting at 58-71 DOM with price reductions
establish a visible rejection point. Pricing the PIQ into
that zone introduces significant absorption risk.
```

**PRD Reference:** Section 4.2, Training Sample 4

---

### US-MTX-007: Scarcity Detection
> As an AA, I want to know when low inventory creates seller leverage so that I can lean toward upper range.

**Acceptance Criteria:**
- [ ] Detects single or zero competing actives
- [ ] Notes condition comparison (inferior competition = leverage)
- [ ] Links to PUSH SUPPORTED outcome when appropriate

**PRD Reference:** Section 4.2, Training Sample 7

---

# EPIC 4: MATRIX TABLE — PENDINGS / BACKUPS

## Epic Summary
Display pending/backup directional interpretation with relevancy tag.

---

### US-MTX-008: PENDINGS Section Display
> As an AA, I want to see a relevancy tag for pendings so that I know if there's a directional signal.

**Acceptance Criteria:**
- [ ] Section header: "MATRIX TABLE — PENDINGS / BACKUPS"
- [ ] Relevancy line: "Relevancy: [HIGH / MID / LOW]"
- [ ] Tag definitions:
  - HIGH: Strong directional signal present
  - MID: Some signal but unconfirmed
  - LOW: Limited or no pending signal
- [ ] 2-4 sentences of interpretation

**PRD Reference:** Section 4.3

---

### US-MTX-009: Pending Direction Analysis
> As an AA, I want to know if pendings are signaling up, down, or stable market direction.

**Acceptance Criteria:**
- [ ] Compares pending pricing to closed average
- [ ] Notes time to contract (fast = demand, slow = resistance)
- [ ] Identifies backup offers as demand signal
- [ ] Warns when pendings contradict closed data

**Example Patterns:**
- Pendings below closed avg → market softening signal
- Fast pendings + backup offers → demand exceeding supply
- No pendings → limited directional signal

**PRD Reference:** Section 4.3, Training Samples 6, 7

---

# EPIC 5: MATRIX TABLE — CLOSED SALES

## Epic Summary
Display closed sales baseline interpretation with relevancy tag.

---

### US-MTX-010: CLOSED SALES Section Display
> As an AA, I want to see a relevancy tag for closed sales so that I know if the baseline is reliable.

**Acceptance Criteria:**
- [ ] Section header: "MATRIX TABLE — CLOSED SALES"
- [ ] Relevancy line: "Relevancy: [HIGH / MID / LOW]"
- [ ] Tag definitions:
  - HIGH: Closed data provides critical signal
  - MID: Usable but some variance or gaps
  - LOW: Standard closed foundation, no special concerns
- [ ] 2-4 sentences of interpretation

**PRD Reference:** Section 4.4

---

### US-MTX-011: Outlier Detection
> As an AA, I want to know when outliers are distorting the average so that I anchor to the correct cluster.

**Acceptance Criteria:**
- [ ] Detects high outliers with non-transferable features (ADU, oversized lot, premium view)
- [ ] Calculates cluster vs blended average difference
- [ ] Recommends cluster anchor when outlier is non-transferable
- [ ] Quantifies potential overstatement percentage

**Example Pattern:**
```
One sale at $378/sqft on a 14,200 sqft lot with permitted ADU
is pulling the mean up materially. The remaining six closed
sales cluster tightly around $289/sqft. The average is
distorted by a non-transferable outlier.
```

**PRD Reference:** Section 4.4, Training Sample 3

---

### US-MTX-012: Mean/Median Analysis
> As an AA, I want to see when mean and median diverge so that I know if the average is reliable.

**Acceptance Criteria:**
- [ ] Calculates mean vs median gap
- [ ] Flags when gap exceeds 5%
- [ ] Explains direction of skew (high outliers vs low outliers)
- [ ] Notes when mean/median alignment indicates tight cluster

**PRD Reference:** Section 4.4, Training Sample 1

---

# EPIC 6: E-VALUE INTERPRETATION

## Epic Summary
Display E-Value outcome interpretation with outcome tag — the core value judgment.

---

### US-MTX-013: E-VALUE Outcome Tag Display
> As an AA, I want to see an outcome tag for E-Value so that I know the bottom-line assessment.

**Acceptance Criteria:**
- [ ] Section header: "E-VALUE INTERPRETATION"
- [ ] Outcome line: "Outcome: [TAG]"
- [ ] Outcome tags:
  - BASELINE CONFIRMED
  - PUSH SUPPORTED
  - BASELINE — CEILING CAPPED
  - BASELINE + STRATEGY WARNING
  - CAUTION — AVERAGE OVERSTATED
  - CAUTION — AVERAGE DISTORTED
  - CAUTION — MARKET SOFTENING
  - CAUTION — PRODUCT TYPE CONTAMINATION
  - CEILING BREACH — NO CLOSED PROOF
  - VERIFY BEFORE MOVING
- [ ] 2-4 sentences of lean interpretation

**PRD Reference:** Section 4.5

---

### US-MTX-014: Outcome Tag Logic
> As a system, I must select the correct outcome tag based on synthesized signals.

**Acceptance Criteria:**
- [ ] BASELINE CONFIRMED: Tight cluster, PIQ aligns, no conflicting signals
- [ ] PUSH SUPPORTED: Scarcity + fast absorption + backup activity
- [ ] CEILING CAPPED: Baseline reliable but stale actives define ceiling
- [ ] STRATEGY WARNING: Value achievable but velocity penalty exists
- [ ] CAUTION tags: Material risk identified (specify reason)
- [ ] CEILING BREACH: E-Value exceeds highest closed
- [ ] VERIFY BEFORE MOVING: Signals conflict materially

**PRD Reference:** Section 4.5, Section 9

---

### US-MTX-015: The Absolute Rule Enforcement
> As a system, I must treat unproven upside as nonexistent so that capital is protected.

**Acceptance Criteria:**
- [ ] PUSH SUPPORTED requires closed evidence of premium achievability
- [ ] Pending evidence alone cannot justify PUSH SUPPORTED
- [ ] Active pricing cannot justify PUSH SUPPORTED
- [ ] CEILING BREACH required when E-Value exceeds highest closed

**PRD Reference:** Section 2 - The Absolute Rule

---

# EPIC 7: MARKET CEILING & PUSH LOGIC

## Epic Summary
Display market ceiling analysis with push/constraint interpretation.

---

### US-MTX-016: CEILING Section Display
> As an AA, I want to see ceiling and push logic so that I know the boundaries of value.

**Acceptance Criteria:**
- [ ] Section header: "MARKET CEILING & PUSH LOGIC"
- [ ] Relevancy line: "Relevancy: [HIGH / MID / LOW]"
- [ ] Tag definitions:
  - HIGH: Ceiling analysis is critical to decision
  - MID: Some ceiling considerations present
  - LOW: No significant ceiling concerns
- [ ] 2-4 sentences explaining ceiling and push evidence

**PRD Reference:** Section 4.6

---

### US-MTX-017: Ceiling Definition Logic
> As an AA, I want to understand what defines the ceiling so that I know my pricing boundaries.

**Acceptance Criteria:**
- [ ] Ceiling defined by highest closed when no push evidence
- [ ] Ceiling defined by buyer rejection (stale actives) when present
- [ ] Push logic requires: scarcity + fast absorption + backup offers
- [ ] Size-adjusted E-Value must not exceed highest closed without proof

**Example Patterns:**
- Highest closed = soft ceiling, no push evidence
- Stale actives with reductions = hard ceiling
- Scarcity + fast pendings = ceiling may be tested

**PRD Reference:** Section 4.6, Training Samples 4, 5, 7

---

# EPIC 8: VERIFICATION TASKS

## Epic Summary
Generate specific, actionable verification tasks.

---

### US-MTX-018: Verification Task Generation
> As an AA, I want specific verification tasks so that I know exactly what to ask before trusting this analysis.

**Acceptance Criteria:**
- [ ] Section header: "WHAT MUST BE VERIFIED"
- [ ] 3-5 bullet points
- [ ] Each task is specific and actionable
- [ ] No generic tasks ("do more research")
- [ ] Optional summary sentence at end

**PRD Reference:** Section 4.7

---

### US-MTX-019: Verification Task Quality
> As an AA, I want verification tasks that ask specific questions about specific properties or concerns.

**Acceptance Criteria:**
- [ ] Tasks reference specific properties when relevant
- [ ] Tasks ask about specific PIQ concerns
- [ ] Tasks can be answered with a phone call, site visit, or data check

**Good Examples:**
```
• Confirm no feasible layout expansion (can the PIQ be converted to 3/2?)
• Get contractor assessment on bedroom/bath addition feasibility and cost
• Confirm buyer resistance to 2/1 layouts at higher prices via agent feedback
• Recalculate acquisition basis using 2/1 cluster anchor (~$241/sqft)
```

**Bad Examples:**
```
• Research the market more
• Verify comparable sales
• Check pending status
• Investigate further
```

**PRD Reference:** Section 4.7

---

# EPIC 9: CONCLUSION

## Epic Summary
Provide summary statement tying analysis together.

---

### US-MTX-020: CONCLUSION Section Display
> As an AA, I want a conclusion that summarizes the key finding so that I have a clear takeaway.

**Acceptance Criteria:**
- [ ] Section header: "CONCLUSION"
- [ ] 2-4 sentences summarizing key finding
- [ ] Ties back to outcome tag
- [ ] Provides recommended stance or action

**Example:**
```
CONCLUSION

The layout mismatch introduces real downward risk. The blended
average masks a structural disadvantage that buyers will
recognize immediately. The PIQ should anchor to the 2/1 cluster,
and the baseline should be treated as a ceiling rather than
a starting point.
```

**PRD Reference:** Section 4.8

---

# EPIC 10: ENGINEERING GUARDRAILS

## Epic Summary
Enforce schema validation and forbidden patterns.

---

### US-MTX-021: Schema Validation
> As a system, I must validate output schema so that structure is consistent.

**Acceptance Criteria:**
- [ ] All 8 sections present
- [ ] Sections in fixed order
- [ ] Each section 1-4, 6 has relevancy tag
- [ ] Section 5 has outcome tag
- [ ] No additional sections allowed

**PRD Reference:** Section 5, Section 6.1

---

### US-MTX-022: Forbidden Pattern Detection
> As a system, I must reject data restatement so that output is interpretation only.

**Acceptance Criteria:**
- [ ] No listing of comp addresses in narrative
- [ ] No tables of comp data
- [ ] Interpretation over information
- [ ] Generic counts OK ("There are seven closed sales...")
- [ ] Specific addresses OK only in verification tasks

**PRD Reference:** Section 6.2

---

# TECHNICAL SPECIFICATIONS

## Data Schema

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
  verification_summary?: string; // optional

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

# ACCEPTANCE CRITERIA CHECKLIST

## Required on Every Output
- [ ] All 8 sections present in order
- [ ] PROPERTY IN QUESTION VS COMPS has relevancy tag
- [ ] MATRIX TABLE — ACTIVES has relevancy tag
- [ ] MATRIX TABLE — PENDINGS / BACKUPS has relevancy tag
- [ ] MATRIX TABLE — CLOSED SALES has relevancy tag
- [ ] E-VALUE INTERPRETATION has outcome tag
- [ ] MARKET CEILING & PUSH LOGIC has relevancy tag
- [ ] WHAT MUST BE VERIFIED has 3-5 bullets
- [ ] CONCLUSION provides clear summary

## Governance Rules Enforced
- [ ] No data restatement (interpretation only)
- [ ] No comp address listings in sections 1-6
- [ ] The Absolute Rule honored
- [ ] CEILING BREACH used when E-Value exceeds closed evidence
- [ ] PUSH SUPPORTED requires closed proof

---

# IMPLEMENTATION PHASES

## Phase 1: Core Output Structure
- IQ button activation
- 8-section skeleton
- Relevancy/Outcome tag system

## Phase 2: Section Intelligence
- PIQ VS COMPS fit interpretation logic
- ACTIVES competition signal logic
- PENDINGS/BACKUPS direction logic
- CLOSED SALES baseline integrity logic

## Phase 3: E-Value & Ceiling Logic
- Outcome tag computation
- Signal synthesis from sections 1-4
- Market ceiling definition logic
- The Absolute Rule enforcement

## Phase 4: Verification & Conclusion
- Task generation logic
- Specific question formulation
- Conclusion synthesis

---

# SCENARIO COVERAGE

| # | Scenario | Typical Outcome | Training Sample |
|---|----------|-----------------|-----------------|
| 1 | Clean Baseline | BASELINE CONFIRMED | Sample 1 |
| 2 | Bed/Bath Mismatch | CAUTION — AVERAGE OVERSTATED | Sample 2 |
| 3 | Outlier Distortion | CAUTION — AVERAGE DISTORTED | Sample 3 |
| 4 | Actives Capping Value | BASELINE — CEILING CAPPED | Sample 4 |
| 5 | Size Ceiling Breach | CEILING BREACH — NO CLOSED PROOF | Sample 5 |
| 6 | Shifting Market | CAUTION — MARKET SOFTENING | Sample 6 |
| 7 | Scarcity Upside | PUSH SUPPORTED | Sample 7 |
| 8 | Over-Improvement Risk | BASELINE + STRATEGY WARNING | Sample 8 |
| 9 | New Construction | CAUTION — PRODUCT TYPE CONTAMINATION | Sample 9 |
| 10 | Mixed Signals | VERIFY BEFORE MOVING | Sample 10 |

---

# SIGN-OFF

| For Nate (CTO) |
|----------------|
| • 22 user stories across 10 epics |
| • TypeScript schema defined |
| • PRD v3.0 LEAN reference linked |
| • 10 training samples provided |
| • 4-phase implementation path |
| • Scenario coverage matrix included |
| • This is interpretation, not information |

---

*Document prepared for FlipIQ Engineering*
*Version 2.0 (LEAN) — December 28, 2024*
