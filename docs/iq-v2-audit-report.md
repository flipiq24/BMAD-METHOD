# iQ v2 Plan vs BMAD Repo — Discrepancy & Cleanup Report
## Version 1.0 — January 3, 2025

---

# 1. WHAT WAS REVIEWED

## 1.1 Section Status Summary

| # | Section | Status | Doc Evidence | Repo Evidence |
|---|---------|--------|--------------|---------------|
| 1 | Introduction to iQ | CONTEXT ONLY | ✓ High-level summary | N/A |
| 2 | User Stories | CONTEXT ONLY (needs v2 cleanup) | ✓ Embedded in doc | Partial in PRDs |
| 3 | Bots/iQ (AA0) | DONE — VERIFY ONLY | ✓ "Single conversational interface" | No PRD (expected) |
| 4 | Bots/Deal Outreach / PiQ / Agent | DONE — VERIFY ONLY | ✓ "DMaster, PIQ, D1-D3" | No PRD (expected) |
| 5 | Bots/Comps > Map | **V2 BUILD SCOPE** | ✓ Geographic analysis | comp-map-prd.md |
| 6 | Bots/Comps > Matrix | **V2 BUILD SCOPE** | ✓ Statistical matrix | comp-matrix-prd.md |
| 7 | Bots/Comps > List | **V2 BUILD SCOPE** | ✓ Condition grouping | comp-list-prd.md |
| 8 | Bots/Investment Analysis | **V2 BUILD SCOPE** | ✓ Buy box alignment | investment-analysis-prd.md |
| 9 | Bots/Post Call and Practice | **V2 BUILD SCOPE** | ✓ Call transcription | post-call-practice-prd.md |
| 10 | Bots/Notes and Communication | **V2 BUILD SCOPE** | ✓ Follow-up sequences | notes-communication-prd.md |
| 11 | Bots/My Stats | **V2 BUILD SCOPE** | ✓ Personal metrics | ❌ **MISSING** |
| 12 | Bots/AM Deal Support | **V2 BUILD SCOPE** | ✓ Management oversight | am-deal-support-prd.md |

---

# 2. V2 ALIGNMENT MATRIX

| Module | Status | Doc Complete (0-10) | Repo Coverage | Evidence (File Paths) | Discrepancies | Missing Definitions | Missing Implementation | Recommended Fix | Priority |
|--------|--------|---------------------|---------------|----------------------|---------------|---------------------|----------------------|-----------------|----------|
| **Comps > Map** | V2 BUILD | 8 | Yes | `docs/comp-map-prd.md`, `docs/comp-map-training-samples.md` | Minor: Doc mentions "5 comp bots" but repo has 3 | User stories missing from PRD | Training data only, no code | None needed | P1 |
| **Comps > Matrix** | V2 BUILD | 9 | Yes | `docs/comp-matrix-prd.md`, `docs/comp-matrix-user-stories.md`, `docs/comp-matrix-training-samples.md` | None significant | None | Training data only, no code | None needed | P1 |
| **Comps > List** | V2 BUILD | 9 | Yes | `docs/comp-list-prd.md`, `docs/comp-list-user-stories.md`, `docs/comp-list-training-samples.md` | Bot ID inconsistency (C3 in PRD vs C-OVERLAY-LIST expected) | None | Training data only, no code | Standardize Bot IDs | P2 |
| **Investment Analysis** | V2 BUILD | 10 | Yes | `docs/investment-analysis-prd.md`, `docs/investment-analysis-user-stories.md`, `docs/investment-analysis-training-samples.md` | None | None | Training data only, no code | None needed | P0 |
| **Post Call & Practice** | V2 BUILD | 9 | Yes | `docs/post-call-practice-prd.md`, `docs/post-call-practice-user-stories.md`, `docs/post-call-practice-training-samples.md` | BLOCKER noted: Dialpad API verification pending | ElevenLabs integration undefined | Training data only, no code | Verify Dialpad API | P0 |
| **Notes & Communication** | V2 BUILD | 9 | Yes | `docs/notes-communication-prd.md`, `docs/notes-communication-user-stories.md`, `docs/notes-communication-training-samples.md` | Depends on PC1 output (circular?) | Gmail/Outlook OAuth flow undefined | Training data only, no code | Define OAuth flow | P1 |
| **My Stats** | V2 BUILD | 0 | **NO** | ❌ No files exist | **CRITICAL: Entire module missing** | Everything: inputs, outputs, metrics, UI | No PRD, no stories, no samples | **CREATE PRD IMMEDIATELY** | **P0** |
| **AM Deal Support** | V2 BUILD | 9 | Yes | `docs/am-deal-support-prd.md`, `docs/am-deal-support-user-stories.md`, `docs/am-deal-support-training-samples.md` | Depends on NC1, PC1, IAMaster (aggregation bot) | Google Sheets API credentials undefined | Training data only, no code | Define API credentials | P1 |
| **iQ (AA0)** | DONE-VERIFY | N/A | N/A | No PRD expected | Doc says "complete" — matches repo assumption | N/A | N/A | Verify consistency | P2 |
| **Deal Outreach/PiQ/Agent** | DONE-VERIFY | N/A | N/A | No PRD expected | Doc says "complete" — matches repo assumption | N/A | N/A | Verify consistency | P2 |

