# COMP MATRIX VALUE INTELLIGENCE BOT
## User Stories & Epics
### For Engineering Implementation

---

| Field | Value |
|-------|-------|
| **Feature ID** | C-MATRIX-VI |
| **Parent System** | PIQ Comps Module |
| **Priority** | P0 - Critical Path |
| **Primary User** | Acquisition Associate (AA) |
| **UI Location** | PIQ → Comps Tab → Matrix View |
| **Handoff To** | Nate (CTO) |
| **PRD Reference** | comp-matrix-prd.md v2.4 |
| **Version** | v1.0 — December 28, 2024 |

---

# EXECUTIVE SUMMARY

The Comp Matrix Value Intelligence Bot is a **judgment discipline system** — not an AVM, not a pricing engine.

**Core Principle:**
> If the system cannot prove upside with closed evidence, it must behave as if upside does not exist.

**What It Does:**
- Interprets existing E-Value (does NOT calculate new values)
- Audits comp data for risk signals
- Generates verification tasks
- Gates deals requiring optimism
- Protects capital through systematic paranoia

---

# EPIC 1: IQ BUTTON & OVERLAY ACTIVATION

## Epic Summary
Enable the IQ button to activate the Value Intelligence overlay on the Matrix view, displaying the 18-section analysis output.

---

### US-MTX-001: IQ Button Activation
> As an AA, I want to click the IQ button on the Matrix view so that I can see the Value Intelligence analysis overlay.

**Acceptance Criteria:**
- [ ] IQ button visible in Matrix view header (matches Map view placement)
- [ ] Click toggles overlay visibility
- [ ] Loading state displayed while analysis runs
- [ ] Error handling if analysis fails
- [ ] Overlay appears below matrix content (not modal)

**PRD Reference:** Section 5 - Context Header

---

### US-MTX-002: Context Header Display
> As an AA, I want to see a context header at the top of every analysis so that I understand this is a risk audit, not a pricing opinion.

**Acceptance Criteria:**
- [ ] Context header is ALWAYS first element
- [ ] Fixed wording (cannot be modified):
```
WHY YOU ARE SEEING THIS

This analysis exists because the matrix shows potential
ambiguity that requires human validation.

This is not a pricing opinion.
This is not a recommendation to buy.
This is a risk audit of the data you already have.
```
- [ ] Visually distinct (box/border treatment)

**PRD Reference:** Section 5

---

### US-MTX-003: Section Navigation
> As an AA, I want to navigate between the 18 required sections so that I can quickly find specific analysis areas.

**Acceptance Criteria:**
- [ ] All 18 sections rendered in fixed order
- [ ] Section anchors for quick navigation
- [ ] Collapsible sections (optional)
- [ ] "No signal detected" displays when section has no data

**PRD Reference:** Section 4 - Required Output Structure

---

# EPIC 2: BASELINE & CONFIDENCE DISPLAY

## Epic Summary
Display the E-Value baseline with proper confidence framing and signal strength indicators.

---

### US-MTX-004: E-Value Baseline Display
> As an AA, I want to see the existing E-Value displayed prominently so that I have a clear anchor for the analysis.

**Acceptance Criteria:**
- [ ] E-Value displayed (bot does NOT calculate new value)
- [ ] Source clearly labeled as existing matrix calculation
- [ ] No modifications to E-Value permitted by bot

**PRD Reference:** Section 1.4 - Non-Goals

---

### US-MTX-005: Confidence Score Display
> As an AA, I want to see a confidence label so that I understand data density (not correctness).

**Acceptance Criteria:**
- [ ] Confidence labels: Strong / Moderate / Thin
- [ ] Definition displayed:
  - Strong: 15+ closed, tight cluster, same tract/school
  - Moderate: 7-14 closed OR mixed signals
  - Thin: <7 closed OR heavy reliance on pendings/actives
- [ ] Required disclaimer: "Confidence reflects data density, not correctness. High confidence does not imply safety."
- [ ] Confidence cannot upgrade a 🔴 stance to 🟡 or 🟢

