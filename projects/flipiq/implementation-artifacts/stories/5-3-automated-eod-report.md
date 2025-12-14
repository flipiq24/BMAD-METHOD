# Story 5.3: Automated 5PM EOD Report (MGT2)

**Epic:** Epic 5 - Management Bots
**Status:** ready-for-dev
**Priority:** P0
**Phase:** 2
**Estimated Effort:** 4 days

---

## Story

**As a** Principal/AM
**I want** an automated end-of-day report at 5PM
**So that** I can review daily performance without manual compilation

---

## Acceptance Criteria

### AC1: Scheduled 5PM Trigger
```gherkin
Given it is 5:00 PM in the operator's timezone
When the MGT2 scheduler runs
Then an EOD report is generated for that operator
And the generation completes within 60 seconds
```

### AC2: Report Email Delivery
```gherkin
Given EOD report generates successfully
When complete
Then email is sent to:
  - All Principals for the operator
  - All AMs for the operator
And email subject is "FlipIQ Daily Report - [Date] - [Company Name]"
And email arrives within 5 minutes of generation
```

### AC3: KPI vs Target Display
```gherkin
Given the report is viewed
When looking at KPIs
Then I see:
  | Metric | Format |
  | Total Offers | Actual / Target (% achieved) |
  | Total Conversations | Actual / Target (% achieved) |
  | New Relationships | Actual / Target (% achieved) |
  | Check-in Compliance | % (# checked in / total) |
  | Properties Reviewed | Count |
  | Pipeline Value | $ amount |
And metrics meeting/exceeding target are marked green
And metrics below target are marked red
```

### AC4: Individual AA Performance
```gherkin
Given the report is viewed
When looking at AA section
Then I see each AA with:
  | Field | Description |
  | Name | AA name |
  | Check-in Time | Time or "Not checked in" |
  | Offers | Count with target % |
  | Conversations | Count with target % |
  | Relationships | Count built |
  | Status | "On Track" / "At Risk" / "Below Target" |
And AAs are sorted by performance (highest to lowest)
And struggling AAs are highlighted
```

### AC5: Team Leaderboard
```gherkin
Given the report is viewed
When looking at leaderboard section
Then I see AAs ranked 1-N by composite score
And top 3 performers have trophy icons (🥇🥈🥉)
And composite score calculation is shown
```

### AC6: Coaching Suggestions
```gherkin
Given the report is generated
When AI analyzes performance patterns
Then specific coaching suggestions appear for AAs who:
  - Missed check-in (suggest accountability talk)
  - Low offers (suggest call quality review)
  - High calls but low conversations (suggest script coaching)
And suggestions are actionable and specific
```

### AC7: Tomorrow's Priorities
```gherkin
Given the report is generated
When viewing priorities section
Then I see auto-generated list including:
  - Follow-up reminders due tomorrow
  - Critical properties requiring attention
  - Pending offers needing updates
And items are sorted by urgency
```

### AC8: Report Viewing in Dashboard
```gherkin
Given I am logged in as Principal/AM
When I navigate to Reports > Daily Reports
Then I see a list of all past EOD reports
And I can click any date to view that report
And reports are stored for 90 days
```

---

## Technical Notes

### Architecture Reference
- See `architecture.md` Section 4.2 - Management Bots (MGT2)
- MGT2 aggregates data from all daily bots and generates coaching insights

