# Comp Matrix Value Intelligence Bot
## Product Requirements Document (PRD)
### Version 2.4 — FINAL Production-Grade Edition

---

**Status:** P0 – Critical Path
**Owner:** FlipIQ
**Primary User:** Acquisition Associate (AA)
**UI Location:** PIQ → Comps → Matrix View
**Dependencies:** Existing E-Value / Matrix / Map / List logic (UNCHANGED)

---

## 1. PRODUCT OVERVIEW

### 1.1 Purpose

The Comp Matrix Value Intelligence Bot is a **paranoid-by-design interpretation layer** that explains why the existing E-Value may be wrong, in which direction, and what must be verified before an Acquisition Associate trusts it.

**This bot does not calculate value.**
**It does not replace pricing logic.**
**It does not behave like an AVM.**

It acts as a **coach, auditor, and risk filter** layered on top of the current system.

### 1.2 Problem Statement

Even with a strong comp matrix and E-Value, Acquisition Associates can:

- Over-trust averages pulled by outliers
- Chase aspirational pricing unsupported by market absorption
- Over-improve properties where velocity matters more than peak ARV
- Miss hidden risks (inventory pressure, condition skew, micro-market splits)
- Fail to ask the right questions of agents at the right time
- Force deals that only work with optimistic assumptions

**AVMs fail here because they:**
- Smooth data
- Ignore execution risk
- Treat time as irrelevant
- Cannot coach human judgment
- Optimize for accuracy at scale, not decision correctness per deal

### 1.3 Product Goal

Enable Acquisition Associates to:

- Understand what the matrix says
- Understand why it could be wrong
- See upside vs downside evidence
- Identify what must be verified
- Decide how aggressive or conservative to be
- Preserve speed, discipline, and capital efficiency
- Know when a deal requires upside to work

### 1.4 Non-Goals (Hard Guardrails)

The system **MUST NOT:**

| ❌ Forbidden | Why |
|-------------|-----|
| Output a new value | Bot interprets, doesn't calculate |
| Replace or modify E-Value | E-Value is MLS-only baseline |
| Calculate ARV | Human decides ARV |
| Recommend an offer price | Outside scope |
| Adjust comps or filters | Map overlay already did this |
| Infer seller motivation | Speculation forbidden |
| Claim certainty without closed proof | Paranoid by design |
| Behave like Zillow / AVM | Different problem definition |
| Justify deals that only work with optimism | Capital protection mandate |

---

## 2. SYSTEM ARCHITECTURE (NON-NEGOTIABLE)

### 2.1 Separation of Concerns

```
┌─────────────────────────────────────────────────────────────┐
│  MAP TABLE (Upstream)                                       │
│  • Pulls comp universe from MLS + PropertyRadar + other     │
│  • Evaluates each comp: KEEP or REMOVE                      │
│  • Stores full dataset with removal reasons                 │
│  • This is the ONLY data pull — no re-running               │
└─────────────────────────────────────────────────────────────┘
                              ↓ (read-only, no re-pull)
┌─────────────────────────────────────────────────────────────┐
│  MATRIX / E-VALUE ENGINE                                    │
│  • Uses KEPT comps only for calculation                     │
│  • Produces baseline E-Value                                │
│  • Must match Map / Matrix / List 100%                      │
└─────────────────────────────────────────────────────────────┘
                              ↓ (read-only)
┌─────────────────────────────────────────────────────────────┐
│  VALUE INTELLIGENCE BOT (THIS PRD)                          │
│  • Reads FULL Map dataset (KEPT + REMOVED)                  │
│  • Uses KEPT comps for primary analysis                     │
│  • Uses REMOVED comps for saturation + alternatives         │
│  • NEVER re-pulls data                                      │
│  • NEVER modifies the Map or Matrix                         │
│  • Interprets from a different perspective                  │
└─────────────────────────────────────────────────────────────┘
```

**Critical Rule:** The Value Intelligence Bot is a LENS on existing data, not a new data source. It re-evaluates information already pulled at the Map table — it does not re-run the report.

### 2.2 Removed Comps Data Architecture

| Comp Status | E-Value Calculation | Bot Analysis |
|-------------|---------------------|--------------|
| **KEPT** | ✅ Full weight | ✅ Primary analysis |
| **REMOVED** | ❌ Excluded | ✅ Secondary signals: saturation + buyer alternatives |

**Why removed comps matter:**
> "We removed these from OUR value calculation, but we can't remove them from the BUYER'S consideration set."