**PRD Reference:** Section 6 - Confidence Score Governance

---

### US-MTX-006: Signal Strength Display
> As an AA, I want to see signal strength so that I understand how aligned multiple signals are.

**Acceptance Criteria:**
- [ ] Signal Strength labels: Strong / Moderate / Weak
- [ ] Definition displayed:
  - Strong: Multiple signals align
  - Moderate: Directional bias but verification-heavy
  - Weak: Signals conflict or hinge on unverified inputs
- [ ] Weak signal strength may trigger escalation
- [ ] Signal strength is informational, not stance-altering

**PRD Reference:** Section 7 - Signal Strength

---

# EPIC 3: PIQ vs COMP SET FIT ANALYSIS

## Epic Summary
Compare PIQ characteristics against comp set average to identify functional gaps.

---

### US-MTX-007: PIQ Fit Comparison Table
> As an AA, I want to see PIQ metrics compared to comp set averages so that I can identify material differences.

**Acceptance Criteria:**
- [ ] Comparison table with columns: Metric | PIQ | Avg Comps | Difference
- [ ] Metrics: Property Type, Bedrooms, Bathrooms, Year Built, Sqft, Lot Size
- [ ] Difference thresholds trigger visual indicators:
  - Minor (no flag): within threshold
  - Material (⚠️): exceeds threshold
  - Major (🔴): significantly exceeds threshold
- [ ] Variable thresholds per PRD Appendix A

**PRD Reference:** Section 10 - PIQ vs Average Comp Fit Logic

---

### US-MTX-008: Functional Mismatch Detection
> As an AA, I want to be alerted when PIQ has a functional mismatch with comps so that I know the average may not apply.

**Acceptance Criteria:**
- [ ] Detect bed/bath count mismatches (±1.0+)
- [ ] Detect sqft mismatches (±15%+)
- [ ] Detect lot size mismatches (±20%+)
- [ ] Clear language: "Matrix average may overstate/understate PIQ value"
- [ ] Link mismatch to downside evidence section

**PRD Reference:** Section 10

---

# EPIC 4: CLOSED SALES REALITY

## Epic Summary
Anchor all analysis to closed sales with proper outlier classification and time-weighting.

---

### US-MTX-009: Closed Sales Summary
> As an AA, I want to see closed sales summary with range and distribution so that I understand the data foundation.

**Acceptance Criteria:**
- [ ] Count of closed comps
- [ ] Average $/sqft with range (low-high)
- [ ] Mean vs Median comparison
- [ ] Gap interpretation per PRD:
  - <5%: Tight cluster, reliable
  - 5-10%: High outliers pulling up — use median
  - >10%: Average misleading — anchor to median

**PRD Reference:** Section 11 - Closed Sales Reality

---

### US-MTX-010: Time-Weighted Evidence Display
> As an AA, I want the most recent closed comp displayed first so that I'm not anchoring to stale data.

**Acceptance Criteria:**
- [ ] "Most Recent Closed" section leads
- [ ] Time-weighted hierarchy enforced:
  - Closed ≤60 days: Highest weight
  - Closed 61-120 days: High
  - Closed >120 days: Context only
- [ ] Narrative leads with recent closed, NOT highest price

**PRD Reference:** Section 8 - Time-Weighted Evidence Bias

---

### US-MTX-011: Outlier Classification
> As an AA, I want every outlier classified with a tag so that I understand transferability.

**Acceptance Criteria:**
- [ ] Every outlier must have a tag (cannot mention without classification)
- [ ] Tags:
  - High-Structural (lot, view, district, pool)
  - High-Condition (renovation-driven)
  - Low-Structural (busy street, 2/1, small lot)
  - Low-Condition (deferred maintenance)
  - False Outlier (data artifact)
- [ ] Transferability indicated for each

**PRD Reference:** Section 9 - Outlier Classification System

---

### US-MTX-012: Average Integrity Statement
> As an AA, I want an Average Integrity Statement so that I know if the average is reliable for this PIQ.

