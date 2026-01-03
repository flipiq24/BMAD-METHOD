# My Stats Bot — User Stories
## Version 2.0 — BMAD iQ
### January 2, 2025

---

## Document Overview

| Field | Value |
|-------|-------|
| **Total Epics** | 5 |
| **Total User Stories** | 24 |
| **Primary Users** | AA (Self-Coaching), AM (Assessment) |
| **Dependencies** | AA1-AA4, Dialpad, Post Call Bot, Offer Status KPIs |

---

## Epic Overview

| # | Epic | Stories | Focus |
|---|------|---------|-------|
| 1 | AA Real-Time Dashboard | 6 | Metrics display, progress tracking |
| 2 | Proactive Alerts | 4 | Mid-day nudges, EOD coaching |
| 3 | Management Dashboard | 6 | Classification, team overview |
| 4 | Strike System | 4 | Accountability, documentation |
| 5 | Pattern Detection & Coaching | 4 | Intervention triggers |

---

## Core Philosophy Reminder

> **The Braces Approach**: Consistent gentle pressure to correct patterns over time—not punishment.
>
> | WE DO | WE DON'T |
> |-------|----------|
> | Acknowledge time invested | Shame for not hitting numbers |
> | Realistic next steps | Push last place to #1 overnight |
> | Catch patterns early | Wait until failure |

---

## Epic 1: AA Real-Time Dashboard

**Goal:** Enable AAs to track their own performance in real-time with clear visibility to daily goals.

---

### US-1.1: Daily Metrics Display

**As an** AA starting my day
**I want to** see all my key metrics displayed on one dashboard
**So that** I know exactly what I need to accomplish today

**Acceptance Criteria:**
- [ ] Dashboard displays all 9 core metrics
- [ ] Each metric shows: current value, daily goal, weekly goal
- [ ] Progress bar for each metric (0-100%)
- [ ] Color coding: Green (≥80%), Yellow (50-79%), Red (<50%)
- [ ] Dashboard loads in < 2 seconds

**Data Structure:**
```typescript
interface DailyMetrics {
  aa_id: string;
  date: Date;
  metrics: {
    calls_made: { current: number; daily_goal: 30; weekly_goal: 150 };
    conversations: { current: number; daily_goal: 10; weekly_goal: 50 };
    new_relationships: { current: number; daily_goal: 5; weekly_goal: 25 };
    offers_sent: { current: number; daily_goal: 5; weekly_goal: 25 };
    time_invested: { current: number; daily_goal: 8; weekly_goal: 40 }; // hours
    daily_review_complete: boolean;
    daily_outreach_complete: boolean;
    campaigns_sent: { current: number; daily_goal: 3; weekly_goal: 15 };
    piq_properties: { current: number; daily_goal: 30; weekly_goal: 150 };
  };
  last_updated: Date;
}
```

---

### US-1.2: Progress Visualization

**As an** AA checking my progress
**I want to** see visual progress bars for each metric
**So that** I can quickly assess where I stand at a glance

**Acceptance Criteria:**
- [ ] Progress bars fill proportionally (current/goal)
- [ ] Bars exceed 100% when goal surpassed (capped at 150% display)
- [ ] Hover/tap shows exact numbers
- [ ] Weekly progress shows below daily progress

---

### US-1.3: Historical Trend View

**As an** AA reviewing my performance
**I want to** see my metrics trend over the past 7 days
**So that** I can identify patterns in my performance

**Acceptance Criteria:**
- [ ] Mini sparkline charts for each metric
- [ ] 7-day rolling view
- [ ] Trend indicator: ↑ improving, ↓ declining, → stable
- [ ] Click to expand to full chart view

---

### US-1.4: Time Investment Tracking

**As an** AA who committed to a full day
**I want to** see my hours invested vs. hours committed
**So that** my performance is evaluated in context

**Acceptance Criteria:**
- [ ] Hours tracked from check-in to last activity
- [ ] Display: "6.5 hrs invested of 8 hrs committed"
- [ ] Percentage displayed: "81% of committed time"
- [ ] If partial day committed, goals adjust proportionally

---

### US-1.5: Task Completion Status

**As an** AA managing my daily tasks
**I want to** see which required tasks are complete
**So that** I don't miss mandatory activities

**Acceptance Criteria:**
- [ ] Daily Review (AA2): Complete / Not Complete
- [ ] Daily Outreach (AA3): Complete / Not Complete
- [ ] Check-in (AA1): Time shown
- [ ] Visual checkmarks for completed tasks

---

### US-1.6: Comparison to Personal History

**As an** AA measuring my improvement
**I want to** see how today compares to my personal average
**So that** I'm competing against myself, not teammates

