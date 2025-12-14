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

### 2. Bot Architecture Pattern

**RULE:** Master bots orchestrate, specialized bots execute.

Each bot category has a "Master" bot that coordinates:
- `DMaster` → D1, D2, D3, D4, D5, D6, D7, D8, PIQ
- `MGTMaster` → MGT2, MGT3
- `MMaster` → M1, M2, M3, M4, M5
- `CMaster` → C1, C2, C3, C4
- `IAMaster` → IARehab

### 3. Deal Focus Index (DFI)

**RULE:** DFI is the primary scoring mechanism for property prioritization.

Formula: `DFI = FixerCondition + InventoryStage + SellerPainLevel + AgentBehavior`

| Component | Source | Range |
|-----------|--------|-------|
| FixerCondition | MLS keywords | 0-3 |
| InventoryStage | MLS DOM | 0-4 |
| SellerPainLevel | PropertyRadar | 0-25+ |
| AgentBehavior | Agent365 | 0-6 |

### 4. User Role Hierarchy

**RULE:** Strict role-based access control.

```
COO (1)
  └── Success Managers (15)
       └── Operators/Principals (375)
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

## Technical Patterns

### API Integration Priorities

1. **MLS** - Real-time via webhooks (already complete)
2. **PropertyRadar** - On-demand with 24hr cache (Phase 2 priority)
3. **Agent365** - 6-hour batch sync (in progress)
4. **DispoPro** - On-demand for wholesale (Phase 2)

### Caching Strategy

| Data Type | Cache Location | TTL |
|-----------|---------------|-----|
| Property data | Redis | 15 min |
| PropertyRadar | Redis | 24 hours |
| Agent data | Redis | 6 hours |
| DFI scores | Redis | Refresh daily at midnight |
| User sessions | Redis | 24 hours |

### Rate Limits

| API | Daily Limit | Strategy |
|-----|-------------|----------|
| MLS | 50,000 | Monitor only |
| PropertyRadar | 10,000 | Queue at 80%, cache aggressively |
| Agent365 | 5,000 | Batch sync only |

### Error Handling

For external API failures:
1. Retry 3 times with exponential backoff
2. Serve cached data if available
3. Display user-friendly message
4. Log error for monitoring
5. Continue with partial data if possible

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

### Agent Classification
```
Tier 1 (Whale): ISC >= 7, Active, Double-end history
Tier 2 (Dolphin): ISC 3-6, Active
Tier 3 (Fish): ISC 1-2 or Inactive
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
PROPERTYRADAR_API_KEY
AGENT365_API_KEY
DISPOPRO_API_KEY

# Services
INTERCOM_API_KEY
SENDGRID_API_KEY
```

### Feature Flags
```
ENABLE_VOICE_INPUT - Voice commands in iQ
ENABLE_CALL_RECORDING - Call recording (requires consent)
ENABLE_AI_COACHING - AI coaching suggestions
```

---

## Phase Status

| Phase | Timeline | Focus | Status |
|-------|----------|-------|--------|
| Phase 1 | Complete | AA1-4, PIQ, D1-3 | ✅ DONE |
| Phase 2 | Dec 2024 | AA0, D4-8, MGT, Marketing, Comps | 🔄 IN PROGRESS |
| Phase 3 | Q1 2025 | SuperMaster, IARehab, C3 | 📅 PLANNED |

---

## Key Contacts

| Role | Responsibility |
|------|----------------|
| Product Owner | Feature prioritization, user stories |
| Tech Lead | Architecture decisions, code review |
| CTO | Technical strategy, integration approvals |

---

*This document should be updated as new patterns emerge or constraints change.*