**Acceptance Criteria:**
- [ ] REQUIRED on every analysis
- [ ] Format: "This average is [reliable/unreliable] because [reason]"
- [ ] Reasons: tight cluster, outlier pull, mixed micro-markets, condition skew, school district blend
- [ ] Cannot be omitted

**PRD Reference:** Section 11

---

# EPIC 5: MARKET BEHAVIOR SIGNALS

## Epic Summary
Analyze inventory, velocity, and pricing patterns to determine market regime.

---

### US-MTX-013: Inventory Pressure Analysis
> As an AA, I want to see inventory pressure including removed comps so that I understand total buyer options.

**Acceptance Criteria:**
- [ ] Count kept actives
- [ ] Count removed actives (still on market)
- [ ] Total buyer inventory calculation
- [ ] Interpretation per saturation levels

**PRD Reference:** Section 12, 13

---

### US-MTX-014: Velocity Signals Display
> As an AA, I want to see DOM patterns by price band and condition so that I can detect absorption ceilings.

**Acceptance Criteria:**
- [ ] DOM by status (Closed/Pending/Active averages)
- [ ] DOM by price band (upper/mid/lower tier)
- [ ] DOM by condition (FLIP vs GOOD vs ORIGINAL)
- [ ] Ceiling detection when upper tier DOM significantly exceeds lower

**PRD Reference:** Section 12 - Velocity Signals

---

### US-MTX-015: List vs Close Analysis
> As an AA, I want to see list vs close patterns so that I understand market direction.

**Acceptance Criteria:**
- [ ] Pattern detection:
  - Closed below list consistently = Downward pressure
  - Closed at list = Balanced
  - Closed above list = Bidding environment
- [ ] Clear interpretation statement

**PRD Reference:** Section 12

---

### US-MTX-016: Market Regime Declaration
> As an AA, I want a one-line market regime call so that I quickly understand market conditions.

**Acceptance Criteria:**
- [ ] REQUIRED on every analysis
- [ ] Exactly one regime declared:
  - Seller-controlled, scarcity-driven
  - Balanced, velocity-driven
  - Buyer-controlled, inventory-heavy
  - Fragmented micro-market
- [ ] Cannot be omitted

**PRD Reference:** Section 12 - Market Regime Declaration

---

# EPIC 6: MARKET SATURATION & COMPETITIVE ALTERNATIVES

## Epic Summary
Surface hidden competition from removed comps and detect buyer alternatives.

---

### US-MTX-017: Market Saturation Display
> As an AA, I want to see total market saturation including removed comps so that I understand true buyer inventory.

**Acceptance Criteria:**
- [ ] Display format:
```
MARKET SATURATION

Kept Actives: [X]
Removed Actives (still on market): [Y]
Total Buyer Inventory: [X + Y]

Saturation Level: [HIGH / MODERATE / LOW]
```
- [ ] Thresholds: HIGH (>15), MODERATE (8-15), LOW (<8)
- [ ] Interpretation statement for each level

**PRD Reference:** Section 13

---

### US-MTX-018: Competitive Alternatives Detection
> As an AA, I want to see removed comps that could steal buyers so that I understand buyer leakage risk.

**Acceptance Criteria:**
- [ ] Logic: Same school district + Lower $/sqft + Equal/better condition
- [ ] When detected, display table:
  - Address, Distance, $/sqft, Condition, School, Signal
- [ ] Warning: "BUYER FRICTION: A motivated buyer may expand search radius..."
- [ ] When none detected: "COMPETITIVE ALTERNATIVES: None detected"
- [ ] Cannot be omitted (must show result either way)

**PRD Reference:** Section 14

---

# EPIC 7: STATUS BUCKET INTELLIGENCE

## Epic Summary
Analyze closed, pending, backup, and active comps with proper interpretation.

---

### US-MTX-019: Status Bucket Summary Table
> As an AA, I want to see comp counts and metrics by status so that I understand market signals.

**Acceptance Criteria:**
- [ ] Table with: Status | Count | Avg $/sqft | Avg DOM | Signal
- [ ] Statuses: Closed, Pending, Backup, Active
- [ ] Signal interpretation for each status
- [ ] Closed anchors value, pendings confirm direction, actives = friction

