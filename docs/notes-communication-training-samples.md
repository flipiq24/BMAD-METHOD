# Notes & Communication Bot — Training Samples
## Version 2.0 — BMAD iQ
### January 2, 2025

---

## Document Overview

| Field | Value |
|-------|-------|
| **Bot ID** | NC1 |
| **Total Samples** | 12 |
| **Coverage** | All 6 Epics |
| **Purpose** | AI training data for note aggregation, pattern detection, and context generation |

---

## Sample Categories

| # | Category | Samples | Focus |
|---|----------|---------|-------|
| 1 | Unified Timeline | 2 | Cross-property note display, PC1 integration |
| 2 | Pattern Detection | 3 | Communication preferences, behavior patterns, one-off detection |
| 3 | AI Context Summary | 2 | Pre-call intelligence generation |
| 4 | Ask iQ | 2 | Free-form query responses |
| 5 | Action Items & Reminders | 2 | Commitment detection, status prompts |
| 6 | Note Capture | 1 | Quick-select and PC1 integration |

---

## Critical Integration Note

> ⚠️ **NC1 works WITH PC1 (Post-Call Bot):**
> - PC1 generates notes AFTER each call
> - NC1 aggregates and analyzes notes BEFORE calls
> - NC1 consumes PC1's output as a data source
> - Training samples reflect this integration

---

## Sample 1: Unified Timeline — Cross-Property Display

### Context

**Scenario:** AA opens property at 123 Main Street listed by Barry Tobin. NC1 must display ALL notes for Barry from across ALL properties and ALL AAs.

**Agent:** Barry Tobin (barry.tobin@remax.com)
**Current Property:** 123 Main Street, San Diego — $485,000
**Previous Properties:** 456 Oak Ave (6 months ago), 789 Pine Dr (8 months ago)

### Input — Notes Database

```json
{
  "agent_id": "agent_barry_tobin_001",
  "notes": [
    {
      "note_id": "n001",
      "aa_name": "Josh Santos",
      "property_address": "456 Oak Ave",
      "timestamp": "2024-07-15T14:30:00Z",
      "channel": "call",
      "content": "Called Barry - he said he doesn't double-end. Wants best & highest only. Very direct, doesn't waste time.",
      "source_bot": null
    },
    {
      "note_id": "n002",
      "aa_name": "Josh Santos",
      "property_address": "456 Oak Ave",
      "timestamp": "2024-07-18T09:15:00Z",
      "channel": "sms",
      "content": "Texted POF - responded in 90 minutes. Prefers text over calls.",
      "source_bot": null
    },
    {
      "note_id": "n003",
      "aa_name": "Maria Lopez",
      "property_address": "789 Pine Dr",
      "timestamp": "2024-05-22T16:45:00Z",
      "channel": "call",
      "content": "Barry picked up on first ring. He's working with another investor but said to keep him in mind. Lost on pricing - seller wanted too much.",
      "source_bot": null
    },
    {
      "note_id": "n004",
      "aa_name": "Anas Aqeel",
      "property_address": "123 Main St",
      "timestamp": "2025-01-02T10:30:00Z",
      "channel": "call",
      "content": "Left VM. Call ended in 45 seconds.",
      "source_bot": "PC1"
    }
  ]
}
```

### Expected Output — Unified Timeline

```
📞 Anas Aqeel | 123 Main St | Today [Current Property]
"Left VM. Call ended in 45 seconds."
[PC1 Generated]

📞 Josh Santos | 456 Oak Ave | 6 months ago [View Property →]
"Called Barry - he said he doesn't double-end. Wants best & highest only. Very direct, doesn't waste time."

💬 Josh Santos | 456 Oak Ave | 6 months ago [View Property →]
"Texted POF - responded in 90 minutes. Prefers text over calls."

📞 Maria Lopez | 789 Pine Dr | 8 months ago [View Property →]
"Barry picked up on first ring. He's working with another investor but said to keep him in mind. Lost on pricing - seller wanted too much."
```

### Validation Criteria

