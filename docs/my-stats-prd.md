# MY STATS BOT

## Product Requirements Document
### Version 2.0 — January 2, 2025

---

| Field | Value |
|-------|-------|
| **Bot ID** | MGT1 (My Stats) |
| **Category** | Management / AA Performance |
| **Priority** | P0 — Critical Path |
| **Primary Users** | AA (Self-Coaching) + AM/Principal (Assessment) |
| **UI Location** | Dashboard → My Stats (AA) + Management Console (AM) |
| **Trigger Method** | Auto (scheduled) + Click (on-demand) + Real-time (alerts) |
| **Integration Points** | AA1-AA4, PIQ, Post Call Bot (D7), Dialpad, Offer Status KPIs |
| **Handoff To** | Eric (PM) / Nate (CTO) / Faizal (UI) |

---

## 1. EXECUTIVE SUMMARY

### 1.1 Purpose

The My Stats Bot V2 is a dual-purpose intelligence system designed to achieve two critical objectives:

1. **Support AAs to improve daily** through realistic, personalized coaching that acknowledges their time investment and provides actionable next steps
2. **Give Management clear visibility** to identify A-players worth investing in versus C-players who need to exit quickly

### 1.2 The Braces Philosophy

> "We apply consistent gentle pressure to correct patterns over time—not punishment that gives people headaches. Our job is to support improvement, to the people who are willing and able to invest the time. But if someone won't follow the process, we don't waste time—that's one of FlipIQ's biggest value propositions to our owner operators."

### 1.3 Mission Alignment

This bot directly supports FlipIQ's core mission: **Enable every Acquisition Associate to reliably close two deals per month.**

### 1.4 The Independent Contractor Reality

- You're here to follow a process—we're here to support you
- You'll be checked in every morning
- Your job is to complete all daily tasks and follow the system
- You must communicate availability—full day, half day, or blocked
- This is NOT a part-time role—agents are available when they're available, not when you have time
- **Three-strike policy**: failure to communicate or deliver results in exit

### 1.5 Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| AAs hit monthly goal | 2 deals/month | Transaction tracking |
| Management identifies underperformers | Within first 2 weeks | Classification accuracy |
| Time saved (AA) | 2 hours/day | Self-assessment eliminated |
| Time saved (Management) | 3 hours/day | Review automation |

---

## 2. CORE PHILOSOPHY: THE BRACES APPROACH

Like orthodontic braces that apply consistent gentle pressure to slowly correct alignment, My Stats Bot V2 provides systematic guidance without punishment.

### What We DO (Support)

| Approach | Example |
|----------|---------|
| Acknowledge time invested vs. committed | "You're doing great for 6 hours—talk to your manager" |
| Provide realistic next steps based on current position | Specific actions, not vague goals |
| Catch patterns early | "Follow-up is slipping" before it's a problem |
| Give specific actions | "3 more calls to hit pace" |

### What We DON'T (Punish)

| Avoid | Why |
|-------|-----|
| Shame people for not hitting numbers | Destroys morale, doesn't improve performance |
| Push last place to become #1 overnight | Unrealistic and demotivating |
| Wait until failure | "You missed quota—you're out" is too late |
| Provide vague feedback | "Do better tomorrow" gives no direction |

### The Three-Strike Policy (Guardrails, Not Weapons)

| Strike | Trigger | Response |
|--------|---------|----------|
| **Strike 1** | Communication breakdown | System flags pattern, AM reaches out to support |
| **Strike 2** | Pattern continues | Formal conversation with AM to identify blockers or mismatch |
| **Strike 3** | No improvement | Mutual exit—this role isn't the right fit, shake hands and move on |

---

## 3. AA-FACING FEATURES: DAILY COACHING

### 3.1 Real-Time Performance Dashboard

