# Story 5.1: Management Dashboard Aggregation (MGTMaster)

**Epic:** Epic 5 - Management Bots
**Status:** ready-for-dev
**Priority:** P0
**Phase:** 2
**Estimated Effort:** 5 days

---

## Story

**As a** Principal/AM
**I want** all team data aggregated in one dashboard
**So that** I can see performance at a glance

---

## Acceptance Criteria

### AC1: Role-Based Data Loading (AM)
```gherkin
Given I am logged in as an AM
When the Management Dashboard loads
Then I see metrics for all AAs assigned to me
And I do NOT see metrics for AAs in other teams
And the dashboard title shows "Team Dashboard - [My Name]"
```

### AC2: Role-Based Data Loading (Principal)
```gherkin
Given I am logged in as a Principal
When the Management Dashboard loads
Then I see company-wide metrics
And I can see all AAs across all teams
And the dashboard title shows "Company Dashboard - [Company Name]"
```

### AC3: Real-Time Data Refresh
```gherkin
Given I am viewing the Management Dashboard
When an AA updates their metrics (offer sent, call made, etc.)
Then the dashboard updates within 5 seconds
And I see a subtle refresh indicator
And existing data does not flash/flicker
```

### AC4: KPI Card Display
```gherkin
Given the dashboard loads
When viewing KPI cards
Then I see the following metrics:
  | Metric | Display |
  | Total Offers Today | Count + vs Target % |
  | Total Conversations | Count + vs Target % |
  | Total Relationships Built | Count + vs Target % |
  | Check-in Compliance | % + visual indicator |
  | Deals in Pipeline | Count + value |
And each card is color-coded (green/yellow/red) based on target achievement
```

### AC5: AA Performance Table
```gherkin
Given the dashboard loads
When viewing the AA performance table
Then I see columns:
  | Column | Description |
  | AA Name | Link to individual view |
  | Check-In | Time or "Not checked in" |
  | Offers | Count / Target |
  | Conversations | Count / Target |
  | Relationships | Count built today |
  | Status | Performance indicator |
And rows are sortable by any column
And underperforming AAs are highlighted
```

### AC6: Time Range Selection
```gherkin
Given I am viewing the dashboard
When I select a time range (Today, This Week, This Month)
Then all metrics update to reflect that range
And the selection persists during my session
```

### AC7: Performance Target Display
```gherkin
Given targets are configured
When viewing any metric
Then the target is displayed alongside actual
And % to target is calculated correctly
And visual progress bar shows achievement level
```

---

## Technical Notes

### Architecture Reference
- See `architecture.md` Section 4.2 - Bot Categories
- MGTMaster orchestrates data from AA1-AA4 and all Deal Analysis bots

### Data Sources
```
┌─────────────────────────────────────────────────────────────┐
│                   MGTMaster Data Flow                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  AA1 (Check-In) ──────┐                                     │
│  AA2 (Deal Review) ───┤                                     │
│  AA3 (Outreach) ──────┼──► MGTMaster ──► Dashboard         │
│  AA4 (Relationships) ─┤     Aggregator                      │
│  Deal Pipeline ───────┘                                     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### API Endpoints Required
```
GET /api/management/dashboard
  Query params:
    - role: 'am' | 'principal'
    - timeRange: 'today' | 'week' | 'month'
    - teamId?: string (for AM filtering)

  Response:
    {
      summary: {
        totalOffers: { actual: number, target: number },
        totalConversations: { actual: number, target: number },
        checkInRate: number,
        // ... other KPIs
      },
      aaPerformance: [
        {
          id: string,
          name: string,
          checkInTime: string | null,
          offers: { actual: number, target: number },
          conversations: { actual: number, target: number },
          relationships: number,
          status: 'on-track' | 'at-risk' | 'underperforming'
        }
      ],
      lastUpdated: string
    }
```

### Real-Time Updates
- Use WebSocket connection for live updates
- Fallback to 30-second polling if WebSocket unavailable
- Debounce UI updates to prevent flicker

### Performance Considerations
- Cache aggregated data for 30 seconds
- Use incremental updates, not full refresh
- Lazy load historical data when time range changes

---

## Tasks / Subtasks

- [ ] **Task 1: Create Dashboard Layout** (AC: 4, 5)
  - [ ] Design KPI card grid layout
  - [ ] Create AA performance table component
  - [ ] Implement responsive design
  - [ ] Add loading states

- [ ] **Task 2: Implement Role-Based Data Loading** (AC: 1, 2)
  - [ ] Create role detection logic
  - [ ] Build AM-scoped data fetching
  - [ ] Build Principal-scoped data fetching
  - [ ] Add appropriate page titles

- [ ] **Task 3: Build KPI Cards** (AC: 4, 7)
  - [ ] Create reusable KPI card component
  - [ ] Implement target comparison logic
  - [ ] Add color-coding (green/yellow/red)
  - [ ] Add progress bar visualization

- [ ] **Task 4: Build AA Performance Table** (AC: 5)
  - [ ] Create sortable table component
  - [ ] Implement column sorting
  - [ ] Add row highlighting for underperformers
  - [ ] Add AA name links

- [ ] **Task 5: Implement Real-Time Updates** (AC: 3)
  - [ ] Set up WebSocket connection
  - [ ] Implement polling fallback
  - [ ] Add refresh indicator
  - [ ] Ensure no UI flicker

- [ ] **Task 6: Time Range Selector** (AC: 6)
  - [ ] Create time range dropdown
  - [ ] Wire to API calls
  - [ ] Persist selection in session

- [ ] **Task 7: Backend API Development** (AC: all)
  - [ ] Create aggregation queries
  - [ ] Implement role-based filtering
  - [ ] Add time range filtering
  - [ ] Optimize for performance

- [ ] **Task 8: Unit & Integration Tests**
  - [ ] Test role-based visibility
  - [ ] Test real-time updates
  - [ ] Test sorting behavior
  - [ ] Test time range filtering

---

## Definition of Done

- [ ] All acceptance criteria pass
- [ ] AM sees only their team's data
- [ ] Principal sees company-wide data
- [ ] Dashboard updates in <5 seconds
- [ ] Works on tablet and desktop
- [ ] Unit tests written and passing
- [ ] Code reviewed and approved

---

## Dev Agent Record

| Field | Value |
|-------|-------|
| Story ID | 5-1 |
| Started | |
| Completed | |
| Blockers | |
| Notes | |

---

*Generated using BMAD Method v6*
