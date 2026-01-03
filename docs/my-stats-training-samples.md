# My Stats Bot — Training Samples
## Version 2.0 — BMAD iQ
### January 2, 2025

---

## Document Overview

| Field | Value |
|-------|-------|
| **Bot ID** | MGT3 |
| **Total Samples** | 12 |
| **Coverage** | All 5 Epics |
| **Purpose** | AI training data for performance coaching, alerts, classification, and pattern detection |

---

## Sample Categories

| # | Category | Samples | Focus |
|---|----------|---------|-------|
| 1-3 | AA Dashboard | 3 | Metrics display, progress visualization |
| 4-5 | Proactive Alerts | 2 | Noon nudge, 3 PM recovery |
| 6-7 | End-of-Day Coaching | 2 | Full day, partial day |
| 8-9 | Management Classification | 2 | A-Player, C-Player |
| 10-11 | Strike System | 2 | Strike trigger, strike history |
| 12 | Pattern Detection | 1 | Coaching suggestion |

---

## Core Philosophy Reminder

> **The Braces Approach**: All training samples must demonstrate supportive coaching, not punishment.
>
> ✓ "You're doing great for 6 hours"
> ✗ "You're failing because you only worked 6 hours"

---

## Sample 1: AA Dashboard — Full Day On-Pace

### Context

**Scenario:** AA is on track at 2 PM, full day committed

**AA:** Tony Martinez
**Check-in Time:** 7:15 AM
**Hours Committed:** 8 hours
**Current Time:** 2:00 PM

### Input — AA Performance Data

```json
{
  "aa_id": "aa_tony_001",
  "aa_name": "Tony Martinez",
  "date": "2025-01-02",
  "check_in_time": "2025-01-02T07:15:00Z",
  "hours_committed": 8,
  "hours_invested": 6.75,
  "current_time": "2025-01-02T14:00:00Z",

  "metrics": {
    "calls_made": { "current": 22, "daily_goal": 30 },
    "conversations": { "current": 7, "daily_goal": 10 },
    "new_relationships": { "current": 3, "daily_goal": 5 },
    "offers_sent": { "current": 4, "daily_goal": 5 },
    "daily_review_complete": true,
    "daily_outreach_complete": false,
    "campaigns_sent": { "current": 2, "daily_goal": 3 },
    "piq_properties": { "current": 25, "daily_goal": 30 }
  }
}
```

### Expected Output — Dashboard Display

```
┌─────────────────────────────────────────────────────────────────┐
│ 📊 MY STATS — Tony Martinez                    January 2, 2025  │
│ ✓ Checked in at 7:15 AM | 6.75 hrs invested of 8 hrs committed │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ CALLS MADE                         22 of 30                      │
│ [████████████████████░░░░░░] 73% ✓ On pace                      │
│                                                                  │
│ CONVERSATIONS                       7 of 10                      │
│ [██████████████████░░░░░░░░] 70% ✓ On pace                      │
│                                                                  │
│ NEW RELATIONSHIPS                   3 of 5                       │
│ [████████████████░░░░░░░░░░] 60%   At risk                      │
│                                                                  │
│ OFFERS SENT                         4 of 5                       │
│ [████████████████████████░░] 80% ✓ On pace                      │
│                                                                  │
│ CAMPAIGNS SENT                      2 of 3                       │
│ [██████████████████░░░░░░░░] 67% ✓ On pace                      │
│                                                                  │
│ PIQ PROPERTIES                      25 of 30                     │
│ [████████████████████████░░] 83% ✓ On pace                      │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│ ✅ TASKS                                                         │
│ ✓ Daily Review (AA2) — Complete                                 │
│ ○ Daily Outreach (AA3) — In Progress                            │
│ ○ Campaigns — 1 more to go                                      │
├─────────────────────────────────────────────────────────────────┤
│ 📈 vs Your Average: +12% calls | +8% conversations | On track!  │
│                                                    Last: 2:00 PM│
└─────────────────────────────────────────────────────────────────┘
```

### Validation Criteria

- [ ] All 6 metrics displayed with progress bars
- [ ] Percentage calculated correctly (current/goal)
- [ ] Color coding: Green (≥80%), Yellow (50-79%), Red (<50%)
- [ ] Hours invested shows correctly
- [ ] "On pace" / "At risk" labels accurate