- [ ] Notes sorted by timestamp (most recent first)
- [ ] Notes from ALL AAs included (Josh, Maria, Anas)
- [ ] Notes from ALL properties included (current + 2 previous)
- [ ] PC1-generated notes marked with [PC1 Generated]
- [ ] Channel icons correct (📞 = call, 💬 = sms, 📧 = email)
- [ ] "View Property →" links present for non-current properties

---

## Sample 2: Unified Timeline — PC1 Integration

### Context

**Scenario:** PC1 (Post-Call Bot) just processed a completed call. NC1 must integrate this output into the agent's timeline without duplicating manual notes.

**Agent:** Sarah Chen (sarah.chen@coldwellbanker.com)
**Call Just Ended:** 3 minutes ago

### Input — PC1 Output

```json
{
  "source": "PC1",
  "agent_id": "agent_sarah_chen_002",
  "property_id": "prop_321_elm_001",
  "aa_id": "aa_anas_001",
  "call_duration": "4:32",
  "quality_rating": "good_call",
  "summary": "Sarah is interested in our offer. She's motivated - seller needs to close by end of February for tax reasons. Sarah wants POF and proof of LLC registration. She doesn't double-end but will present any serious offer.",
  "next_steps": [
    "Send POF by EOD",
    "Send LLC registration docs",
    "Follow up Friday if no response"
  ],
  "sentiment": "positive",
  "timestamp": "2025-01-02T14:45:00Z"
}
```

### Expected Output — Timeline Entry

```
📞 Anas Aqeel | 321 Elm Blvd | 3 minutes ago [Current Property]
"Sarah is interested in our offer. She's motivated - seller needs to close by end of February for tax reasons. Sarah wants POF and proof of LLC registration. She doesn't double-end but will present any serious offer."

Call Duration: 4:32 | Rating: Good Call
[PC1 Generated]

📋 NEXT STEPS EXTRACTED:
□ Send POF by EOD
□ Send LLC registration docs
□ Follow up Friday if no response
```

### Validation Criteria

- [ ] PC1 summary integrated as timeline note
- [ ] Call duration and quality rating displayed
- [ ] Next steps shown below summary
- [ ] Source marked as PC1
- [ ] No duplicate entry if AA also added manual note

---

## Sample 3: Pattern Detection — Communication Preferences

### Context

**Scenario:** NC1 analyzes 6 months of interaction data to determine Barry Tobin's communication preferences.

**Agent:** Barry Tobin
**Data Points:** 12 call attempts, 8 text attempts, 4 email attempts

### Input — Communication History

```json
{
  "agent_id": "agent_barry_tobin_001",
  "communication_attempts": [
    {"channel": "call", "timestamp": "2024-12-15T10:00:00Z", "response": false},
    {"channel": "call", "timestamp": "2024-12-16T14:30:00Z", "response": true, "response_time_minutes": null},
    {"channel": "sms", "timestamp": "2024-12-17T09:00:00Z", "response": true, "response_time_minutes": 45},
    {"channel": "sms", "timestamp": "2024-12-18T11:00:00Z", "response": true, "response_time_minutes": 90},
    {"channel": "email", "timestamp": "2024-12-19T08:00:00Z", "response": false},
    {"channel": "call", "timestamp": "2024-12-20T15:00:00Z", "response": false},
    {"channel": "sms", "timestamp": "2024-12-21T13:00:00Z", "response": true, "response_time_minutes": 120},
    {"channel": "call", "timestamp": "2024-12-22T10:00:00Z", "response": false},
    {"channel": "call", "timestamp": "2024-12-23T16:00:00Z", "response": true, "response_time_minutes": null},
    {"channel": "sms", "timestamp": "2024-12-26T09:30:00Z", "response": true, "response_time_minutes": 30},
    {"channel": "email", "timestamp": "2024-12-27T10:00:00Z", "response": false},
    {"channel": "call", "timestamp": "2024-12-28T14:00:00Z", "response": false}
  ],
  "response_timestamps": [
    {"channel": "call", "time_of_day": "14:30"},
    {"channel": "call", "time_of_day": "16:00"},
    {"channel": "sms", "time_of_day": "09:45"},
    {"channel": "sms", "time_of_day": "12:30"},
    {"channel": "sms", "time_of_day": "15:00"},
    {"channel": "sms", "time_of_day": "10:00"}
  ]
}
```