| Metric | Daily Goal | Weekly Goal | Data Source |
|--------|------------|-------------|-------------|
| Calls Made | 30 | 150 | Dialpad + IDX Note |
| Conversations | 8-10 (25%) | 40-50 | Post Call Bot logs |
| New Relationships | 3-5 | 15-25 | Agent 365 |
| Offers Sent | 3-5 | 15-25 | Offer Status KPIs |
| Time Invested | 8 hours | 40 hours | AA1 Check-in |
| Daily Review | 100% | 100% | AA2 completion |
| Daily Outreach | 100% | 100% | AA3 completion |
| Campaigns Sent | 3 | 15 | Campaign logs |
| PIQ Properties | 30+ | 150+ | PIQ activity |

### 3.2 Proactive Mid-Day Alerts

Supportive nudges—not warnings:

**Noon Alert (if off-pace):**
```
Hey Tony! Quick check-in—it's noon and you've made 8 calls.
To hit 30 by EOD, you'll need ~66 minutes of calling time.

Quick tip: Front-load calls after lunch while agents are available.
Need help? Talk to iQ or your AM!
```

**3 PM Alert (if significantly behind):**
```
Tony, you're at 15 calls with 2 hours left.

Options:
(1) Power through—15 more calls in 2 hours
(2) Prioritize quality—finish Priority Agent callbacks
(3) Talk to your AM—adjust tomorrow's plan

What works?
```

### 3.3 End-of-Day Contextual Coaching

Acknowledges the AA's actual situation with personalized guidance based on:
- Hours committed vs. hours invested
- Task completion percentage
- Quality metrics (conversation rate, offer rate)
- Comparison to personal historical performance (not team ranking)

---

## 4. MANAGEMENT-FACING FEATURES: AA ASSESSMENT

The critical question: **"Is this person worth my time to invest in?"**

### 4.1 AA Classification System

| Class | Criteria | Action | Investment |
|-------|----------|--------|------------|
| **A-PLAYER** | 2+ deals/month, 100% process compliance, 8 hours/day, Self-correcting | Celebrate publicly, Promote to Senior AA, Leadership tasks | HIGH |
| **B-PLAYER** | 1+ deals/month, 80%+ process compliance, 6-8 hours invested, Improving with coaching | Weekly 1:1 coaching, Monitor gaps, Clear 60-day path | MODERATE |
| **C-PLAYER** | <0.5 deal/month, <70% process compliance, Inconsistent hours, Not improving | Final warning, Exit within 2 weeks, Don't waste time | EXIT QUICKLY |

### 4.2 Key Management Questions (Answered in 10 Seconds)

| Question | How Bot Answers |
|----------|-----------------|
| Who should I fire today? | C-Players with 2+ strikes + not improving |
| Who's worth coaching time? | B-Players improving + responds to feedback |
| Who should I promote? | A-Players 90+ day tenure + leadership indicators |
| Is this person following process? | Task completion % + Check-in consistency |
| Are they putting in hours? | Check-in time + module tracking vs committed |

### 4.3 Management Dashboard Components

```
┌─────────────────────────────────────────────────────────────────┐
│ 📊 AA PERFORMANCE OVERVIEW — January 2, 2025                     │
├─────────────────────────────────────────────────────────────────┤
│ TEAM: 4 AAs                     MTD CLOSES: 3 of 8 goal         │
│                                                                  │
│ A-PLAYERS (2)         B-PLAYERS (1)         C-PLAYERS (1)       │
│ ┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐ │
│ │ Josh S.  ★★★    │   │ Maria T. ★★     │   │ Sarah L. ★      │ │
│ │ 2 deals MTD     │   │ 1 deal MTD      │   │ 0 deals MTD     │ │
│ │ 100% compliance │   │ 85% compliance  │   │ 62% compliance  │ │
│ │ 8.2 hrs/day avg │   │ 7.1 hrs/day avg │   │ 4.5 hrs/day avg │ │
│ │ 0 strikes       │   │ 0 strikes       │   │ 2 strikes ⚠️    │ │
│ └─────────────────┘   └─────────────────┘   └─────────────────┘ │
│                                                                  │
│ 🚨 ALERTS                                                        │
│ • Sarah L.: 2 strikes, <70% compliance 5 days → EXIT CANDIDATE  │
│ • Maria T.: Improving trend, schedule coaching session          │
├─────────────────────────────────────────────────────────────────┤
│ [View Full Report] [Export to Sheets] [Schedule 1:1s]           │
└─────────────────────────────────────────────────────────────────┘
```