---

## Sample 2: AA Dashboard — Partial Day Behind

### Context

**Scenario:** AA committed to half day (4 hours), behind pace

**AA:** Sarah Lopez
**Hours Committed:** 4 hours
**Current Time:** 1:00 PM

### Input — AA Performance Data

```json
{
  "aa_id": "aa_sarah_001",
  "aa_name": "Sarah Lopez",
  "date": "2025-01-02",
  "check_in_time": "2025-01-02T09:00:00Z",
  "hours_committed": 4,
  "hours_invested": 4.0,
  "current_time": "2025-01-02T13:00:00Z",

  "metrics": {
    "calls_made": { "current": 8, "daily_goal": 15 },
    "conversations": { "current": 2, "daily_goal": 5 },
    "new_relationships": { "current": 1, "daily_goal": 2 },
    "offers_sent": { "current": 1, "daily_goal": 2 },
    "daily_review_complete": true,
    "daily_outreach_complete": false,
    "campaigns_sent": { "current": 1, "daily_goal": 2 },
    "piq_properties": { "current": 10, "daily_goal": 15 }
  },

  "adjusted_goals_note": "Goals adjusted for 4-hour commitment (50% of full day)"
}
```

### Expected Output — Dashboard Display

```
┌─────────────────────────────────────────────────────────────────┐
│ 📊 MY STATS — Sarah Lopez                      January 2, 2025  │
│ ✓ Checked in at 9:00 AM | Half Day (4 hrs committed)           │
├─────────────────────────────────────────────────────────────────┤
│ ⚠️ Goals adjusted for half-day commitment                       │
│                                                                  │
│ CALLS MADE                          8 of 15                      │
│ [████████████░░░░░░░░░░░░░░] 53%   Behind pace                  │
│                                                                  │
│ CONVERSATIONS                       2 of 5                       │
│ [████████░░░░░░░░░░░░░░░░░░] 40% ⚠️ At risk                     │
│                                                                  │
│ OFFERS SENT                         1 of 2                       │
│ [████████████████░░░░░░░░░░] 50%   Close!                       │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│ 💡 TIP: 7 more calls to hit your half-day goal.                 │
│    Lunch agents are available—front-load afternoon calls.       │
└─────────────────────────────────────────────────────────────────┘
```

### Validation Criteria

- [ ] Goals adjusted for half-day (50%)
- [ ] Note explains adjustment
- [ ] Behind pace indicators show (not punitive)
- [ ] Tip provides specific action

---

## Sample 3: AA Dashboard — Exceeding Goals

### Context

**Scenario:** Top performer exceeding daily goals

**AA:** Josh Smith
**Current Time:** 4:00 PM

### Input — AA Performance Data

```json
{
  "aa_id": "aa_josh_001",
  "aa_name": "Josh Smith",
  "date": "2025-01-02",
  "hours_committed": 8,
  "hours_invested": 8.75,

  "metrics": {
    "calls_made": { "current": 38, "daily_goal": 30 },
    "conversations": { "current": 12, "daily_goal": 10 },
    "new_relationships": { "current": 6, "daily_goal": 5 },
    "offers_sent": { "current": 7, "daily_goal": 5 },
    "daily_review_complete": true,
    "daily_outreach_complete": true,
    "campaigns_sent": { "current": 4, "daily_goal": 3 },
    "piq_properties": { "current": 42, "daily_goal": 30 }
  }
}
```

### Expected Output — Dashboard Display

```
┌─────────────────────────────────────────────────────────────────┐
│ 📊 MY STATS — Josh Smith                       January 2, 2025  │
│ ⭐ CRUSHING IT TODAY!                                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ CALLS MADE                         38 of 30                      │
│ [██████████████████████████████] 127% 🔥 Goal smashed!          │
│                                                                  │
│ CONVERSATIONS                      12 of 10                      │
│ [██████████████████████████████] 120% 🔥 Goal smashed!          │
│                                                                  │
│ OFFERS SENT                         7 of 5                       │
│ [██████████████████████████████] 140% 🔥 Goal smashed!          │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│ ✅ ALL TASKS COMPLETE                                            │
│ ✓ Daily Review | ✓ Daily Outreach | ✓ Campaigns                 │
├─────────────────────────────────────────────────────────────────┤
│ 🏆 vs Your Average: +25% above your typical day                 │
│ You're in the zone—keep this momentum for the week!             │
└─────────────────────────────────────────────────────────────────┘
```

