# FlipIQ AI Overlay System
## Master Technical Specification v2.0
### BMAD Handoff Document — Source of Truth

**Updated**: January 2, 2026
**Version**: 2.0 + Bot Spec 3.0 (Combined)

> *Together, We Flip Smarter*

---

# EXECUTIVE SUMMARY

FlipIQ is an AI-powered overlay system that transforms real estate acquisition teams into consistent deal-closing machines. The system deploys specialized AI bots on top of existing Command platform infrastructure, requiring zero database changes while delivering immediate productivity gains.

## Core Mission

**Reliably turn acquisition talent into consistent deal producers.** Enable average, non-experienced Acquisition Associates to close two transactions per month, predictably and repeatably, using the FlipIQ system.

With PropertyRadar integration now live, FlipIQ provides real-time distress signals (foreclosure, tax delinquency, bankruptcy) that identify motivated sellers before competitors. The Deal Focus Index (DFI) automatically scores every property, ensuring AAs focus on highest-probability opportunities instead of random prospecting.

## Current Results

| Metric | Before | After |
|--------|--------|-------|
| Morning setup | 60 minutes | 30 seconds |
| Deal analysis | 15 minutes | 2 minutes |
| Daily offers | 1-2 | 5 consistently |
| Management oversight | 3 hours/day | 15 minutes/day |

---

# DEVELOPMENT PHASE SUMMARY

| Phase | Bot Count | Status | Target |
|-------|-----------|--------|--------|
| **Phase 1** | 2 Workflows | ✅ **COMPLETE** - Production | — |
| **Phase 2 (v2)** | 8 Bots | 🔨 **BUILD** - Current Focus | Now |
| **Phase 3 (v3)** | 4 Master Bots | 📋 **FUTURE** | End Feb 2026 |

---

# MISSION & VISION

## FlipIQ Mission

FlipIQ's mission is to reliably turn acquisition talent into consistent deal producers. Everything we build, deploy, and support is designed to take an average, non-experienced Acquisition Associate and enable them to close two transactions per month, predictably and repeatably, using the FlipIQ system.

**When acquisition associates win, operators win. When operators win, FlipIQ scales.**

## FlipIQ Vision

FlipIQ's vision is to become the operating system that powers the most productive real estate acquisition teams in the country. We believe real estate does not scale through leads, software features, or dashboards alone. It scales when people are given:

- Clear process
- Disciplined execution
- Technology that removes friction instead of adding it

## Core Operating Principle (Non-Negotiable)

Everything we do is designed around one outcome: **getting each Acquisition Associate to two deals per month.**

This principle governs:
- How we select operators
- How we train and onboard teams
- How the technology is built
- How the roadmap is prioritized
- How success is measured
- How leadership is held accountable

### One-Line Internal North Star

> "Does this help an Acquisition Associate close two deals per month?"
>
> **If yes** → build it, support it, prioritize it.
> **If no** → it's noise.

---

# FLIPIQ USER MASTER (FUM) — Central Integration Layer

The FlipiQ User Master provides unified context to ALL bots. Every bot reads from and writes to FUM for consistent user state, permissions, and workflow management.

## FUM Context Object (Available to All Bots)

```json
{
  "user_id": "string",
  "role": "AA | AM | Principal | COO | SuperAdmin",
  "company_id": "string",
  "team_id": "string",
  "am_id": "string",
  "current_property_id": "string",
  "current_agent_id": "string",
  "session_state": {},
  "permissions": [],
  "daily_metrics": {},
  "preferences": {}
}
```

---

# PHASE 1: COMPLETE ✅

The iQ system consists of 2 integrated workflows that are built and running in production. These handle the core AA daily process.

| Bot ID | Name | Primary Function |
|--------|------|------------------|
| **iQ-1** | Morning Check-In | Daily kickoff, availability, blockers → routes to AM |
| **iQ-2** | Deal Outreach/PIQ/Agent | Property intel, agent scripts, daily outreach, 30 contacts |