**Acceptance Criteria:**
- [ ] "Today vs Your Average" comparison
- [ ] Shows: +15% above your average calls
- [ ] Does NOT show team rankings by default
- [ ] Encouraging tone for above-average days

---

## Epic 2: Proactive Alerts

**Goal:** Provide supportive mid-day nudges to help AAs course-correct before EOD.

---

### US-2.1: Noon Check-In Alert

**As an** AA who is off-pace at noon
**I want to** receive a supportive reminder with specific actions
**So that** I can adjust my afternoon approach

**Acceptance Criteria:**
- [ ] Triggers if calls < 15 by 12:00 PM
- [ ] Tone is supportive, not punitive
- [ ] Includes specific action: "~66 minutes of calling time needed"
- [ ] Offers help: "Talk to iQ or your AM"
- [ ] Dismissible with one tap

**Example Alert:**
```
Hey Tony! Quick check-in—it's noon and you've made 8 calls.
To hit 30 by EOD, you'll need ~66 minutes of calling time.

Quick tip: Front-load calls after lunch while agents are available.
Need help? Talk to iQ or your AM!

[Got it] [Talk to AM]
```

---

### US-2.2: 3 PM Recovery Alert

**As an** AA significantly behind at 3 PM
**I want to** see my options for the remaining time
**So that** I can make an informed decision about my afternoon

**Acceptance Criteria:**
- [ ] Triggers if calls < 20 by 3:00 PM
- [ ] Presents 3 options (not demands)
- [ ] Respects AA autonomy
- [ ] Links to AM chat if needed

**Example Alert:**
```
Tony, you're at 15 calls with 2 hours left.

Options:
(1) Power through—15 more calls in 2 hours
(2) Prioritize quality—finish Priority Agent callbacks
(3) Talk to your AM—adjust tomorrow's plan

What works?

[Power Through] [Prioritize Quality] [Talk to AM]
```

---

### US-2.3: End-of-Day Summary

**As an** AA finishing my day
**I want to** see a contextual summary of my performance
**So that** I understand how I did relative to my time investment

**Acceptance Criteria:**
- [ ] Triggers at 5 PM or when AA logs out
- [ ] Acknowledges hours invested
- [ ] Highlights wins (metrics exceeded)
- [ ] Provides ONE specific action for tomorrow
- [ ] Tone matches performance (celebratory for wins, supportive for misses)

**Example (Partial Day):**
```
Great work today, Tony!

📊 Your Day (6.5 hours invested):
✓ 22 calls (73% of goal—great for 6.5 hrs!)
✓ 6 conversations (60% of goal)
✓ 3 offers sent (on target!)

💡 Tomorrow's Focus:
Front-load calls before 11 AM—your conversation rate
is highest in the morning.

You're doing great for your time invested.
Talk to your AM if you need to adjust hours.

[See Full Stats] [Close]
```

---

### US-2.4: Alert Preferences

**As an** AA who wants to customize alerts
**I want to** set my alert preferences
**So that** I receive nudges in my preferred way

**Acceptance Criteria:**
- [ ] Toggle: Noon alert on/off
- [ ] Toggle: 3 PM alert on/off
- [ ] Channel: In-app only / Include push notification
- [ ] Max 2 alerts per day (prevent fatigue)
- [ ] AM can override to require alerts for C-Players

---

## Epic 3: Management Dashboard

**Goal:** Give AMs 10-second answers to critical questions about their team.

---

### US-3.1: Team Overview

**As an** AM viewing my team
**I want to** see all AAs with their classification at a glance
**So that** I know where to focus my attention

**Acceptance Criteria:**
- [ ] All AAs displayed in classification groups (A/B/C)
- [ ] Each AA shows: name, deals MTD, compliance %, hours/day avg
- [ ] Visual distinction: A=Green, B=Yellow, C=Red
- [ ] Count per classification displayed

---

### US-3.2: AA Classification Logic

**As an** AM assessing an AA
**I want to** understand why they're classified as A/B/C
**So that** the classification is transparent and defensible

**Acceptance Criteria:**
- [ ] Classification criteria displayed on hover/click
- [ ] A-Player: 2+ deals/month, 95%+ compliance, 7.5+ hrs/day
- [ ] B-Player: 1+ deals/month, 80%+ compliance, improving
- [ ] C-Player: <0.5 deals, <70% compliance, not improving
- [ ] "Improving" indicator based on 7-day trend

