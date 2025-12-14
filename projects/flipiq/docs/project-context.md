# FlipIQ Project Context

**Last Updated:** December 2024
**Purpose:** Critical information for developers working on FlipIQ

---

## Overview

FlipIQ is an AI-powered overlay system for real estate acquisition teams. It deploys 32 specialized bots on top of the existing Command platform to help Acquisition Associates close 2 deals per month consistently.

---

## Critical Rules

### 1. Command Platform Constraint

**RULE:** Zero modifications to Command platform database.

FlipIQ is an OVERLAY system. All FlipIQ-specific data must be stored in the FlipIQ database, not Command.

**Allowed:**
- Read data from Command via API
- Display Command data in FlipIQ UI
- Store FlipIQ metadata (notes, relationships, scores) separately

**NOT Allowed:**
- Write to Command database tables
- Modify Command database schema
- Add triggers to Command database

**Access:** Question for Nate on how to access Command platform code/data.

### 2. Bot Architecture Pattern

**RULE:** Master bots orchestrate, specialized bots execute.

Each bot category has a "Master" bot that coordinates:
- `DMaster` → D1, D2, D3, D4, D5, D6, D7, D8, PIQ
- `MGTMaster` → MGT2, MGT3
- `MMaster` → M1, M2, M3, M4, M5
- `CMaster` → C1, C2, C3, C4
- `IAMaster` → IARehab

**Bot Hierarchy:** Bots need hierarchy based on data connection and relevancy (Nate to think through).

### 3. Deal Focus Index (DFI)

**RULE:** DFI is calculated ON-DEMAND, not nightly batch.

Formula: `DFI = FixerCondition + InventoryStage + SellerPainLevel + AgentBehavior`

| Component | Source | Range |
|-----------|--------|-------|
| FixerCondition | MLS keywords | 0-3 |
| InventoryStage | MLS DOM | 0-4 |
| SellerPainLevel | PropertyRadar | 0-25+ |
| AgentBehavior | DispoPro Agent Reports | 0-6 |

**DFI Accuracy:** Based on raw MLS data feed, PropertyRadar API, and DispoPro agent reports.

### 4. User Role Hierarchy

**RULE:** Strict role-based access control.

```
COO (1)
  └── Success Managers (15)
       └── Operators/Principals (375) [Currently 7 active]
            └── AMs (~100)
                 └── AAs (1,500)
```

- AA sees: Own assigned properties, own agents, own metrics
- AM sees: Team properties, team agents, team metrics
- Principal sees: Company-wide data
- COO sees: All operators nationwide

### 5. Performance Targets

**RULE:** These are the non-negotiable goals that drive all features.

| Role | Daily | Monthly |
|------|-------|---------|
| AA | 30 conversations, 5 offers | 2 deals |
| Operator | Team compliance | 2 flips + 6 wholesales |
| COO | 375 operators | $6.75M MRR |

---

## Data Architecture

### Integration Microservices

**All integrations are being built as microservices:**

| Service | Description | Owner |
|---------|-------------|-------|
| **National Data** | PropertyRadar wrapper for distress signals | FlipIQ |
| **Agent Reports** | DispoPro agent intelligence (our product) | FlipIQ |
| **Command Reader** | Read-only Command platform access | Question for Nate |

### DispoPro Agent Reports

DispoPro provides comprehensive agent intelligence:
- Every agent who has worked with an investor
- Every investor's transaction history
- Relationships with lenders, agents, and title companies
- Used for Agent tier classification and script generation

### Data Retention

**RULE:** Keep all data - it has long-term value.

- Transaction data: 7+ years
- Bot decisions: Indefinite
- User actions: Indefinite
- Personalization data: Indefinite

---

## Terminology Clarifications

### Temperature Classifications

**Deal Temperature (Property Focus):**
- Critical → Hot → Warm → Cold → New
- Based on: DFI score, DOM, seller distress signals

**Relationship Status (Agent Focus):**
- Priority → Hot → Warm → Cold
- Based on: Communication recency, deal history

### Agent Tiers (Fish/Dolphin/Whale)

**Based on investor transaction count:**