---

# 3. DISCREPANCIES (RANKED BY IMPACT)

## 3.1 P0 BLOCKERS — Must Fix Before Coding

### DISCREPANCY #1: My Stats Bot — ENTIRELY MISSING

| Field | Detail |
|-------|--------|
| **Problem** | My Stats is listed in v2 BUILD scope but has NO documentation in repo |
| **Impact** | Cannot implement. 1 of 8 v2 modules has 0% specification. |
| **Doc Evidence** | Google Doc: "My Stats - Personal performance metrics and trends" |
| **Repo Evidence** | `docs/bot-coordination-haris.md:41` mentions it, but no PRD exists |
| **Fix Required** | CREATE: `my-stats-prd.md`, `my-stats-user-stories.md`, `my-stats-training-samples.md` |

**Required My Stats PRD Content (UNKNOWN — needs definition):**
- [ ] What metrics are tracked? (calls, offers, closes, conversion rates?)
- [ ] What time periods? (daily, weekly, monthly, YTD?)
- [ ] What comparisons? (vs team avg, vs personal goals, vs historical?)
- [ ] What UI location? (PIQ tab? Dedicated page?)
- [ ] What data sources? (Dialpad, Pipeline DB, Deal Close records?)
- [ ] What visualizations? (charts, tables, trends?)

---

### DISCREPANCY #2: Dialpad API — Unverified Blocker

| Field | Detail |
|-------|--------|
| **Problem** | Post-Call Bot (PC1) depends on Dialpad API providing immediate transcript with speaker separation |
| **Impact** | If Dialpad doesn't provide this, +4 hours to build secondary transcription layer |
| **Doc Evidence** | `post-call-practice-prd.md:85-87` — explicit BLOCKER callout |
| **Repo Evidence** | No verification documented |
| **Fix Required** | VERIFY: Does Dialpad API provide immediate transcript with speaker separation? Document Go/No-Go decision. |

---

### DISCREPANCY #3: Investment Analysis — P0 Critical Path but No Timeline Clarity

| Field | Detail |
|-------|--------|
| **Problem** | Investment Analysis PRD says "MVP (Week 1)" but total dev timeline says "14 hrs (~2 days)" |
| **Impact** | Conflicting timeline guidance for engineering |
| **Doc Evidence** | `investment-analysis-prd.md:96-104` says "Week 1" MVP |
| **Repo Evidence** | Development Plan says "14 hrs (~2 days)" |
| **Fix Required** | ALIGN: Update PRD to remove "Week 1" terminology, use hours only |

---

## 3.2 P1 ISSUES — Fix During Implementation

### DISCREPANCY #4: Bot ID Inconsistency

| Field | Detail |
|-------|--------|
| **Problem** | Bot IDs use inconsistent naming conventions |
| **Impact** | Engineering confusion, code organization issues |
| **Doc Evidence** | IAMaster, PC1, NC1, AMD1, C3, C-OVERLAY-MAP |
| **Repo Evidence** | Mixed patterns across PRDs |
| **Fix Required** | STANDARDIZE: Define naming convention (e.g., `{CATEGORY}{NUMBER}` or `{CATEGORY}-{FUNCTION}`) |