A buyer doesn't care about our comp radius. They care about:
- What else is available in the same school district
- What they could get for the same money 2 miles away
- Whether driving a few extra minutes gets them a better deal

---

## 3. CORE GOVERNANCE RULES

| Rule | Enforcement |
|------|-------------|
| Never output a value | Directional language only |
| Closed comps are anchor | All narratives start with sold |
| Actives ≠ value | Treated as competition / friction |
| Pendings ≠ proof | Directional only, must verify |
| Silence = correct | Do not mention irrelevant variables |
| Opinion allowed | ONLY if evidence exists |
| Uncertainty → questions | Generate VERIFY tasks |

### 3.1 Bias Rejection Rule

**The system must treat upside and downside asymmetrically.**

- **Downside risk blocks decisions** — caution is mandatory
- **Upside only invites verification** — optimism requires proof

The bot must never assume upside is preferable to downside. It reports asymmetry, not desire. This protects against "deal-hunger bias" that destroys capital.

### 3.2 False Precision Kill-Switch

**The bot must never use exact dollar deltas or precise percentages in narrative language.**

| ✅ Allowed | ❌ Forbidden |
|-----------|-------------|
| "Materially higher" | "+$43,200" |
| "Upper range" | "Exactly 6.2% higher" |
| "Below median cluster" | "$17,500 above average" |
| "Modest premium" | "3.7% premium" |
| "Significant discount" | "$52,000 discount" |

**Exception:** Tables and data summaries may contain exact numbers. Narrative interpretation must use directional language.

### 3.3 Default to NO Rule

**Incomplete verification defaults to conservative execution.**

If Tier-1 tasks are unanswered, the system must assume downside risk until proven otherwise.

**Output pattern when T1 incomplete:**

```
⚠️ TIER-1 VERIFICATION INCOMPLETE

Stance is PROVISIONAL. Do not lean aggressive.
Assume downside risk until T1 tasks are answered.

Unanswered T1 tasks:
□ [Task 1]
□ [Task 2]
```

**Rule:** This stops "soft yes" behavior. No aggression without verification.

### 3.4 THE ABSOLUTE RULE (Immutable)

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│  IF THE SYSTEM CANNOT PROVE UPSIDE WITH CLOSED EVIDENCE,    │
│  IT MUST BEHAVE AS IF UPSIDE DOES NOT EXIST.                │
│                                                             │
│  Not "be cautious."                                         │
│  Not "recommend verification."                              │
│  ASSUME IT'S NOT THERE.                                     │
│                                                             │
│  This rule cannot be overridden by any engineer, PM,        │
│  operator, or executive. It is the philosophical spine      │
│  of this system.                                            │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 4. REQUIRED OUTPUT STRUCTURE (FIXED)

Every response must follow this order:

```
1. CONTEXT HEADER (Why You Are Seeing This)
2. BASELINE (E-Value + Confidence Score + Signal Strength)
3. PIQ vs COMP SET FIT
4. CLOSED SALES REALITY (includes Average Integrity Statement)
5. MARKET BEHAVIOR SIGNALS (includes Market Regime Declaration)
6. MARKET SATURATION (includes Removed Comps)
7. COMPETITIVE ALTERNATIVES (if detected)
8. STATUS BUCKET INTELLIGENCE
9. UPWARD EVIDENCE (if any) + Transferability Test
10. DOWNWARD EVIDENCE (if any)
11. RENOVATION SCOPE INTELLIGENCE (if triggered)
12. WHAT WOULD BREAK THIS CONCLUSION
13. BOT LIMITATION FLAG (if triggered)
14. DEAL VIABILITY GATE
15. CAPITAL AT RISK
16. ESCALATION CHECK
17. FINAL STANCE (🟢🟡🔴 + Signal Strength)
18. VERIFICATION TASKS (Tier 1 + Tier 2)
```

If a section has no signal → explicitly state "No signal detected."

---

## 5. CONTEXT HEADER (Required on Every Output)

### Purpose

Pre-frame the bot as a risk auditor, not a value pusher. Prevents emotional overweighting of confident-sounding sections.

### Required Format

Every output must begin with:

```
┌─────────────────────────────────────────────────────────────┐
│  WHY YOU ARE SEEING THIS                                    │
│                                                             │
│  This analysis exists because the matrix shows potential    │
│  ambiguity that requires human validation.                  │
│                                                             │
│  This is not a pricing opinion.                             │
│  This is not a recommendation to buy.                       │
│  This is a risk audit of the data you already have.         │
└─────────────────────────────────────────────────────────────┘
```