| Tier | Name | Definition |
|------|------|------------|
| Tier 3 | Fish | 1-4 investor transactions |
| Tier 2 | Dolphin | 5-10 investor transactions |
| Tier 1 | Whale | 10+ investor transactions |

### Script Categories

Scripts are specific to listing scenarios:
1. **New Listing** - Just hit market
2. **Back on Market** - Fell out of escrow
3. **Price Reduction** - Just reduced price
4. **Pending Backup** - Accept backup offers
5. **3 Days** - 3 days on market
6. **10 Days** - 10 days on market
7. **20 Days** - 20 days on market
8. **Aged Inventory** - 70+ days on market

Each script type uses all available variables (agent history, propensity, keywords) to generate approach.

---

## Technical Patterns

### LLM Strategy

**RULE:** Best quality over lowest cost. Willing to pay more for better results.

- Current: ChatGPT (GPT-4)
- Voice: OpenAI Realtime API for voice input
- Transcription: Transcription only (NO call recording)

### API Integration Priorities

1. **MLS** - Real-time via webhooks (already complete)
2. **PropertyRadar** - Via National Data microservice
3. **DispoPro Agent Reports** - Via Agent Reports microservice
4. **Command Platform** - Read-only access (Nate to clarify)

### Caching Strategy

| Data Type | Cache Location | TTL |
|-----------|---------------|-----|
| Property data | Redis | 15 min |
| PropertyRadar | Redis | 24 hours |
| Agent data | Redis | 6 hours |
| DFI scores | Redis | On-demand (no batch) |
| User sessions | Redis | 24 hours |

### Error Handling

For external API failures:
1. Retry 3 times with exponential backoff
2. Serve cached data if available
3. Display user-friendly message
4. Log error for monitoring
5. Continue with partial data if possible

---

## UI Specifications

### Platform Constraints

- **NOT mobile-first** - Desktop optimized
- **Maybe phone calls** - Potential future mobile for calling
- **No offline capabilities** - Internet required
- **No keyboard shortcuts** - Not needed

### Comp UI Locations

| Comp View | Position |
|-----------|----------|
| Map | Overlay below the map |
| Matrix | Above the table |
| List | Sidebar on right |
| Investment Analysis | Below |
| Agent | Overlay |

### My Stats Dashboard

Reference screenshot provided - shows:
- Weekly view with date range
- KPI cards with trend sparklines (Calls, Relationships, Offers Sent, In Negotiations, Accepted, Acquired, Time)
- Team Leaderboard with rankings
- Daily Performance Report with breakdown

---

## AA3 - Daily Outreach Processing Logic

### Initial Filter (Eligibility)