**Current Bot IDs:**
| Bot | Current ID | Suggested Standard |
|-----|------------|-------------------|
| Investment Analysis | IAMaster | IA1 |
| Comp Map | C-OVERLAY-MAP | CM1 |
| Comp Matrix | (unlabeled in PRD) | CMX1 |
| Comp List | C3 | CL1 |
| Post-Call & Practice | PC1 | PC1 ✓ |
| Notes & Communication | NC1 | NC1 ✓ |
| AM Deal Support | AMD1 | AMD1 ✓ |
| My Stats | (missing) | MS1 |

---

### DISCREPANCY #5: NC1/PC1 Circular Dependency

| Field | Detail |
|-------|--------|
| **Problem** | NC1 consumes PC1 output, but both need to be built. Unclear which is first. |
| **Impact** | Build order confusion |
| **Doc Evidence** | `notes-communication-prd.md:23-30` — "NC1 consumes PC1's output" |
| **Repo Evidence** | Both listed as P1 priority |
| **Fix Required** | CLARIFY: PC1 must be built FIRST (or stub its output schema for NC1 dev) |

---

### DISCREPANCY #6: Google Doc Says "5 Comp Bots" — Repo Has 3

| Field | Detail |
|-------|--------|
| **Problem** | Google Doc summary says "Comp Analysis (5 Bots)" but repo has Map, Matrix, List (3 bots) |
| **Impact** | Unknown scope — are 2 bots missing or is doc outdated? |
| **Doc Evidence** | Google Doc: "Three-level education system" + "Computer vision property updates" |
| **Repo Evidence** | Only 3 PRDs: comp-map, comp-matrix, comp-list |
| **Fix Required** | CLARIFY: What are the other 2 comp bots? Or update doc to say 3 bots. |

---

### DISCREPANCY #7: Gmail/Outlook OAuth Undefined

| Field | Detail |
|-------|--------|
| **Problem** | NC1 lists Gmail/Outlook API integration but no OAuth flow defined |
| **Impact** | Cannot implement email aggregation without OAuth spec |
| **Doc Evidence** | `notes-communication-prd.md:349-350` — "Gmail API: Email sync (OAuth per AA) — P1 (Phase 2)" |
| **Repo Evidence** | No OAuth implementation details |
| **Fix Required** | DEFINE: OAuth flow, scopes required, token storage, refresh logic |

---

## 3.3 P2 ISSUES — Fix When Convenient

### DISCREPANCY #8: Comp Map PRD Missing User Stories File

| Field | Detail |
|-------|--------|
| **Problem** | Comp Map has user stories EMBEDDED in PRD but no separate user-stories.md file |
| **Impact** | Inconsistent documentation structure |
| **Doc Evidence** | `comp-map-prd.md:100-150` — US-MAP-001 through US-MAP-004 embedded |
| **Repo Evidence** | No `comp-map-user-stories.md` file exists |
| **Fix Required** | CREATE: Extract user stories to separate file OR document that embedding is intentional |

---

### DISCREPANCY #9: Training Samples Format Inconsistency

| Field | Detail |
|-------|--------|
| **Problem** | Training samples use different formats across bots |
| **Impact** | AI training inconsistency |
| **Doc Evidence** | comp-map uses UI mockups, investment-analysis uses JSON I/O |
| **Repo Evidence** | Compare `comp-map-training-samples.md` vs `investment-analysis-training-samples.md` |
| **Fix Required** | STANDARDIZE: Define canonical training sample format |

---

# 4. V2 USER STORIES (CLEAN REWRITE)

## 4.1 Format Standard

All user stories follow Given/When/Then with explicit outputs:

```
**Story ID**: {MODULE}-US-{NUMBER}
**As a**: {Role}
**I want to**: {Action}
**So that**: {Benefit}

**Given**: {Preconditions}
**When**: {Trigger}
**Then**: {Expected Output}

**Output Artifact**: {Exact format}
**Stored/Displayed**: {Location}
**Acceptance Criteria**: {Testable conditions}
```

---

## 4.2 DONE — VERIFY ONLY (Reference Stories)

### iQ Bot (AA0) — DONE; Not in v2 Build

> **Note**: These stories are REFERENCE ONLY. iQ Bot is complete.

**IQ-US-001**: As an AA, I want to enter properties via voice/text so that I can quickly add new leads.
- Status: DONE — Verify voice input still works