---

## 5. DATA INTEGRATIONS

### 5.1 Data Sources

| System | Data Retrieved | Purpose |
|--------|----------------|---------|
| AA1 (Check-In) | check_in_time, available_today, blockers | Time commitment tracking |
| AA2 (Deal Review) | completion_status, time_spent | Daily Review completion |
| AA3 (Outreach) | calls_made, conversations, completion_% | Daily Outreach tracking |
| AA4 (Agents) | relationships_added, tier_upgrades | Relationship building |
| PIQ | properties_processed, time_per_property | Property analysis activity |
| Post Call Bot (D7) | call_quality_score, script_adherence | Call coaching |
| Offer Status KPIs | offers_sent, negotiations, accepted | Pipeline progression |
| Dialpad | total_calls, call_duration | Call activity verification |

### 5.2 Data Flow Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         MGT3 (My Stats)                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐            │
│  │   AA1   │  │   AA2   │  │   AA3   │  │   AA4   │            │
│  │Check-In │  │ Review  │  │Outreach │  │ Agents  │            │
│  └────┬────┘  └────┬────┘  └────┬────┘  └────┬────┘            │
│       │            │            │            │                   │
│       ▼            ▼            ▼            ▼                   │
│  ┌──────────────────────────────────────────────────┐          │
│  │              Performance Aggregator               │          │
│  │  - Daily metrics calculation                      │          │
│  │  - Trend analysis                                 │          │
│  │  - Strike detection                               │          │
│  │  - Classification algorithm                       │          │
│  └──────────────────────────────────────────────────┘          │
│                         │                                        │
│       ┌─────────────────┼─────────────────┐                     │
│       ▼                 ▼                 ▼                     │
│  ┌─────────┐      ┌─────────┐      ┌─────────┐                 │
│  │   AA    │      │   AM    │      │ Alerts  │                 │
│  │Dashboard│      │Dashboard│      │ System  │                 │
│  └─────────┘      └─────────┘      └─────────┘                 │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 6. PATTERN DETECTION LOGIC

### 6.1 Strike Triggers

| Trigger | Condition | Action |
|---------|-----------|--------|
| Missed Check-In | No check-in by 10 AM without notice | Alert AM + increment strike count |
| Zero Production | 0 offers when committed full day | Flag for AM review |
| 3 Days Below Min | <3 offers for 3 consecutive days | Formal warning |
| Process Non-Compliance | <70% Daily Review/Outreach for 3 days | Strike flag |

### 6.2 Coaching Triggers

| Pattern | Diagnosis | Intervention |
|---------|-----------|--------------|
| High calls, low conversations | Script issue | Assign practice mode (PC1) |
| High conversations, low offers | Ask weakness | Offer mechanics training |
| High offers, low negotiations | Follow-up gap | Emphasize AA3 Outreach completion |

### 6.3 Classification Algorithm

```typescript
function classifyAA(aa: AAPerformance): Classification {
  const { deals_mtd, compliance_rate, avg_hours, improving, strikes } = aa;

  // A-PLAYER: High performer, self-correcting
  if (deals_mtd >= 2 && compliance_rate >= 0.95 && avg_hours >= 7.5) {
    return 'A_PLAYER';
  }

  // C-PLAYER: Underperformer, not improving
  if (deals_mtd < 0.5 && compliance_rate < 0.70 && !improving) {
    return 'C_PLAYER';
  }

  // C-PLAYER: Strike threshold exceeded
  if (strikes >= 2 && !improving) {
    return 'C_PLAYER';
  }

  // B-PLAYER: Everyone else (developing)
  return 'B_PLAYER';
}
```