### Expected Output — Pattern Analysis

```
📊 COMMUNICATION PATTERNS — Barry Tobin

PREFERRED CHANNEL: TEXT 💬
├── Text Response Rate: 75% (6/8 attempts)
├── Avg Response Time: 71 minutes
└── Call Response Rate: 25% (2/8 attempts)

BEST TIME TO REACH: Late Morning (9am-12pm)
├── 4 of 6 responses in this window
└── Afternoon responses also common

AVOID: Email (0% response rate)

💡 RECOMMENDATION: Text Barry in the morning. He rarely answers calls but responds to texts within 2 hours.
```

### Validation Criteria

- [ ] Response rate calculated correctly per channel
- [ ] Average response time calculated for text
- [ ] Best time window identified from response timestamps
- [ ] Clear recommendation generated
- [ ] Minimum 3 data points required for pattern (achieved: 8 texts)

---

## Sample 4: Pattern Detection — Behavior Patterns

### Context

**Scenario:** NC1 extracts behavioral patterns from note content to identify agent preferences on double-ending, offer format, and negotiation style.

**Agent:** Marcus Williams
**Notes Analyzed:** 8 notes across 3 properties

### Input — Note Content

```json
{
  "agent_id": "agent_marcus_williams_003",
  "notes_for_analysis": [
    "Marcus said he doesn't double-end, needs to protect his client relationship.",
    "He wants best and highest offers only - no negotiating back and forth.",
    "Marcus always asks for POF before he'll even present an offer.",
    "Called Marcus - he said again he won't dual agency, it's company policy.",
    "Sent POF as he requested. He confirmed receipt.",
    "Marcus said he already has investor partners so he's familiar with cash deals.",
    "He prefers all offers submitted by Friday so seller can review over weekend.",
    "Lost the deal - our offer was $15K under best & highest winner."
  ]
}
```

### Expected Output — Behavior Patterns

```
🎯 BEHAVIOR PATTERNS — Marcus Williams

DOUBLE-END: ❌ NO (stated 2x)
├── "doesn't double-end, needs to protect client relationship"
├── "won't dual agency, it's company policy"
└── Don't ask - go straight to best & highest

OFFER FORMAT: Best & Highest Only
├── "wants best and highest offers only"
├── "no negotiating back and forth"
└── Submit your best number first

POF REQUIREMENT: Before Offer Presentation
├── "asks for POF before he'll even present"
└── Send POF with initial contact

INVESTOR FAMILIARITY: YES ✓
├── "already has investor partners"
├── "familiar with cash deals"
└── Speak investor language

⏰ TIMELINE PREFERENCE:
├── "offers submitted by Friday"
├── "seller can review over weekend"
└── Submit Thursday or Friday

💡 KEY INSIGHT: Marcus is experienced with investors but strict on process. Lead with POF, submit best & highest by Thursday, don't ask about double-ending.
```

### Validation Criteria

- [ ] Double-end stance detected with evidence count
- [ ] Offer format preference extracted
- [ ] POF requirement timing identified
- [ ] Investor familiarity flagged
- [ ] Timeline preferences noted
- [ ] Key insight summarizes actionable approach

---

## Sample 5: Pattern Detection — One-Off Detection

### Context

**Scenario:** NC1 flags when a property is unusual for an agent's typical profile, indicating they may be less experienced with this type of deal.

**Agent:** Jennifer Adams
**Agent's Typical Profile:** $1.2M average listing price, luxury market focus
**Current Property:** 425 Budget Lane — $285,000

### Input — Agent Profile Data

