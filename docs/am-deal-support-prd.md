# AM DEAL SUPPORT BOT

## Product Requirements Document
### Version 1.1 — January 2, 2025

---

| Field | Value |
|-------|-------|
| **Bot ID** | MGT2 (AM Deal Support) |
| **Category** | Deal Intelligence / Pipeline Management |
| **Priority** | P1 — Core Feature |
| **Primary User** | Acquisition Manager (AM) |
| **Secondary User** | AA (Phase 2 - personal pipeline view) |
| **UI Location** | FlipIQ → My Deals Tab → iQ Button |
| **Trigger Method** | iQ Button click (AM role-gated) |
| **Integration Points** | D3, D2, D1, Dialpad, Google Sheets |
| **Handoff To** | Eric (PM) / Nate (CTO) / Faizal (UI) |

---

## CRITICAL: Bot Relationship Clarification

> ⚠️ **MGT2 is an AGGREGATION bot that CONSUMES data from other iQ bots:**
>
> | Source Bot | Data Consumed | MGT2 Use |
> |------------|---------------|----------|
> | **D3** (Notes & Communication) | Agent sentiment, engagement tags, patterns | Propensity score, response detection |
> | **D2** (Post-Call Bot) | Call quality scores, conversation progression | Propensity calculation |
> | **D1** (Investment Analysis) | ARV, Rehab, Comps, Wholesale | Checklist verification |
>
> MGT2 does NOT duplicate these bots' work — it aggregates their outputs for AM-level decision support.

---

## 1. EXECUTIVE SUMMARY

### 1.1 Problem Statement

Acquisition Managers currently lack visibility into which deals in their team's pipeline have the highest probability of closing. They spend time reviewing all properties equally instead of focusing on deals where agent engagement signals strong opportunity. When AAs receive positive responses from agents, there's no system to ensure timely follow-up—leading to missed opportunities and relationship decay.

Additionally, AMs cannot quickly identify which AAs excel at specific deal types (agent relationships, off-market, wholesale), preventing optimal task allocation and coaching.

### 1.2 Solution

An AI-powered Deal Support overlay that analyzes Notes & Communication, Post-Call data, and Pipeline status to surface the specific properties where AM attention will have the highest impact on closing deals.

### 1.3 Core Capabilities

| Capability | Description |
|------------|-------------|
| Deal Propensity Score | HIGH/MID/LOW closing probability with explanation |
| Unanswered Response Alerts | Surface positive agent responses awaiting AA follow-up |
| Property Verification Checklist | Notes → Comps → Rehab → ARV → Wholesale |
| Pipeline Health Tracking | By source, status, and AA expertise |
| AM Task Completion | Review tracking through notes integration |
| Pipeline Reports & Forecast | Monthly projections with Google Sheets export |

### 1.4 Philosophy: Human-in-the-Loop

This bot does **NOT** calculate what's wrong with a deal. It surfaces properties that need human verification and provides a checklist for the experienced AM to "back into it" with the AA.

**The goal is to tell the AA:**
- "You don't have a deal" OR
- "You have a deal, adjust these numbers"

No technology can replace 31 years of experience needed to fine-tune investment analysis—**this bot directs attention, humans make decisions.**

### 1.5 Key Distinction: Propensity vs Verdict

| Concept | Bot | Question Answered |
|---------|-----|-------------------|
| **Verdict** | IAMaster | "Is this property a good investment?" |
| **Propensity** | AMD1 | "Is this deal likely to close?" |

A property can have an "Ideal Flip" verdict (great investment) but LOW propensity (agent not engaging). AMD1 helps AMs focus on deals that will actually close.

### 1.6 Success Metric

Increase team deal close rate to **2 deals per AA per month** by focusing AM coaching time on high-propensity properties—measured by ratio of reviewed properties to closed deals.

---

## 2. USER STORIES

### 2.1 AM Deal Focus

**AS AN** Acquisition Manager reviewing my team's pipeline
**I WANT TO** see which properties have the highest closing probability
**SO THAT** I invest my coaching time where it will close deals.

### 2.2 AM Unanswered Response Alert