**PRD Reference:** Section 15

---

### US-MTX-020: Pending/Backup Verification Triggers
> As an AA, I want pendings and backups to automatically generate verification questions so that I don't assume unverified data.

**Acceptance Criteria:**
- [ ] Every pending generates T1 task: "What did it contract for vs list?"
- [ ] Every backup generates T1 task: "Why backup? Multiple offers or buyer issue?"
- [ ] Tasks appear in Verification section
- [ ] Cannot proceed without addressing T1 tasks

**PRD Reference:** Section 15, 25

---

# EPIC 8: UPWARD EVIDENCE & TRANSFERABILITY

## Epic Summary
Present upside evidence with mandatory transferability testing.

---

### US-MTX-021: Upward Evidence Display
> As an AA, I want to see upward evidence with supporting data so that I understand potential upside.

**Acceptance Criteria:**
- [ ] List upside indicators with evidence
- [ ] Best comparable comp identified with matching characteristics
- [ ] Upside requires closed proof (not pendings or actives alone)

**PRD Reference:** Section 16

---

### US-MTX-022: Transferability Test
> As an AA, I want a transferability test whenever upside is claimed so that I know if the premium applies to PIQ.

**Acceptance Criteria:**
- [ ] REQUIRED when any upward evidence presented
- [ ] Four questions answered:
  - Does PIQ share the SAME premium driver?
  - Is that driver permanent or cosmetic?
  - Is it repeatable without over-capitalizing?
  - Did more than one buyer pay for it?
- [ ] Verdict: ✅ Transferable / ❌ Not transferable / ⚠️ Conditional
- [ ] THE ABSOLUTE RULE: If not proven with closed evidence → assume premium doesn't exist

**PRD Reference:** Section 16

---

# EPIC 9: DOWNWARD EVIDENCE

## Epic Summary
Surface all downside risks with proper classification.

---

### US-MTX-023: Downward Evidence Display
> As an AA, I want to see all downside risk factors so that I don't miss hidden dangers.

**Acceptance Criteria:**
- [ ] List all risk factors
- [ ] Each factor classified with outlier tag
- [ ] Note if PIQ matches low-outlier profile
- [ ] Recommend appropriate cluster anchor if mismatch detected

**PRD Reference:** Section 17

---

### US-MTX-024: Unique Negative Detection
> As an AA, I want to be alerted when PIQ has a unique negative no comp shares so that I know the discount is uncertain.

**Acceptance Criteria:**
- [ ] Detect unique negatives (busy street, backing, power lines)
- [ ] Flag when no comp shares the negative
- [ ] Trigger escalation for senior review
- [ ] Generate T1 verification task

**PRD Reference:** Section 17, 23

---

# EPIC 10: RENOVATION SCOPE INTELLIGENCE

## Epic Summary
Detect when light rehab may outperform full flip based on velocity data.

---

### US-MTX-025: Scope Intelligence Trigger Detection
> As an AA, I want to be alerted when market data suggests light rehab may outperform full flip so that I don't over-improve.

**Acceptance Criteria:**
- [ ] Trigger conditions:
  - FLIP DOM > GOOD DOM by 30+ days
  - Premium actives sitting 75+ DOM
  - Pendings favor "good enough"
  - Price band velocity break
  - Only one flip closed, high DOM
- [ ] Section only renders when triggers fire
- [ ] Clear guidance on scope optimization

**PRD Reference:** Section 18

---

### US-MTX-026: Scope Comparison Table
> As an AA, I want to see scope comparison when triggered so that I can make capital-efficient decisions.

**Acceptance Criteria:**
- [ ] Table: Strategy | Target Tier | Est. DOM | Holding Cost
- [ ] Strategies: Full Flip, Paint & Carpet, As-Is
- [ ] Analysis of velocity penalty vs premium gain
- [ ] Never ties scope to "making deal work"

**PRD Reference:** Section 18

---

# EPIC 11: RISK GATES & ESCALATION