### Validation Criteria

- [ ] Progress bars show >100% (capped at 150% width)
- [ ] Celebratory tone and messaging
- [ ] "Goal smashed" indicators
- [ ] Comparison to personal average (not team)

---

## Sample 4: Proactive Alert — Noon Nudge

### Context

**Scenario:** AA is significantly behind at noon, supportive reminder needed

**AA:** Tony Martinez
**Current Calls:** 8 of 30
**Time:** 12:00 PM

### Input — Alert Trigger Data

```json
{
  "aa_id": "aa_tony_001",
  "aa_name": "Tony",
  "current_time": "2025-01-02T12:00:00Z",
  "trigger": "noon_check",
  "calls_made": 8,
  "calls_goal": 30,
  "calls_remaining": 22,
  "hours_remaining": 5,
  "avg_calls_per_hour": 6.5,
  "minutes_of_calling_needed": 66
}
```

### Expected Output — Noon Alert

```
┌─────────────────────────────────────────────────────────────────┐
│ 👋 Hey Tony! Quick check-in                                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ It's noon and you've made 8 calls.                              │
│ To hit 30 by EOD, you'll need ~66 minutes of calling time.      │
│                                                                  │
│ 💡 Quick tip: Front-load calls after lunch while agents          │
│    are available. The 1-3 PM window is peak response time.      │
│                                                                  │
│ Need help? Talk to iQ or your AM!                               │
│                                                                  │
│              [Got it]              [Talk to AM]                 │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Validation Criteria

- [ ] Tone is supportive, not punitive
- [ ] Specific number provided (66 minutes)
- [ ] Actionable tip included
- [ ] Help options offered
- [ ] Easy to dismiss

---

## Sample 5: Proactive Alert — 3 PM Recovery

### Context

**Scenario:** AA is significantly behind at 3 PM, present options

**AA:** Tony Martinez
**Current Calls:** 15 of 30
**Time:** 3:00 PM

### Input — Alert Trigger Data

```json
{
  "aa_id": "aa_tony_001",
  "aa_name": "Tony",
  "current_time": "2025-01-02T15:00:00Z",
  "trigger": "three_pm_recovery",
  "calls_made": 15,
  "calls_goal": 30,
  "calls_remaining": 15,
  "hours_remaining": 2,
  "priority_callbacks_pending": 4
}
```

### Expected Output — 3 PM Alert

```
┌─────────────────────────────────────────────────────────────────┐
│ ⏰ Tony, quick check-in at 3 PM                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ You're at 15 calls with 2 hours left.                           │
│                                                                  │
│ Options:                                                         │
│                                                                  │
│ 1️⃣ Power through                                                │
│    15 more calls in 2 hours (7-8 calls/hr)                      │
│    [Start Power Hour]                                           │
│                                                                  │
│ 2️⃣ Prioritize quality                                          │
│    Finish your 4 Priority Agent callbacks                       │
│    [View Priority List]                                         │
│                                                                  │
│ 3️⃣ Talk to your AM                                             │
│    Adjust tomorrow's plan together                              │
│    [Message AM]                                                 │
│                                                                  │
│ What works for you?                                             │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Validation Criteria

- [ ] Presents 3 options (not demands)
- [ ] Respects AA autonomy
- [ ] Specific actions for each option
- [ ] Links to relevant features
- [ ] No shame or blame

---

## Sample 6: End-of-Day Coaching — Full Day Success

### Context

**Scenario:** AA completed full day, met most goals

**AA:** Tony Martinez
**Time:** 5:00 PM
**Hours Invested:** 8.5

### Input — EOD Summary Data

```json
{
  "aa_id": "aa_tony_001",
  "aa_name": "Tony",
  "date": "2025-01-02",
  "hours_committed": 8,
  "hours_invested": 8.5,

  "metrics_final": {
    "calls_made": { "current": 32, "goal": 30, "pct": 107 },
    "conversations": { "current": 9, "goal": 10, "pct": 90 },
    "new_relationships": { "current": 4, "goal": 5, "pct": 80 },
    "offers_sent": { "current": 5, "goal": 5, "pct": 100 },
    "daily_review_complete": true,
    "daily_outreach_complete": true
  },

  "wins": ["calls_made", "offers_sent", "task_completion"],
  "improvement_areas": ["new_relationships"],
  "tomorrow_focus": "Front-load relationship building calls before 11 AM"
}
```