**AS AN** Acquisition Manager
**I WANT TO** know when an AA hasn't responded to a positive agent message
**SO THAT** I can ensure no warm lead goes cold due to delayed follow-up.

### 2.3 AM Investment Analysis Review

**AS AN** Acquisition Manager coaching an AA
**I WANT TO** have a checklist of what to verify on each property
**SO THAT** I can help them back into the correct numbers.

### 2.4 AM Pipeline Intelligence

**AS AN** Acquisition Manager
**I WANT TO** understand deal sources and AA strengths
**SO THAT** I can optimize assignments and identify training needs.

### 2.5 AM Task Completion

**AS AN** Acquisition Manager completing a review
**I WANT TO** mark my review complete with comments
**SO THAT** the system knows what was addressed and why.

### 2.6 AM Pipeline Report & Forecast

**AS AN** Acquisition Manager
**I WANT TO** see a forecast of expected closes based on current pipeline
**SO THAT** I can identify gaps and take action before month-end.

### 2.7 AM Historical Tracking

**AS AN** Acquisition Manager
**I WANT TO** export pipeline data daily to Google Sheets
**SO THAT** I can track trends and mine data over time.

---

## 3. FEATURE SPECIFICATIONS

### 3.1 Feature 1: iQ Button + Side Panel

**Trigger:** AM opens My Active Deals tab, applies filters, clicks iQ Button (AM-only feature)

**Process Flow:**
1. AM navigates to My Active Deals tab
2. AM applies filters (All Team, Individual AA, Status, Source, etc.)
3. AM clicks iQ Button (visible based on AM role)
4. Side panel opens with Deal Review list based on applied filters
5. Properties displayed in priority order with Propensity Score

**Side Panel Card Display:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📍 1587 Scioto, Banning, CA 92220
   $499,900 | MLS | Josh S.
   Status: 60% In Negotiations

   Propensity: HIGH 🟢
   Why: Agent engaged, presenting Friday,
        good conversation flow, guiding on terms

   ✅ Agent responding & guiding
   📋 Check: Notes | Comps | ARV | Rehab

   [Jump to Property] [Copy to Notes]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### 3.2 Feature 2: Deal Acquisition Propensity Score

**Data Sources:**

| Source | Data Retrieved | Weight |
|--------|----------------|--------|
| NC1 (Notes & Communication) | Agent sentiment, engagement level, guidance signals | 40% |
| PC1 (Post-Call Bot) | Call quality scores, conversation progression | 25% |
| Pipeline Status | Current stage (higher % = closer to close) | 25% |
| Contact Recency | Days since last meaningful contact | 10% |

**Score Definitions:**

| Score | Signals | Example Explanation |
|-------|---------|---------------------|
| **HIGH** 🟢 | Agent engaged + responding + guiding + recent contact | "Agent presenting Friday, guiding on terms, conversation progressing well" |
| **MID** 🟡 | Some engagement but stalled, or good property but conversation cold | "Agent responded positively but no contact in 3 days, needs follow-up" |
| **LOW** 🔴 | No response despite multiple contacts, or agent explicitly not engaging | "5 calls made, no response, agent not returning voicemails" |

> **Note:** Propensity Score can be posted to property Notes and used as email template.

---

### 3.3 Feature 3: Deal Review List Auto-Population

**Triggers for Deal Review:**

A property is automatically added to the Deal Review list when:

1. **Positive Response Received:** Agent sends positive email/text (tagged by NC1)
2. **AA Hasn't Responded:** Positive response exists but AA has not followed up within 24-48 hours
3. **Status Requires Attention:** Property at 60%+ In Negotiations with no activity in 2+ days
4. **Initial Contact Stalled:** Property at 10% Initial Contact Started but no agent conversation yet
5. **Tagged by System:** NC1 flags property as "needs attention"

**Priority Sorting Logic:**

| Priority | Factor | Rationale |
|----------|--------|-----------|
| 1 | Offer Status % | Higher = closer to money |
| 2 | Agent Engagement | Willing & open conversations rise |
| 3 | Unanswered Positive | Urgent — warm lead going cold |
| 4 | Propensity Score | HIGH > MID > LOW |

> **Note:** Lots of calls with no response = LOWER priority (agent not engaging)

---

### 3.4 Feature 4: Text/Email Response Indicator

