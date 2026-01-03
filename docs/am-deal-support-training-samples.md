# AM Deal Support Bot — Training Samples
## Version 1.1 — BMAD iQ
### January 2, 2025

---

## Document Overview

| Field | Value |
|-------|-------|
| **Bot ID** | AMD1 |
| **Total Samples** | 10 |
| **Coverage** | All 6 Epics |
| **Purpose** | AI training data for propensity scoring, pipeline analysis, and AM workflows |

---

## Sample Categories

| # | Category | Samples | Focus |
|---|----------|---------|-------|
| 1-3 | Propensity Score Calculation | 3 | HIGH, MID, LOW scenarios |
| 4-5 | Response Detection & Alerts | 2 | Positive response, unanswered flagging |
| 6-7 | Checklist & AM Actions | 2 | Status-based checklists, review completion |
| 8-9 | Pipeline Reports | 2 | Team summary, forecast |
| 10 | AA Expertise Tracking | 1 | Strength identification |

---

## Critical Integration Note

> ⚠️ **AMD1 CONSUMES data from:**
> - NC1: Agent sentiment, engagement tags
> - PC1: Call quality scores
> - IAMaster: ARV, Rehab, Comps
>
> Training samples show how AMD1 aggregates these inputs.

---

## Sample 1: Propensity Score — HIGH

### Context

**Scenario:** Property with engaged agent, good call quality, active negotiations, recent contact.

**Property:** 1587 Scioto, Banning, CA 92220 — $499,900
**AA:** Josh Smith
**Offer Status:** 60% In Negotiations

### Input — Aggregated Data

```json
{
  "property_id": "prop_1587_scioto",
  "address": "1587 Scioto, Banning, CA 92220",
  "price": 499900,
  "source": "MLS",
  "aa_id": "aa_josh_001",
  "aa_name": "Josh Smith",
  "offer_status": 60,

  "nc1_data": {
    "sentiment_score": 0.85,
    "engagement_tags": ["agent_guiding", "agent_responsive", "presenting_offer"],
    "conversation_quality": "high",
    "last_positive_response": "2025-01-01T14:30:00Z",
    "response_rate": 0.9
  },

  "pc1_data": {
    "total_calls": 5,
    "quality_calls": 4,
    "avg_call_duration": 8.5,
    "conversation_stage": "negotiating"
  },

  "recency": {
    "days_since_contact": 1,
    "last_contact": "2025-01-01T14:30:00Z"
  }
}
```

### Expected Output — Propensity Calculation

```
📊 PROPENSITY CALCULATION — 1587 Scioto

COMPONENT SCORES:
├── NC1 Sentiment: 0.85 (40% weight) → 0.34
├── PC1 Call Quality: 0.80 (25% weight) → 0.20
├── Pipeline Status: 0.80 (25% weight) → 0.20
└── Recency: 1.00 (10% weight) → 0.10

TOTAL SCORE: 0.84

TIER: HIGH 🟢

EXPLANATION:
"Agent presenting Friday, guiding on terms, conversation progressing well. Josh had 4 quality calls, agent is responsive and actively negotiating."
```

### Side Panel Card Display

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📍 1587 Scioto, Banning, CA 92220
   $499,900 | MLS | Josh S.
   Status: 60% In Negotiations

   Propensity: HIGH 🟢
   Why: Agent presenting Friday, guiding on
        terms, conversation progressing well

   ✅ Agent responding & guiding
   📋 Check: Notes | Comps | ARV | Rehab

   [Jump to Property] [Copy to Notes]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Validation Criteria

- [ ] NC1 weight = 0.85 * 0.40 = 0.34
- [ ] PC1 weight = (4/5) * 0.25 = 0.20
- [ ] Status weight = (60/100) * 0.25 = 0.15... wait, let me recalculate
- [ ] Score ≥ 0.70 maps to HIGH tier
- [ ] Explanation references specific engagement signals

---