### Expected Output — EOD Summary

```
┌─────────────────────────────────────────────────────────────────┐
│ 🌟 Great work today, Tony!                      January 2, 2025 │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ 📊 YOUR DAY (8.5 hours invested):                               │
│                                                                  │
│ ✓ 32 calls (107% of goal—exceeded!)                             │
│ ✓ 9 conversations (90% of goal—solid!)                          │
│ △ 4 new relationships (80%—close!)                              │
│ ✓ 5 offers sent (100%—on target!)                               │
│                                                                  │
│ ✅ Tasks: Daily Review ✓ | Daily Outreach ✓                     │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│ 💡 TOMORROW'S FOCUS                                              │
│                                                                  │
│ Front-load relationship building calls before 11 AM.            │
│ Your conversation rate is highest in the morning—use that       │
│ momentum for relationship calls.                                │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│ 🎯 MTD Progress: 6 offers sent | On pace for 2 deals this month │
│                                                                  │
│              [See Full Stats]         [Close]                   │
└─────────────────────────────────────────────────────────────────┘
```

### Validation Criteria

- [ ] Acknowledges hours invested
- [ ] Celebrates wins
- [ ] Identifies ONE improvement area
- [ ] Provides specific tomorrow action
- [ ] Shows MTD progress

---

## Sample 7: End-of-Day Coaching — Partial Day

### Context

**Scenario:** AA worked half day, performance evaluated in context

**AA:** Maria Torres
**Hours Committed:** 4 hours
**Hours Invested:** 4.25 hours

### Input — EOD Summary Data

```json
{
  "aa_id": "aa_maria_001",
  "aa_name": "Maria",
  "date": "2025-01-02",
  "hours_committed": 4,
  "hours_invested": 4.25,
  "partial_day": true,

  "metrics_final": {
    "calls_made": { "current": 14, "adjusted_goal": 15, "pct": 93 },
    "conversations": { "current": 4, "adjusted_goal": 5, "pct": 80 },
    "offers_sent": { "current": 2, "adjusted_goal": 2, "pct": 100 }
  }
}
```

### Expected Output — EOD Summary

```
┌─────────────────────────────────────────────────────────────────┐
│ 👍 Nice half-day, Maria!                        January 2, 2025 │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ 📊 YOUR DAY (4.25 hours invested of 4 committed):               │
│                                                                  │
│ ✓ 14 calls (93% of adjusted goal—almost there!)                 │
│ ✓ 4 conversations (80% of adjusted goal)                        │
│ ✓ 2 offers sent (100%—target hit!)                              │
│                                                                  │
│ You're doing great for your time invested.                      │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│ 💬 NOTE FOR TOMORROW                                             │
│                                                                  │
│ If you're planning a half-day again, no problem!                │
│ Just communicate with your AM so we set you up for success.     │
│                                                                  │
│              [See Full Stats]         [Close]                   │
└─────────────────────────────────────────────────────────────────┘
```

### Validation Criteria

- [ ] Goals adjusted for partial day
- [ ] Acknowledges partial commitment positively
- [ ] No shaming for shorter day
- [ ] Encourages communication

---

## Sample 8: Management Classification — A-Player

### Context

**Scenario:** High-performing AA classification

**AA:** Josh Smith
**Tenure:** 120 days
**MTD Deals:** 3

### Input — Classification Data

```json
{
  "aa_id": "aa_josh_001",
  "aa_name": "Josh Smith",
  "tenure_days": 120,

  "performance": {
    "deals_mtd": 3,
    "deals_monthly_goal": 2,
    "compliance_rate": 0.98,
    "avg_hours_per_day": 8.2,
    "strike_count": 0,
    "is_improving": true,
    "trend_7d": "+15%"
  }
}
```

### Expected Output — Classification Card