---

## iQ-1: Morning Check-In Bot

| Field | Value |
|-------|-------|
| **Bot ID** | iQ-1 |
| **Bot Category** | Daily Ops |
| **Bot Priority** | P0 |
| **Primary User** | Acquisition Associate (AA) |
| **Primary UI Location** | Dashboard → Action Plan |
| **UI Element** | Morning Check-In Modal (iQ Overlay) |
| **Visual Position** | iQ Overlay slides from right |
| **Trigger Method** | Auto (first login of day) |
| **Success Metrics** | 100% check-in compliance |
| **Time Saved** | 59.5 minutes (60 min → 30 sec) |

### FUM Integration

| Operation | Fields |
|-----------|--------|
| **READS** | user_id, role, am_id, preferences |
| **WRITES** | session_state.checked_in, daily_metrics.check_in_time |

### Primary Purpose

Kick off the day with energy, readiness, and tactical focus. Confirm AA's full-day availability and identify any blockers before activating the Deal Outreach workflow.

### Processing Logic

1. Record: `check_in_time`, `available_today`, `help_requested`, `blockers`
2. If help or blockers present → route AM task (uses `FUM.am_id`)
3. Store session flag: `FUM.session_state.checked_in = true`
4. Push control flow to iQ-2 (Deal Outreach)

### Data Inputs

| Source | Fields |
|--------|--------|
| User Provided | Availability confirmation, help needed, blockers |
| System Retrieved | Login time, FUM.user_id |

---

## iQ-2: Deal Outreach/PIQ/Agent Bot

| Field | Value |
|-------|-------|
| **Bot ID** | iQ-2 |
| **Bot Category** | Daily Ops / Property Intelligence |
| **Bot Priority** | P0 |
| **Primary User** | Acquisition Associate (AA) |
| **Primary UI Location** | Dashboard → Action Plan → Deal Review → Daily Outreach |
| **Trigger Method** | Auto (after check-in) - requires `FUM.session_state.checked_in = true` |
| **Success Metrics** | 100% deal prioritization, 30 conversations/day, 5 offers/day |

### FUM Integration

| Operation | Fields |
|-----------|--------|
| **READS** | user_id, current_property_id, preferences.markets |
| **WRITES** | session_state.workflow_stage, daily_metrics.properties_reviewed, current_agent_id |

### Primary Purpose

Navigate AA through Deal Review (process properties through PIQ → Comps → Investment Analysis → Agent → Offer), then Daily Outreach (30 agent conversations, Priority Agents, Campaigns).

### Core Workflow Stages

1. **Action Plan**: Present prioritized work (Critical → Hot → Warm → Cold → New)
2. **Deal Review**: Process each property through 5 tabs (sets `FUM.current_property_id`)
3. **Daily Outreach**: 30 new agent relationships
4. **Priority Agents**: 5 priority agent calls (sets `FUM.current_agent_id`)
5. **Campaigns**: 3 campaigns (Hot/Warm/Cold)

### Data Sources

| Source | Fields |
|--------|--------|
| **MLS** | caretslistingstatus, dom/cdom, ptfv, future_value, keywords, price_change |
| **Tax (PropertyRadar)** | confidence_score (Propensity 0-8) |
| **PIQ** | offer_status, listagentagentid, assigned, relationship_status |
| **Agent365** | investor_source_count, last_communicate, transaction history |

---

# PHASE 2: BUILD 🔨 (Current Focus)

8 bots to build across 3 categories. This is the current development focus. All bots integrate with FUM.

| Bot ID | Category | Name | Primary Function | Priority |
|--------|----------|------|------------------|----------|
| **C1** | Comps | Comps > Map | Geographic comp visualization | P1 |
| **C2** | Comps | Comps > Matrix | Value intelligence interpretation | **P0** |
| **C3** | Comps | Comps > List | Bucket classification & sorting | P1 |
| **D1** | Deal | Investment Analysis | Buy box alignment & ROI calculation | **P0** |
| **D2** | Deal | Post Call and Practice | Call transcription & coaching | P2 |
| **D3** | Deal | Notes and Communication | Deal context & action items | P2 |
| **MGT1** | Management | My Stats | AA performance dashboard | P1 |
| **MGT2** | Management | AM Deal Support | AM oversight & deal escalation | P1 |