**IQ-US-002**: As an AA, I want to ask natural language questions so that I can navigate the system without memorizing commands.
- Status: DONE — Verify NLP routing works

---

### Deal Outreach / PiQ / Agent (DMaster, D1-D3) — DONE; Not in v2 Build

> **Note**: These stories are REFERENCE ONLY. Deal Outreach is complete.

**DO-US-001**: As an AA, I want to see a Deal Focus Index for each property so that I can prioritize my outreach.
- Status: DONE — Verify DFI calculation

**DO-US-002**: As an AA, I want agent transaction history from Agent365 so that I know how to approach each agent.
- Status: DONE — Verify Agent365 integration

---

## 4.3 V2 BUILD SCOPE — User Stories

### Comps > Map (CM1)

**CM1-US-001**: View Analyzed Comps on Map
- **As a**: AA viewing comps
- **I want to**: see my pre-filtered comps displayed on a map with KEEP/REMOVE classifications
- **So that**: I understand geographic relevance at a glance

- **Given**: AA has filtered comps for a subject property
- **When**: AA clicks iQ button on Comps Map view
- **Then**: Overlay displays with:
  - Subject property as "S" pin
  - Comp pins with price labels
  - KEEP section (left) with High confidence comps
  - REMOVE section (right) with Low relevance comps
  - ABC Summary (Property/Lot/Location) comparing subject to comps

- **Output Artifact**: Map overlay with classified comp cards
- **Stored/Displayed**: PIQ → Comps Tab → Map View (overlay below map)
- **Acceptance Criteria**:
  - [ ] Subject displays as "S" pin distinct from comps
  - [ ] Each comp shows confidence badge (High/Weak)
  - [ ] KEEP/REMOVE sections sort by relevance
  - [ ] "WHY KEPT" / "WHY REMOVED" explanations display

---

**CM1-US-002**: ABC Summary Comparison
- **As a**: AA evaluating comps
- **I want to**: see a 3-column summary (Property/Lot/Location) comparing subject to comps
- **So that**: I understand key differences without reading every detail

- **Given**: Comps have been analyzed
- **When**: iQ overlay is displayed
- **Then**: ABC Summary shows:
  - Column A (Property): Garage, Solar, Stories differences
  - Column B (Lot): Pool, Lot Shape, Size differences
  - Column C (Location): Tract, HOA, Golf Course differences

- **Output Artifact**: 3-column text summary
- **Stored/Displayed**: PIQ → Comps Tab → Map View → Below map, above KEEP/REMOVE
- **Acceptance Criteria**:
  - [ ] Only variables that DIFFER are shown
  - [ ] Each difference includes relevance explanation
  - [ ] Loads in < 2 seconds

---

### Comps > Matrix (CMX1)

**CMX1-US-001**: E-Value Interpretation
- **As a**: AA reviewing comps in matrix view
- **I want to**: understand what the E-Value MEANS (not just what it shows)
- **So that**: I can make informed pricing decisions

- **Given**: E-Value has been calculated by existing system
- **When**: AA clicks iQ button on Matrix view
- **Then**: Bot provides 8-section interpretation:
  1. Property vs Comps summary
  2. Actives analysis
  3. Pendings/Backups analysis
  4. Closed Sales analysis
  5. E-Value interpretation
  6. Market ceiling & push logic
  7. What must be verified
  8. Conclusion

- **Output Artifact**: 8-section structured text (< 500 words total)
- **Stored/Displayed**: PIQ → Comps Tab → Matrix View → Overlay panel
- **Acceptance Criteria**:
  - [ ] Does NOT output a new value
  - [ ] Does NOT restate visible data
  - [ ] Explains MEANING, not information
  - [ ] "Absolute Rule" enforced: No upside without closed evidence

---

### Comps > List (CL1)

**CL1-US-001**: Bucket Classification
- **As a**: AA viewing comp list
- **I want to**: see comps grouped into PREMIUM/HIGH/MID/LOW buckets
- **So that**: I understand market stratification

- **Given**: C-OVERLAY has ranked comps by relevance
- **When**: AA views List with iQ overlay
- **Then**: Comps display in 4 buckets with:
  - Bucket label (PREMIUM/HIGH/MID/LOW)
  - Reason array explaining classification
  - Ceiling comp flagged if applicable
  - Attribute chips auto-generated