**Classification Schema:**
```typescript
interface AAClassification {
  aa_id: string;
  classification: 'A_PLAYER' | 'B_PLAYER' | 'C_PLAYER';
  criteria_met: {
    deals_mtd: number;
    compliance_rate: number;
    avg_hours_per_day: number;
    is_improving: boolean;
    strike_count: number;
  };
  calculated_at: Date;
  classification_reason: string;
}
```

---

### US-3.3: Quick Question Filters

**As an** AM with limited time
**I want to** filter AAs by specific questions
**So that** I get answers in 10 seconds

**Acceptance Criteria:**
- [ ] Filter: "Who should I fire?" → C-Players with 2+ strikes
- [ ] Filter: "Who to coach?" → B-Players improving
- [ ] Filter: "Who to promote?" → A-Players 90+ days tenure
- [ ] Filter: "Process non-compliance?" → <70% task completion
- [ ] Filter results update instantly

---

### US-3.4: AA Detail Card

**As an** AM drilling into one AA
**I want to** see detailed metrics and history
**So that** I have full context for 1:1 conversations

**Acceptance Criteria:**
- [ ] Full metrics table (all 9 metrics, 7-day history)
- [ ] Strike history with dates and triggers
- [ ] Trend charts for key metrics
- [ ] Coaching notes from previous sessions
- [ ] Quick actions: [Schedule 1:1] [Add Note] [View Calls]

---

### US-3.5: Team Comparison View

**As an** AM comparing AA performance
**I want to** see a side-by-side comparison table
**So that** I can identify coaching patterns across the team

**Acceptance Criteria:**
- [ ] Table view with all AAs as rows
- [ ] Sortable by any metric column
- [ ] Highlight top/bottom performers per metric
- [ ] Export to Google Sheets

---

### US-3.6: MTD Progress Tracking

**As an** AM tracking monthly goals
**I want to** see team progress toward 8 deals/month (2 per AA)
**So that** I know if we're on pace

**Acceptance Criteria:**
- [ ] Team deals closed MTD vs goal
- [ ] Days remaining in month
- [ ] "On pace" / "Behind pace" indicator
- [ ] Per-AA deals MTD breakdown

---

## Epic 4: Strike System

**Goal:** Document accountability with transparent, fair tracking.

---

### US-4.1: Strike Triggers

**As a** System
**I want to** detect strike conditions automatically
**So that** accountability is consistent and unbiased

**Acceptance Criteria:**
- [ ] Missed check-in: No AA1 by 10 AM without notice
- [ ] Zero production: 0 offers on full-day commitment
- [ ] 3-day minimum miss: <3 offers for 3 consecutive days
- [ ] Process non-compliance: <70% AA2/AA3 completion for 3 days

**Strike Schema:**
```typescript
interface Strike {
  strike_id: string;
  aa_id: string;
  strike_number: 1 | 2 | 3;
  trigger: 'missed_checkin' | 'zero_production' | 'below_minimum' | 'non_compliance';
  trigger_date: Date;
  details: string;
  am_notified: boolean;
  am_action_taken: string | null;
  resolved: boolean;
}
```

---

### US-4.2: Strike History View

**As an** AM reviewing an AA's accountability
**I want to** see their complete strike history
**So that** I have documentation for exit conversations

**Acceptance Criteria:**
- [ ] Chronological strike list
- [ ] Each strike shows: date, trigger, details
- [ ] AM notes attached to each strike
- [ ] Exportable for HR documentation

---

### US-4.3: AM Strike Notification

**As an** AM responsible for my team
**I want to** be notified immediately when a strike occurs
**So that** I can take timely action

**Acceptance Criteria:**
- [ ] Push notification on strike trigger
- [ ] In-app alert badge
- [ ] Email notification option
- [ ] Quick link to AA detail card

---

### US-4.4: Strike Visibility (AA Side)

**As an** AA subject to accountability
**I want to** know my strike status
**So that** I understand where I stand

**Acceptance Criteria:**
- [ ] Strike count visible in AA dashboard
- [ ] Strike details shown (what triggered it)
- [ ] Clear path to resolve: "Talk to your AM"
- [ ] Encouraging tone: "Let's course-correct together"

---

## Epic 5: Pattern Detection & Coaching

**Goal:** Identify performance patterns and trigger appropriate interventions.

---

### US-5.1: Performance Pattern Detection

**As a** System
**I want to** detect patterns in AA metrics
**So that** I can suggest targeted coaching

**Acceptance Criteria:**
- [ ] Pattern: High calls, low conversations → Script issue
- [ ] Pattern: High conversations, low offers → Ask weakness
- [ ] Pattern: High offers, low negotiations → Follow-up gap
- [ ] Pattern detection runs at EOD