---

## 7. USER STORIES

### 7.1 AA Self-Coaching

**US-MS-001**: Real-Time Dashboard
- **As an** AA tracking my daily performance
- **I want to** see my key metrics (calls, conversations, offers) in real-time
- **So that** I know exactly where I stand vs. daily goals

**US-MS-002**: Mid-Day Alerts
- **As an** AA who is off-pace at midday
- **I want to** receive a supportive nudge with specific actions
- **So that** I can course-correct before EOD

**US-MS-003**: End-of-Day Coaching
- **As an** AA finishing my day
- **I want to** see personalized feedback based on my actual hours invested
- **So that** I understand my performance in context

### 7.2 Management Assessment

**US-MS-004**: AA Classification View
- **As an** AM reviewing my team
- **I want to** see each AA classified as A/B/C-Player
- **So that** I know who to invest coaching time in

**US-MS-005**: Quick Questions Answered
- **As an** AM with limited time
- **I want to** answer "Who should I fire today?" in 10 seconds
- **So that** I don't waste time on underperformers

**US-MS-006**: Strike Tracking
- **As an** AM managing accountability
- **I want to** see strike counts and triggers for each AA
- **So that** I have documented evidence for exit decisions

---

## 8. ACCEPTANCE CRITERIA

### 8.1 AA Dashboard

- [ ] AC1: Dashboard loads in < 2 seconds
- [ ] AC2: All 9 metrics display with daily and weekly targets
- [ ] AC3: Progress bars show percentage to goal
- [ ] AC4: Color coding: Green (on-pace), Yellow (at-risk), Red (behind)
- [ ] AC5: Last updated timestamp displays

### 8.2 Alerts

- [ ] AC6: Noon alert triggers if <15 calls by 12 PM
- [ ] AC7: 3 PM alert triggers if <20 calls by 3 PM
- [ ] AC8: Alerts are supportive, not punitive in tone
- [ ] AC9: Alerts include specific actions (not just "do more")

### 8.3 Management Dashboard

- [ ] AC10: A/B/C classification displays for all AAs
- [ ] AC11: Strike count visible with trigger history
- [ ] AC12: "Who to fire" filter returns C-Players with 2+ strikes
- [ ] AC13: Trend indicators show improving/declining
- [ ] AC14: Export to Google Sheets works

### 8.4 Pattern Detection

- [ ] AC15: Missed check-in by 10 AM flags AA
- [ ] AC16: <70% compliance for 3 days triggers strike
- [ ] AC17: Coaching triggers display relevant intervention
- [ ] AC18: Classification recalculates daily at EOD

---

## 9. DEVELOPMENT PLAN

> **Note:** Timelines reflect BMAD Method + Claude Code (AI-assisted development)

### Phase 0: API Verification — 2 hrs (BLOCKER)

| Task | Hours |
|------|-------|
| 0.1 AA1-AA4 data availability confirmation | 0.5 |
| 0.2 Dialpad API call data access | 0.5 |
| 0.3 Offer Status KPI schema review | 0.5 |
| 0.4 Database schema for strikes/classification | 0.5 |

---

### Epic 1: AA Dashboard — 6 hrs

| Task | Hours |
|------|-------|
| 1.1 Metrics aggregation endpoint | 1.5 |
| 1.2 Real-time dashboard UI | 2 |
| 1.3 Progress bars and color coding | 1 |
| 1.4 Historical trend mini-charts | 1.5 |

---

### Epic 2: Alert System — 4 hrs