---

## 6. CONFIDENCE SCORE GOVERNANCE

### Purpose

Contextual framing only. **Confidence does not override evidence, stance, or verification tasks.**

### Rules

| Rule | Enforcement |
|------|-------------|
| Confidence is descriptive, not numeric authority | Never treat as a score that "proves" anything |
| Confidence cannot upgrade a stance | 🔴 stays 🔴 regardless of data density |
| Confidence can increase required verification | Low confidence = more questions, not softer language |
| High confidence ≠ safety | Dense data can still mislead |

### Confidence Labels

| Label | Definition |
|-------|------------|
| **Strong** | 15+ closed comps, tight cluster, same tract/school |
| **Moderate** | 7–14 closed comps OR mixed signals present |
| **Thin** | <7 closed comps OR heavy reliance on pendings/actives |

### Required Statement

Every output must include:

> "Confidence reflects data density, not correctness. High confidence does not imply safety."

---

## 7. SIGNAL STRENGTH

### Purpose

Separate from confidence. Indicates how aligned multiple signals are — not data density, but signal coherence.

### Signal Strength Labels

| Label | Definition | Effect |
|-------|------------|--------|
| **Strong** | Multiple signals align (closed + velocity + fit + transferability) | Standard verification |
| **Moderate** | Directional bias exists but verification-heavy | Increased verification |
| **Weak** | Signals conflict or hinge on unverified inputs | Maximum verification, consider escalation |

### Rules

- Signal Strength cannot override stance
- Signal Strength affects how aggressive verification must be
- Weak signal strength may trigger escalation

### Output Format

```
Stance: 🟡 Baseline Reliable
Confidence: Strong (14 closed)
Signal Strength: Moderate — relies on pending confirmation
```

---

## 8. TIME-WEIGHTED EVIDENCE BIAS

### Purpose

Prevent recent pendings from psychologically outweighing proven closed sales.

### Evidence Hierarchy

| Data Type | Weight | Narrative Priority |
|-----------|--------|-------------------|
| Closed ≤ 60 days | **Highest** | Lead with these |
| Closed 61–120 days | High | Secondary anchor |
| Closed > 120 days | Moderate | Context only |
| Pending | Medium | Directional signal |
| Active | Low | Friction/competition only |

### Enforcement Rule

**Narrative must start with most recent closed, not highest price.**

---

## 9. OUTLIER CLASSIFICATION SYSTEM

### Purpose

Not all outliers are equal. Classification determines transferability.

### Required Tags

Every outlier mentioned must be classified:

| Tag | Meaning | Transferability |
|-----|---------|-----------------|
| **High-Structural** | Feature-based premium (lot, view, district, pool) | Low — requires same feature |
| **High-Condition** | Renovation-driven premium | Medium — achievable with capital |
| **Low-Structural** | Location/functional penalty (busy street, 2/1, small lot) | Low — permanent discount |
| **Low-Condition** | Deferred maintenance | Medium — fixable |
| **False Outlier** | Data artifact (permits, concessions, miscoding) | None — exclude from analysis |

### Transferability Rule

> "Averages pulled by High-Structural outliers are less transferable than those pulled by High-Condition outliers."

---

## 10. PIQ vs AVERAGE COMP FIT LOGIC

### Purpose

Identify whether the matrix average represents the PIQ fairly or hides functional gaps.

### Triggered Variables (Only When Relevant)

| Variable | Minor (No Mention) | Material (Flag) | Major (Lead With) |
|----------|-------------------|-----------------|-------------------|
| Bedroom diff | ±0.3 | ±0.5–0.9 | ±1.0+ |
| Bathroom diff | ±0.3 | ±0.5–0.9 | ±1.0+ |
| Sqft diff | ±5% | ±5–10% | ±15%+ |
| Lot diff | ±5% | ±10–15% | ±20%+ |
| Year built | ±3 years | ±5–10 years | ±15+ |

### Functional Feasibility Rules

| PIQ Sqft | Guidance |
|----------|----------|
| < 1,000 sqft | Bedroom expansion unlikely; consider bath optimization only |
| 1,000–1,150 sqft | Possible but uncertain; verify layout with agent/GC |
| > 1,150 sqft | Likely feasible; factor renovation cost |

---

## 11. CLOSED SALES REALITY (ANCHOR)

### Requirements