```json
{
  "agent_id": "agent_jennifer_adams_004",
  "name": "Jennifer Adams",
  "agent365_data": {
    "avg_listing_price": 1200000,
    "typical_price_range": {"min": 850000, "max": 2500000},
    "deals_last_year": 24,
    "luxury_designations": ["Luxury Home Specialist", "Million Dollar Guild"],
    "investor_deals": 2,
    "double_end_rate": 0.12
  },
  "current_property": {
    "address": "425 Budget Lane",
    "price": 285000,
    "condition": "fixer",
    "days_on_market": 45
  }
}
```

### Expected Output — One-Off Flag

```
⚠️ ONE-OFF LISTING DETECTED — Jennifer Adams

VARIANCE: This $285K listing is 76% BELOW her typical range
├── Jennifer's Average: $1,200,000
├── This Property: $285,000
├── Her Usual Range: $850K - $2.5M

IMPLICATIONS:
├── Likely inherited or probate situation
├── May be less experienced with investor deals (only 2 last year)
├── May not understand AS-IS/fixer dynamics
└── Could be more motivated to close (outside comfort zone)

AGENT PROFILE:
├── 24 deals/year (experienced overall)
├── Luxury designations: YES
├── Investor experience: LOW
├── Double-end rate: 12% (occasionally)

💡 APPROACH: Jennifer knows real estate but not investor deals. Educate on process, be patient with AS-IS concerns, emphasize certainty of close over price. She may be more flexible than her usual luxury clients.
```

### Validation Criteria

- [ ] Variance calculated correctly: |1,200,000 - 285,000| / 1,200,000 = 76%
- [ ] Flag triggered because variance > 30%
- [ ] Implications specific to the situation
- [ ] Approach recommendation tailored to one-off context
- [ ] Agent's overall experience acknowledged

---

## Sample 6: AI Context Summary — Pre-Call Generation

### Context

**Scenario:** AA clicks "Call" button. NC1 generates a 30-second context summary combining seller distress, agent patterns, and deal status.

**Property:** 789 Distressed Drive, Phoenix — $375,000
**Agent:** Robert Chen
**Days on Market:** 92

### Input — Aggregated Context Data

```json
{
  "property": {
    "address": "789 Distressed Drive, Phoenix",
    "price": 375000,
    "days_on_market": 92,
    "price_reductions": [
      {"date": "2024-11-15", "from": 425000, "to": 399000},
      {"date": "2024-12-20", "from": 399000, "to": 375000}
    ],
    "condition": "AS-IS"
  },
  "seller_distress": {
    "nod_filed": true,
    "nod_date": "2024-10-15",
    "days_since_nod": 79,
    "estimated_auction": "2025-02-15",
    "other_flags": ["tax_default_2_years"]
  },
  "agent_patterns": {
    "preferred_channel": "text",
    "response_rate_text": 0.80,
    "double_end": "no",
    "offer_format": "best_highest",
    "pof_required": "with_offer"
  },
  "previous_interactions": [
    {
      "aa_name": "Maria Lopez",
      "date": "2024-12-15",
      "outcome": "Left VM, no callback",
      "property": "789 Distressed Drive"
    },
    {
      "aa_name": "Josh Santos",
      "date": "2024-12-22",
      "outcome": "Connected briefly, agent was busy, said to text",
      "property": "789 Distressed Drive"
    }
  ],
  "deal_status": "Contacted - Pending Response"
}
```

### Expected Output — Context Summary

```
📋 PRE-CALL SUMMARY — 789 Distressed Drive

STATUS: Contacted — Pending Response (Last: Josh, 12/22)

🔥 SELLER PAIN (HIGH):
• NOD filed 79 days ago — Auction est. Feb 15
• 2 price reductions totaling $50K
• Tax default (2 years)
• 92 DOM — motivated seller

📊 AGENT PATTERNS — Robert Chen:
• Prefers TEXT (80% response rate)
• Does NOT double-end
• Wants Best & Highest format
• POF required with offer

📝 PREVIOUS ATTEMPTS:
• Maria (12/15): Left VM, no callback
• Josh (12/22): Connected, agent busy — said to TEXT

⏰ OPEN QUESTIONS:
□ Has seller received any offers?
□ Timeline flexibility?
□ Backup buyer agents?

📋 NEXT STEPS:
□ Text instead of call (per agent preference)
□ Include POF with any offer discussion
□ Go straight to best & highest — no double-end ask

🚨 OFFER PROGRESSION:
□ If no response 2 days → Send self-rep offer direct
□ Auction deadline Feb 15 — urgency is REAL
```