## Epic Summary
Implement deal viability gate, capital at risk indicator, and escalation triggers.

---

### US-MTX-027: What Would Break Display
> As an AA, I want to see what would break the conclusion so that I maintain intellectual humility.

**Acceptance Criteria:**
- [ ] REQUIRED before Final Stance
- [ ] 2-3 specific facts that would invalidate stance
- [ ] Cannot be omitted

**PRD Reference:** Section 19

---

### US-MTX-028: Bot Limitation Flag
> As an AA, I want to be warned when the bot is more likely to be wrong so that I apply extra scrutiny.

**Acceptance Criteria:**
- [ ] Trigger when known failure modes detected:
  - High data density + micro-market contamination
  - Most recent closed non-representative
  - Renovation tiers misclassified
  - Agent info contradicts MLS
  - Market transitioning regimes
- [ ] Warning format with specific concern
- [ ] Triggers escalation recommendation

**PRD Reference:** Section 20

---

### US-MTX-029: Deal Viability Gate
> As an AA, I want a deal viability check so that I don't force deals requiring optimism.

**Acceptance Criteria:**
- [ ] REQUIRED on every analysis
- [ ] Three options:
  - ☑ Deal works at baseline
  - ☑ Deal only works at upper range → ESCALATION REQUIRED
  - ☑ Deal fails without optimistic assumptions → AUTOMATIC 🔴
- [ ] Cannot be bypassed
- [ ] THE ABSOLUTE RULE enforced

**PRD Reference:** Section 21

---

### US-MTX-030: Capital at Risk Indicator
> As an AA, I want to see capital at risk level so that I understand exposure beyond price.

**Acceptance Criteria:**
- [ ] REQUIRED on every analysis
- [ ] Levels: Low / Moderate / High
- [ ] Conditions per PRD:
  - Low: As-is or light cosmetic, fast market, baseline pricing
  - Moderate: Standard renovation, normal DOM
  - High: Full flip, extended DOM, upper range pricing
- [ ] Explanation statement
- [ ] High + upper range only → Mandatory senior review

**PRD Reference:** Section 22

---

### US-MTX-031: Escalation Check
> As an AA, I want automatic escalation triggers so that complex cases get senior review.

**Acceptance Criteria:**
- [ ] REQUIRED on every analysis (even if "No escalation required")
- [ ] Trigger criteria per PRD Section 23
- [ ] Warning format with specific trigger identified
- [ ] "Junior AAs should not finalize acquisition basis without senior validation"

**PRD Reference:** Section 23

---

# EPIC 12: FINAL STANCE OUTPUT

## Epic Summary
Compute and display final stance with proper framing.

---

### US-MTX-032: Final Stance Display
> As an AA, I want a clear final stance so that I know the bot's assessment.

**Acceptance Criteria:**
- [ ] Exactly one stance (computed, not free-text):
  - 🟢 Upper Range Supported
  - 🟡 Baseline Reliable
  - 🔴 Caution — Downside Risk Present
- [ ] Stance wording is fixed (cannot modify)
- [ ] Conditions per PRD Section 24

**PRD Reference:** Section 24

---

### US-MTX-033: Stance Cannot Override Rules
> As an AA, I want stance rules enforced so that confidence doesn't override evidence.

**Acceptance Criteria:**
- [ ] 🔴 with Strong confidence remains 🔴
- [ ] Deal fails without optimism → automatic 🔴
- [ ] Signal Strength is informational, not stance-altering
- [ ] Confidence cannot upgrade stance

**PRD Reference:** Section 24 - Rules

---

### US-MTX-034: Baseline Reliable Clarification
> As an AA, I want a clarification that Baseline Reliable ≠ low risk so that I don't misinterpret.

**Acceptance Criteria:**
- [ ] REQUIRED when stance is 🟡
- [ ] Fixed wording: "Remember: 🟡 Baseline Reliable does NOT mean low risk. It means the average is mathematically representative. Execution risk, negotiation risk, and capital risk still apply."

**PRD Reference:** Section 24 - Critical Clarification

---