- Always show range
- Explain spread drivers (condition, lot, location)
- Separate condition tiers
- Compare mean vs median
- **Classify all outliers with tags**
- **Lead with most recent closed (time-weighted)**
- **Include Average Integrity Statement**

### Mean vs Median Rule

| Scenario | Interpretation |
|----------|----------------|
| Mean ≈ Median (within 5%) | Tight cluster, reliable average |
| Mean > Median by 5–10% | High outliers pulling up — use median |
| Mean > Median by 10%+ | Average is misleading — anchor to median cluster |
| Mean < Median | Low outliers dragging down |

### Average Integrity Statement (Required)

Every Closed Sales Reality section must end with:

> **Average Integrity:** "This average is [reliable/unreliable] because [tight cluster | outlier pull | mixed micro-markets | condition skew | school district blend]."

---

## 12. MARKET BEHAVIOR SIGNALS

### Inventory Pressure

- Count ALL comps, including removed
- High removed actives = hidden competition
- Low inventory = seller leverage

### Velocity Signals

| Signal | Detection | Interpretation |
|--------|-----------|----------------|
| DOM by price band | Compare DOM above/below price threshold | Ceiling detection |
| DOM by condition | FLIP DOM vs GOOD DOM | Absorption preference |
| Pending velocity | Days to pending | Demand strength |

### List vs Close Spread

| Pattern | Interpretation |
|---------|----------------|
| Closed below list consistently | Downward pressure / discounting market |
| Closed at list | Balanced market |
| Closed above list | Bidding environment / strength |

### Market Regime Declaration (Required)

Every Market Behavior section must include a one-line regime call:

| Regime | Definition |
|--------|------------|
| **Seller-controlled, scarcity-driven** | Low inventory, fast absorption, above-list closes |
| **Balanced, velocity-driven** | Moderate inventory, normal DOM, at-list closes |
| **Buyer-controlled, inventory-heavy** | High inventory, slow absorption, below-list closes |
| **Fragmented micro-market** | Mixed signals, district/tract splits, no clear direction |

---

## 13. MARKET SATURATION (Including Removed Comps)

### Purpose

Measure total inventory pressure from the buyer's perspective — including comps we removed from value calculation.

### Logic

```
TOTAL UNIVERSE = KEPT actives + REMOVED actives (that are still active listings)

IF total universe > 15 actives → HIGH saturation
IF total universe 8-15 → MODERATE saturation
IF total universe < 8 → LOW saturation
```

### Required Output

```
MARKET SATURATION

Kept Actives: [X]
Removed Actives (still on market): [Y]
Total Buyer Inventory: [X + Y]

Saturation Level: [HIGH / MODERATE / LOW]
```

### Interpretation Rules

| Saturation | Signal |
|------------|--------|
| **HIGH (>15)** | "Even though only X comps are directly comparable, buyers see Y options in their search. This creates pricing pressure." |
| **MODERATE (8-15)** | "Buyer has options. Standard competitive environment." |
| **LOW (<8)** | "Limited alternatives. PIQ has positioning leverage if priced appropriately." |

---

## 14. COMPETITIVE ALTERNATIVES (Removed Comps Analysis)

### Purpose

Surface "removed" comps that could steal buyers from PIQ. We removed them from value calculation — but buyers can still choose them.

### Logic

```
FOR EACH removed comp:
  IF same school district as PIQ
  AND price/sqft is LOWER than PIQ target
  AND condition is EQUAL or BETTER
  THEN → flag as COMPETITIVE ALTERNATIVE
```

### Required Output (When Detected)

```
COMPETITIVE ALTERNATIVES DETECTED

The following removed comps share PIQ's school district but offer
potentially better value positioning:

| Address | Distance | $/sqft | Condition | School | Signal |
|---------|----------|--------|-----------|--------|--------|
| [addr]  | [X] mi   | $[X]   | [cond]    | [name] | [why]  |

⚠️ BUYER FRICTION: A motivated buyer may expand search radius and
find these alternatives. PIQ must compete on features or price.
```

### When No Alternatives Detected

```
COMPETITIVE ALTERNATIVES: None detected

No removed comps in PIQ's school district offer materially better
value positioning. PIQ has reduced buyer leakage risk.
```

---

## 15. STATUS BUCKET INTELLIGENCE

| Status | What It Tells You | What Can Mislead | Required Action |
|--------|-------------------|------------------|-----------------|
| **Closed** | What buyers paid | Condition/location mix | Anchor here |
| **Pending** | Direction of demand | Contract price unknown | Call agent |
| **Backup** | Hot listing OR deal fell apart | Ambiguous | Ask why backup |
| **Active** | Competition / friction | List prices can be fantasy | Measure DOM + pressure |