### Validation Criteria

- [ ] Summary generates in < 5 seconds
- [ ] Seller distress quantified with dates
- [ ] Agent patterns from Sample 3-4 format included
- [ ] Previous AA attempts referenced
- [ ] Open questions identified
- [ ] Offer progression logic included
- [ ] Max 200 words

---

## Sample 7: AI Context Summary — Agent Profile View

### Context

**Scenario:** AA opens Agent Profile (not a specific property). NC1 generates a relationship summary for the agent across ALL properties.

**Agent:** Lisa Martinez
**Total Interactions:** 14 across 4 properties over 18 months

### Input — Aggregated Agent Data

```json
{
  "agent_id": "agent_lisa_martinez_005",
  "name": "Lisa Martinez",
  "office": "Keller Williams San Diego",
  "agent365_summary": {
    "deals_per_year": 38,
    "investor_source_count": 12,
    "avg_listing_price": 550000,
    "double_end_rate": 0.18
  },
  "interaction_summary": {
    "total_properties": 4,
    "total_interactions": 14,
    "outcomes": {
      "won": 1,
      "lost_price": 2,
      "lost_timing": 0,
      "active": 1
    }
  },
  "property_history": [
    {"address": "111 First St", "outcome": "WON", "close_date": "2024-03-15", "aa": "Josh Santos"},
    {"address": "222 Second Ave", "outcome": "LOST", "reason": "Seller wanted more", "aa": "Maria Lopez"},
    {"address": "333 Third Blvd", "outcome": "LOST", "reason": "Went with higher offer", "aa": "Josh Santos"},
    {"address": "444 Fourth Dr", "outcome": "ACTIVE", "status": "Offer pending", "aa": "Anas Aqeel"}
  ],
  "known_patterns": {
    "double_end": "sometimes",
    "offer_format": "direct",
    "communication": "call_morning",
    "notes": ["Prefers morning calls", "Will double-end on older listings", "Responsive within 24 hours"]
  }
}
```

### Expected Output — Agent Profile Summary

```
👤 AGENT PROFILE — Lisa Martinez
Keller Williams San Diego

📊 AGENT365 INTELLIGENCE:
├── Volume: 38 deals/year (HIGH)
├── Investor Experience: 12 investor sources (WORKS WITH INVESTORS ✓)
├── Avg Price: $550K
└── Double-End Rate: 18% (sometimes)

🏠 OUR HISTORY (4 Properties):
├── ✅ WON: 111 First St (Josh, Mar 2024)
├── ❌ LOST: 222 Second Ave — Seller wanted more (Maria)
├── ❌ LOST: 333 Third Blvd — Higher offer (Josh)
└── 🔄 ACTIVE: 444 Fourth Dr — Offer pending (Anas)

📈 RELATIONSHIP STATUS:
├── Win Rate: 25% (1/4)
├── Loss Reason: Usually PRICE
├── Relationship: WARM (repeat engagement)
└── Time Since First Contact: 18 months

🎯 KNOWN PATTERNS:
├── Prefers morning calls
├── Will double-end on older listings
├── Responds within 24 hours
├── Direct offer format (no B&H games)

💡 RELATIONSHIP STRATEGY:
Lisa knows us and has closed with us before. She's a HIGH VALUE agent (38 deals/year, works with investors). We've lost on price twice — consider going higher on 444 Fourth Dr. She'll double-end on aged listings. Call in the morning.
```

### Validation Criteria

- [ ] Agent365 data integrated
- [ ] Full property history shown with outcomes
- [ ] Win rate calculated correctly
- [ ] Loss patterns identified
- [ ] Relationship status assessed
- [ ] Strategy recommendation tailored to history

---

## Sample 8: Ask iQ — Communication Query