**UI Enhancement:**

Add Text/Email indicator to property row in My Deals:

```
☀️ Medium | To do: Not set | • 0 Critical • 0 Reminders • 1 Text/Email
```

**Data Source:** Dialpad integration (already built) provides:
- Incoming text messages from agents
- Incoming emails from agents
- Timestamp of last agent communication

**Positive Response Detection:**

NC1 tags responses as "Positive" based on engagement signals. **Only POSITIVE responses** trigger the Text/Email indicator and Deal Review addition.

---

### 3.5 Feature 5: Property Verification Checklist

**Standard Checklist:**

| Item | Source | AM Verification |
|------|--------|-----------------|
| Notes | NC1 | Review conversation history and agent feedback |
| Comps | IAMaster | Verify comparable properties are accurate |
| Rehab | IAMaster | Check construction cost estimate (too high/too low?) |
| ARV | IAMaster | Validate After Repair Value (realistic for market?) |
| Wholesale | IAMaster | If wholesale deal, is the price reasonable? |

**Status-Based Checklist Variations:**

| Offer Status | Checklist Focus |
|--------------|-----------------|
| 10% Initial Contact | ✓ Has agent been called? ✓ Is property qualified? |
| 30-50% Offer Sent | ✓ Notes ✓ Comps ✓ ARV ✓ Rehab ✓ Wholesale numbers |
| 60% In Negotiations | ✓ Notes ✓ Counter terms ✓ Final numbers ✓ Agent guidance |
| 80% Under Contract | ✓ Contract terms correct ✓ Escrow status ✓ Contingencies |

---

### 3.6 Feature 6: AM Task Completion Tracking

**Workflow:**

1. AM reviews property in side panel
2. AM clicks [Copy to Notes] button
3. System copies propensity summary and checklist to property Notes
4. AM adds comments explaining what was addressed and outcome
5. System marks review complete based on Note entry

**Example Note Entry:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[AM REVIEW — 01/02/2025]
Propensity: HIGH
Agent engaged, presenting Friday

Verified: ✓Notes ✓Comps ✓ARV ✓Rehab

AM Comments: ARV was $15K high. Adjusted
with Josh. Rehab estimate solid. Good deal,
push for close.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### 3.7 Feature 7: AA Expertise Tracking

**Data Source:** D2 (Agent Bot) + Historical close data

System identifies AA strengths based on historical close rates by source:

| AA | Strength | Evidence | Recommendation |
|----|----------|----------|----------------|
| Josh S. | Agent Relationships | 8/10 closes from MLS | Assign MLS leads |
| Kevin R. | Off-Market Deals | 6/8 closes from cold | Assign off-market |
| Maria T. | Text Campaigns | 5/6 closes from SMS | Lead text campaigns |

---

### 3.8 Feature 8: Pipeline Report & Forecast

**Trigger:** AM clicks [Run Pipeline Report] OR scheduled daily at 6 PM

**Report Sections:**

**Section 1: Team Pipeline Summary**
```
📊 PIPELINE REPORT — January 2, 2025

TEAM: 4 Acquisition Associates
ACTIVE PROPERTIES: 127 total

BY STATUS:
│ 10% Initial Contact │  42  │  33%  │
│ 20-30% Working      │  35  │  28%  │
│ 50% Contract Sent   │  28  │  22%  │
│ 60% In Negotiations │  15  │  12%  │
│ 80% Under Contract  │   7  │   5%  │

BY PROPENSITY:
│ HIGH 🟢 │  18  │  14%  │
│ MID 🟡  │  45  │  35%  │
│ LOW 🔴  │  64  │  51%  │
```

**Section 2: Monthly Forecast**
```
🎯 EXPECTED CLOSES THIS MONTH: 6-8 deals

• 7 Under Contract (80%) → Expected: 6 closes (85% conv)
• 15 In Negotiations (60%) → Expected: 2 closes (15% conv)
• 18 HIGH Propensity → Expected: 7 closes (40% conv)

GOAL: 8 deals (2 per AA)
GAP: Need 0-2 more closes
```