**Pendings and backups ALWAYS generate verification questions.**

---

## 16. UPWARD EVIDENCE + TRANSFERABILITY TEST

### Purpose

Prevent AAs from assuming premium comps apply to PIQ.

### Transferability Test (Required When Upside Exists)

Whenever upward evidence is presented, the bot must answer these questions:

```
TRANSFERABILITY CHECK:
□ Does PIQ share the SAME premium driver?
□ Is that driver permanent or cosmetic?
□ Is it repeatable without over-capitalizing?
□ Did more than one buyer pay for it?
```

### Rule

> "If a premium driver is not transferable, it cannot justify upper range positioning."

### The Absolute Rule Applies

If transferability cannot be proven with closed evidence → **assume the premium does not exist.**

---

## 17. DOWNWARD EVIDENCE

### Requirements

- List all risk factors
- Classify using outlier tags
- Note if PIQ matches low-outlier profile
- Anchor to appropriate cluster if mismatch detected

---

## 18. RENOVATION SCOPE INTELLIGENCE (CRITICAL)

### Purpose

Prevent over-improvement and capital drag. Identify when light rehab outperforms full flip.

### Scope Governance Rule

**Renovation guidance exists to optimize velocity, risk, and capital efficiency — not to maximize price or justify a higher valuation.**

The system must never:
- Tie rehab scope to hitting a value target
- Say "worth $X if flipped"
- Assume renovation = automatic premium
- Recommend scope to "make the deal work"

### Trigger Conditions

| Trigger | Detection |
|---------|-----------|
| FLIP DOM > GOOD DOM by 30+ days | Absorption ceiling |
| Premium actives sitting 75+ DOM | Market rejecting top tier |
| Pendings favor "good enough" | Buyers choosing velocity |
| Price band velocity break | Threshold where market chokes |
| Only one flip closed, high DOM | Premium not repeatable |

---

## 19. WHAT WOULD BREAK THIS CONCLUSION (Required)

### Purpose

Force intellectual humility. Prevent overconfidence in any stance.

### Required Format

Before Final Stance, output must include:

```
WHAT WOULD BREAK THIS CONCLUSION:

1. [Specific fact that would invalidate stance]
2. [Specific fact that would invalidate stance]
3. [Specific fact that would invalidate stance]
```

---

## 20. BOT FAILURE MODES (Required When Triggered)

### Purpose

Prevent blind trust. The bot is not an oracle — it has known failure modes.

### The Bot Is More Likely to Be Wrong When:

| Failure Mode | Why | Detection |
|--------------|-----|-----------|
| High data density + micro-market contamination | Volume masks blend | District/tract mixing detected |
| Most recent closed is non-representative | Recency bias | Recent sale differs materially from cluster |
| Renovation tiers misclassified by photos | Image interpretation limits | Photos ambiguous or unavailable |
| Agent-provided information is incorrect but confident | Source quality | Agent response contradicts MLS |
| Market is transitioning regimes | Current state ≠ near future | Rate shock, inventory spike, major event |

### Required Output Pattern (When Detected)

```
⚠️ BOT LIMITATION DETECTED

Current signals may over-weight [specific concern].
Human validation is especially critical.

Specific concern: [description]
```

---

## 21. DEAL VIABILITY GATE (Critical)

### Purpose

Prevent humans from abusing "Upper Range Supported" to justify thin deals. Protect capital, not ego.

### Required Output (Every Analysis)

```
DEAL VIABILITY CHECK:

□ Deal works at baseline
□ Deal only works at upper range
□ Deal fails without optimistic assumptions
```

### Interpretation Rules

| Viability | Action |
|-----------|--------|
| **Works at baseline** | Standard proceed with verification |
| **Only works at upper range** | ⚠️ **ESCALATION REQUIRED** — senior must validate |
| **Fails without optimistic assumptions** | 🔴 **AUTOMATIC CAUTION** — regardless of other signals |

### Rule

> "If a deal requires upside to work, and upside cannot be proven with closed evidence, the deal does not work."

This is the Deal Viability Gate. It cannot be bypassed.

---

## 22. CAPITAL AT RISK (Required)

### Purpose

Reframe decisions away from price obsession toward capital exposure.

### Capital at Risk Indicator