### Context

**Scenario:** AA types free-form question about how to reach an agent.

**Query:** "Should I call or text David Park?"

### Input — Agent Data for Query

```json
{
  "query": "Should I call or text David Park?",
  "agent_id": "agent_david_park_006",
  "communication_data": {
    "call_attempts": 8,
    "call_responses": 2,
    "call_response_rate": 0.25,
    "text_attempts": 5,
    "text_responses": 4,
    "text_response_rate": 0.80,
    "avg_text_response_time": 45,
    "email_attempts": 2,
    "email_responses": 0
  },
  "recent_notes": [
    "David never picks up, always returns calls via text",
    "Texted David - got response in 30 min"
  ]
}
```

### Expected Output — Ask iQ Response

```
💬 TEXT David Park.

EVIDENCE:
├── Text Response Rate: 80% (4/5 attempts)
├── Call Response Rate: 25% (2/8 attempts)
├── Avg Text Response: 45 minutes
├── Notes: "never picks up, always returns calls via text"

📱 He responds to texts quickly but ignores calls. Send a text.
```

### Validation Criteria

- [ ] Answer in < 3 seconds
- [ ] Clear recommendation (TEXT)
- [ ] Evidence cited with numbers
- [ ] Note content referenced
- [ ] Max 100 words

---

## Sample 9: Ask iQ — History Query

### Context

**Scenario:** AA asks about prior relationship with an agent.

**Query:** "Has anyone talked to Karen Wilson before?"

### Input — Agent Interaction Data

```json
{
  "query": "Has anyone talked to Karen Wilson before?",
  "agent_id": "agent_karen_wilson_007",
  "interaction_history": [
    {
      "aa_name": "Josh Santos",
      "property": "567 Oak Lane",
      "date": "2024-06-15",
      "outcome": "LOST",
      "reason": "Went with cash buyer, we were financing",
      "notes_summary": "Karen was responsive but seller chose cash. She said to stay in touch for future listings."
    },
    {
      "aa_name": "Maria Lopez",
      "property": "890 Maple Dr",
      "date": "2024-09-22",
      "outcome": "LOST",
      "reason": "Price too high",
      "notes_summary": "Karen tried to work with us but seller was unrealistic. She appreciated our professionalism."
    }
  ]
}
```

### Expected Output — Ask iQ Response

```
Yes — 2 previous contacts:

1️⃣ Josh Santos (Jun 2024) — 567 Oak Lane
├── Outcome: LOST (Seller chose cash buyer)
├── Karen was responsive
└── She said to stay in touch

2️⃣ Maria Lopez (Sep 2024) — 890 Maple Dr
├── Outcome: LOST (Price too high)
├── Karen appreciated our professionalism
└── Seller was unrealistic

💡 INSIGHT: Karen knows us and likes working with us. We've lost twice but for deal-specific reasons, not relationship issues. She's a warm contact — reference past interactions.
```

### Validation Criteria

- [ ] Previous AAs identified with dates
- [ ] Properties and outcomes listed
- [ ] Notes summarized
- [ ] Actionable insight provided
- [ ] Answer in < 3 seconds

---

## Sample 10: Action Items — Commitment Detection

### Context

**Scenario:** NC1 analyzes a call note and extracts action items with due dates.

**Note Content:** "Great call with Jennifer. She said to send the POF and call her back Tuesday around 2pm. She's submitting best & highest on Friday so we need to have our number finalized by Thursday."

### Input — Note for Analysis

```json
{
  "note_id": "n_123",
  "agent_id": "agent_jennifer_ross_008",
  "property_id": "prop_999",
  "aa_id": "aa_anas_001",
  "content": "Great call with Jennifer. She said to send the POF and call her back Tuesday around 2pm. She's submitting best & highest on Friday so we need to have our number finalized by Thursday.",
  "timestamp": "2025-01-02T11:00:00Z",
  "current_date": "2025-01-02"
}
```

### Expected Output — Extracted Action Items