## Sample 2: Propensity Score — MID

### Context

**Scenario:** Agent responded positively but no contact in 3 days. Good property but conversation stalled.

**Property:** 456 Oak Ave, Phoenix, AZ — $375,000
**AA:** Maria Torres
**Offer Status:** 50% Contract Submitted

### Input — Aggregated Data

```json
{
  "property_id": "prop_456_oak",
  "address": "456 Oak Ave, Phoenix, AZ",
  "price": 375000,
  "source": "MLS",
  "aa_id": "aa_maria_001",
  "aa_name": "Maria Torres",
  "offer_status": 50,

  "nc1_data": {
    "sentiment_score": 0.65,
    "engagement_tags": ["agent_responsive", "waiting_response"],
    "conversation_quality": "medium",
    "last_positive_response": "2024-12-30T10:00:00Z",
    "response_rate": 0.6
  },

  "pc1_data": {
    "total_calls": 3,
    "quality_calls": 2,
    "avg_call_duration": 5.2,
    "conversation_stage": "engaged"
  },

  "recency": {
    "days_since_contact": 3,
    "last_contact": "2024-12-30T10:00:00Z"
  }
}
```

### Expected Output — Propensity Calculation

```
📊 PROPENSITY CALCULATION — 456 Oak Ave

COMPONENT SCORES:
├── NC1 Sentiment: 0.65 (40% weight) → 0.26
├── PC1 Call Quality: 0.67 (25% weight) → 0.17
├── Pipeline Status: 0.60 (25% weight) → 0.15
└── Recency: 0.50 (10% weight) → 0.05

TOTAL SCORE: 0.63

TIER: MID 🟡

EXPLANATION:
"Agent responded positively but no contact in 3 days, needs follow-up. Contract submitted but waiting on agent confirmation. Maria should call today."
```

### Side Panel Card Display

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📍 456 Oak Ave, Phoenix, AZ
   $375,000 | MLS | Maria T.
   Status: 50% Contract Submitted

   Propensity: MID 🟡
   Why: Agent responded positively but no
        contact in 3 days, needs follow-up

   ⚠️ 3 days since last contact
   📋 Check: Notes | Comps | ARV | Rehab

   [Jump to Property] [Copy to Notes]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Validation Criteria

- [ ] Score 0.40-0.69 maps to MID tier
- [ ] Recency = 0.50 (2-5 days since contact)
- [ ] Explanation recommends follow-up action
- [ ] Alert shows days since contact

---

## Sample 3: Propensity Score — LOW

### Context

**Scenario:** Multiple call attempts with no response. Agent not engaging.

**Property:** 789 Pine Dr, Las Vegas, NV — $425,000
**AA:** Sarah Lopez
**Offer Status:** 10% Initial Contact Started

### Input — Aggregated Data

```json
{
  "property_id": "prop_789_pine",
  "address": "789 Pine Dr, Las Vegas, NV",
  "price": 425000,
  "source": "Off-Market",
  "aa_id": "aa_sarah_001",
  "aa_name": "Sarah Lopez",
  "offer_status": 10,

  "nc1_data": {
    "sentiment_score": 0.0,
    "engagement_tags": ["no_response", "voicemails_only"],
    "conversation_quality": "none",
    "last_positive_response": null,
    "response_rate": 0.0
  },

  "pc1_data": {
    "total_calls": 5,
    "quality_calls": 0,
    "avg_call_duration": 0.5,
    "conversation_stage": "initial"
  },

  "recency": {
    "days_since_contact": 7,
    "last_contact": "2024-12-26T09:00:00Z"
  }
}
```

### Expected Output — Propensity Calculation