| Level | Meaning | Conditions |
|-------|---------|------------|
| **Low** | Short hold, liquid buyer pool | As-is or light cosmetic, fast market, baseline pricing |
| **Moderate** | Rehab + market exposure | Standard renovation, normal DOM, baseline pricing |
| **High** | Long hold + premium pricing + leverage | Full flip, extended DOM, upper range pricing, tight margins |

### Required Output

```
CAPITAL AT RISK: [Low / Moderate / High]

[Explanation: e.g., "Premium pricing + extended DOM + full renovation
exposure creates significant capital drag if market softens."]
```

### Escalation Trigger

If Capital at Risk = High AND Deal Viability = "Only works at upper range" → **Mandatory senior review**

---

## 23. ESCALATION TRIGGERS (Required)

### Purpose

Protect junior AAs from overreaching. Automatically flag cases for senior review.

### Escalation Criteria

**Escalate to senior review when ANY of the following occur:**

| Trigger | Why |
|---------|-----|
| 🔴 Caution + Moderate/Strong Confidence | Unusual pattern — normally 🔴 has thin data |
| Transferability Test partially passes | Ambiguous upside — needs expert judgment |
| Renovation Scope Intelligence triggers AND acquisition basis is tight | Capital risk requires senior validation |
| Unique negative detected with no matching comps | Cannot properly discount without senior input |
| Agent responses contradict MLS in material way | Data integrity issue |
| Signal Strength = Weak | Signals conflict — not suitable for junior decision |
| Bot Limitation Flag triggered | Known failure mode — needs human override consideration |
| Deal only works at upper range | Capital protection mandate |
| Capital at Risk = High + marginal viability | Maximum exposure requires validation |

### Required Output Pattern

```
⚠️ ESCALATION RECOMMENDED

This case meets criteria for senior review due to: [specific trigger]

Junior AAs should not finalize acquisition basis without senior validation.
```

---

## 24. FINAL STANCE (MANDATORY)

### Stance Options

| Icon | Label | Meaning | Conditions |
|------|-------|---------|------------|
| 🟢 | **Upper Range Supported** | Evidence supports upper range | Closed confirm highs + fast pendings + PIQ aligns + low inventory + no unique negatives + transferability PASSED + deal works at baseline |
| 🟡 | **Baseline Reliable** | Tight cluster, no pressure | Mean ≈ median + strong closed count + PIQ aligns + no dominant outliers |
| 🔴 | **Caution — Downside Risk Present** | Downside risk identified | Outliers pull average + high inventory + PIQ has unique negatives + functional mismatch + transferability FAILED + OR deal fails without optimism |

### Critical Clarification

> **🟡 Baseline Reliable does NOT mean low risk.**
>
> It means the average is mathematically representative of the comp set.
>
> Execution risk, negotiation risk, and capital risk still apply.
>
> "Baseline Reliable" describes the data — not the deal.

### Stance + Signal Strength + Viability Output

```
STANCE: 🟡 Baseline Reliable
CONFIDENCE: Strong (14 closed)
SIGNAL STRENGTH: Moderate — relies on pending confirmation
DEAL VIABILITY: Works at baseline
CAPITAL AT RISK: Moderate
```

### Rules

- Exactly one stance must be selected
- Stance cannot be overridden by confidence
- A 🔴 with Strong confidence remains 🔴
- If Deal Viability = "Fails without optimism" → stance is automatically 🔴
- Signal Strength is informational, not stance-altering

---

## 25. VERIFICATION TASK GENERATION

### Task Prioritization

All verification tasks must be classified into two tiers:

#### Tier 1 — Mandatory (Must Complete Before Decision)

These tasks **block** trusting the stance. Cannot proceed without answers.

| Category | Examples |
|----------|----------|
| Pending/Backup contract price | "What did it contract for vs list?" |
| Unique PIQ negatives | "Any busy street, backing, power line exposure?" |
| Functional feasibility | "Can layout support 3/2 conversion?" |
| Any trigger that changed stance color | "Why did this comp achieve/fail at this price?" |
| Sqft anomalies | "Is sqft permitted or an addition?" |
| Transferability gaps | "Does PIQ actually have the premium feature?" |

#### Tier 2 — Conditional (If Time / If Conflicting)

These tasks **refine** strategy but do not block decisions.

| Category | Examples |
|----------|----------|
| Stale actives feedback | "Why hasn't the active at $X sold?" |
| Renovation scope nuance | "What specific updates did the FLIP comp include?" |
| Buyer profile nuance | "First-time buyers or move-up in this neighborhood?" |
| Secondary comp explanation | "Why did the low comp sell so cheap?" |