**Section 3: Source Analysis & Recommendations**
```
📈 SOURCE ANALYSIS

│ MLS          │  78  │  61%  │  3.2%  │
│ Off-Market   │  22  │  17%  │  8.5%  │
│ Wholesaler   │  19  │  15%  │  5.1%  │
│ Seller Direct│   8  │   7%  │ 12.0%  │

🔴 RECOMMENDATIONS:
1. NEED MORE OFF-MARKET DEALS
   Current: 22 (17%) | Target: 35 (25%)
   Assign to: Kevin R. (excels at off-market)

2. SELLER DIRECT OPPORTUNITY
   Current: 8 (7%) | Conversion: 12%
   Action: Highest conversion rate - expand
```

**Section 4: Action Items**
```
✅ TODAY'S ACTION ITEMS

URGENT:
□ 3 properties with unanswered positive responses
□ 2 properties at 60%+ with no contact in 48+ hours
□ Sarah L. coaching session (low HIGH propensity)
```

---

### 3.9 Feature 9: Google Sheets Export (Daily Append)

**Trigger:**
- **Manual:** AM clicks [Export to Sheets] button
- **Automatic:** Daily at 6 PM (after Pipeline Report generation)

**Export Behavior:**
- Exports to **same Google Sheet** every day
- **Appends new row** with date stamp (does not overwrite)
- Creates historical record for trend analysis

**Data Exported Per Row:**

| Column | Data |
|--------|------|
| Date | 2025-01-02 |
| Total Active | 127 |
| HIGH/MID/LOW Count | 18/45/64 |
| Status Breakdown | 42/35/28/15/7 |
| Source Breakdown | 78/22/19/8 |
| Forecast Closes | 6-8 |
| MTD Closes | 2 |
| Per-AA Active/HIGH | Josh: 38/6, Kevin: 32/5, etc. |

**Historical Mining Use Cases:**
- Trend Analysis: "Is our HIGH propensity count growing?"
- Conversion Tracking: "What % of HIGH properties actually closed?"
- Source Optimization: "Which source is producing the best ROI?"
- AA Development: "Is Sarah's HIGH count improving with coaching?"

---

## 4. OFFER STATUS DEFINITIONS

| % | Status | Definition & Required Action |
|---|--------|------------------------------|
| 0% | None | File needs attention. Update status and set priority. |
| 10% | Initial Contact Started | Property assigned. **MUST CALL** agent. |
| 20% | Continue to Follow | Not ready to accept price, may sell later. |
| 30% | Back Up / Offer Terms Sent | Pending with other buyer OR terms sent. |
| 50% | Contract Submitted | RPA sent to listing agent. **CALL** to confirm. |
| 60% | In Negotiations | Agent engaging. **MUST CALL** daily. |
| 80% | Offer Accepted | In Escrow. Verify contract terms. |
| 100% | Acquired | Closed Escrow. Update Agent 365 Report. |

**Source Definitions:**
- **MLS:** Multiple Listing Service deals
- **Off-Market:** Manually added by AA
- **Wholesaler:** Emailed to Deals@
- **Seller Direct:** Direct from property owner

---

## 5. DATA ARCHITECTURE

### 5.1 Data Sources

| Source | Data Provided | Used For |
|--------|---------------|----------|
| NC1 (Notes & Communication) | Agent sentiment, engagement tags, conversation quality | Propensity score, positive response detection |
| PC1 (Post-Call Bot) | Call quality scores, conversation progression | Propensity score calculation |
| D2 (Agent Bot) | Agent profile, ISC, transaction history | AA expertise tracking |
| IAMaster (Investment Analysis) | ARV, Rehab, Comps, Wholesale | Checklist verification items |
| Dialpad Integration | Text/email responses, timestamps | Text/Email indicator, unanswered alerts |
| Pipeline Database | Offer status, source, AA assignment | Priority sorting, pipeline metrics |
| Historical Closes | Past deal data by AA, source, status | Conversion rates, forecasting, AA expertise |

### 5.2 Integration Diagram