---

## COMP ANALYSIS BOTS (C1-C3)

### C1: Comps > Map

| Field | Value |
|-------|-------|
| **Bot ID** | C1 |
| **Bot Category** | Comps/Analysis |
| **Bot Priority** | P1 |
| **Primary User** | Acquisition Associate (AA) |
| **Primary UI Location** | PIQ → Comps → Map View |
| **Trigger Method** | Click (from PIQ Comps tab) |
| **Status** | 📋 SPEC NEEDED |

#### FUM Integration

| Operation | Fields |
|-----------|--------|
| **READS** | FUM.current_property_id (PIQ context), FUM.preferences.markets |
| **WRITES** | None (read-only visualization) |

#### Primary Purpose

Geographic visualization of comparable properties relative to PIQ. Display distance, price/sqft, and property characteristics on interactive map. Overlays school districts, busy streets, boundaries.

> **Note**: Prototype exists at v0-ai-comps.vercel.app - needs full specification.

---

### C2: Comps > Matrix

| Field | Value |
|-------|-------|
| **Bot ID** | C2 |
| **Bot Category** | Comps/Analysis |
| **Bot Priority** | **P0 - Critical Path** |
| **Primary User** | Acquisition Associate (AA) |
| **Primary UI Location** | PIQ → Comps → Matrix View |
| **Trigger Method** | Click (from PIQ Comps tab) |
| **Status** | 📋 PRD v3.0 COMPLETE |

#### FUM Integration

| Operation | Fields |
|-----------|--------|
| **READS** | FUM.current_property_id, FUM.user_id (for personalization) |
| **WRITES** | None (interpretation layer) |

#### Primary Purpose

AI interpretation of comp data. Only surface what the data CANNOT tell you on its own. **Interpret, don't repeat.**

#### Core Principle

> ⚠️ **Bot should interpret, not repeat.**
> - PIQ vs Comp table already visible
> - Matrix status buckets already visible
> - E-Value calculation already visible

---

### C3: Comps > List

| Field | Value |
|-------|-------|
| **Bot ID** | C3 |
| **Bot Category** | Comps/Analysis |
| **Bot Priority** | P1 - Core Feature |
| **Primary UI Location** | PIQ → Comps → List View |
| **Status** | 📋 PRD v2.0 COMPLETE (Hardened) |

#### FUM Integration

| Operation | Fields |
|-----------|--------|
| **READS** | FUM.current_property_id |
| **WRITES** | comp_bucket_classification to property context |

#### Primary Purpose

Bucket classification (PREMIUM, HIGH, MID, LOW), value ceiling logic, chip/tag generation, and sort order logic.

#### Key Features

- Shared Data Schema
- Bucket Classification
- Value Ceiling Logic
- Chip/Tag Generation
- Sort Order Logic (Primary, Secondary, Tertiary)

---

## DEAL ANALYSIS BOTS (D1-D3)

### D1: Investment Analysis

| Field | Value |
|-------|-------|
| **Bot ID** | D1 |
| **Bot Category** | Deal Approach |
| **Bot Priority** | **P0 - Critical Path** |
| **Primary UI Location** | PIQ → Investment Analysis Tab |
| **Status** | 📋 SPEC IN PROGRESS |

#### FUM Integration

| Operation | Fields |
|-----------|--------|
| **READS** | FUM.current_property_id, FUM.company_id (for buy box) |
| **WRITES** | investment_recommendation to property context |

#### Primary Purpose

