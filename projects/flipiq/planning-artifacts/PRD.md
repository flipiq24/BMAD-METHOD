---
stepsCompleted: ['vision', 'success-criteria', 'user-journeys', 'domain-analysis', 'innovation', 'project-type', 'scoping', 'functional-requirements', 'non-functional-requirements', 'review']
inputDocuments: ['FlipIQ Master Technical Specification December 2024']
workflowType: 'prd'
track: 'BMad Method'
---

# Product Requirements Document - FlipIQ AI Overlay System

**Author:** FlipIQ Product Team
**Date:** December 2024
**Version:** 2.0 (Phase 2)
**Status:** Approved for Development

---

## 1. Vision & Goals

### 1.1 Product Vision

FlipIQ is an AI-powered overlay system that transforms real estate acquisition teams into consistent deal-closing machines. The system deploys specialized AI bots on top of existing Command platform infrastructure, requiring zero database changes while delivering immediate productivity gains.

### 1.2 Core Mission

Enable every Acquisition Associate (AA) to reliably close **two deals per month** through:
- Systematic workflows
- Intelligent property prioritization
- Relationship management automation
- Real-time performance tracking

### 1.3 Strategic Goals

| Goal | Target | Baseline |
|------|--------|----------|
| Morning setup time | 30 seconds | 60 minutes |
| Deal analysis time | 2 minutes | 15 minutes |
| Daily offers generated | 5 consistently | 1-2 |
| Management oversight | 15 minutes daily | 3 hours daily |
| AA deals per month | 2 flips + 6 wholesales | Variable |

### 1.4 Key Value Propositions

1. **For Acquisition Associates**: Eliminate guesswork - know exactly what to chase, why, and how
2. **For Acquisition Managers**: Exception-based management with real-time visibility
3. **For Principals**: Scale from 3 to 20+ AAs without proportional time investment
4. **For Enterprise (COO)**: Manage 375+ operators generating $6.75M MRR

---

## 2. Success Criteria

### 2.1 Primary KPIs

| Metric | Target | Measurement Method |
|--------|--------|-------------------|
| AA Daily Conversations | 30 | System logged |
| AA Daily Offers | 5 | Offer Terms module |
| AA Daily Relationships Built | 5 | Agent CRM status changes |
| Monthly Flips per Operator | 2 | Pipeline closed deals |
| Monthly Wholesales per Operator | 6 | Pipeline closed deals |
| Check-in Compliance | 100% | Morning bot logs |
| Deal Review Completion | 100% | Workflow completion |

### 2.2 Revenue Metrics (Per Operator)

| Revenue Stream | Calculation | Monthly Target |
|----------------|-------------|----------------|
| Flip Revenue | $300k × 2 × 0.5% | $3,000 |
| Wholesale Revenue | $25k × 6 × 10% | $15,000 |
| **Total per Operator** | | **$18,000** |

### 2.3 Scale Metrics (COO Level)

| Metric | Target |
|--------|--------|
| Active Operators | 375 |
| Total AAs Deployed | 1,500 |
| Monthly Revenue | $6,750,000 |
| Annual Run Rate | $81,000,000 |

---

## 3. User Personas & Journeys

### 3.1 User Personas

#### Persona 1: Acquisition Associate (AA) - Josh

**Role:** Front-line deal closer
**Goals:** Close 2 deals/month, build 100 elite agent relationships
**Pain Points:**
- Don't know which properties to prioritize
- Scripts vary in effectiveness
- Forget follow-ups
- Miss agent relationship opportunities

**Daily Journey:**
1. 7:15 AM - Check-in (30 seconds)
2. 7:20 AM - Deal Review (work through prioritized deals)
3. 9:00 AM - Daily Outreach (30 high-propensity properties)
4. 11:30 AM - Priority Agent Calls
5. 12:00 PM - Send Campaigns
6. 1:00 PM - Continue Outreach
7. 3:00 PM - Offer Generation (5 offers)
8. 5:00 PM - EOD Report

#### Persona 2: Acquisition Manager (AM) - Eric