- **Output Artifact**: Stratified comp list with bucket cards
- **Stored/Displayed**: PIQ → Comps Tab → List View
- **Acceptance Criteria**:
  - [ ] Exactly 4 buckets (some may be empty)
  - [ ] Ceiling comp explicitly identified
  - [ ] "WHY" explanation for ceiling reason
  - [ ] Chips: condition, features, delta indicators

---

### Investment Analysis (IA1)

**IA1-US-001**: Buy Box Verdict
- **As a**: AA evaluating a property
- **I want to**: see an instant verdict on whether this property fits our Buy Box
- **So that**: I can decide in < 30 seconds whether to pursue

- **Given**: Property is loaded in PIQ
- **When**: AA clicks iQ button
- **Then**: Investment Analysis panel shows:
  - Verdict: One of 5 types (Ideal Flip, Good Wholesale, Marginal Flip, Marginal Wholesale, Pass)
  - Summary: 3-5 bullet points explaining WHY
  - Fit Analysis: 6-category table (Property Type, Price, Condition, Location, Agent, Timeline)
  - Agent Question: Context-aware question to ask agent

- **Output Artifact**: Verdict card with explanation
- **Stored/Displayed**: PIQ → iQ Button → Investment Analysis Panel (appears ABOVE existing iQ Property Intelligence)
- **Acceptance Criteria**:
  - [ ] Verdict displays in < 2 seconds
  - [ ] 95%+ accuracy vs manual Buy Box review
  - [ ] Pass verdicts require < 3 seconds to dismiss
  - [ ] Agent question ready to read verbatim

---

**IA1-US-002**: Fit Analysis Table
- **As a**: AA reviewing a Marginal verdict
- **I want to**: see exactly which criteria are met/unmet
- **So that**: I understand what makes this property borderline

- **Given**: Verdict is Marginal Flip or Marginal Wholesale
- **When**: Fit Analysis table displays
- **Then**: Table shows 6 categories with:
  - Status: ✓ FITS / ⚠️ CONDITIONAL / ❌ FAILS
  - Value: Current property value
  - Buy Box: What Buy Box requires
  - Delta: Gap between value and requirement

- **Output Artifact**: 6-row table
- **Stored/Displayed**: PIQ → Investment Analysis Panel → Below verdict
- **Acceptance Criteria**:
  - [ ] All 6 categories displayed
  - [ ] CONDITIONAL items explain condition
  - [ ] Delta shown as $ or % where relevant

---

### Post-Call & Practice (PC1)

**PC1-US-001**: Post-Call Transcript Processing
- **As a**: AA who just finished a call
- **I want to**: see an AI-generated summary with next steps extracted from the transcript
- **So that**: I can capture accurate notes without manual typing

- **Given**: Call ended in Dialpad, transcript available
- **When**: Webhook triggers post-call processing
- **Then**: Summary displays with:
  - Key information extracted (availability, motivation, competition, flexibility)
  - Recommended next steps
  - Agent classification update suggestions
  - Copy-to-notes button

- **Output Artifact**: Unstructured quick-read notes (< 200 words)
- **Stored/Displayed**: PIQ → Post-Call Column
- **Acceptance Criteria**:
  - [ ] Processing completes in < 30 seconds after call end
  - [ ] Speaker separation (AA vs Agent) accurate
  - [ ] Next steps are actionable
  - [ ] One-click copy to property notes

---

**PC1-US-002**: Practice Mode
- **As a**: new AA
- **I want to**: practice calls with an AI-simulated agent
- **So that**: I can build confidence before going live

- **Given**: AA clicks "Practice" button
- **When**: Practice session starts
- **Then**: System provides:
  - AI agent voice (via ElevenLabs)
  - Realistic agent persona based on Agent365 patterns
  - Real-time feedback on script adherence
  - Post-practice score and improvement areas

- **Output Artifact**: Practice session with scoring
- **Stored/Displayed**: PIQ → Practice Mode (separate view)
- **Acceptance Criteria**:
  - [ ] Voice latency < 1 second
  - [ ] Agent responses contextually appropriate
  - [ ] Score based on script adherence + objection handling
  - [ ] Manager can view practice summaries

---

### Notes & Communication (NC1)