**Pattern Schema:**
```typescript
interface PerformancePattern {
  aa_id: string;
  pattern_type: 'script_issue' | 'ask_weakness' | 'followup_gap' | 'time_management';
  evidence: {
    metric_1: { name: string; value: number; benchmark: number };
    metric_2: { name: string; value: number; benchmark: number };
  };
  detected_at: Date;
  intervention_suggested: string;
}
```

---

### US-5.2: Coaching Intervention Mapping

**As an** AM seeing a detected pattern
**I want to** know the recommended intervention
**So that** my coaching is targeted and effective

**Acceptance Criteria:**
- [ ] Script issue → Assign Practice Mode (PC1)
- [ ] Ask weakness → Review offer training materials
- [ ] Follow-up gap → Emphasize AA3 Outreach
- [ ] Time management → Adjust daily schedule

---

### US-5.3: Coaching Suggestion Display

**As an** AM coaching an AA
**I want to** see AI-suggested coaching points
**So that** I don't have to analyze data manually

**Acceptance Criteria:**
- [ ] "Suggested Focus" section in AA detail card
- [ ] Pattern explanation in plain language
- [ ] Recommended action with rationale
- [ ] Link to relevant training/practice resources

**Example Display:**
```
💡 COACHING SUGGESTION — Josh Smith

Pattern Detected: High calls (32/day avg), low conversations (5/day avg)

Diagnosis: Likely script delivery or opening issue. Josh is
making the calls but not engaging agents effectively.

Recommended Action:
1. Review call recordings for opening delivery
2. Assign 3 Practice Mode sessions (PC1)
3. Role-play opening script in next 1:1

[Assign Practice] [Schedule 1:1] [View Call Recordings]
```

---

### US-5.4: Improvement Tracking

**As an** AM monitoring coaching effectiveness
**I want to** see if my coaching is working
**So that** I can adjust my approach

**Acceptance Criteria:**
- [ ] "Before coaching" baseline metrics saved
- [ ] "After coaching" metrics compared
- [ ] Improvement percentage displayed
- [ ] Time since intervention shown
- [ ] Success: Pattern resolved indicator

---

## Data Schemas

### AA Performance Record

```typescript
interface AAPerformance {
  aa_id: string;
  aa_name: string;
  date: Date;

  // Daily Metrics
  calls_made: number;
  conversations: number;
  conversation_rate: number;
  new_relationships: number;
  offers_sent: number;
  offer_rate: number;

  // Time
  check_in_time: Date | null;
  hours_committed: number;
  hours_invested: number;

  // Task Completion
  daily_review_complete: boolean;
  daily_outreach_complete: boolean;
  compliance_rate: number;

  // Calculated
  classification: 'A_PLAYER' | 'B_PLAYER' | 'C_PLAYER';
  strike_count: number;
  is_improving: boolean;
  patterns_detected: string[];
}
```

### Team Summary

```typescript
interface TeamSummary {
  date: Date;
  team_size: number;

  classification_counts: {
    a_players: number;
    b_players: number;
    c_players: number;
  };

  deals_mtd: number;
  deals_goal: number;
  on_pace: boolean;

  alerts: {
    strikes_today: number;
    coaching_needed: string[];
    top_performer: string;
    needs_attention: string[];
  };
}
```

---

## Story Dependencies

```
US-1.1 → US-1.2 → US-1.3 (Metrics → Progress → Trends)
US-1.4 + US-1.5 → US-1.6 (Time + Tasks → Comparison)

US-2.1 + US-2.2 → US-2.3 (Noon + 3PM → EOD)
US-2.4 controls US-2.1, US-2.2

US-3.1 → US-3.2 (Overview → Classification)
US-3.3 filters US-3.1 results
US-3.4 expands from US-3.1 cards

US-4.1 → US-4.2 + US-4.3 (Detection → History + Notification)
US-4.4 displays from US-4.1 data

US-5.1 → US-5.2 → US-5.3 (Detection → Mapping → Display)
US-5.4 measures US-5.2 effectiveness
```

---

## Acceptance Testing Checklist

- [ ] AA Dashboard displays all 9 metrics with progress bars
- [ ] Color coding correct (Green/Yellow/Red)
- [ ] Noon alert triggers at 12 PM if off-pace
- [ ] 3 PM alert triggers with 3 options
- [ ] EOD summary acknowledges hours invested
- [ ] A/B/C classification displays correctly
- [ ] Strike triggers fire on conditions
- [ ] AM notified immediately on strike
- [ ] Pattern detection identifies script issues
- [ ] Coaching suggestions display with actions
- [ ] Export to Google Sheets works

---

**Document Version:** 2.0
**Last Updated:** January 2, 2025
**Status:** Ready for Development