**Role:** Team gatekeeper and coach
**Goals:** Ensure team hits metrics, intervene on exceptions
**Pain Points:**
- Can't monitor 8+ AAs effectively
- Reactive instead of proactive coaching
- No visibility into AA quality

**Key Activities:**
- Morning exception review
- Performance tracking
- Coaching triggers
- Deal approvals

#### Persona 3: Principal/Broker - Tony

**Role:** 4-office KW broker running wholesale operation
**Goals:** $150k/month profit with 2 minutes/day involvement
**Pain Points:**
- Training death spiral (90 hours per hire, 80% quit)
- B-player plague (can't fire invested hires)
- Time trap (3 hours daily checking on AAs)

**Key Activities:**
- 30-second dashboard review
- Exception-based decisions
- Terminate/promote approvals

#### Persona 4: COO - Ethan

**Role:** Manages 375 operators nationwide
**Goals:** $6.75M MRR, 15 success managers
**Pain Points:**
- Scaling oversight across regions
- Churn prevention
- Collection management

**Key Activities:**
- National dashboard review
- Regional performance analysis
- Success manager deployment
- Churn intervention

---

## 4. Domain Analysis

### 4.1 Core Domain Entities

```
┌─────────────────────────────────────────────────────────────┐
│                    FlipIQ Domain Model                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  PROPERTIES                 AGENTS                          │
│  ├── MLS Data              ├── Agent365 Profile             │
│  ├── PropertyRadar Data    ├── Relationship Status          │
│  ├── Propensity Score      ├── Investor Source Count        │
│  ├── DFI Score             ├── Transaction History          │
│  └── Offer Status          └── Tier (Whale/Dolphin/Fish)   │
│                                                              │
│  USERS                      DEALS                           │
│  ├── AA (Associates)       ├── Pipeline Status              │
│  ├── AM (Managers)         ├── Offer Terms                  │
│  ├── Principal             ├── Comps Analysis               │
│  └── COO                   └── Investment Analysis          │
│                                                              │
│  BOTS (32 Total)            INTEGRATIONS                    │
│  ├── Daily Ops (4)         ├── MLS API                      │
│  ├── Deal Analysis (10)    ├── PropertyRadar API            │
│  ├── Management (3)        ├── Agent365                     │
│  ├── Marketing (6)         ├── DispoPro                     │
│  ├── Comps (5)             ├── Intercom                     │
│  ├── Investment (2)        └── Command Platform             │
│  ├── Communication (1)                                       │
│  └── Goal (1)                                                │
└─────────────────────────────────────────────────────────────┘
```

### 4.2 Data Sources

| Source | Data Retrieved | Usage |
|--------|---------------|-------|
| **MLS** | caretslistingstatus, dom/cdom, ptfv, future_value, keywords, price_change | Property behavior |
| **PropertyRadar** | inForeclosure, NoticeOfDefault, isTaxDefaulted, AffidavitOfDeath, Bankruptcy, isSiteVacant, EstimatedEquity | Seller motivation |
| **Agent365** | InvestorSourceCount, LastClosingDate, DoubleEndedCount, TransactionHistory, TopInvestorPartners | Agent evaluation |
| **DispoPro** | Buyer lists, disposition data | Wholesale matching |

### 4.3 Deal Focus Index (DFI)

**Formula:** `DFI = [FixerCondition] + [InventoryStage] + [SellerPainLevel] + [AgentBehavior]`

**Propensity Score Weights:**
| Signal | Weight |
|--------|--------|
| Notice of Trustee Sale | +8 |
| Notice of Default | +6 |
| Tax Delinquency | +5 |
| Affidavit of Death | +5 |
| Bankruptcy / Judgment | +4 |
| High Mortgage / Debt | +3 |
| Vacant Property | +3 |

---

## 5. Innovation & Differentiation

### 5.1 Key Innovations

1. **Zero Database Changes**: Overlay system deploys on existing Command infrastructure
2. **AI-Driven Scripts**: Personalized scripts using agent's actual transaction history
3. **Deal Focus Index**: Proprietary scoring combining MLS, PropertyRadar, and Agent365 data
4. **Exception-Based Management**: Managers only see problems, not routine activity
5. **Conversation Tracking**: Measures conversations, not just calls (quality over quantity)

### 5.2 Competitive Advantages

| Feature | FlipIQ | Competitors |
|---------|--------|-------------|
| Integrated distress signals | Real-time PropertyRadar | Manual research |
| Agent relationship automation | AI-driven scripts | CRM only |
| Propensity scoring | Automated DFI | Manual assessment |
| Management oversight | 15 min/day | 3+ hours/day |
| Training time | 5 minutes | 90 hours |

---

## 6. Project Type Classification

**Classification:** Enterprise SaaS Platform with AI Overlay
**Track:** BMad Method (10-50+ stories)
**Architecture Pattern:** Event-driven microservices with bot orchestration

### 6.1 System Characteristics

- **Multi-tenant**: Supports 375+ operators
- **Real-time**: Live property and agent data
- **AI-powered**: 32 specialized bots
- **Integration-heavy**: 5+ external data sources
- **Mobile-responsive**: Dashboard accessible anywhere

---

## 7. Scoping & Constraints

### 7.1 Phase Breakdown

| Phase | Timeline | Scope |
|-------|----------|-------|
| **Phase 1** | Complete | AA1-AA4 bots, DMaster, PIQ, D1-D3 (9 bots) |
| **Phase 2a** | December 2024 | My Stats, D4-D8, CMaster, C1, C2, C4, IAMaster, Com1 (12 bots) |
| **Phase 2b** | January 2025 | AA0, MGT1-3, M1-M5 (9 bots) |
| **Phase 3** | Q1 2025 | C3, IARehab, SuperMaster, User AI Personalization (4 bots + personalization engine) |

### 7.2 December 2024 Priority (Phase 2a)

**Goal:** Ship the Deal Machine for 7 operators by Dec 31.

| Priority | Bot | Purpose |
|----------|-----|---------|
| P0 | My Stats | AA/Principal performance dashboard |
| P0 | D4 (Notes) | Deal context and history |
| P0 | D5 (Reminders) | Zero missed follow-ups |
| P0 | D6 (Activity) | Automatic interaction logging |
| P0 | D7 (Post Call) | Transcription + coaching |
| P0 | D8 (Close Report) | Post-deal learning |
| P0 | CMaster | Comp orchestration |
| P0 | C1 (Map) | Visual comp validation |
| P0 | C2 (Matrix) | Statistical analysis |
| P0 | C4 (AI Mapping) | Photo-based condition adjustment |
| P0 | IAMaster | Investment analysis |
| P0 | Com1 | Auto-follow-up sequences |

**Deferred to January (Phase 2b):**
- AA0 (Universal Interface) - Voice/NLP complexity
- MGT1-3 (Management) - My Stats covers basic needs
- M1-M5 (Marketing) - Not critical for 7 operators

### 7.3 Technical Constraints

1. **Command Platform Dependency**: Must overlay without modifying Command database
2. **API Rate Limits**: PropertyRadar, MLS APIs have usage limits
3. **Real-time Requirements**: Check-in and pipeline updates must be <2 seconds
4. **Browser Compatibility**: Chrome, Safari, Firefox support required

### 7.4 Business Constraints

1. **Revenue Model**: 0.5% of flip purchase + 10% of wholesale profit
2. **Scaling Target**: 500 operators by Q2 2025
3. **Success Manager Ratio**: 1:25 operators

---

## 8. Functional Requirements

### FR-001: Universal Interface Bot (AA0)

**Priority:** P0 (Phase 2)
**User:** All users

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-001.1 | Voice/text property entry | User speaks/types address → property record created in <5 seconds |
| FR-001.2 | Wholesale property qualifying | Qualifying questions auto-populate DispoPro |
| FR-001.3 | Bug reporting | Direct Intercom integration, ticket created automatically |
| FR-001.4 | Natural language navigation | "show me pending deals" → navigates to filtered view |

### FR-002: Morning Check-In Bot (AA1)

**Priority:** P0 (Complete)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-002.1 | Auto-trigger on first login | Bot activates within 2 seconds of daily first login |
| FR-002.2 | Availability confirmation | Captures yes/no with timestamp |
| FR-002.3 | Blocker identification | Routes blockers to AM within 30 seconds |
| FR-002.4 | Session flag storage | flipiq_morning_checkin = today stored |

### FR-003: Deal Review Bot (AA2)

**Priority:** P0 (Complete)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-003.1 | Priority sorting | Deals sorted: Critical → Hot → Warm → Cold → New |
| FR-003.2 | Property counts | Accurate counts displayed for each category |
| FR-003.3 | Navigation generation | One-click navigation to Deal Review page |
| FR-003.4 | Tab workflow | 5-tab progression: PIQ → Comps → Investment → Agent → Offer |

### FR-004: Daily Outreach Bot (AA3)

**Priority:** P0 (Complete)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-004.1 | 30-property selection | System selects 30 highest-propensity properties |
| FR-004.2 | Assigned agent prioritization | Priority → Hot → Warm → Cold agents first |
| FR-004.3 | Script generation | Personalized scripts using agent transaction history |
| FR-004.4 | Conversation tracking | Counts conversations, not just calls |
| FR-004.5 | Auto-remove logic | Property removed when offer_status changes from "None" |

### FR-005: Agent Relationship Bot (AA4)

**Priority:** P0 (Complete)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-005.1 | 100 elite agent tracking | Dashboard shows progress to 100 relationships |
| FR-005.2 | Touchpoint scheduling | Follow-up reminders based on tier |
| FR-005.3 | Agent categorization | Whale/Dolphin/Fish classification |
| FR-005.4 | Call queue generation | 30 prioritized agents with scripts |

### FR-006: Property Intelligence Bot (PIQ)

**Priority:** P0 (Complete)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-006.1 | Data source merging | MLS + PropertyRadar + Agent365 in single view |
| FR-006.2 | DFI calculation | Score generated using formula |
| FR-006.3 | What/Why/How output | Clear recommendation structure |
| FR-006.4 | 30-second analysis | Full report generated in <30 seconds |

### FR-007: MLS Deal Review Bot (D1)

**Priority:** P0 (Complete)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-007.1 | Keyword extraction | Identifies: as-is, investor, estate, fixer, etc. |
| FR-007.2 | Red flag identification | Flags: tenant occupied, pending litigation, etc. |
| FR-007.3 | Potential scoring | 0-100 score with confidence level |
| FR-007.4 | Deal recommendation | Chase/Don't Chase with reasoning |

### FR-008: Agent Review Bot (D2)

**Priority:** P0 (90% Complete)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-008.1 | Transaction history pull | Last 2 years of transactions displayed |
| FR-008.2 | Investor-friendliness score | Score based on investor transaction % |
| FR-008.3 | Approach strategy generation | Tier-specific script recommendation |
| FR-008.4 | Buyer ecosystem display | Top investors, lenders, title companies |

### FR-009: Propensity Analysis Bot (D3)

**Priority:** P0 (Complete)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-009.1 | PropertyRadar integration | Real-time distress signal pull |
| FR-009.2 | Weighted scoring | NOD +6, Tax +5, Death +5, Bankruptcy +4 |
| FR-009.3 | Pain score calculation | Total weight with urgency rating |
| FR-009.4 | Seller pain narrative | AI-generated explanation of motivation |

### FR-010: Notes Bot (D4)

**Priority:** P1 (Phase 2)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-010.1 | Auto-categorization | Notes tagged by type automatically |
| FR-010.2 | Action item extraction | Tasks extracted from note text |
| FR-010.3 | Context briefing | Summary displayed on property load |

### FR-011: Reminders Bot (D5)

**Priority:** P1 (Phase 2)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-011.1 | Stage-based reminders | Reminder timing based on deal stage |
| FR-011.2 | Auto-follow-up sequences | Day 3/7/10/15 pending property follow-ups |
| FR-011.3 | Zero missed follow-ups | 100% reminder delivery |

### FR-012: Activity Bot (D6)

**Priority:** P1 (Phase 2)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-012.1 | Automatic logging | Calls, emails, texts logged without manual entry |
| FR-012.2 | Complete timeline | Chronological interaction history |
| FR-012.3 | Activity search | Filter by type, date, outcome |

### FR-013: Post Call Bot (D7)

**Priority:** P1 (Phase 2)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-013.1 | Call recording | Automated recording with consent |
| FR-013.2 | AI transcription | Accurate transcription in <2 minutes |
| FR-013.3 | Performance analysis | Script adherence scoring |
| FR-013.4 | Coaching suggestions | Specific improvement recommendations |

### FR-014: Close Deal Report Bot (D8)

**Priority:** P2 (Phase 2)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-014.1 | Success factor analysis | What worked identified |
| FR-014.2 | Failure pattern detection | What didn't work flagged |
| FR-014.3 | Market data capture | ARV accuracy, days to close recorded |
| FR-014.4 | Lessons learned | Actionable insights generated |

### FR-015: Management Master Bot (MGTMaster)

**Priority:** P0 (Phase 2)
**User:** Principal/AM

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-015.1 | Data aggregation | All bot data in single dashboard |
| FR-015.2 | Exception highlighting | Only problems surfaced |
| FR-015.3 | Real-time updates | <5 second refresh |

### FR-016: End-of-Day Debrief Bot (MGT2)

**Priority:** P0 (Phase 2)
**User:** Principal/AM

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-016.1 | 5PM auto-trigger | Report generated daily at 5PM |
| FR-016.2 | Metric compilation | All KPIs vs targets |
| FR-016.3 | AI coaching suggestions | Specific coaching recommendations |
| FR-016.4 | Team scorecards | Individual performance rankings |

### FR-017: Mailbox Monitor Bot (MGT3)

**Priority:** P1 (Phase 2)
**User:** System

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-017.1 | Email scanning | Real-time inbox monitoring |
| FR-017.2 | Urgency detection | Critical items flagged |
| FR-017.3 | Priority inbox | Filtered view of important messages |

### FR-018: Marketing Master Bot (MMaster)

**Priority:** P1 (Phase 2)
**User:** System

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-018.1 | Campaign orchestration | All marketing bots coordinated |
| FR-018.2 | Performance metrics | Open rates, click rates tracked |
| FR-018.3 | ChatGPT integration | AI-powered marketing training |

### FR-019: Honeypot Bot (M1)

**Priority:** P1 (Phase 2)
**User:** VA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-019.1 | VA instructions | Daily posting tasks generated |
| FR-019.2 | Lead tracking | 10+ inbound leads/week target |
| FR-019.3 | Conversion metrics | Lead-to-deal tracking |

### FR-020: Deal Email Pipeline Bot (M2)

**Priority:** P1 (Phase 2)
**User:** System

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-020.1 | Email parsing | Property data extracted from deal@ emails |
| FR-020.2 | Auto-record creation | Property card created automatically |
| FR-020.3 | 100% intake | No manual entry required |

### FR-021: Retargeting Bot (M3)

**Priority:** P1 (Phase 2)
**User:** Principal/AM

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-021.1 | Agent segmentation | Tier-based grouping |
| FR-021.2 | Campaign scheduling | Monthly touch calendar |
| FR-021.3 | 2x monthly touches | Each agent contacted twice |

### FR-022: Bi-Monthly Content Bot (M4)

**Priority:** P2 (Phase 2)
**User:** Principal/AM

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-022.1 | Market analysis | Local market data compiled |
| FR-022.2 | Content generation | Email/social content created |
| FR-022.3 | >20% open rates | Email performance target |

### FR-023: Digital Encapsulation Bot (M5)

**Priority:** P2 (Phase 2)
**User:** Principal/AM

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-023.1 | Presence monitoring | Online presence tracked |
| FR-023.2 | Improvement suggestions | Actionable recommendations |
| FR-023.3 | Digital scorecard | Comprehensive presence score |

### FR-024: Comp Master Bot (CMaster)

**Priority:** P0 (Phase 2)
**User:** System

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-024.1 | Three-level education | Beginner/Intermediate/Advanced paths |
| FR-024.2 | Bot coordination | C1-C4 orchestrated |
| FR-024.3 | Comprehensive report | Full comp analysis output |

### FR-025: Map Review Bot (C1)

**Priority:** P1 (Phase 2)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-025.1 | Visual validation | Map-based comp review |
| FR-025.2 | Boundary overlays | School districts, streets shown |
| FR-025.3 | Quality indicators | Good/questionable comp markers |

### FR-026: Matrix Summary Bot (C2)

**Priority:** P1 (Phase 2)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-026.1 | Statistical analysis | Price/sqft calculated |
| FR-026.2 | Valid comp filtering | Outliers removed |
| FR-026.3 | Range determination | Min/max/median established |

### FR-027: List Grouping Bot (C3)

**Priority:** P1 (Phase 3)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-027.1 | Condition grouping | High/Mid/Low categories |
| FR-027.2 | Value ceiling | Maximum ARV with explanation |
| FR-027.3 | Better ARV accuracy | <5% variance from actual |

### FR-028: Comp AI Mapping Bot (C4)

**Priority:** P2 (Phase 2)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-028.1 | Photo analysis | Computer vision condition assessment |
| FR-028.2 | Tax data integration | Property details updated |
| FR-028.3 | Condition adjustment | Values adjusted for condition |

### FR-029: Investment Master Bot (IAMaster)

**Priority:** P0 (Phase 2)
**User:** System

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-029.1 | Buy box alignment | Property vs criteria matching |
| FR-029.2 | Return calculation | ROI/IRR computed |
| FR-029.3 | Deal structuring | Optimal structure suggested |
| FR-029.4 | Investment recommendation | Go/No-Go with tactics |

### FR-030: Repair Calculator Bot (IARehab)

**Priority:** P0 (Phase 3)
**User:** AA

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-030.1 | Photo analysis | Damage assessment from images |
| FR-030.2 | Cost modeling | Market-specific costs applied |
| FR-030.3 | Line-item estimate | Detailed repair breakdown |
| FR-030.4 | <10% accuracy | Within 10% of actual costs |

### FR-031: Auto Connect Bot (Com1)

**Priority:** P1 (Phase 2)
**User:** System

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-031.1 | Multi-touch sequences | Automated follow-up after manual attempts |
| FR-031.2 | Response tracking | Conversation started flagged |
| FR-031.3 | 100% follow-up rate | No missed follow-ups |

### FR-032: Super Admin Bot (SuperMaster)

**Priority:** P0 (Phase 3)
**User:** System

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-032.1 | Enterprise tracking | All companies, teams, individuals |
| FR-032.2 | Executive dashboard | C-level visibility |
| FR-032.3 | 375+ operator scale | Support 375+ concurrent operators |

### FR-033: User AI Personalization Engine

**Priority:** P0 (Phase 3)
**User:** All users

| ID | Requirement | Acceptance Criteria |
|----|-------------|-------------------|
| FR-033.1 | Buy Box Upload | User uploads buy box criteria (price range, property type, markets, quality tier) → stored per user |
| FR-033.2 | Bot Output Customization | User fine-tunes AI output style (tone, verbosity, script preferences) → applied to all bot responses |
| FR-033.3 | Preference Learning | System collects data points from user actions → refines recommendations over time |
| FR-033.4 | Custom Script Templates | User creates/edits script templates → AI uses personalized scripts instead of baseline |
| FR-033.5 | Agent Approach Preferences | User sets preferred communication style per agent tier → scripts reflect preferences |
| FR-033.6 | Investment Criteria Storage | User stores ROI targets, renovation budgets, hold periods → Investment Analysis auto-populates |
| FR-033.7 | AI Output Preview | User previews customized AI output before saving → confirms personalization is correct |
| FR-033.8 | Reset to Baseline | User can reset any bot to baseline behavior → removes all customizations |

**Key Data Points Collected for Personalization:**
- Deal outcomes (success/fail patterns)
- Script usage and effectiveness
- Agent relationship progression
- Time spent per module
- Offer acceptance rates
- Preferred communication channels
- Market focus areas
- Property type preferences

**Personalization Scope:**
- AA1-AA4 Daily Process Bots: Tone, script style, prioritization weights
- D1-D8 Deal Analysis Bots: Analysis depth, report format, recommendation style
- CMaster Comp Bots: Bucket weighting, outlier tolerance, clustering preferences
- IAMaster Investment Bots: ROI targets, risk tolerance, buy box criteria
- Agent Scripts: Communication style, value proposition emphasis, closing approach

---

## 9. Non-Functional Requirements

### NFR-001: Performance

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-001.1 | Page load time | <2 seconds |
| NFR-001.2 | Bot response time | <3 seconds |
| NFR-001.3 | API response time | <500ms |
| NFR-001.4 | Concurrent users | 1,500 AAs |

### NFR-002: Scalability

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-002.1 | Operator capacity | 500+ operators |
| NFR-002.2 | Property volume | 50 properties per operator |
| NFR-002.3 | Daily transactions | 3,000 deals/month |

### NFR-003: Reliability

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-003.1 | Uptime | 99.9% |
| NFR-003.2 | Data durability | 99.999% |
| NFR-003.3 | Disaster recovery | RTO <4 hours |

### NFR-004: Security

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-004.1 | Authentication | OAuth 2.0 + MFA |
| NFR-004.2 | Data encryption | AES-256 at rest, TLS 1.3 in transit |
| NFR-004.3 | Role-based access | AA/AM/Principal/COO permissions |
| NFR-004.4 | Audit logging | All actions logged with user/timestamp |

### NFR-005: Integration

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-005.1 | MLS API | Real-time sync |
| NFR-005.2 | PropertyRadar API | <5 second data freshness |
| NFR-005.3 | Agent365 | Batch sync every 6 hours |
| NFR-005.4 | Command Platform | Zero database changes |

### NFR-006: Usability

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-006.1 | Onboarding time | 5 minutes (no training) |
| NFR-006.2 | Mobile responsiveness | Full functionality on tablet/phone |
| NFR-006.3 | Accessibility | WCAG 2.1 AA compliance |

### NFR-007: Compliance

| ID | Requirement | Target |
|----|-------------|--------|
| NFR-007.1 | Call recording consent | State-specific compliance |
| NFR-007.2 | Data retention | 7 years for transactions |
| NFR-007.3 | CCPA compliance | California data privacy |

---

## 10. Dependencies & Risks

### 10.1 External Dependencies

| Dependency | Risk Level | Mitigation |
|------------|------------|------------|
| PropertyRadar API | High | Rate limit monitoring, caching |
| MLS Data Feed | High | Redundant connections |
| Agent365 | Medium | Batch sync fallback |
| Command Platform | High | No modifications policy |

### 10.2 Key Risks

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| API rate limit exceeded | Medium | High | Implement caching, queue requests |
| User adoption resistance | Medium | High | Gamification, clear value demo |
| Data quality issues | Medium | Medium | Validation rules, cleanup bots |
| Scale performance degradation | Low | High | Load testing, auto-scaling |

---

## 11. Glossary

| Term | Definition |
|------|------------|
| **AA** | Acquisition Associate - front-line deal closer |
| **AM** | Acquisition Manager - team supervisor |
| **DFI** | Deal Focus Index - proprietary property scoring |
| **PTFV** | Price-to-Future-Value ratio |
| **DOM** | Days on Market |
| **CDOM** | Cumulative Days on Market |
| **NOD** | Notice of Default |
| **NOTS** | Notice of Trustee Sale |
| **PIQ** | Property Intelligence - data aggregation view |
| **ISC** | Investor Source Count |
| **ARV** | After Repair Value |

---

## 12. Approval

| Role | Name | Date | Signature |
|------|------|------|-----------|
| Product Owner | | | |
| Tech Lead | | | |
| Stakeholder | | | |

---

*Document generated using BMAD Method v6*
