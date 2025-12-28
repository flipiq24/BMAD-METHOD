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
| **Version** | v2.0 — December 28, 2024 |

---

# EXECUTIVE SUMMARY

The Comp Matrix Value Intelligence Bot is a **lean interpretation layer** — not an AVM, not a pricing engine.

**v3.0 LEAN Principle:**
> Interpretation, not information. 200-400 words. 6 sections. No data restatement.

**What It Does:**
- Interprets what comp data means (does NOT restate visible data)
- Provides relevancy tags for each data category
- Delivers clear outcome interpretation for E-Value
- Generates specific verification tasks

**6 Required Output Sections:**
1. PIQ VS COMPS
2. ACTIVES
3. PENDINGS / BACKUPS
4. CLOSED SALES
5. E-VALUE INTERPRETATION
6. WHAT MUST BE VERIFIED

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

### US-MTX-002: 6-Section Output Display
> As an AA, I want to see all 6 sections in fixed order so that I get consistent, scannable output.

**Acceptance Criteria:**
- [ ] All 6 sections rendered in fixed order:
  1. PIQ VS COMPS
  2. ACTIVES
  3. PENDINGS / BACKUPS
  4. CLOSED SALES
  5. E-VALUE INTERPRETATION
  6. WHAT MUST BE VERIFIED
- [ ] Sections cannot be omitted
- [ ] Sections cannot be reordered
- [ ] Total output is 200-400 words

**PRD Reference:** Section 3

---

# EPIC 2: PIQ VS COMPS SECTION

## Epic Summary
Display PIQ fit interpretation with relevancy tag.

---

### US-MTX-003: PIQ VS COMPS Relevancy Display
> As an AA, I want to see a relevancy tag for PIQ vs Comps fit so that I know if the average applies to my PIQ.

**Acceptance Criteria:**
- [ ] Section header: "PIQ VS COMPS: [TAG]"
- [ ] Relevancy tags: HIGH / MID / LOW
- [ ] Tag definitions:
  - HIGH: Comps closely match PIQ profile
  - MID: Some functional gaps but usable
  - LOW: Material mismatch — average may not apply
- [ ] 1-3 sentences of interpretation (not data listing)

**PRD Reference:** Section 4.1

---

### US-MTX-004: PIQ VS COMPS Interpretation
> As an AA, I want interpretation of the fit, not a list of metrics, so that I understand what the comparison means.

**Acceptance Criteria:**
- [ ] Output explains what the fit means, not what the numbers are
- [ ] No listing of individual comp addresses
- [ ] No table of metrics (user can see matrix)
- [ ] Focus on: Does the average represent PIQ fairly?

**Forbidden Pattern:**
```
❌ "PIQ is 1,380 sqft. Comps average 1,650 sqft. Difference is 270 sqft."
✅ "Comps skew larger than PIQ. This pulls the average up."
```

**PRD Reference:** Section 4.1, Section 6.3

---

# EPIC 3: ACTIVES SECTION

## Epic Summary
Display active competition interpretation with relevancy tag.

---

### US-MTX-005: ACTIVES Relevancy Display
> As an AA, I want to see a relevancy tag for active inventory so that I know if competition is a pricing factor.

**Acceptance Criteria:**
- [ ] Section header: "ACTIVES: [TAG]"
- [ ] Relevancy tags: HIGH / MID / LOW
- [ ] Tag definitions:
  - HIGH: Active inventory creates meaningful pricing pressure
  - MID: Some competition but not dominant
  - LOW: Limited active competition
- [ ] 1-3 sentences of interpretation

**PRD Reference:** Section 4.2

---

### US-MTX-006: ACTIVES Competition Interpretation
> As an AA, I want interpretation of competition signal, not a list of actives, so that I understand market friction.

**Acceptance Criteria:**
- [ ] Output explains what active inventory means for pricing
- [ ] No listing of individual active addresses
- [ ] Focus on: absorption signal, DOM patterns, pricing pressure
- [ ] May reference count and average DOM without listing each