```
📋 ACTION ITEMS DETECTED — Jennifer Ross

✅ CREATED:

1️⃣ Send POF to Jennifer
├── Due: TODAY (immediate commitment)
├── Source: "She said to send the POF"
└── Priority: HIGH

2️⃣ Call Jennifer back
├── Due: Tuesday, Jan 7 @ 2:00 PM
├── Source: "call her back Tuesday around 2pm"
└── Priority: MEDIUM

3️⃣ Finalize offer number
├── Due: Thursday, Jan 9
├── Source: "need to have our number finalized by Thursday"
└── Priority: HIGH (blocks B&H submission)

⏰ DEADLINE ALERT:
Best & Highest submission: Friday, Jan 10
Work backwards: Finalize Thursday → Submit Friday
```

### Validation Criteria

- [ ] All 3 commitments detected
- [ ] Due dates extracted correctly (Tuesday, Thursday)
- [ ] Immediate items flagged (POF = today)
- [ ] External deadline noted (Friday B&H)
- [ ] Priority assigned based on urgency

---

## Sample 11: Action Items — Status Update Prompt

### Context

**Scenario:** AA just had a positive call with an agent who was previously marked as COLD. NC1 suggests a status update but does NOT auto-update.

**Current Agent Status:** COLD
**Call Sentiment:** Positive (detected from note)

### Input — Sentiment Analysis

```json
{
  "agent_id": "agent_tom_brady_009",
  "current_status": "COLD",
  "latest_note": {
    "content": "Finally got Tom on the phone! He's actually interested now - the property has been sitting and his seller is getting anxious. He asked for our offer range and said to send details. Very different attitude from before.",
    "sentiment_score": 0.85,
    "sentiment_label": "positive"
  },
  "previous_notes": [
    {"content": "Tom didn't return call", "sentiment": "neutral"},
    {"content": "Left another VM, no response", "sentiment": "neutral"},
    {"content": "Tom was dismissive, said they have other offers", "sentiment": "negative"}
  ]
}
```

### Expected Output — Status Update Prompt

```
💡 SUGGESTED STATUS UPDATE

Current Status: ❄️ COLD
Latest Conversation: ✅ POSITIVE

DETECTED SIGNALS:
├── "actually interested now"
├── "seller is getting anxious"
├── "asked for our offer range"
├── "said to send details"

PREVIOUS STATUS: Agent was dismissive with other offers

SUGGESTED UPDATE: 🔥 WARM

Reason: Agent's attitude shifted significantly. Seller motivation + time on market changed dynamics.

┌─────────────────────────────────────────┐
│  [Remind Me Later]  [Skip]  [Update Now]│
└─────────────────────────────────────────┘

⚠️ This is a SUGGESTION only. Status will NOT change unless you click "Update Now".
```

### Validation Criteria

- [ ] Current status displayed
- [ ] Sentiment signals extracted
- [ ] Previous context referenced
- [ ] Suggested status with reasoning
- [ ] Three action options provided
- [ ] Clear disclaimer: NO auto-update
- [ ] AA must manually confirm

---

## Sample 12: Note Capture — Quick-Select + PC1 Integration

### Context

**Scenario:** AA finishes a call. PC1 has generated a summary, and NC1 presents it for review with quick-select options.

**Call Just Ended:** With agent Alex Rivera about 123 Oak Street

### Input — PC1 Output + Quick-Select Context

```json
{
  "pc1_output": {
    "summary": "Alex is interested but wants to see POF first. Seller is motivated due to divorce. Alex mentioned he occasionally double-ends on distressed properties. Asked us to call back tomorrow after he talks to seller.",
    "next_steps": ["Send POF", "Call back tomorrow"],
    "quality_rating": "good_call",
    "call_duration": "5:22"
  },
  "quick_select_options": [
    "Interested", "Not Interested", "Call Back", "Left VM",
    "Sent Docs", "Scheduled Showing", "Made Offer",
    "Double-End Confirmed", "Double-End Rejected"
  ],
  "agent_id": "agent_alex_rivera_010",
  "property_id": "prop_123_oak"
}
```

### Expected Output — Note Capture Interface