Only include properties where:
- `offer_status = "None"`
- AND `assigned = Yes` (for user's agents)
- Once all assigned exhausted, include unassigned

### Phase 1: Assigned Properties

1. Prioritize by Relationship Status: Priority → Hot → Warm → Cold
2. Filter by property status: Active (≥70 DOM) → Backup → Pending
3. Sort by PTFV (lowest first = highest discount)
4. Apply Keyword and Propensity weighting
5. Highest Investor Source Count first

### Phase 2: Unassigned Properties

1. Prioritize Unassigned agents with highest Investor Source Count
2. Filter by property status: Active (≥70 DOM) → Backup → Pending
3. Sort by PTFV (lowest first = highest discount)
4. Apply Keyword and Propensity weighting

### Auto-Remove Logic

Remove from daily outreach when:
- `offer_status ≠ "None"` OR `assigned = False`

### Conversation Counting

**Important:** Counts CONVERSATIONS not calls. Must connect with agent to count toward 30.

---

## Buy Box Logic

### Buy Box Matrix

Buy boxes are configurable by:
- **Location** (county-level)
- **Price point** (ranges)
- **Year built** (ranges)

Tables can be variable:
- Applied across all markets
- OR by specific county
- OR by year built
- OR by price point

### Quality Tiers

Comp buckets are tied to the subject property baseline:
- Identify what needs to be fixed
- Determine at what level similar properties sell
- Calculate what the subject property needs to reach that level

---

## Naming Conventions

### Bot IDs
```
AA[0-5] - Acquisition Associate bots
D[1-8]  - Deal Analysis bots
MGT[1-3] - Management bots
M[1-5]  - Marketing bots
C[1-4]  - Comp Analysis bots
IA[x]   - Investment Analysis bots
Com[1]  - Communication bots
```

### Database Tables (FlipIQ Overlay Store)
```
flipiq_property_overlay
flipiq_agent_overlay
flipiq_check_in_log
flipiq_daily_metrics
flipiq_notes
flipiq_activities
flipiq_propensity_data
flipiq_user_preferences (Phase 3 - personalization)
flipiq_buy_box_matrix (Phase 3 - personalization)
```

### API Endpoints
```
GET /api/v1/properties/:id/piq       - Property Intelligence
GET /api/v1/properties/:id/dfi       - Deal Focus Index
GET /api/v1/agents/:id/profile       - Agent Profile
GET /api/v1/management/dashboard     - Management Dashboard
POST /api/v1/check-in                - Morning Check-in
```

---

## Key Formulas

### Propensity Score (Seller Pain)
```
Notice of Trustee Sale: +8
Notice of Default: +6
Tax Delinquency: +5
Affidavit of Death: +5
Bankruptcy: +4
High LTV (>80%): +3
Vacant: +3

TOTAL = Sum of applicable signals
Severity: 0-5 (LOW), 6-10 (MODERATE), 11-15 (HIGH), 16+ (EXTREME)
```

### Agent Classification (Whale/Dolphin/Fish)
```
Tier 1 (Whale): 10+ investor transactions
Tier 2 (Dolphin): 5-9 investor transactions
Tier 3 (Fish): 1-4 investor transactions
```

### Revenue Calculation
```
Per Operator Monthly:
- Flips: $300k × 2 × 0.5% = $3,000
- Wholesales: $25k × 6 × 10% = $15,000
- Total: $18,000

375 Operators × $18,000 = $6,750,000 MRR
```

---

## Testing Standards

### Required Tests
1. Unit tests for all business logic
2. Integration tests for API endpoints
3. E2E tests for critical user flows (check-in, offer creation)

**No A/B testing planned.**

### Critical User Flows to Test
1. AA Morning Check-in (AA1)
2. Deal Review completion (AA2)
3. Daily Outreach 30 properties (AA3)
4. Property Intelligence load (PIQ)
5. EOD Report generation (MGT2)

### Performance Benchmarks
| Operation | Target |
|-----------|--------|
| Page load | <2 sec |
| API response | <500ms |
| Bot response | <3 sec |
| DFI calculation | <1 sec |

### Scale Context
Currently 7 operators - don't worry about massive concurrent load yet.

---

## Deployment Notes

### Environment Variables Required
```
# Database
FLIPIQ_DB_URL
COMMAND_DB_URL (read-only)
REDIS_URL

# APIs
MLS_API_KEY
PROPERTYRADAR_API_KEY (via National Data microservice)
DISPOPRO_API_KEY

# Services
OPENAI_API_KEY
INTERCOM_API_KEY
SENDGRID_API_KEY
```

### Feature Flags
```
ENABLE_VOICE_INPUT - Voice commands in iQ (OpenAI Realtime)
ENABLE_TRANSCRIPTION - Call transcription (no recording)
ENABLE_AI_COACHING - AI coaching suggestions
```

---

## Phase Status

| Phase | Timeline | Focus | Status |
|-------|----------|-------|--------|
| Phase 1 | Complete | AA1-4, PIQ, D1-3 | ✅ DONE |
| Phase 2 | Dec 2024 | AA0, D4-8, MGT, Marketing, Comps | 🔄 IN PROGRESS |
| Phase 3 | Q1 2025 | SuperMaster, IARehab, C3, Personalization | 📅 PLANNED |

---

## Key Contacts

| Role | Responsibility |
|------|----------------|
| Product Owner | Feature prioritization, user stories |
| Tech Lead (Nate) | Architecture decisions, code review, UI layouts |
| CTO | Technical strategy, integration approvals |

---

*This document should be updated as new patterns emerge or constraints change.*