### Report Structure
```
┌─────────────────────────────────────────────────────────────┐
│         FlipIQ Daily Report - December 13, 2025             │
│                   [Company Name]                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  📊 DAILY SUMMARY                                           │
│  ┌──────────────┬─────────────┬──────────────┐              │
│  │   Offers     │ Conversations│ Relationships│              │
│  │   42/40 ✓    │   251/240 ✓  │   43/40 ✓    │              │
│  │   (105%)     │   (104%)     │   (107%)     │              │
│  └──────────────┴─────────────┴──────────────┘              │
│                                                              │
│  👥 INDIVIDUAL PERFORMANCE                                  │
│  ┌───────────────────────────────────────────────────────┐  │
│  │ 🏆 Maria    | 7 offers | 42 calls | CRUSHING IT      │  │
│  │ ✅ Mike     | 6 offers | 38 calls | Solid            │  │
│  │ ⚠️ Josh     | 0 offers | 0 calls  | TERMINATE?       │  │
│  └───────────────────────────────────────────────────────┘  │
│                                                              │
│  💡 AI COACHING SUGGESTIONS                                 │
│  • Maria: Ready for team lead role                          │
│  • Josh: Not following process - recommend termination      │
│                                                              │
│  📋 TOMORROW'S PRIORITIES                                   │
│  • Follow up on 3 pending offers                            │
│  • Call back 2 hot agents                                   │
│  • Review 4 new MLS properties                              │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Scheduled Job Implementation
```javascript
// Cron expression for 5PM in each timezone
// Requires timezone-aware scheduler (e.g., Bull with timezone support)

{
  "schedule": "0 17 * * *", // 5PM daily
  "timezone": "operator.timezone", // Each operator's local time
  "job": "generateEODReport",
  "params": {
    "operatorId": "string"
  }
}
```

### Email Template
- Use responsive HTML email template
- Include plain text fallback
- Optimize for mobile email clients
- Include "View in Browser" link

### AI Coaching Logic
```
IF aa.checkIn === null THEN
  suggestion = "Missed check-in. Schedule accountability conversation."

IF aa.offers < target * 0.5 AND aa.calls > target * 0.8 THEN
  suggestion = "High activity, low output. Review call quality."

IF aa.conversations / aa.calls < 0.3 THEN
  suggestion = "Low conversation rate. Coach on script delivery."

IF aa.offers >= target * 1.2 THEN
  suggestion = "Top performer. Consider for mentorship role."
```

---

## Tasks / Subtasks

- [ ] **Task 1: Build Report Data Aggregator** (AC: 3, 4, 5)
  - [ ] Create query to aggregate daily metrics
  - [ ] Calculate target percentages
  - [ ] Implement AA ranking logic
  - [ ] Add composite score calculation

- [ ] **Task 2: Implement 5PM Scheduler** (AC: 1)
  - [ ] Set up timezone-aware job scheduler
  - [ ] Create job for each operator's timezone
  - [ ] Add monitoring for job execution
  - [ ] Implement retry on failure

- [ ] **Task 3: Create Report Template** (AC: 3, 4, 5)
  - [ ] Design HTML email template
  - [ ] Create plain text version
  - [ ] Add responsive styling
  - [ ] Include all required sections

- [ ] **Task 4: Email Delivery** (AC: 2)
  - [ ] Integrate email service (SendGrid/SES)
  - [ ] Build recipient list (Principals + AMs)
  - [ ] Add delivery tracking
  - [ ] Handle bounce/failure

- [ ] **Task 5: AI Coaching Engine** (AC: 6)
  - [ ] Define coaching rule set
  - [ ] Implement pattern detection
  - [ ] Generate actionable suggestions
  - [ ] Add severity levels

- [ ] **Task 6: Tomorrow's Priorities Generator** (AC: 7)
  - [ ] Query upcoming reminders
  - [ ] Identify critical properties
  - [ ] Find pending offers
  - [ ] Sort by urgency

- [ ] **Task 7: Report Archive & Viewer** (AC: 8)
  - [ ] Store reports in database
  - [ ] Build report list view
  - [ ] Create report detail view
  - [ ] Implement 90-day retention

- [ ] **Task 8: Testing**
  - [ ] Test scheduler across timezones
  - [ ] Test email delivery
  - [ ] Test coaching suggestions
  - [ ] Test report rendering

---

## Definition of Done

- [ ] All acceptance criteria pass
- [ ] Report generates at 5PM in operator's timezone
- [ ] Email delivered to all Principals and AMs
- [ ] Coaching suggestions are relevant and actionable
- [ ] Reports viewable in dashboard for 90 days
- [ ] Unit tests written and passing
- [ ] Code reviewed and approved

---

## Dev Agent Record

| Field | Value |
|-------|-------|
| Story ID | 5-3 |
| Started | |
| Completed | |
| Blockers | |
| Notes | |

---

*Generated using BMAD Method v6*