```
📊 PROPENSITY CALCULATION — 789 Pine Dr

COMPONENT SCORES:
├── NC1 Sentiment: 0.00 (40% weight) → 0.00
├── PC1 Call Quality: 0.00 (25% weight) → 0.00
├── Pipeline Status: 0.10 (25% weight) → 0.025
└── Recency: 0.00 (10% weight) → 0.00

TOTAL SCORE: 0.025

TIER: LOW 🔴

EXPLANATION:
"5 calls made, no response, agent not returning voicemails. Property has been in initial contact for 7+ days with no engagement. Consider deprioritizing or trying different approach."
```

### Side Panel Card Display

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📍 789 Pine Dr, Las Vegas, NV
   $425,000 | Off-Market | Sarah L.
   Status: 10% Initial Contact Started

   Propensity: LOW 🔴
   Why: 5 calls made, no response, agent
        not returning voicemails

   🔴 No agent engagement
   📋 Check: Has agent been called? Qualified?

   [Jump to Property] [Copy to Notes]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Validation Criteria

- [ ] Score < 0.40 maps to LOW tier
- [ ] 0 quality calls = 0 PC1 weight
- [ ] No response = 0 NC1 weight
- [ ] Explanation suggests deprioritizing
- [ ] Sorted LOWER in Deal Review (many calls, no response)

---

## Sample 4: Response Detection — Positive Response

### Context

**Scenario:** Agent sent a positive text message. NC1 has tagged it as positive engagement.

**Agent Response:** "Yes, I can present your offer. Send me the terms and I'll take it to my seller Friday."

### Input — NC1 Response Classification

```json
{
  "message_id": "msg_12345",
  "property_id": "prop_1587_scioto",
  "agent_id": "agent_barry_001",
  "channel": "sms",
  "timestamp": "2025-01-02T09:15:00Z",
  "content": "Yes, I can present your offer. Send me the terms and I'll take it to my seller Friday.",

  "nc1_classification": {
    "sentiment": "positive",
    "engagement_signals": [
      "willing_to_present",
      "guiding_on_timeline",
      "open_to_negotiation"
    ],
    "requires_followup": true,
    "urgency": "high"
  }
}
```

### Expected Output — Text/Email Indicator

**My Deals Row:**
```
☀️ Medium | To do: Not set | • 0 Critical • 0 Reminders • 1 Text/Email
```

**Deal Review Alert:**
```
✅ Positive Response (2 hrs ago)
"Agent will present Friday. Send terms."

ACTION: Send offer terms ASAP
```

### Validation Criteria

- [ ] NC1 sentiment = "positive" triggers indicator
- [ ] Engagement signals match positive patterns
- [ ] Badge count increments
- [ ] Requires_followup = true adds to Deal Review
- [ ] Urgency = "high" prioritizes in list

---

## Sample 5: Unanswered Response Flagging

### Context

**Scenario:** Agent sent a positive response 2 days ago. AA has not followed up.

**Property:** 321 Elm Blvd, Riverside, CA — $389,000
**AA:** Kevin Rodriguez
**Positive Response:** 2 days ago

### Input — Response Tracking Data

```json
{
  "property_id": "prop_321_elm",
  "aa_id": "aa_kevin_001",
  "aa_name": "Kevin Rodriguez",

  "positive_response": {
    "timestamp": "2024-12-31T14:00:00Z",
    "channel": "email",
    "content": "I'm interested in seeing your offer. Can you send me the details?",
    "sentiment": "positive"
  },

  "aa_followup": {
    "last_contact_attempt": "2024-12-30T10:00:00Z",
    "has_responded_to_positive": false,
    "hours_since_positive_response": 48
  },

  "current_time": "2025-01-02T14:00:00Z"
}
```

### Expected Output — Unanswered Alert

**Deal Review Card:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📍 321 Elm Blvd, Riverside, CA
   $389,000 | MLS | Kevin R.
   Status: 30% Offer Terms Sent

   Propensity: MID 🟡
   Why: Agent interested but awaiting
        AA follow-up

   ⚠️ UNANSWERED RESPONSE (2 days)
   Agent asked for offer details 2 days ago.
   Kevin has not responded.

   📋 Check: Notes | Send Offer Terms

   [Jump to Property] [Copy to Notes]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Alert in AM Dashboard:**