**NC1-US-001**: Unified Agent Timeline
- **As a**: AA about to call an agent
- **I want to**: see ALL notes for this agent across ALL properties and ALL AAs
- **So that**: I have complete context before the call

- **Given**: AA opens a property or agent profile
- **When**: NC1 aggregates history
- **Then**: Timeline displays:
  - All notes sorted by timestamp (newest first)
  - Source property linked for each note
  - Channel indicator (📞📧💬📝)
  - PC1-generated notes marked
  - Other AAs' notes included

- **Output Artifact**: Chronological timeline with linked notes
- **Stored/Displayed**: PIQ → Property Panel → Notes Tab OR Agent Profile → Agent Notes
- **Acceptance Criteria**:
  - [ ] Timeline loads in < 2 seconds
  - [ ] Notes from all AAs visible
  - [ ] "View Property →" link works
  - [ ] PC1 output integrated

---

**NC1-US-002**: Agent Pattern Detection
- **As a**: AA calling an agent I've never spoken to
- **I want to**: see detected patterns (communication preference, behavior, offer format)
- **So that**: I can tailor my approach

- **Given**: Agent has 3+ historical interactions
- **When**: NC1 analyzes patterns
- **Then**: Pattern summary shows:
  - Preferred channel with response rate
  - Best time to reach
  - Double-end stance (with evidence count)
  - Offer format preference
  - Previous AA interactions

- **Output Artifact**: Pattern card with evidence citations
- **Stored/Displayed**: PIQ → Notes Tab → Pattern section
- **Acceptance Criteria**:
  - [ ] Minimum 3 data points required
  - [ ] Evidence count shown ("stated 2x")
  - [ ] Dynamic filtering hides already-confirmed info

---

### My Stats (MS1) — **MISSING: NEEDS DEFINITION**

> ⚠️ **CRITICAL**: These stories are PLACEHOLDERS. My Stats PRD does not exist.

**MS1-US-001**: Personal Performance Dashboard
- **As a**: AA tracking my performance
- **I want to**: see my key metrics (calls, offers, closes, conversion rates)
- **So that**: I know how I'm performing vs goals

- **Given**: AA has activity history
- **When**: AA opens My Stats
- **Then**: Dashboard shows:
  - **UNKNOWN** — Metrics not defined
  - **UNKNOWN** — Time periods not defined
  - **UNKNOWN** — Comparisons not defined

- **Output Artifact**: UNKNOWN
- **Stored/Displayed**: UNKNOWN — PIQ tab? Dedicated page?
- **Acceptance Criteria**: UNKNOWN

**ACTION REQUIRED**: Create My Stats PRD defining:
- [ ] Which metrics are tracked
- [ ] Which time periods (daily/weekly/monthly)
- [ ] Which comparisons (vs team, vs goals, vs historical)
- [ ] UI location and layout
- [ ] Data sources

---

### AM Deal Support (AMD1)

**AMD1-US-001**: Deal Propensity Score
- **As a**: Acquisition Manager reviewing pipeline
- **I want to**: see which properties have highest closing probability
- **So that**: I focus coaching time on high-impact deals

- **Given**: AM opens My Deals and clicks iQ button
- **When**: AMD1 calculates propensity
- **Then**: Side panel shows:
  - Properties sorted by priority
  - Propensity score (HIGH/MID/LOW) with explanation
  - Data sources: NC1 sentiment (40%), PC1 quality (25%), Status (25%), Recency (10%)

- **Output Artifact**: Prioritized deal review list
- **Stored/Displayed**: FlipIQ → My Deals Tab → iQ Side Panel
- **Acceptance Criteria**:
  - [ ] iQ button visible only to AM role
  - [ ] Propensity explanation cites specific signals
  - [ ] Panel loads in < 2 seconds

---

**AMD1-US-002**: Unanswered Response Alert
- **As a**: AM
- **I want to**: know when an AA hasn't responded to a positive agent message
- **So that**: I ensure no warm lead goes cold

- **Given**: Agent sent positive response 24-48 hours ago
- **When**: AA has not followed up
- **Then**: Alert displays:
  - "⚠️ Unanswered response (2 days)"
  - Property and AA identified
  - Link to contact AA

