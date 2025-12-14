# Story 12.2: PropertyRadar Integration

**Epic:** Epic 12 - Data Integrations
**Status:** ready-for-dev
**Priority:** P0
**Phase:** 2
**Estimated Effort:** 5 days

---

## Story

**As a** System
**I want** PropertyRadar data available on demand
**So that** distress signals are current for propensity scoring

---

## Acceptance Criteria

### AC1: On-Demand Property Lookup
```gherkin
Given an AA views a property in PIQ
When the property loads
Then PropertyRadar data is fetched within 5 seconds
And distress signals are displayed in the Propensity section
```

### AC2: Distress Signal Extraction
```gherkin
Given PropertyRadar returns data for a property
When processing the response
Then the following signals are extracted:
  | Signal | Weight | Display |
  | Notice of Trustee Sale | +8 | 🔴 NOTS: [date] |
  | Notice of Default | +6 | 🟠 NOD: [date] |
  | Tax Delinquency | +5 | 🟡 Tax Delinquent: $[amount] |
  | Affidavit of Death | +5 | ⚫ Death Record: [date] |
  | Bankruptcy | +4 | 🟣 Bankruptcy: [date] |
  | High LTV | +3 | LTV: [%] |
  | Vacant Property | +3 | 🏚️ Vacant |
And the total Pain Score is calculated
```

### AC3: Rate Limit Management
```gherkin
Given PropertyRadar allows 10,000 requests/day
When 80% of daily limit is reached
Then requests are queued instead of immediate
And cached data is served when available
And an alert is sent to system admin
```

### AC4: 24-Hour Caching
```gherkin
Given PropertyRadar data is fetched for a property
When the same property is viewed within 24 hours
Then cached data is served
And response time is <500ms
And a "Last updated: [time]" indicator shows
```

### AC5: Error Handling
```gherkin
Given PropertyRadar API is unavailable
When a property is viewed
Then a friendly error message displays
And the rest of PIQ still loads
And the system retries 3 times with exponential backoff
```

### AC6: Data Not Found Handling
```gherkin
Given PropertyRadar has no data for an APN
When the property is viewed
Then "No distress signals found" displays
And this is NOT treated as an error
And the property can still be analyzed
```

### AC7: Batch Refresh for Daily Outreach
```gherkin
Given AA3 (Daily Outreach) selects 30 properties
When prioritization runs
Then PropertyRadar data for all 30 is batch-refreshed
And stale data (>24 hours) is updated
And fresh data is used for DFI scoring
```

---

## Technical Notes

### Architecture Reference
- See `architecture.md` Section 5.2 - Integration Specifications
- PropertyRadar provides seller motivation signals for DFI calculation

### PropertyRadar API Fields
```javascript
// Key fields to extract from PropertyRadar response
{
  // Foreclosure indicators
  inForeclosure: boolean,
  ForeclosureStage: string,  // 'NOD', 'NOTS', etc.
  NoticeOfDefault: date,
  NoticeOfTrusteeSale: date,
  DefaultAmount: number,
  SaleDate: date,            // Trustee sale date

  // Financial distress
  isTaxDefaulted: boolean,
  AnnualTaxes: number,
  EstimatedEquity: number,
  LTV: number,
  TotalLoanBalance: number,

  // Life events
  AffidavitOfDeath: boolean,
  isProbate: boolean,
  Bankruptcy: boolean,
  BankruptcyDate: date,

  // Vacancy
  isSiteVacant: boolean,
  isMailVacant: boolean,

  // Contact info
  OwnerPhone: string,
  OwnerEmail: string,
  OwnerAddress: string
}
```

### Pain Score Calculation
```javascript
function calculatePainScore(prData) {
  let score = 0;

  // Foreclosure (highest weight)
  if (prData.ForeclosureStage === 'NOTS') score += 8;
  else if (prData.ForeclosureStage === 'NOD') score += 6;

  // Tax
  if (prData.isTaxDefaulted) score += 5;

  // Death/Estate
  if (prData.AffidavitOfDeath || prData.isProbate) score += 5;

  // Bankruptcy
  if (prData.Bankruptcy) score += 4;

  // High LTV (over 80%)
  if (prData.LTV > 80) score += 3;

  // Vacancy
  if (prData.isSiteVacant || prData.isMailVacant) score += 3;

  return {
    score,
    severity: score >= 11 ? 'EXTREME' :
              score >= 6 ? 'HIGH' :
              score >= 3 ? 'MODERATE' : 'LOW'
  };
}
```