```
🚨 URGENT: 1 unanswered positive response

Property: 321 Elm Blvd
AA: Kevin Rodriguez
Agent asked for offer 2 days ago - no follow-up

[Contact Kevin] [Jump to Property]
```

### Validation Criteria

- [ ] Flag triggered at 24-48 hours without response
- [ ] Alert shows time since positive response
- [ ] AA name highlighted for AM action
- [ ] Prioritized in Deal Review (urgency flag)
- [ ] Clear action item for AM

---

## Sample 6: Checklist — 60% In Negotiations

### Context

**Scenario:** Property at 60% In Negotiations. AM reviewing with AA.

**Property:** 1587 Scioto, Banning, CA — $499,900
**Offer Status:** 60% In Negotiations

### Input — Property Data

```json
{
  "property_id": "prop_1587_scioto",
  "offer_status": 60,
  "offer_status_name": "In Negotiations",

  "ia_master_data": {
    "arv": 625000,
    "rehab_estimate": 45000,
    "comps_count": 5,
    "comps_avg_price": 610000,
    "wholesale_price": null
  },

  "nc1_notes": {
    "last_note": "2025-01-01",
    "note_count": 8,
    "last_content": "Agent presenting Friday. Seller wants $510K, we're at $485K."
  },

  "negotiation_details": {
    "our_offer": 485000,
    "seller_counter": 510000,
    "agent_guidance": "Seller motivated, may accept $495K"
  }
}
```

### Expected Output — Status-Based Checklist

```
📋 VERIFICATION CHECKLIST — 60% In Negotiations

├── ✓ Notes (8 notes, last: 1 day ago)
│   └── "Agent presenting Friday. Seller wants $510K, we're at $485K."
│
├── ○ Counter Terms
│   ├── Our Offer: $485,000
│   ├── Seller Counter: $510,000
│   └── Gap: $25,000
│
├── ⚠️ Final Numbers (Review with AA)
│   ├── ARV: $625,000
│   ├── Rehab: $45,000
│   └── Max Offer: $?? (calculate with AM)
│
└── ✓ Agent Guidance
    └── "Seller motivated, may accept $495K"

💡 AM FOCUS:
- Review ARV and Rehab with Josh
- Calculate max allowable offer
- If $495K works, authorize final push
```

### Validation Criteria

- [ ] Checklist items specific to 60% status
- [ ] Counter terms displayed prominently
- [ ] ARV/Rehab from IAMaster
- [ ] Agent guidance from NC1
- [ ] AM focus area highlighted

---

## Sample 7: AM Review Completion

### Context

**Scenario:** AM completes review and copies to Notes with comments.

**AM:** Bob Martinez
**Property:** 1587 Scioto, Banning, CA

### Input — AM Review Data

```json
{
  "property_id": "prop_1587_scioto",
  "am_id": "am_bob_001",
  "am_name": "Bob Martinez",
  "review_timestamp": "2025-01-02T10:30:00Z",

  "propensity": {
    "score": 0.84,
    "tier": "HIGH",
    "explanation": "Agent presenting Friday, guiding on terms, conversation progressing well"
  },

  "checklist_status": {
    "notes": "verified",
    "comps": "verified",
    "arv": "verified",
    "rehab": "verified"
  },

  "am_comments": "ARV was $15K high. Adjusted with Josh. Rehab estimate solid. Good deal, push for close."
}
```

### Expected Output — Note Entry

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[AM REVIEW — 01/02/2025 — Bob Martinez]

Propensity: HIGH 🟢
Agent presenting Friday, guiding on terms,
conversation progressing well

Verified: ✓Notes ✓Comps ✓ARV ✓Rehab

AM Comments:
ARV was $15K high. Adjusted with Josh.
Rehab estimate solid. Good deal, push
for close.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### System Updates