```
📝 CAPTURE NOTE — Alex Rivera | 123 Oak St

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📞 PC1 GENERATED SUMMARY (Good Call - 5:22)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Alex is interested but wants to see POF first. Seller is motivated due to divorce. Alex mentioned he occasionally double-ends on distressed properties. Asked us to call back tomorrow after he talks to seller.

[Edit Note] [Save as PC1] [Save as Manual]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚡ QUICK-SELECT (Click to add)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[✓ Interested] [Call Back] [Sent Docs]
[Double-End Confirmed] [Made Offer]

Selected: Interested, Call Back

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 NEXT STEPS DETECTED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

□ Send POF → [Create Reminder: TODAY]
□ Call back tomorrow → [Create Reminder: Tomorrow 10am]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 NOTE TYPE (Optional)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[ ] Communication    [✓] Critical

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

        [Cancel]            [Save Note]
```

### Validation Criteria

- [ ] PC1 output pre-populates note field
- [ ] Edit option available
- [ ] Source tracking (PC1 vs Manual if edited)
- [ ] Quick-select buttons available
- [ ] Multiple selections allowed
- [ ] Next steps shown with reminder creation
- [ ] Note type classification optional
- [ ] Critical flag highlights in timeline

---

## Validation Summary

| Sample | Epic | Feature Tested | Key Validation |
|--------|------|----------------|----------------|
| 1 | 1 | Unified Timeline | Cross-property notes, multiple AAs |
| 2 | 1 | PC1 Integration | Call summary, next steps extraction |
| 3 | 2 | Communication Patterns | Channel preference, response time |
| 4 | 2 | Behavior Patterns | Double-end, offer format, keywords |
| 5 | 2 | One-Off Detection | Price variance >30%, approach adjust |
| 6 | 3 | Pre-Call Summary | Seller distress + patterns + status |
| 7 | 3 | Agent Profile Summary | Full relationship view |
| 8 | 4 | Ask iQ (Comm) | Channel recommendation with evidence |
| 9 | 4 | Ask iQ (History) | Previous AA interactions |
| 10 | 5 | Commitment Detection | Action items with due dates |
| 11 | 5 | Status Update Prompt | Suggest but don't auto-update |
| 12 | 6 | Note Capture | Quick-select + PC1 integration |

---

## Edge Case Samples

### Edge Case A: New Agent (No History)

```json
{
  "agent_id": "agent_new_001",
  "interaction_history": [],
  "agent365_data": {
    "deals_per_year": 15,
    "investor_source_count": 3
  }
}
```

**Expected Output:**
```
📊 AGENT PATTERNS — New Agent

⚠️ NO PRIOR INTERACTION HISTORY

Available from Agent365:
├── Volume: 15 deals/year (MID)
├── Investor Sources: 3 (LOW)
└── No communication preference data

💡 RECOMMENDATION: Start fresh. Text first to gauge responsiveness, follow up with call if no response in 2 hours.
```

---

### Edge Case B: Conflicting Pattern Data

```json
{
  "agent_id": "agent_conflict_001",
  "notes": [
    {"content": "He said he double-ends all the time", "date": "2024-06-01"},
    {"content": "Agent refused to double-end, company policy", "date": "2024-12-15"}
  ]
}
```

**Expected Output:**
```
🎯 BEHAVIOR PATTERNS — Conflicting Agent

DOUBLE-END: ⚠️ CONFLICTING DATA
├── Jun 2024: "double-ends all the time"
├── Dec 2024: "refused to double-end, company policy"
└── MOST RECENT: Does NOT double-end

💡 Policy may have changed. Go with most recent data point. Ask carefully if unsure.
```

---

### Edge Case C: High Volume Agent (50+ Notes)

**Expected Behavior:**
```
├── Load most recent 20 notes initially
├── "Load More" button for older notes
├── Pattern detection uses ALL notes (not just visible)
├── Timeline loads in < 2 seconds
└── Pagination prevents UI lag
```

---

**Document Version:** 2.0
**Last Updated:** January 2, 2025
**Status:** Ready for AI Training