```
┌─────────────────────────────────────────────────────────────────┐
│ 🌟 A-PLAYER                                                      │
│ Josh Smith                                                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ 📊 METRICS                                                       │
│ • Deals MTD: 3 (150% of goal)                                   │
│ • Compliance: 98%                                               │
│ • Avg Hours: 8.2/day                                            │
│ • Strikes: 0                                                    │
│ • Trend: ↑ +15% (7 days)                                        │
│                                                                  │
│ ✅ CLASSIFICATION CRITERIA MET:                                  │
│ ✓ 2+ deals/month                                                │
│ ✓ 95%+ compliance                                               │
│ ✓ 7.5+ hrs/day average                                          │
│ ✓ Self-correcting (no coaching needed)                          │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│ 🎯 RECOMMENDED ACTIONS                                           │
│ • Celebrate publicly in team meeting                            │
│ • Consider promotion to Senior AA                               │
│ • Assign leadership task (mentor new AA)                        │
│                                                                  │
│ INVESTMENT: HIGH — Worth your time                              │
│                                                                  │
│      [Schedule 1:1]    [Assign Mentee]    [View Details]        │
└─────────────────────────────────────────────────────────────────┘
```

### Validation Criteria

- [ ] Classification clearly displayed
- [ ] All criteria shown with checkmarks
- [ ] Specific recommended actions
- [ ] Investment level stated

---

## Sample 9: Management Classification — C-Player

### Context

**Scenario:** Underperforming AA needing exit

**AA:** Sarah Lopez
**Tenure:** 45 days
**MTD Deals:** 0

### Input — Classification Data

```json
{
  "aa_id": "aa_sarah_001",
  "aa_name": "Sarah Lopez",
  "tenure_days": 45,

  "performance": {
    "deals_mtd": 0,
    "compliance_rate": 0.62,
    "avg_hours_per_day": 4.5,
    "strike_count": 2,
    "is_improving": false,
    "trend_7d": "-8%"
  },

  "strikes": [
    { "date": "2024-12-28", "trigger": "missed_checkin", "details": "No check-in by 10 AM" },
    { "date": "2025-01-01", "trigger": "non_compliance", "details": "<70% task completion 3 days" }
  ]
}
```

### Expected Output — Classification Card

```
┌─────────────────────────────────────────────────────────────────┐
│ ⚠️ C-PLAYER — EXIT CANDIDATE                                    │
│ Sarah Lopez                                                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ 📊 METRICS                                                       │
│ • Deals MTD: 0                                                  │
│ • Compliance: 62%                                               │
│ • Avg Hours: 4.5/day                                            │
│ • Strikes: 2 ⚠️                                                  │
│ • Trend: ↓ -8% (declining)                                      │
│                                                                  │
│ ❌ CLASSIFICATION CRITERIA:                                      │
│ ✗ <0.5 deals/month                                              │
│ ✗ <70% compliance                                               │
│ ✗ Inconsistent hours                                            │
│ ✗ Not improving with coaching                                   │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│ 📋 STRIKE HISTORY                                                │
│ • Dec 28: Missed check-in (no notice)                           │
│ • Jan 1: <70% task completion for 3 days                        │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│ 🎯 RECOMMENDED ACTION                                            │
│ This role may not be the right fit.                             │
│ Consider exit conversation within 2 weeks.                      │
│                                                                  │
│ INVESTMENT: EXIT QUICKLY — Don't waste time                     │
│                                                                  │
│      [Schedule Exit Conv]    [View Strike History]              │
└─────────────────────────────────────────────────────────────────┘
```

### Validation Criteria

- [ ] Clear "Exit Candidate" label
- [ ] Strike count and history displayed
- [ ] Not improving trend shown
- [ ] Direct recommendation (exit)
- [ ] Professional tone (not punitive)

---

## Sample 10: Strike Trigger — Missed Check-In

### Context

**Scenario:** AA hasn't checked in by 10 AM, no notice

**AA:** Kevin Rodriguez
**Time:** 10:15 AM
**Last Check-In:** Yesterday

### Input — Strike Trigger Data

```json
{
  "aa_id": "aa_kevin_001",
  "aa_name": "Kevin Rodriguez",
  "current_time": "2025-01-02T10:15:00Z",
  "last_checkin": "2025-01-01T07:30:00Z",
  "notice_given": false,

  "trigger": "missed_checkin",
  "previous_strikes": 0
}
```

### Expected Output — Strike Creation