| Task | Hours |
|------|-------|
| 2.1 Alert trigger logic (noon, 3 PM) | 1.5 |
| 2.2 Alert content generation (LLM) | 1 |
| 2.3 Alert delivery (in-app notification) | 1 |
| 2.4 Alert dismissal and tracking | 0.5 |

---

### Epic 3: Management Dashboard — 6 hrs

| Task | Hours |
|------|-------|
| 3.1 Classification algorithm implementation | 1.5 |
| 3.2 Team overview UI | 2 |
| 3.3 AA detail cards with metrics | 1.5 |
| 3.4 Filter/sort by classification | 1 |

---

### Epic 4: Strike System — 4 hrs

| Task | Hours |
|------|-------|
| 4.1 Strike trigger detection | 1.5 |
| 4.2 Strike history logging | 1 |
| 4.3 Strike display in UI | 1 |
| 4.4 AM notification on strike | 0.5 |

---

### Epic 5: Pattern Detection & Coaching — 4 hrs

| Task | Hours |
|------|-------|
| 5.1 Pattern detection rules engine | 1.5 |
| 5.2 Coaching intervention mapping | 1 |
| 5.3 Coaching suggestion display | 1 |
| 5.4 Integration with PC1 Practice Mode | 0.5 |

---

### Timeline Summary

| Phase | Hours | Deliverable |
|-------|-------|-------------|
| Phase 0: API Verification | 2 hrs | Go/no-go decision |
| Epic 1: AA Dashboard | 6 hrs | Self-coaching live |
| Epic 2: Alert System | 4 hrs | Mid-day nudges working |
| Epic 3: Management Dashboard | 6 hrs | AM visibility complete |
| Epic 4: Strike System | 4 hrs | Accountability tracking |
| Epic 5: Pattern Detection | 4 hrs | Coaching triggers live |
| **TOTAL** | **26 hrs (~3-4 days)** | **Bot fully deployed** |

---

## 10. RISKS AND MITIGATIONS

| Risk | Impact | Mitigation |
|------|--------|------------|
| AA1-AA4 data not available | Cannot calculate metrics | Verify API access Day 0 |
| Classification feels punitive | AA morale drops | Emphasize "Braces Philosophy" in all copy |
| Strike system misused | Management weaponizes data | Document policy, train AMs |
| Alert fatigue | AAs ignore nudges | Limit to 2 alerts/day max |
| Dialpad data delayed | Real-time metrics stale | Show "last updated" timestamp |

---

## 11. OPEN QUESTIONS

| Question | Options | Decision |
|----------|---------|----------|
| Hours tracking method | Check-in to last activity vs. manual EOD entry | TBD |
| Strike visibility | AAs see their count vs. Management only | TBD |
| Mid-day alert channel | Push notification, email, or in-app only | TBD |
| Leaderboard display | Show bottom performers or softer approach | TBD |

---

## HANDOFF SUMMARY

### For Eric (PM)

| Item | Detail |
|------|--------|
| Problem | AAs lack self-coaching visibility; Management wastes time on underperformers |
| Solution | Dual dashboard: AA self-coaching + Management assessment |
| Key Feature | A/B/C Classification with strike tracking |
| Philosophy | "Braces Approach" — gentle pressure, not punishment |
| **Timeline** | **26 hrs (~3-4 days) with BMAD + Claude Code** |

### For Nate (CTO)

| Item | Detail |
|------|--------|
| Architecture | Aggregates AA1-AA4, Dialpad, Offer Status data |
| APIs | AA1-AA4 completion APIs, Dialpad call logs, Offer Status KPIs |
| Critical Path | Epic 1 (AA Dashboard) — 6 hrs |
| New Tables | strikes, aa_classification, coaching_interventions |
| **Timeline** | **26 hrs (~3-4 days) with BMAD + Claude Code** |

---

**Document prepared for FlipIQ Engineering**
**Version 2.0 — January 2, 2025**
**Status:** APPROVED FOR DEVELOPMENT