```
┌─────────────────────────────────────────────────────────┐
│                       AMD1                               │
│              AM Deal Support Bot                         │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐    │
│  │   NC1   │  │   PC1   │  │IAMaster │  │   D2    │    │
│  │ Notes & │  │Post-Call│  │Invest.  │  │ Agent   │    │
│  │ Comm    │  │         │  │Analysis │  │  Bot    │    │
│  └────┬────┘  └────┬────┘  └────┬────┘  └────┬────┘    │
│       │            │            │            │          │
│       ▼            ▼            ▼            ▼          │
│  ┌──────────────────────────────────────────────────┐  │
│  │           Propensity Score Engine                 │  │
│  │  - Sentiment (NC1) → 40%                         │  │
│  │  - Call Quality (PC1) → 25%                      │  │
│  │  - Pipeline Status → 25%                         │  │
│  │  - Recency → 10%                                 │  │
│  └──────────────────────────────────────────────────┘  │
│                         │                               │
│                         ▼                               │
│  ┌──────────────────────────────────────────────────┐  │
│  │           AM Dashboard / Side Panel               │  │
│  │  - Deal Review List                               │  │
│  │  - Pipeline Reports                               │  │
│  │  - Google Sheets Export                           │  │
│  └──────────────────────────────────────────────────┘  │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

---

## 6. SUCCESS METRICS

| Metric | Target | Measurement |
|--------|--------|-------------|
| Deal Close Rate | 2 deals/AA/month | Monthly acquisition count per AA |
| Response Time | <24 hours to positive responses | Avg time from agent response to AA follow-up |
| HIGH Propensity Conversion | >40% close rate | HIGH-scored properties that close |
| AM Review Completion | 100% of HIGH properties reviewed | % of HIGH propensity with AM notes |
| Forecast Accuracy | Within 20% of actual | Predicted vs actual monthly closes |
| Source Balance | 25% off-market, 20% wholesale | Pipeline distribution by source |

---

## 7. DEVELOPMENT PLAN

> **Note:** Timelines reflect BMAD Method + Claude Code (AI-assisted development)

### Phase 0: API Verification — 2 hrs (BLOCKER)

| Task | Hours |
|------|-------|
| 0.1 NC1 data availability confirmation | 0.5 |
| 0.2 PC1 data availability confirmation | 0.5 |
| 0.3 Pipeline database schema review | 0.5 |
| 0.4 Google Sheets API credentials setup | 0.5 |

---

### Epic 1: Core UI + Side Panel — 6 hrs

| Task | Hours |
|------|-------|
| 1.1 iQ Button with AM role-based visibility | 1 |
| 1.2 Side panel shell with sliding animation | 1.5 |
| 1.3 Deal Review list with filter integration | 1.5 |
| 1.4 Property card component | 1.5 |
| 1.5 Jump to Property navigation | 0.5 |

---

### Epic 2: Propensity Score Engine — 6 hrs

| Task | Hours |
|------|-------|
| 2.1 NC1 sentiment data integration | 1 |
| 2.2 PC1 call quality integration | 1 |
| 2.3 Pipeline status weighting | 1 |
| 2.4 Propensity calculation algorithm | 1.5 |
| 2.5 LLM explanation generation | 1 |
| 2.6 Score display in property cards | 0.5 |

---

### Epic 3: Response Detection & Alerts — 4 hrs

| Task | Hours |
|------|-------|
| 3.1 Text/Email indicator badge | 1 |
| 3.2 Positive response tagging (via NC1) | 1 |
| 3.3 Unanswered response flagging | 1 |
| 3.4 Alert display in Deal Review | 1 |

---

### Epic 4: Checklist & AM Actions — 4 hrs

| Task | Hours |
|------|-------|
| 4.1 Checklist component (status-based) | 1.5 |
| 4.2 Copy to Notes functionality | 1 |
| 4.3 AM review completion tracking | 1 |
| 4.4 Note entry formatting | 0.5 |

---

### Epic 5: Pipeline Reports & Forecast — 6 hrs

| Task | Hours |
|------|-------|
| 5.1 Pipeline summary aggregation | 1 |
| 5.2 Forecast calculation engine | 1.5 |
| 5.3 Source analysis logic | 1 |
| 5.4 Recommendations generation | 1 |
| 5.5 Report display UI | 1 |
| 5.6 AA expertise tracking | 0.5 |

---

### Epic 6: Google Sheets Export — 4 hrs

| Task | Hours |
|------|-------|
| 6.1 Google Sheets API integration | 1.5 |
| 6.2 Daily append functionality | 1 |
| 6.3 Manual export button | 0.5 |
| 6.4 Error handling & retry logic | 1 |

---

### Timeline Summary

| Phase | Hours | Deliverable |
|-------|-------|-------------|
| Phase 0: API Verification | 2 hrs | Go/no-go decision |
| Epic 1: Core UI + Side Panel | 6 hrs | Deal Review live |
| Epic 2: Propensity Engine | 6 hrs | Scores calculated |
| Epic 3: Response Detection | 4 hrs | Alerts working |
| Epic 4: Checklist & Actions | 4 hrs | AM workflow complete |
| Epic 5: Pipeline Reports | 6 hrs | Reports & forecast |
| Epic 6: Google Sheets | 4 hrs | Export working |
| **TOTAL** | **32 hrs (~4 days)** | **Bot fully deployed** |

> **Note:** Original PRD stated 30 hours / 4 days. Adjusted to 32 hours with detailed epic breakdown. Epic 2 (Propensity Engine) is the critical path.

---

## 8. RISKS AND MITIGATIONS

| Risk | Impact | Mitigation |
|------|--------|------------|
| NC1/PC1 data latency | Stale propensity scores | Show "last updated" timestamp, implement caching |
| Propensity score inaccurate | AM distrust | Show explanation, human-in-loop validation, feedback mechanism |
| Over-reliance on bot | Missed deals | Position as supplement to AM judgment, not replacement |
| AA sees as surveillance | Resistance | Frame as coaching tool, focus on deal support |
| Google Sheets API limits | Export failures | Implement retry logic, batch writes, error notifications |
| Forecast inaccuracy early on | Wrong expectations | Label as "estimate" until model improves |

---

## 9. ACCEPTANCE CRITERIA

- [ ] AC1: iQ Button visible only to AM role
- [ ] AC2: Side panel displays filtered properties
- [ ] AC3: Propensity score calculated from NC1 + PC1 + Status
- [ ] AC4: Score explanation generated by LLM
- [ ] AC5: Text/Email indicator shows on property rows
- [ ] AC6: Unanswered positive responses flagged
- [ ] AC7: Checklist varies by offer status
- [ ] AC8: Copy to Notes creates formatted entry
- [ ] AC9: Pipeline Report generates in < 5 seconds
- [ ] AC10: Forecast shows expected closes
- [ ] AC11: Google Sheets export appends daily
- [ ] AC12: AA expertise tracked by source

---

## 10. FUTURE ENHANCEMENTS

### 10.1 AA Version (Phase 2)

Build AA-facing version scoped to individual AA's pipeline only. AAs see their own propensity scores, checklists, and unanswered response alerts.

### 10.2 Predictive Forecasting v2

Machine learning model trained on historical Google Sheets data to improve forecast accuracy over time.

### 10.3 Automated Task Assignment

Based on AA expertise tracking, automatically suggest or assign new leads to the AA most likely to close that deal type.

### 10.4 Slack/Email Alerts

Push notifications when:
- HIGH propensity property goes 48+ hours without contact
- AA hasn't responded to positive agent message
- Weekly forecast shows gap to goal

---

## HANDOFF SUMMARY

### For Eric (PM)

| Item | Detail |
|------|--------|
| Problem | AMs lack visibility into high-probability deals |
| Solution | Propensity scoring + Pipeline reports |
| Key Feature | Deal Review list sorted by closing probability |
| Integration | Aggregates NC1, PC1, IAMaster data |
| **Timeline** | **32 hrs (~4 days) with BMAD + Claude Code** |

### For Nate (CTO)

| Item | Detail |
|------|--------|
| Architecture | Aggregation bot consuming NC1/PC1/IAMaster |
| APIs | NC1, PC1, IAMaster, Dialpad, Google Sheets |
| Critical Path | Epic 2 (Propensity Engine) — 6 hrs |
| New Integration | Google Sheets daily append |
| **Timeline** | **32 hrs (~4 days) with BMAD + Claude Code** |

---

**Document prepared for FlipIQ Engineering**
**Version 1.1 — January 2, 2025**