### US-MTX-035: Combined Stance Output
> As an AA, I want to see stance with all supporting indicators in one view.

**Acceptance Criteria:**
- [ ] Format:
```
STANCE: 🟡 Baseline Reliable
CONFIDENCE: Strong (14 closed)
SIGNAL STRENGTH: Moderate — relies on pending confirmation
DEAL VIABILITY: Works at baseline
CAPITAL AT RISK: Moderate
```

**PRD Reference:** Section 24

---

# EPIC 13: VERIFICATION TASK SYSTEM

## Epic Summary
Generate tiered verification tasks that enforce the Default to NO rule.

---

### US-MTX-036: Tier 1 Task Generation
> As an AA, I want mandatory Tier 1 tasks generated so that I don't proceed without critical verification.

**Acceptance Criteria:**
- [ ] T1 tasks are BLOCKING (must complete before decision)
- [ ] Categories:
  - Pending/Backup contract prices
  - Unique PIQ negatives
  - Functional feasibility
  - Stance-changing triggers
  - Sqft anomalies
  - Transferability gaps
- [ ] Each task prefixed with [T1]

**PRD Reference:** Section 25

---

### US-MTX-037: Tier 2 Task Generation
> As an AA, I want Tier 2 tasks generated for refinement so that I can dig deeper when needed.

**Acceptance Criteria:**
- [ ] T2 tasks are CONDITIONAL (if time / if conflicting)
- [ ] Categories:
  - Stale actives feedback
  - Renovation scope nuance
  - Buyer profile
  - Secondary comp explanation
- [ ] Each task prefixed with [T2]

**PRD Reference:** Section 25

---

### US-MTX-038: Default to NO Enforcement
> As an AA, I want stance marked PROVISIONAL when T1 incomplete so that I don't lean aggressive.

**Acceptance Criteria:**
- [ ] When T1 tasks incomplete, display:
```
⚠️ TIER-1 VERIFICATION INCOMPLETE

Stance is PROVISIONAL. Do not lean aggressive.
Assume downside risk until T1 tasks are answered.

Unanswered T1 tasks:
□ [Task 1]
□ [Task 2]
```
- [ ] Cannot remove this warning without completing T1
- [ ] System assumes downside until proven otherwise

**PRD Reference:** Section 3.3, Section 25

---

# EPIC 14: HUMAN OVERRIDE PROTOCOL

## Epic Summary
Allow senior override with mandatory documentation and bias tracking.

---

### US-MTX-039: Override Documentation Form
> As a senior operator, I want to document overrides so that we maintain learning loops.

**Acceptance Criteria:**
- [ ] Required fields:
  - Bot Stance
  - Override Stance
  - Signal Ignored
  - Rationale
  - Operator Bias Check
  - Outcome (post-close)
- [ ] Cannot submit override without all fields

**PRD Reference:** Section 27

---

### US-MTX-040: Operator Bias Check
> As a senior operator, I want to self-identify potential bias so that patterns can be detected.

**Acceptance Criteria:**
- [ ] Required selection:
  - Fear
  - FOMO
  - Deal Hunger
  - Time Pressure
  - None
- [ ] Definitions displayed for each
- [ ] Institutional learning: 3+ same bias in 30 days → coaching flag

**PRD Reference:** Section 27

---

# EPIC 15: POST-CLOSE LEARNING LOOP

## Epic Summary
Capture outcomes to improve bot accuracy over time.

---

### US-MTX-041: Post-Close Logging Fields
> As a system, I want to log post-close outcomes so that the bot improves over time.

**Acceptance Criteria:**
- [ ] Required fields:
  - Final Sale Price vs E-Value
  - Final Sale Price vs Bot-Adjusted Range
  - Which signals mattered
  - Which signals were noise
  - Bot stance accuracy
  - Human override outcome
  - Time to close vs predicted DOM
  - Actual vs recommended renovation scope
  - Deal Viability accuracy
  - Capital at Risk accuracy
- [ ] Target: 80%+ deals logged

**PRD Reference:** Section 29

---

# EPIC 16: ENGINEERING GUARDRAILS