**Forbidden Pattern:**
```
❌ "Active at 123 Main $275K, 456 Oak $282K, 789 Elm $269K..."
✅ "Heavy active inventory creates pricing pressure at upper tier."
```

**PRD Reference:** Section 4.2, Section 6.3

---

# EPIC 4: PENDINGS / BACKUPS SECTION

## Epic Summary
Display pending/backup directional interpretation with relevancy tag.

---

### US-MTX-007: PENDINGS / BACKUPS Relevancy Display
> As an AA, I want to see a relevancy tag for pendings so that I know if there's a directional signal.

**Acceptance Criteria:**
- [ ] Section header: "PENDINGS / BACKUPS: [TAG]"
- [ ] Relevancy tags: HIGH / MID / LOW
- [ ] Tag definitions:
  - HIGH: Strong directional signal present
  - MID: Some signal but unconfirmed
  - LOW: Limited or no pending signal
- [ ] 1-3 sentences of interpretation

**PRD Reference:** Section 4.3

---

### US-MTX-008: PENDINGS / BACKUPS Direction Interpretation
> As an AA, I want interpretation of directional signal, not a list of pendings, so that I understand market momentum.

**Acceptance Criteria:**
- [ ] Output explains what pendings suggest about market direction
- [ ] No listing of individual pending addresses
- [ ] Always notes that contract prices are unverified
- [ ] Focus on: tier direction, market acceptance signals

**Forbidden Pattern:**
```
❌ "Pending at 742 Oak listed at $289K, 891 Pine at $278K..."
✅ "Pending activity in upper tier suggests market acceptance above baseline."
```

**PRD Reference:** Section 4.3, Section 6.3

---

# EPIC 5: CLOSED SALES SECTION

## Epic Summary
Display closed sales baseline interpretation with relevancy tag.

---

### US-MTX-009: CLOSED SALES Relevancy Display
> As an AA, I want to see a relevancy tag for closed sales so that I know if the baseline is reliable.

**Acceptance Criteria:**
- [ ] Section header: "CLOSED SALES: [TAG]"
- [ ] Relevancy tags: HIGH / MID / LOW
- [ ] Tag definitions:
  - HIGH: Strong closed foundation, tight cluster
  - MID: Usable but some variance or gaps
  - LOW: Weak foundation — thin data or outlier-driven
- [ ] 1-3 sentences of interpretation

**PRD Reference:** Section 4.4

---

### US-MTX-010: CLOSED SALES Baseline Interpretation
> As an AA, I want interpretation of baseline integrity, not a list of closed comps, so that I understand data reliability.

**Acceptance Criteria:**
- [ ] Output explains if baseline is mathematically reliable
- [ ] No listing of individual closed addresses
- [ ] May reference mean/median alignment, cluster tightness
- [ ] Notes outlier impact only if material

**Forbidden Pattern:**
```
❌ "Closed: 123 Main $268K, 456 Oak $275K, 789 Elm $262K..."
✅ "Tight cluster with mean/median alignment under 4%. Baseline reliable."
```

**PRD Reference:** Section 4.4, Section 6.3

---

# EPIC 6: E-VALUE INTERPRETATION SECTION

## Epic Summary
Display E-Value outcome interpretation with outcome tag — the core value judgment.

---

### US-MTX-011: E-VALUE Outcome Tag Display
> As an AA, I want to see an outcome tag for E-Value so that I know the bottom-line assessment.

**Acceptance Criteria:**
- [ ] Section header: "E-VALUE INTERPRETATION: [TAG]"
- [ ] Outcome tags: BASELINE CONFIRMED / POSSIBLE PUSH / CAUTION / CEILING BREACH
- [ ] Tag definitions:
  - BASELINE CONFIRMED: E-Value is reliable for this PIQ
  - POSSIBLE PUSH: Evidence supports exploring upper range
  - CAUTION: Material risk factors present
  - CEILING BREACH: E-Value may exceed what market will support
- [ ] 2-4 sentences of lean logic

**PRD Reference:** Section 4.5

---

### US-MTX-012: E-VALUE Lean Interpretation
> As an AA, I want interpretation of what the E-Value means, not what it is, so that I can make informed decisions.