```json
{
  "strike_id": "strike_kevin_001_20250102",
  "aa_id": "aa_kevin_001",
  "aa_name": "Kevin Rodriguez",
  "strike_number": 1,
  "trigger": "missed_checkin",
  "trigger_date": "2025-01-02T10:15:00Z",
  "details": "No check-in by 10 AM without prior notice. Last check-in: Jan 1.",
  "am_notified": true,
  "am_action_taken": null,
  "resolved": false
}
```

### Expected Output — AM Notification

```
┌─────────────────────────────────────────────────────────────────┐
│ ⚠️ STRIKE ALERT                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ Kevin Rodriguez — Strike 1 of 3                                 │
│                                                                  │
│ Trigger: Missed check-in                                        │
│ Details: No check-in by 10 AM without prior notice              │
│ Time: January 2, 2025 at 10:15 AM                              │
│                                                                  │
│ Recommended Action:                                              │
│ Reach out to Kevin—there may be a legitimate reason.            │
│ This is a coaching opportunity, not punishment.                 │
│                                                                  │
│      [Call Kevin]    [Send Message]    [View Profile]           │
└─────────────────────────────────────────────────────────────────┘
```

### Validation Criteria

- [ ] Strike logged with details
- [ ] AM notified immediately
- [ ] Coaching language (not punitive)
- [ ] Action options provided

---

## Sample 11: Strike History — Documentation

### Context

**Scenario:** AM reviewing AA's strike history for exit conversation

**AA:** Sarah Lopez
**Total Strikes:** 2

### Input — Strike History

```json
{
  "aa_id": "aa_sarah_001",
  "aa_name": "Sarah Lopez",
  "total_strikes": 2,

  "strikes": [
    {
      "strike_number": 1,
      "trigger": "missed_checkin",
      "trigger_date": "2024-12-28T10:30:00Z",
      "details": "No check-in by 10 AM without notice",
      "am_action": "Called Sarah—said she overslept. Discussed importance of communication.",
      "resolved": true
    },
    {
      "strike_number": 2,
      "trigger": "non_compliance",
      "trigger_date": "2025-01-01T17:00:00Z",
      "details": "<70% task completion for 3 consecutive days (Dec 29-31)",
      "am_action": "1:1 meeting scheduled for Jan 2",
      "resolved": false
    }
  ]
}
```

### Expected Output — Strike History View

```
┌─────────────────────────────────────────────────────────────────┐
│ 📋 STRIKE HISTORY — Sarah Lopez                                 │
│ 2 of 3 strikes                                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ STRIKE 1 — December 28, 2024                         ✓ Resolved │
│ ────────────────────────────────────────────────────────────────│
│ Trigger: Missed check-in                                        │
│ Details: No check-in by 10 AM without notice                    │
│ AM Action: Called Sarah—said she overslept.                     │
│            Discussed importance of communication.               │
│                                                                  │
│ STRIKE 2 — January 1, 2025                           ○ Open     │
│ ────────────────────────────────────────────────────────────────│
│ Trigger: Process non-compliance                                 │
│ Details: <70% task completion for 3 consecutive days            │
│          (Dec 29, 30, 31)                                       │
│ AM Action: 1:1 meeting scheduled for Jan 2                      │
│                                                                  │
├─────────────────────────────────────────────────────────────────┤
│ ⚠️ WARNING: One more strike triggers exit consideration         │
│                                                                  │
│      [Add Note]    [Export for HR]    [Schedule 1:1]            │
└─────────────────────────────────────────────────────────────────┘
```

### Validation Criteria

- [ ] Chronological strike list
- [ ] AM actions documented
- [ ] Resolved/Open status clear
- [ ] Export option for HR
- [ ] Warning about strike 3

---

## Sample 12: Pattern Detection — Coaching Suggestion

### Context

**Scenario:** System detects high calls but low conversations, suggests coaching

**AA:** Maria Torres
**Pattern:** Script issue

### Input — Pattern Detection Data

```json
{
  "aa_id": "aa_maria_001",
  "aa_name": "Maria Torres",
  "detection_date": "2025-01-02",

  "pattern": {
    "type": "script_issue",
    "evidence": {
      "calls_7d_avg": 32,
      "calls_benchmark": 30,
      "conversations_7d_avg": 5,
      "conversations_benchmark": 10,
      "conversation_rate": 0.156,
      "benchmark_rate": 0.333
    },
    "diagnosis": "High call volume but low conversation rate indicates script delivery or opening issue"
  }
}
```