## Epic Summary
Enforce schema validation and prevent drift.

---

### US-MTX-042: Schema Validation
> As a system, I want output schema validated so that no sections are omitted or reordered.

**Acceptance Criteria:**
- [ ] All 18 sections present (even if "No signal detected")
- [ ] Sections cannot be reordered
- [ ] "No signal detected" is valid output
- [ ] Stance wording is fixed enum

**PRD Reference:** Section 30

---

### US-MTX-043: False Precision Kill-Switch
> As a system, I want narrative to use directional language only so that we don't imply false precision.

**Acceptance Criteria:**
- [ ] No exact dollar deltas in narrative (tables OK)
- [ ] No precise percentages in narrative
- [ ] Allowed: "materially higher", "upper range", "modest premium"
- [ ] Forbidden: "+$43,200", "exactly 6.2% higher", "$17,500 above average"

**PRD Reference:** Section 3.2

---

### US-MTX-044: The Absolute Rule Enforcement
> As a system, I must treat unproven upside as nonexistent so that capital is protected.

**Acceptance Criteria:**
- [ ] If upside cannot be proven with closed evidence → assume it doesn't exist
- [ ] This rule cannot be overridden by any logic
- [ ] Not "be cautious" — ASSUME IT'S NOT THERE
- [ ] Philosophical spine of the system

**PRD Reference:** Section 3.4

---

# TECHNICAL SPECIFICATIONS

## Data Schema

```typescript
interface MatrixIntelligenceOutput {
  // Section 1: Context Header (fixed)
  contextHeader: string;

  // Section 2: Baseline
  baseline: {
    eValue: number;
    confidence: 'Strong' | 'Moderate' | 'Thin';
    confidenceReason: string;
    signalStrength: 'Strong' | 'Moderate' | 'Weak';
    signalStrengthReason: string;
  };

  // Section 3: PIQ vs Comp Set Fit
  piqFit: {
    comparison: FitComparisonRow[];
    assessment: string;
    hasMajorMismatch: boolean;
  };

  // Section 4: Closed Sales Reality
  closedSales: {
    count: number;
    avgPricePerSqft: number;
    range: { low: number; high: number };
    mean: number;
    median: number;
    meanMedianGap: number;
    mostRecentClosed: ClosedComp;
    outliers: OutlierClassification[];
    averageIntegrity: string;
  };

  // Section 5: Market Behavior
  marketBehavior: {
    inventoryPressure: string;
    velocitySignals: VelocitySignal[];
    listVsClose: string;
    marketRegime: MarketRegime;
  };

  // Section 6: Market Saturation
  marketSaturation: {
    keptActives: number;
    removedActives: number;
    totalBuyerInventory: number;
    saturationLevel: 'HIGH' | 'MODERATE' | 'LOW';
  };

  // Section 7: Competitive Alternatives
  competitiveAlternatives: {
    detected: boolean;
    alternatives: CompetitiveAlt[];
    interpretation: string;
  };

  // Section 8: Status Bucket Intelligence
  statusBuckets: StatusBucket[];

  // Section 9: Upward Evidence
  upwardEvidence: {
    indicators: string[];
    bestComparable?: ClosedComp;
    transferabilityTest?: TransferabilityTest;
  };

  // Section 10: Downward Evidence
  downwardEvidence: {
    riskFactors: RiskFactor[];
    uniqueNegatives: string[];
  };

  // Section 11: Renovation Scope (conditional)
  renovationScope?: {
    triggered: boolean;
    triggerReason: string;
    scopeComparison: ScopeOption[];
    guidance: string;
  };

  // Section 12: What Would Break
  whatWouldBreak: string[];

  // Section 13: Bot Limitation (conditional)
  botLimitation?: {
    triggered: boolean;
    concern: string;
  };

  // Section 14: Deal Viability Gate
  dealViability: {
    worksAtBaseline: boolean;
    onlyWorksAtUpperRange: boolean;
    failsWithoutOptimism: boolean;
  };

  // Section 15: Capital at Risk
  capitalAtRisk: {
    level: 'Low' | 'Moderate' | 'High';
    explanation: string;
  };

  // Section 16: Escalation Check
  escalation: {
    required: boolean;
    triggers: string[];
  };

  // Section 17: Final Stance
  finalStance: {
    stance: '🟢 Upper Range Supported' | '🟡 Baseline Reliable' | '🔴 Caution — Downside Risk Present';
    confidence: string;
    signalStrength: string;
    dealViability: string;
    capitalAtRisk: string;
  };

  // Section 18: Verification Tasks
  verificationTasks: {
    tier1: Task[];
    tier2: Task[];
    tier1Complete: boolean;
  };
}

type MarketRegime =
  | 'Seller-controlled, scarcity-driven'
  | 'Balanced, velocity-driven'
  | 'Buyer-controlled, inventory-heavy'
  | 'Fragmented micro-market';

interface OutlierClassification {
  address: string;
  pricePerSqft: number;
  tag: 'High-Structural' | 'High-Condition' | 'Low-Structural' | 'Low-Condition' | 'False Outlier';
  transferability: 'High' | 'Medium' | 'Low' | 'None';
}

interface TransferabilityTest {
  sharesPremiumDriver: boolean | 'Conditional';
  isPermanentOrCosmetic: 'Permanent' | 'Cosmetic' | 'N/A';
  repeatableWithoutOvercapitalizing: boolean | 'Unknown';
  multipleBuyersPaid: boolean | 'N/A';
  verdict: '✅ Transferable' | '❌ Not transferable' | '⚠️ Conditional';
}

interface Task {
  tier: 'T1' | 'T2';
  task: string;
  completed: boolean;
}
```