**Acceptance Criteria:**
- [ ] Output explains what E-Value means for this PIQ
- [ ] No restating the E-Value number (user can see it)
- [ ] Synthesizes signals from previous 4 sections
- [ ] Ties to verification requirements

**Forbidden Pattern:**
```
❌ "E-Value is $267,500 based on 8 closed comps averaging $185/sqft..."
✅ "Baseline is supported. Upper range lacks closed proof. Heavy inventory reinforces conservative stance."
```

**PRD Reference:** Section 4.5, Section 6.3

---

### US-MTX-013: The Absolute Rule Enforcement
> As a system, I must treat unproven upside as nonexistent so that capital is protected.

**Acceptance Criteria:**
- [ ] If upside cannot be proven with closed evidence → POSSIBLE PUSH is not allowed
- [ ] Pending evidence alone cannot justify POSSIBLE PUSH
- [ ] Active pricing cannot justify POSSIBLE PUSH
- [ ] CEILING BREACH required when E-Value exceeds closed cluster

**PRD Reference:** Section 2 - The Absolute Rule

---

# EPIC 7: VERIFICATION TASKS SECTION

## Epic Summary
Generate specific, actionable verification tasks.

---

### US-MTX-014: Verification Task Generation
> As an AA, I want specific verification tasks so that I know exactly what to ask before trusting this analysis.

**Acceptance Criteria:**
- [ ] Section header: "WHAT MUST BE VERIFIED:"
- [ ] 4-6 bullet points maximum
- [ ] Each task is specific and actionable
- [ ] No generic tasks ("do more research")
- [ ] Focus on deal-breaking unknowns

**PRD Reference:** Section 4.6

---

### US-MTX-015: Verification Task Quality
> As an AA, I want verification tasks that ask specific questions, not vague guidance.

**Acceptance Criteria:**
- [ ] Tasks reference specific pending addresses if relevant
- [ ] Tasks ask about specific PIQ concerns
- [ ] Tasks can be answered with a phone call or site visit

**Good Examples:**
```
• What did 742 Oak pending contract for vs $289K list?
• Does PIQ have any backing/busy street exposure?
• Is PIQ sqft permitted or unpermitted addition?
• Why is 1847 Pine still active at 78 DOM?
```

**Bad Examples:**
```
• Research the market more
• Verify comparable sales
• Check pending status
• Investigate further
```

**PRD Reference:** Section 4.6

---

# EPIC 8: ENGINEERING GUARDRAILS

## Epic Summary
Enforce schema validation, word limits, and forbidden patterns.

---

### US-MTX-016: Word Count Enforcement
> As a system, I must enforce word count limits so that output stays lean.

**Acceptance Criteria:**
- [ ] Total output: 200-400 words
- [ ] Sections 1-4: 1-3 sentences each
- [ ] E-Value section: 2-4 sentences
- [ ] Verification: 4-6 bullets max
- [ ] Validation error if limits exceeded

**PRD Reference:** Section 6.1

---

### US-MTX-017: Schema Validation
> As a system, I must validate output schema so that structure is consistent.

**Acceptance Criteria:**
- [ ] All 6 sections present
- [ ] Sections in fixed order
- [ ] Each section has required tag
- [ ] No additional sections allowed

**PRD Reference:** Section 5, Section 6.2

---

### US-MTX-018: Forbidden Pattern Detection
> As a system, I must reject data restatement so that output is interpretation only.

**Acceptance Criteria:**
- [ ] No listing of comp addresses in sections 1-5
- [ ] No exact E-Value dollar amount in narrative
- [ ] No tables of comp data
- [ ] Exception: Verification tasks may reference specific addresses

**PRD Reference:** Section 6.3

---

### US-MTX-019: No Exact Dollar Values Rule
> As a system, I must use directional language in narrative so that false precision is avoided.

**Acceptance Criteria:**
- [ ] Narrative uses: "upper tier", "baseline", "above average", "below cluster"
- [ ] Narrative avoids: "$267,500", "+$15,000", "exactly 6.2%"
- [ ] Exception: Verification tasks may reference list prices for context