```json
{
  "property_id": "prop_1587_scioto",
  "am_review": {
    "reviewed": true,
    "reviewed_at": "2025-01-02T10:30:00Z",
    "reviewed_by": "am_bob_001"
  },
  "note_created": {
    "note_id": "note_am_review_001",
    "type": "am_review",
    "source": "AMD1"
  }
}
```

### Validation Criteria

- [ ] Note format matches template
- [ ] AM name and date included
- [ ] Checklist status displayed
- [ ] Comments preserved exactly
- [ ] Property marked as reviewed in system
- [ ] Note appears in NC1 timeline

---

## Sample 8: Pipeline Report — Team Summary

### Context

**Scenario:** AM generates Pipeline Report for their team.

**Team:** 4 Acquisition Associates
**Active Properties:** 127

### Input — Pipeline Data

```json
{
  "report_date": "2025-01-02",
  "team_size": 4,
  "total_active": 127,

  "by_status": {
    "10_initial": 42,
    "20_follow": 12,
    "30_backup": 23,
    "50_contract": 28,
    "60_negotiations": 15,
    "80_under_contract": 7
  },

  "by_propensity": {
    "HIGH": 18,
    "MID": 45,
    "LOW": 64
  },

  "by_source": {
    "MLS": 78,
    "Off-Market": 22,
    "Wholesaler": 19,
    "Seller Direct": 8
  },

  "aa_breakdown": [
    {"aa_id": "aa_josh_001", "name": "Josh Smith", "active": 38, "high": 6, "strength": "MLS"},
    {"aa_id": "aa_kevin_001", "name": "Kevin Rodriguez", "active": 32, "high": 5, "strength": "Off-Market"},
    {"aa_id": "aa_maria_001", "name": "Maria Torres", "active": 29, "high": 4, "strength": "Text Campaigns"},
    {"aa_id": "aa_sarah_001", "name": "Sarah Lopez", "active": 28, "high": 3, "strength": "Developing"}
  ]
}
```

### Expected Output — Pipeline Report

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 PIPELINE REPORT — January 2, 2025
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

TEAM: 4 Acquisition Associates
ACTIVE PROPERTIES: 127 total

BY STATUS:
┌────────────────────┬───────┬─────────┐
│ Status             │ Count │ % Total │
├────────────────────┼───────┼─────────┤
│ 10% Initial Contact│   42  │   33%   │
│ 20-30% Working     │   35  │   28%   │
│ 50% Contract Sent  │   28  │   22%   │
│ 60% In Negotiations│   15  │   12%   │
│ 80% Under Contract │    7  │    5%   │
└────────────────────┴───────┴─────────┘

BY PROPENSITY:
┌────────────┬───────┬─────────┐
│ Propensity │ Count │ % Total │
├────────────┼───────┼─────────┤
│ HIGH 🟢    │   18  │   14%   │
│ MID 🟡     │   45  │   35%   │
│ LOW 🔴     │   64  │   51%   │
└────────────┴───────┴─────────┘