Buy box alignment analysis. Generate box showing why property fits (or doesn't) and recommended purchase pricing.

#### Key Logic

- ARV calculation
- EquityPercent analysis
- isFlip detection
- Ceiling Comp identification
- Purchase price recommendation

---

### D2: Post Call and Practice

| Field | Value |
|-------|-------|
| **Bot ID** | D2 |
| **Bot Category** | Deal Approach |
| **Bot Priority** | P2 |
| **Trigger Method** | Post-call / Manual |
| **Status** | 📋 CONCEPT ONLY |

#### FUM Integration

| Operation | Fields |
|-----------|--------|
| **READS** | FUM.user_id, FUM.current_agent_id |
| **WRITES** | FUM.daily_metrics.calls_analyzed, coaching_notes |

#### Primary Purpose

AI call transcription and coaching. Records, transcribes, analyzes call performance. Outputs call summary with improvement suggestions.

---

### D3: Notes and Communication

| Field | Value |
|-------|-------|
| **Bot ID** | D3 |
| **Bot Category** | Deal Approach |
| **Bot Priority** | P2 |
| **Trigger Method** | Auto / Manual |
| **Status** | 📋 CONCEPT ONLY |

#### FUM Integration

| Operation | Fields |
|-----------|--------|
| **READS** | FUM.current_property_id, FUM.user_id |
| **WRITES** | property.notes[], property.action_items[] |

#### Primary Purpose

Maintains deal context and history. Auto-categorizes notes, extracts action items. Outputs context briefing for each property interaction.

---

## MANAGEMENT BOTS (MGT1-MGT2)

### MGT1: My Stats

| Field | Value |
|-------|-------|
| **Bot ID** | MGT1 |
| **Bot Category** | Management |
| **Bot Priority** | P1 |
| **Primary User** | Acquisition Associate (AA) |
| **Primary UI Location** | Dashboard → My Stats |
| **Trigger Method** | Click / End of Day Auto |
| **Status** | 📋 SPEC NEEDED |

#### FUM Integration

| Operation | Fields |
|-----------|--------|
| **READS** | FUM.user_id, FUM.team_id, FUM.daily_metrics |
| **WRITES** | None (aggregation/display only) |

#### Primary Purpose

Daily performance dashboard for AA. Shows offers, calls, conversations, relationships, team leaderboard, and tomorrow's priorities.

#### Key Metrics

| Metric | Goal |
|--------|------|
| Offers | 3-5/day |
| Calls | 30/day |
| Conversations | 5/day |
| Relationships | Progress to 100 Elite |
| Team Ranking | Visible |

---

### MGT2: AM Deal Support

| Field | Value |
|-------|-------|
| **Bot ID** | MGT2 |
| **Bot Category** | Management |
| **Bot Priority** | P1 |
| **Primary User** | Acquisition Manager (AM) |
| **Primary UI Location** | Dashboard → AM Deal Support |
| **Trigger Method** | Click / Alert-based |
| **Status** | 📋 SPEC NEEDED |

#### FUM Integration

| Operation | Fields |
|-----------|--------|
| **READS** | FUM.role (must be AM), FUM.team_id, all AA daily_metrics in team |
| **RECEIVES** | Escalations from iQ-1 via FUM.am_id routing |

#### Primary Purpose

AM oversight dashboard. Receives escalations from AAs, monitors team performance, provides deal coaching and intervention.

---

# PHASE 3: FUTURE 📋 (End Feb 2026)

4 Master Bots planned for end of February 2026. These orchestrate specialized sub-bots and provide the FUM infrastructure.

| Bot ID | Name | Purpose |
|--------|------|---------|
| **AA0** | iQ Universal Interface | Dynamic conversational AI - answers anything, universal navigation |
| **MMaster** | Marketing Master | Orchestrates M1-M5 marketing automation bots |
| **MGTMaster** | Management Master | Orchestrates all management/reporting functions |
| **FUM** | FlipiQ User Master | Management/reporting functions for all users |

---

## AA0: iQ Universal Interface

| Field | Value |
|-------|-------|
| **Bot ID** | AA0 |
| **Bot Category** | Universal Interface |
| **Bot Priority** | P0 |
| **Primary User** | All Users |
| **Primary UI Location** | Dashboard overlay - persistent chat |
| **Trigger Method** | Voice/text command |
| **Target** | End of February 2026 |

### FUM Integration

**FULL ACCESS**: Reads/writes all FUM context | Routes to any bot based on intent | Universal Q&A across entire system

### Primary Purpose

Single conversational interface for ALL FlipIQ functions. Upgrade from current basic navigation to dynamic, universal assistant.

### Key Functions

- **Add Property**: Parse address/price → Create record
- **Wholesale Property**: Qualifying questions → DispoPro integration
- **Bug Reporting**: Route to Intercom
- **System Navigation**: Intent recognition → Navigate anywhere
- **Answer Anything**: Full knowledge of all FlipIQ systems

---

## MMaster: Marketing Master

Orchestrates all marketing automation.

### Sub-Bots to be Orchestrated

| Bot ID | Name | Function |
|--------|------|----------|
| M1 | Honeypot Bot | Social media posting via VA instructions |
| M2 | Deal Email Pipeline | Route deal@company emails to pipeline |
| M3 | Retargeting Bot | Segment agents and run targeted campaigns |
| M4 | Bi-Monthly Content | Create and send value content campaigns |
| M5 | Digital Encapsulation | Manage company digital presence |

### FUM Integration

**READS**: FUM.company_id, FUM.permissions.marketing | Orchestrates M1-M5 with company context

---

## MGTMaster: Management Master

Orchestrates all reporting and monitoring. Aggregates data from all bots. Outputs executive dashboard.

### Key Functions

- Team performance aggregation
- Deal pipeline health monitoring
- Predictive coaching recommendations
- EOD Debrief automation

### FUM Integration

**READS**: FUM.role (Principal/COO), FUM.company_id | Aggregates all team FUM.daily_metrics

---

## FlipiQ User Master (FUM)

The central orchestration layer that provides unified user context, permissions, and state management across ALL bots.

### Key Functions

1. User Authentication & Session Management
2. Role Detection (AA, AM, Principal, COO, SuperAdmin)
3. Permission Matrix (what each user can see/do/edit)
4. Context Provider (current property, agent, deal context)
5. State Manager (workflow progress, check-in status)
6. Performance Tracker (user metrics, goals, streak data)
7. Team Hierarchy (reports to, direct reports, company)
8. Preference Store (UI settings, notification prefs)

### FUM Data Model

```json
{
  "user_id": "string",
  "role": "AA | AM | Principal | COO | SuperAdmin",
  "company_id": "string",
  "team_id": "string",
  "am_id": "string",
  "current_property_id": "string",
  "current_agent_id": "string",
  "session_state": {},
  "permissions": [],
  "daily_metrics": {},
  "preferences": {}
}
```

---

# THE 100 RELATIONSHIPS SYSTEM

> "Know who to call, why to call them, and what value you bring — powered by data."

## Why 100 Relationships Matter

The entire system works because we have the strongest agent–investor dataset in the industry. It tells us how each agent interacts with investors: listings sold to/for investors, double-ended flips, which investor entities they sell to, whether they use buyer's agents, their sell velocity, preferred investor types, and deal patterns.

**When you understand how agents make money, you know:**
- Who to call
- How to approach them
- What value you bring
- Which script to use
- How to position your company
- WHEN NOT TO WASTE YOUR TIME

## The Spectrum of Agents

| Classification | Characteristics | Strategy |
|----------------|-----------------|----------|
| **WHALES** | Fat & happy, tied to one investor or internal team, hard to penetrate | Heavy value and credibility required. High effort / High return |
| **DOLPHINS** | Smart, dynamic, follow the money, willing to send deals if you bring value | **Best ROI - focus most time here. Ideal target.** |
| **FISH** | Busy, scattered, small investor activity (1 deal every 3-5 years), relies on MLS | High volume automation. Digital encapsulation. Right place, right time. |

## The Economics of 100 Relationships

- 2 meaningful new relationships per week
- 50 weeks a year
- **100 relationships per year**
- If each gives you one deal every other year → **50 deals/year**
- That's **4 deals/month** - real, consistent pipeline

> **Power Move**: Once they know and trust you, they will follow you for the rest of your career.

---

# DATA SOURCES & INTEGRATION

## MLS Data Fields

```
caretslistingstatus, dom/cdom, ptfv, future_value, keywords, price_change,
StandardStatus, DaysOnMarket, OriginalListPrice, ListPrice, Remarks,
PrivateRemarks, ShowingInstructions, ListingTerms
```

## PropertyRadar Fields (API Required)

| Category | Fields |
|----------|--------|
| **Foreclosure** | inForeclosure, ForeclosureStage, DefaultAmount, SaleDate, NOD, NOTS |
| **Financial** | EstimatedEquity, LTV, TotalLoanBalance, isJudgment, isTaxDefaulted |
| **Life Events** | AffidavitOfDeath, isProbate, Bankruptcy |
| **Vacancy** | isSiteVacant, isMailVacant |

## Agent365 Fields

```
ActiveInLast2Years, LastClosingDate, InvestorSourceCount, DoubleEnded,
BuyerAgentCompany, OfficeName, InvestorLender, Top Investor Partners,
Market Footprint, Top Title Company
```

## Propensity Score Weights

| Signal | Weight |
|--------|--------|
| Notice of Trustee Sale | +8 |
| Notice of Default | +6 |
| Tax Delinquency | +5 |
| Affidavit of Death | +5 |
| Bankruptcy / Judgment | +4 |
| Vacant Property | +3 |

---

# BMAD HANDOFF CHECKLIST

## For Eric (PM)

- [ ] Create epics for each Phase 2 bot with FUM integration requirements
- [ ] Break into user stories using BMAD method
- [ ] Prioritize: **C2 (Matrix) → D1 (Investment) → C3 (List) → C1 (Map)**
- [ ] Add FUM reads/writes to each story acceptance criteria
- [ ] Add to Asana with proper dependencies

## For Nate (CTO)

- [ ] Use Claude Code with these specs to generate tasks
- [ ] PropertyRadar API integration is critical path blocker
- [ ] Implement FUM service as central context provider (Redis recommended for session_state)
- [ ] Implement specs in BMAD repo (flipiq24/BMAD-METHOD)
- [ ] Each bot needs: agent YAML, workflow definition, FUM hooks, test scenarios

## Known Blockers

| Blocker | Owner |
|---------|-------|
| PropertyRadar API connection | Haris |
| Propensity Score showing N/A (needs Tax data feed) | — |
| Agent Data fields not in PIQ database | — |
| FUM service architecture decision (microservice vs embedded) | Nate |

---

# TEAM

| Name | Role |
|------|------|
| **Tony** | Principal / Domain Expert (31 years, 1,100+ flips) |
| **Eric** | PM / AWS Infrastructure |
| **Nate** | CTO / Front-end Development |
| **Haris** | Product |
| **Faizal** | UI Developer |

---

# BOT ID MAPPING (Old → New)

For reference, here is how previous bot IDs map to the new standard:

| Old ID | New ID | Name |
|--------|--------|------|
| C-OVERLAY-MAP | **C1** | Comps > Map |
| — | **C2** | Comps > Matrix |
| — | **C3** | Comps > List |
| IAMaster | **D1** | Investment Analysis |
| PC1 | **D2** | Post Call and Practice |
| NC1 | **D3** | Notes and Communication |
| MGT3 | **MGT1** | My Stats |
| AMD1 | **MGT2** | AM Deal Support |

---

**— END OF DOCUMENT —**

**FlipIQ: Together, We Flip Smarter**
Version 2.0 | January 2, 2026