#### Default to NO Rule Enforcement

If T1 tasks remain incomplete:

```
⚠️ TIER-1 VERIFICATION INCOMPLETE

Stance is PROVISIONAL. Do not lean aggressive.
Assume downside risk until T1 tasks are answered.

Unanswered T1 tasks:
□ [Task 1]
□ [Task 2]
```

---

## 26. AGENT QUESTION QUALITY SCORING

### Purpose

Train AAs to evaluate agent responses, not just collect them.

### Response Quality Matrix

| Agent Response Pattern | Interpretation | Action |
|------------------------|----------------|--------|
| Specific + confident | Likely reliable | Accept with normal caution |
| Specific + hesitant | May be guessing | Seek secondary confirmation |
| Vague + deflecting | Red flag | Escalate suspicion |
| "I don't know" | Honest | Requires secondary confirmation |
| Changes story | Major red flag | Treat all claims as unverified |
| Contradicts MLS data | Data error or agent error | Verify source of truth, flag Bot Limitation |

---

## 27. HUMAN OVERRIDE PROTOCOL

### Purpose

Allow senior operators to disagree while preserving learning loops.

### Rule

> "Human override is allowed, but must document which bot signal is being ignored and why."

### Required Documentation

When overriding bot stance, operator must record:

| Field | Requirement |
|-------|-------------|
| Bot Stance | What the bot concluded |
| Override Stance | What operator decided |
| Signal Ignored | Which specific evidence was discounted |
| Rationale | Why operator believes they have better information |
| **Operator Bias Check** | Fear / FOMO / Deal Hunger / Time Pressure / None |
| Outcome | (Filled post-close) Was override correct? |

### Operator Bias Flag

**Required field:** When override occurs, operator must self-identify potential bias:

| Bias Type | Definition |
|-----------|------------|
| **Fear** | Avoiding a good deal due to excessive caution |
| **FOMO** | Chasing deal due to fear of missing opportunity |
| **Deal Hunger** | Forcing deal to hit quota or justify effort |
| **Time Pressure** | Rushing decision due to external deadline |
| **None** | Genuine disagreement with data interpretation |

### Institutional Learning Rule

> If the same operator logs the same bias type 3+ times in 30 days → internal coaching flag.

---

## 28. KNOWN BLIND SPOTS

### Purpose

Build trust by acknowledging what the bot cannot see.

### The Bot Cannot Directly Observe:

| Blind Spot | Why It Matters | Mitigation |
|------------|----------------|------------|
| Seller psychology | Motivation affects negotiation, not value | Generate question, don't assume |
| Off-market buyer demand | Pipeline not visible in MLS | Cannot observe — state limitation |
| Agent competence or motivation | Listing quality varies | Cannot observe — verify with calls |
| Unlisted defects | Foundation, neighbors, stigma, mold | Generate inspection/verification task |
| Negotiated concessions | Unless verified with agent | Always ask on pendings |
| Permit history | Unless cross-referenced with county | Cross-check if sqft anomaly |
| Future market shifts | Bot sees current state only | State current-state-only limitation |

### Rule

> "When blind spots exist, the bot must generate questions, not assumptions."

---

## 29. POST-CLOSE LEARNING LOOP (Required)

### Purpose

Turn FlipIQ into a learning valuation system, not a static one.

### Required Post-Close Logging

For every closed deal analyzed by the bot, log:

| Field | Purpose |
|-------|---------|
| Final Sale Price vs E-Value | Calibration accuracy |
| Final Sale Price vs Bot-Adjusted Range | Directional accuracy |
| Which signals mattered | Pattern reinforcement |
| Which signals were noise | Suppression learning |
| Bot stance accuracy | QA and calibration |
| Human override outcome | Bias detection |
| Time to close vs predicted DOM | Velocity accuracy |
| Actual renovation scope vs recommended | Scope intelligence calibration |
| Deal Viability accuracy | Gate effectiveness |
| Capital at Risk accuracy | Exposure prediction |

### Rule

> "Unlogged outcomes degrade system intelligence over time."

---

## 30. ENGINEERING GUARDRAILS

### Purpose

Prevent future drift and ensure consistent behavior.

### Required Constraints