### Expected Output — Coaching Suggestion Card

```
┌─────────────────────────────────────────────────────────────────┐
│ 💡 COACHING SUGGESTION — Maria Torres                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ 🔍 PATTERN DETECTED: Script Issue                               │
│                                                                  │
│ Evidence (7-day average):                                       │
│ • Calls: 32/day (above benchmark of 30) ✓                       │
│ • Conversations: 5/day (below benchmark of 10) ✗                │
│ • Conversation Rate: 15.6% (benchmark: 33%)                     │
│                                                                  │
│ ────────────────────────────────────────────────────────────────│
│                                                                  │
│ 📊 DIAGNOSIS                                                     │
│ Maria is making plenty of calls but not engaging agents.        │
│ This typically indicates a script delivery issue—either         │
│ the opening isn't grabbing attention or she's not               │
│ handling initial objections effectively.                        │
│                                                                  │
│ ────────────────────────────────────────────────────────────────│
│                                                                  │
│ 🎯 RECOMMENDED INTERVENTION                                      │
│                                                                  │
│ 1. Review 3 call recordings from this week                      │
│    Focus on opening 15 seconds and first objection              │
│                                                                  │
│ 2. Assign 3 Practice Mode sessions (PC1)                        │
│    Scenario: Cold call to unresponsive agent                    │
│                                                                  │
│ 3. Role-play in next 1:1                                        │
│    You be the agent, have Maria practice opening                │
│                                                                  │
│      [Assign Practice]    [View Recordings]    [Schedule 1:1]   │
└─────────────────────────────────────────────────────────────────┘
```

### Validation Criteria

- [ ] Pattern type clearly identified
- [ ] Evidence with specific numbers
- [ ] Plain language diagnosis
- [ ] 3 specific intervention steps
- [ ] Links to relevant actions

---

## Edge Case Samples

### Edge Case A: New AA (Day 1)

```json
{
  "aa_id": "aa_new_001",
  "aa_name": "Alex Chen",
  "tenure_days": 1,
  "first_day": true
}
```

**Expected Output:**
```
┌─────────────────────────────────────────────────────────────────┐
│ 👋 Welcome to FlipIQ, Alex!                                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ Today's Goals (Adjusted for Day 1):                             │
│ • Complete onboarding checklist                                 │
│ • Make 10 practice calls                                        │
│ • Meet with your AM                                             │
│                                                                  │
│ Don't worry about full metrics yet—we'll ramp you up over       │
│ the first week. Focus on learning the system today.             │
│                                                                  │
│      [Start Onboarding]    [View Training]                      │
└─────────────────────────────────────────────────────────────────┘
```

---

### Edge Case B: AA on Approved Leave

```json
{
  "aa_id": "aa_kevin_001",
  "aa_name": "Kevin Rodriguez",
  "status": "approved_leave",
  "leave_reason": "Medical appointment",
  "leave_duration": "Full day"
}
```

**Expected Output:**
```
No alerts triggered.
No strike recorded.
Dashboard shows: "On approved leave—see you tomorrow!"
```

---

## Validation Summary

| Sample | Epic | Feature Tested | Key Validation |
|--------|------|----------------|----------------|
| 1 | 1 | Dashboard On-Pace | Metrics display, color coding |
| 2 | 1 | Dashboard Partial Day | Goal adjustment, supportive tone |
| 3 | 1 | Dashboard Exceeding | Celebration, >100% display |
| 4 | 2 | Noon Alert | Supportive nudge, specific action |
| 5 | 2 | 3 PM Alert | Options presented, autonomy respected |
| 6 | 2 | EOD Full Day | Context acknowledgment, tomorrow focus |
| 7 | 2 | EOD Partial Day | Adjusted expectations, positive tone |
| 8 | 3 | A-Player Classification | Criteria met, high investment |
| 9 | 3 | C-Player Classification | Exit recommendation, professional tone |
| 10 | 4 | Strike Trigger | Logging, AM notification |
| 11 | 4 | Strike History | Documentation, HR export |
| 12 | 5 | Pattern Detection | Diagnosis, intervention steps |

---

**Document Version:** 2.0
**Last Updated:** January 2, 2025
**Status:** Ready for AI Training