### Caching Strategy
```
┌─────────────────────────────────────────────────────────────┐
│                PropertyRadar Caching Flow                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Request                                                     │
│     │                                                        │
│     ▼                                                        │
│  ┌───────────────┐                                          │
│  │ Check Redis   │                                          │
│  │ Cache         │                                          │
│  └───────┬───────┘                                          │
│          │                                                   │
│     ┌────┴────┐                                             │
│     │         │                                             │
│  HIT│      MISS│                                            │
│     ▼         ▼                                             │
│  Return    ┌──────────────┐                                 │
│  Cached    │ Check Rate   │                                 │
│  Data      │ Limit        │                                 │
│            └───────┬──────┘                                 │
│                    │                                         │
│            ┌───────┴──────┐                                 │
│            │              │                                 │
│         OK │           LIMIT│                               │
│            ▼              ▼                                 │
│     Call PropertyRadar   Queue Request                      │
│            │              │                                 │
│            ▼              ▼                                 │
│     Store in Redis    Return Stale                          │
│     (TTL: 24 hours)   or Error                              │
│            │                                                 │
│            ▼                                                 │
│     Return Fresh Data                                        │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### API Rate Limit Implementation
```javascript
// Redis-based rate limiter
const DAILY_LIMIT = 10000;
const RATE_LIMIT_KEY = `pr:rate:${date}`;

async function checkRateLimit() {
  const used = await redis.incr(RATE_LIMIT_KEY);
  if (used === 1) {
    await redis.expire(RATE_LIMIT_KEY, 86400); // 24 hours
  }

  if (used >= DAILY_LIMIT * 0.8) {
    await sendAdminAlert('PropertyRadar rate limit at 80%');
  }

  return used < DAILY_LIMIT;
}
```

---

## Tasks / Subtasks

- [ ] **Task 1: PropertyRadar API Client** (AC: 1, 2)
  - [ ] Create API client wrapper
  - [ ] Implement authentication
  - [ ] Build response parser
  - [ ] Extract all distress signals
  - [ ] Handle API errors

- [ ] **Task 2: Pain Score Calculator** (AC: 2)
  - [ ] Implement scoring algorithm
  - [ ] Apply weights per signal type
  - [ ] Calculate severity levels
  - [ ] Add score breakdown display

- [ ] **Task 3: Redis Caching Layer** (AC: 4)
  - [ ] Set up Redis cache for PR data
  - [ ] Implement 24-hour TTL
  - [ ] Add cache key by APN
  - [ ] Include "Last updated" tracking

- [ ] **Task 4: Rate Limit Management** (AC: 3)
  - [ ] Implement daily counter
  - [ ] Add 80% threshold alert
  - [ ] Build request queue for overflow
  - [ ] Serve cached data when limited

- [ ] **Task 5: Error Handling** (AC: 5, 6)
  - [ ] Implement retry with backoff
  - [ ] Handle "no data" gracefully
  - [ ] Display user-friendly messages
  - [ ] Log errors for debugging

- [ ] **Task 6: Batch Refresh** (AC: 7)
  - [ ] Build batch lookup endpoint
  - [ ] Optimize for 30 properties
  - [ ] Update stale entries only
  - [ ] Integrate with AA3 bot

- [ ] **Task 7: PIQ Integration**
  - [ ] Add Propensity section to PIQ
  - [ ] Display all distress signals
  - [ ] Show Pain Score prominently
  - [ ] Add severity indicator

- [ ] **Task 8: Testing**
  - [ ] Unit test pain score calculation
  - [ ] Test caching behavior
  - [ ] Test rate limiting
  - [ ] Test error scenarios
  - [ ] Integration test with PIQ

---

## Definition of Done

- [ ] All acceptance criteria pass
- [ ] PropertyRadar data displays in PIQ within 5 seconds
- [ ] Pain Score calculated correctly
- [ ] Caching works (24-hour TTL)
- [ ] Rate limiting prevents overuse
- [ ] Errors handled gracefully
- [ ] Unit tests written and passing
- [ ] Code reviewed and approved

---

## Dependencies

- [ ] PropertyRadar API credentials configured
- [ ] Redis instance available
- [ ] PIQ Propensity section UI ready

---

## Dev Agent Record

| Field | Value |
|-------|-------|
| Story ID | 12-2 |
| Started | |
| Completed | |
| Blockers | |
| Notes | |

---

*Generated using BMAD Method v6*