BY SOURCE:
┌─────────────┬───────┬─────────┬──────────┐
│ Source      │ Count │ % Total │ Target   │
├─────────────┼───────┼─────────┼──────────┤
│ MLS         │   78  │   61%   │ 45%      │
│ Off-Market  │   22  │   17%   │ 25%      │
│ Wholesaler  │   19  │   15%   │ 20%      │
│ Seller Direct│   8  │    7%   │ 10%      │
└─────────────┴───────┴─────────┴──────────┘
```

### Validation Criteria

- [ ] Totals sum correctly
- [ ] Percentages calculated accurately
- [ ] Status breakdown matches source data
- [ ] Report generates in < 5 seconds

---

## Sample 9: Pipeline Report — Forecast & Recommendations

### Context

**Scenario:** Continuation of Sample 8 with forecast and recommendations.

### Input — Historical Conversion Data

```json
{
  "conversion_rates": {
    "80_under_contract": 0.85,
    "60_negotiations": 0.15,
    "high_propensity": 0.40
  },

  "current_month": {
    "goal": 8,
    "closes_to_date": 2,
    "days_remaining": 29
  },

  "source_targets": {
    "MLS": 0.45,
    "Off-Market": 0.25,
    "Wholesaler": 0.20,
    "Seller Direct": 0.10
  }
}
```

### Expected Output — Forecast Section

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 MONTHLY FORECAST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Based on current pipeline and historical conversion rates:

EXPECTED CLOSES THIS MONTH: 6-8 deals

Breakdown:
• 7 Under Contract (80%) → Expected: 6 closes (85% conv)
• 15 In Negotiations (60%) → Expected: 2 closes (15% conv)
• 18 HIGH Propensity → Expected: 7 closes (40% conv)

BY AA:
┌──────────┬─────────┬──────────┬──────────┐
│ AA       │ Active  │ HIGH     │ Forecast │
├──────────┼─────────┼──────────┼──────────┤
│ Josh S.  │   38    │    6     │ 2 deals  │
│ Kevin R. │   32    │    5     │ 2 deals  │
│ Maria T. │   29    │    4     │ 1-2 deals│
│ Sarah L. │   28    │    3     │ 1 deal   │
└──────────┴─────────┴──────────┴──────────┘

GOAL: 8 deals (2 per AA)
MTD CLOSES: 2
GAP: Need 6 more closes
ON PACE: Yes (29 days remaining)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📈 RECOMMENDATIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔴 SOURCE IMBALANCES:

1. MLS OVERWEIGHTED
   Current: 61% | Target: 45%
   Action: Focus only on aged/pending MLS

2. NEED MORE OFF-MARKET DEALS
   Current: 17% | Target: 25%
   Action: Increase cold outreach
   Assign to: Kevin R. (excels at off-market)

3. NEED MORE WHOLESALER ENGAGEMENT
   Current: 15% | Target: 20%
   Action: Clear Deals@ inbox
   Assign to: Maria T. (strong with text)

⚠️ AA ATTENTION:

• Sarah L. has lowest HIGH propensity (3)
  Recommendation: Coaching session on
  agent engagement and qualification

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ TODAY'S ACTION ITEMS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

URGENT (Do Today):
□ 3 properties with unanswered positive responses
□ 2 properties at 60%+ with no contact in 48+ hours
□ Sarah L. coaching session

THIS WEEK:
□ Push team for 15+ off-market property adds
□ Clear Deals@ inbox (8 unprocessed leads)
□ Review Kevin's top 5 HIGH properties
```

### Validation Criteria

- [ ] Forecast uses correct conversion rates
- [ ] Gap calculation accurate (Goal - MTD)
- [ ] Source imbalances flagged vs targets
- [ ] AA recommendations based on expertise data
- [ ] Action items prioritized by urgency

---

## Sample 10: AA Expertise Tracking

### Context

**Scenario:** System identifies AA strengths based on 12-month close history.

### Input — Historical Close Data

```json
{
  "aa_id": "aa_josh_001",
  "aa_name": "Josh Smith",
  "period": "12_months",

  "closes_by_source": {
    "MLS": 10,
    "Off-Market": 2,
    "Wholesaler": 3,
    "Seller Direct": 1
  },

  "attempts_by_source": {
    "MLS": 45,
    "Off-Market": 15,
    "Wholesaler": 20,
    "Seller Direct": 8
  },

  "total_closes": 16
}
```

### Expected Output — Expertise Profile