---

# ACCEPTANCE CRITERIA CHECKLIST

## Required on Every Output
- [ ] Context Header present at top
- [ ] All 18 sections present (or "No signal detected")
- [ ] Confidence label present
- [ ] Signal Strength present
- [ ] Average Integrity Statement present
- [ ] Market Regime Declaration present
- [ ] Market Saturation includes removed comps
- [ ] Competitive Alternatives checked
- [ ] What Would Break section present
- [ ] Deal Viability Gate present
- [ ] Capital at Risk present
- [ ] Escalation Check present
- [ ] Final Stance present with correct wording
- [ ] Verification Tasks tiered (T1/T2)

## Governance Rules Enforced
- [ ] No value output beyond E-Value
- [ ] Directional language only (no exact deltas in narrative)
- [ ] Outlier tags mandatory when mentioning outliers
- [ ] Transferability Test required for upward evidence
- [ ] Bot Limitation Flag when triggered
- [ ] 🔴 if deal fails without optimism
- [ ] Confidence cannot upgrade stance
- [ ] Default to NO when T1 incomplete
- [ ] The Absolute Rule honored

---

# IMPLEMENTATION PHASES

## Phase 1: Core Output Structure
- Context Header + 18 section skeleton
- Baseline display with confidence/signal strength
- Final stance computation
- Basic verification task generation

## Phase 2: Analysis Engine
- PIQ vs Comp Set Fit logic
- Closed Sales Reality with outlier classification
- Mean vs Median computation
- Average Integrity Statement generation

## Phase 3: Market Intelligence
- Market Behavior Signals
- Market Saturation calculation
- Competitive Alternatives detection
- Market Regime declaration

## Phase 4: Risk Gates
- Deal Viability Gate
- Capital at Risk indicator
- Escalation triggers
- Bot Limitation detection
- What Would Break generation

## Phase 5: Advanced Features
- Renovation Scope Intelligence
- Transferability Test
- Human Override Protocol
- Post-Close Learning Loop

---

# SIGN-OFF

| For Nate (CTO) |
|----------------|
| • 44 user stories across 16 epics |
| • TypeScript schema defined |
| • PRD v2.4 reference linked |
| • 5-phase implementation path |
| • Governance rules codified |
| • QA checklist included |
| • This is a judgment discipline system, not an AVM |

---

*Document prepared for FlipIQ Engineering*
*Version 1.0 — December 28, 2024*