- **Output Artifact**: Alert badge + detail card
- **Stored/Displayed**: My Deals row + AMD1 side panel
- **Acceptance Criteria**:
  - [ ] Flag triggers at 24-48 hours
  - [ ] Only POSITIVE responses trigger alert
  - [ ] Clears when AA responds

---

# 5. REQUIRED CHANGES TO REACH "IMPLEMENTABLE V2"

## 5.1 Doc Edits Required

| # | File | Edit Required |
|---|------|---------------|
| 1 | **NEW** | Create `my-stats-prd.md` — Define metrics, time periods, UI, data sources |
| 2 | **NEW** | Create `my-stats-user-stories.md` — 5-10 user stories |
| 3 | **NEW** | Create `my-stats-training-samples.md` — 5+ training examples |
| 4 | `investment-analysis-prd.md` | Remove "Week 1" terminology, use hours only |
| 5 | `comp-list-prd.md` | Standardize Bot ID to CL1 (currently C3) |
| 6 | `comp-map-prd.md` | Extract embedded user stories to separate file OR add note that embedding is intentional |
| 7 | `post-call-practice-prd.md` | Add Go/No-Go checkpoint for Dialpad API verification |
| 8 | `notes-communication-prd.md` | Define Gmail/Outlook OAuth flow for Phase 2 |
| 9 | `am-deal-support-prd.md` | Define Google Sheets API credential setup |
| 10 | **NEW** | Create `bot-naming-convention.md` — Standardize Bot IDs |

## 5.2 Repo Structure Changes

```
docs/
├── bot-naming-convention.md (NEW)
├── my-stats-prd.md (NEW - P0)
├── my-stats-user-stories.md (NEW - P0)
├── my-stats-training-samples.md (NEW - P0)
├── comp-map-user-stories.md (NEW - P2, or note embedding)
└── [existing PRDs - updates noted above]
```

## 5.3 Next 10 Engineering Tasks (Ordered)

| # | Task | Blocker? | Owner | Hours Est |
|---|------|----------|-------|-----------|
| 1 | **VERIFY: Dialpad API transcript availability** | YES | Nate | 2 |
| 2 | **CREATE: My Stats PRD** | YES | Tony | 4 |
| 3 | **CREATE: My Stats User Stories + Training Samples** | YES | Tony | 4 |
| 4 | Standardize Bot IDs across all PRDs | No | Any | 2 |
| 5 | Define build order: PC1 → NC1 → AMD1 (dependency chain) | No | Nate | 1 |
| 6 | Implement Investment Analysis (IA1) — P0 Critical Path | No | Dev | 14 |
| 7 | Implement Comp Map overlay (CM1) | No | Dev | 7 |
| 8 | Implement Comp Matrix overlay (CMX1) | No | Dev | 6 |
| 9 | Implement Comp List overlay (CL1) | No | Dev | 8 |
| 10 | Implement Post-Call Bot (PC1) — depends on Task #1 | Yes (#1) | Dev | 28 |

---

# 6. READINESS VERDICT

## Overall Readiness: **MEDIUM**

### Summary

| Metric | Value |
|--------|-------|
| V2 Modules Defined | 7 of 8 (87.5%) |
| V2 Modules Missing | 1 (My Stats) |
| P0 Blockers | 3 |
| P1 Issues | 5 |
| P2 Issues | 2 |

### P0 Blockers (Must Fix Before Coding)

| # | Blocker | Impact | Resolution |
|---|---------|--------|------------|
| 1 | **My Stats PRD Missing** | Cannot implement 12.5% of v2 scope | Create PRD immediately |
| 2 | **Dialpad API Unverified** | PC1 may need +4 hours if API doesn't support speaker separation | Verify Day 0 |
| 3 | **Timeline Terminology Inconsistent** | Engineering confusion | Standardize to hours |

### Go/No-Go Recommendation

**CONDITIONAL GO** — Proceed with implementation of 7 defined modules while:
1. Tony creates My Stats PRD (parallel track)
2. Nate verifies Dialpad API (Day 0 blocker)
3. Bot ID standardization completed (Day 1)

Once My Stats PRD exists and Dialpad is verified, full v2 scope is implementable.

---

**Report Generated**: January 3, 2025
**Repo Commit**: `claude/bot-coordination-docs-Nx6f4`
**Next Review**: After My Stats PRD creation