**PRD Reference:** Section 6.2, Section 6.3

---

# TECHNICAL SPECIFICATIONS

## Data Schema

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

// Validation rules
const WORD_LIMITS = {
  total: { min: 200, max: 400 },
  perSection: { min: 1, max: 3 }, // sentences
  eValueSection: { min: 2, max: 4 }, // sentences
  verificationTasks: { min: 4, max: 6 } // items
};
```

---

# ACCEPTANCE CRITERIA CHECKLIST

## Required on Every Output
- [ ] Total output is 200-400 words
- [ ] All 6 sections present in order
- [ ] PIQ VS COMPS has relevancy tag
- [ ] ACTIVES has relevancy tag
- [ ] PENDINGS / BACKUPS has relevancy tag
- [ ] CLOSED SALES has relevancy tag
- [ ] E-VALUE INTERPRETATION has outcome tag
- [ ] WHAT MUST BE VERIFIED has 4-6 bullets

## Governance Rules Enforced
- [ ] No data restatement (interpretation only)
- [ ] No exact dollar values in narrative
- [ ] No comp address listings in sections 1-5
- [ ] The Absolute Rule honored
- [ ] CEILING BREACH used when E-Value exceeds closed evidence

---

# IMPLEMENTATION PHASES

## Phase 1: Core Output Structure
- IQ button activation
- 6-section skeleton
- Relevancy/Outcome tag system
- Word count validation

## Phase 2: Section Intelligence
- PIQ VS COMPS fit interpretation logic
- ACTIVES competition signal logic
- PENDINGS/BACKUPS direction logic
- CLOSED SALES baseline integrity logic

## Phase 3: E-Value Synthesis
- Outcome tag computation
- Signal synthesis from sections 1-4
- The Absolute Rule enforcement
- CEILING BREACH detection

## Phase 4: Verification Engine
- Task generation logic
- Specific question formulation
- Pending/backup verification triggers
- PIQ-specific concern detection

---

# v2.4 → v3.0 USER STORY MIGRATION

| v2.4 Stories (44) | v3.0 Treatment |
|-------------------|----------------|
| US-MTX-001 to 003 (IQ Button) | KEPT — US-MTX-001, 002 |
| US-MTX-004 to 006 (Confidence/Signal) | REMOVED — replaced by tags |
| US-MTX-007 to 008 (PIQ Fit) | SIMPLIFIED — US-MTX-003, 004 |
| US-MTX-009 to 012 (Closed Sales) | SIMPLIFIED — US-MTX-009, 010 |
| US-MTX-013 to 016 (Market Behavior) | FOLDED into ACTIVES — US-MTX-005, 006 |
| US-MTX-017 to 018 (Saturation) | FOLDED into ACTIVES |
| US-MTX-019 to 020 (Status Buckets) | SPLIT — ACTIVES + PENDINGS |
| US-MTX-021 to 022 (Upward Evidence) | FOLDED into E-VALUE |
| US-MTX-023 to 024 (Downward Evidence) | FOLDED into E-VALUE + CAUTION tag |
| US-MTX-025 to 026 (Renovation Scope) | REMOVED — separate bot |
| US-MTX-027 to 031 (Risk Gates) | EXPRESSED through OUTCOME tags |
| US-MTX-032 to 035 (Final Stance) | REPLACED by OUTCOME tags |
| US-MTX-036 to 038 (Verification) | SIMPLIFIED — US-MTX-014, 015 |
| US-MTX-039 to 041 (Override/Learning) | REMOVED — system-level |
| US-MTX-042 to 044 (Guardrails) | KEPT — US-MTX-016 to 019 |

**Result:** 44 user stories → 19 user stories

---

# SIGN-OFF

| For Nate (CTO) |
|----------------|
| • 19 user stories across 8 epics (down from 44/16) |
| • TypeScript schema defined |
| • PRD v3.0 LEAN reference linked |
| • 4-phase implementation path |
| • Governance rules simplified |
| • QA checklist included |
| • This is interpretation, not information |

---

*Document prepared for FlipIQ Engineering*
*Version 2.0 (LEAN) — December 28, 2024*