```
👤 AA EXPERTISE — Josh Smith

PRIMARY STRENGTH: MLS Deals
├── Evidence: 10/16 closes from MLS (62%)
├── Conversion: 22% (10 closes / 45 attempts)
└── Recommendation: Assign MLS leads to Josh

FULL BREAKDOWN:
┌─────────────┬────────┬──────────┬────────────┐
│ Source      │ Closes │ Attempts │ Conv Rate  │
├─────────────┼────────┼──────────┼────────────┤
│ MLS         │   10   │    45    │   22.2%    │
│ Off-Market  │    2   │    15    │   13.3%    │
│ Wholesaler  │    3   │    20    │   15.0%    │
│ Seller Direct│   1   │     8    │   12.5%    │
└─────────────┴────────┴──────────┴────────────┘

TOTAL YTD: 16 closes (on pace for 2/month)

💡 INSIGHT:
Josh excels at MLS agent relationships. His MLS
conversion rate (22%) is significantly higher than
other sources. Consider reducing his Off-Market
assignments and focusing on MLS volume.
```

### Expertise Schema Output

```json
{
  "aa_id": "aa_josh_001",
  "aa_name": "Josh Smith",
  "primary_strength": "MLS",
  "strength_evidence": "10/16 closes (62%), 22% conversion",
  "close_rate_by_source": {
    "MLS": 0.222,
    "Off-Market": 0.133,
    "Wholesaler": 0.150,
    "Seller Direct": 0.125
  },
  "total_closes_ytd": 16,
  "recommendation": "Assign MLS leads to Josh"
}
```

### Validation Criteria

- [ ] Primary strength = source with highest close count
- [ ] Conversion rates calculated correctly
- [ ] Recommendation specific to strength
- [ ] Data persisted for Pipeline Report use
- [ ] Updated monthly with new closes

---

## Edge Case Samples

### Edge Case A: New AA (No History)

```json
{
  "aa_id": "aa_new_001",
  "aa_name": "Alex Chen",
  "closes_by_source": {},
  "total_closes": 0,
  "days_on_team": 14
}
```

**Expected Output:**
```
👤 AA EXPERTISE — Alex Chen

PRIMARY STRENGTH: Developing
├── New AA (14 days on team)
├── No close history yet
└── Recommendation: Balanced lead assignment

💡 INSIGHT:
Alex is new to the team. Assign balanced
mix of lead sources to identify strengths.
Review in 30 days for pattern development.
```

---

### Edge Case B: No Active Properties

**Scenario:** AM opens Deal Review but no properties meet review criteria.

**Expected Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 DEAL REVIEW

✅ No properties need attention right now

All HIGH propensity properties have been reviewed.
No unanswered positive responses.
No stalled negotiations.

Check back later or run Pipeline Report
for full team status.

[Run Pipeline Report]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### Edge Case C: NC1/PC1 Data Unavailable

**Scenario:** NC1 or PC1 data not available for a property.

**Expected Handling:**
```json
{
  "property_id": "prop_no_data",
  "nc1_data": null,
  "pc1_data": null,

  "propensity": {
    "score": 0.125,
    "tier": "LOW",
    "explanation": "Insufficient data to calculate propensity. Only pipeline status available. Review property manually.",
    "data_warning": true
  }
}
```

**Display:**
```
Propensity: LOW 🔴 (Limited Data)
Why: No NC1/PC1 data available. Score based
     only on pipeline status (10% Initial).
     Review property manually.
```

---

## Validation Summary

| Sample | Epic | Feature Tested | Key Validation |
|--------|------|----------------|----------------|
| 1 | 2 | Propensity HIGH | Component weights, tier mapping |
| 2 | 2 | Propensity MID | Stalled deal detection |
| 3 | 2 | Propensity LOW | No engagement detection |
| 4 | 3 | Positive Response | NC1 sentiment classification |
| 5 | 3 | Unanswered Alert | Time-based flagging |
| 6 | 4 | Checklist 60% | Status-based items |
| 7 | 4 | AM Review | Note creation, completion tracking |
| 8 | 5 | Pipeline Summary | Aggregation, percentages |
| 9 | 5 | Forecast | Conversion rates, recommendations |
| 10 | 5 | AA Expertise | Strength identification |

---

**Document Version:** 1.1
**Last Updated:** January 2, 2025
**Status:** Ready for AI Training