| Constraint | Enforcement |
|------------|-------------|
| Output must be schema-validated | No free-form responses |
| Sections cannot be omitted | All 18 sections present (even if "No signal detected") |
| Sections cannot be reordered | Fixed sequence |
| Context Header is mandatory | First element always |
| Renovation Scope only renders when triggers fire | Conditional section |
| Stance color is computed, not free-text | 🟢🟡🔴 only |
| Stance wording is fixed | "Upper Range Supported" / "Baseline Reliable" / "Caution — Downside Risk Present" |
| "No signal detected" is valid and preferred | Silence > noise |
| Verification tasks must be tiered | T1/T2 classification required |
| Average Integrity Statement is mandatory | Cannot skip |
| Confidence label is mandatory | Strong/Moderate/Thin |
| Signal Strength is mandatory | Strong/Moderate/Weak |
| Outlier tags are mandatory | Cannot mention outlier without classification |
| Market Regime is mandatory | One-line call required |
| Market Saturation is mandatory | Include removed comps |
| Competitive Alternatives is mandatory | Even if "none detected" |
| Transferability Test required for upside | Cannot claim upper range without test |
| What Would Break is mandatory | 2–3 items before stance |
| Bot Limitation Flag when triggered | Cannot suppress known failure modes |
| Deal Viability Gate is mandatory | Every analysis |
| Capital at Risk is mandatory | Every analysis |
| Escalation Check is mandatory | Even if "No escalation required" |
| No exact dollar deltas in narrative | Tables only |
| The Absolute Rule cannot be overridden | Philosophical spine |

---

## 31. ACCEPTANCE CRITERIA (QA)

- [ ] Context Header present at top
- [ ] No value output beyond E-Value
- [ ] Every claim tied to data
- [ ] No irrelevant variables surfaced
- [ ] Directional language only (no exact deltas in narrative)
- [ ] Verification tasks generated when uncertain
- [ ] Verification tasks properly tiered (T1/T2)
- [ ] Default to NO enforced when T1 incomplete
- [ ] Renovation scope only appears when triggered
- [ ] Mean vs median computed when variance exists
- [ ] Average Integrity Statement present
- [ ] All outliers classified with tags
- [ ] Time-weighted evidence (recent closed first)
- [ ] Market Regime Declaration present
- [ ] Market Saturation includes removed comps
- [ ] Competitive Alternatives checked
- [ ] Transferability Test present when upside claimed
- [ ] What Would Break section present
- [ ] Bot Limitation Flag present when triggered
- [ ] Deal Viability Gate present
- [ ] Capital at Risk present
- [ ] Escalation Check present
- [ ] Signal Strength present
- [ ] Map / Matrix / List remain untouched
- [ ] Final stance always present with correct wording
- [ ] Baseline Reliable clarification included
- [ ] Stance cannot be overridden by confidence
- [ ] Stance is 🔴 if deal fails without optimism
- [ ] Agent questions specific and actionable
- [ ] Blind spots acknowledged when relevant
- [ ] Confidence label present and accurate
- [ ] Post-close fields defined for logging
- [ ] Operator Bias Check field in override protocol
- [ ] The Absolute Rule honored

---

## 32. SUCCESS METRICS

| Metric | Target |
|--------|--------|
| AA decision confidence | ↑ measurable improvement |
| Time to clarity | 15+ min → < 3 min |
| Over-improved rehabs | ↓ fewer capital traps |
| ARV disputes | ↓ fewer internal challenges |
| Deal velocity | ↑ faster capital turns |
| Bad buy prevention | Zero preventable losses |
| Tier 1 task completion rate | 100% before decision |
| Override documentation rate | 100% when override occurs |
| Operator bias pattern detection | Active monitoring |
| Post-close feedback capture | 80%+ deals logged |
| Escalation compliance | 100% when triggered |
| Bot accuracy (post-close) | Tracked and improving |
| Deal Viability Gate effectiveness | Zero "fails without optimism" approved |
| Capital at Risk accuracy | Predicted vs actual exposure |

---

## 33. ONE-LINE ENGINEERING SUMMARY

Build a paranoid, evidence-driven interpretation layer that challenges the existing comp matrix without altering it, uses removed comps for saturation and alternative signals, classifies outliers by transferability, leads with time-weighted evidence, forces intellectual humility through break-this-conclusion questions, flags its own limitations, gates deals that require optimism to work, quantifies capital at risk, tracks operator bias patterns, triggers escalation when appropriate, defaults to NO when verification is incomplete, treats upside as nonexistent until proven with closed evidence, documents all overrides, and feeds outcomes back into institutional learning.

---

*Document prepared for FlipIQ Engineering*
*Version 2.4 (FINAL) — December 28, 2024*
