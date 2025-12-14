# FlipIQ December 2024 Sprint Plan

**For:** Nate (CTO)
**From:** Product Team
**Date:** December 14, 2024
**Goal:** Ship the Deal Machine by Dec 31

---

## Executive Summary

We're focusing December on getting the **core deal processing machine** working for our 7 operators. Management dashboards, marketing automation, and voice interface are deferred to January.

**December Deliverables:**
- My Stats Dashboard
- D4-D8 Deal Analysis Suite
- CMaster + C1 + C2 + C4 Comp Analysis
- IAMaster Investment Analysis
- Com1 Auto-Follow-Up

---

## What We're Building (December)

### Priority 1: My Stats Dashboard

**Why:** Operators need visibility into AA performance NOW.

**Reference:** See My Stats screenshot in project files.

**Components:**
| Component | Description |
|-----------|-------------|
| KPI Cards | Calls, Relationships, Offers Sent, In Negotiations, Accepted, Acquired, Time |
| Sparklines | Week-over-week trend for each KPI |
| Team Leaderboard | Ranked by composite score |
| Daily Performance Report | Breakdown with team averages |
| AI Summary | Pipeline overview with coaching feedback |

**Data Sources:**
- `flipiq_daily_metrics` table
- `flipiq_check_in_log` table
- Real-time aggregation from bot activity

**Estimated Effort:** 3 dev days

---

### Priority 2: D4 - Notes Bot

**Why:** AAs lose context between calls. Notes maintain deal history.

**Bot_ID:** D4
**UI Location:** Property detail → Notes tab
**Trigger:** Property load + manual add

**Features:**
| Feature | Description |
|---------|-------------|
| Auto-categorization | Notes tagged: call summary, price discussion, seller info, agent feedback |
| Action item extraction | "Call back Monday" → creates task |
| Context briefing | Summary displayed when property loads |
| Search/filter | Find notes by category, date, keyword |

**Data Model:**
```sql
CREATE TABLE flipiq_notes (
  id UUID PRIMARY KEY,
  property_id UUID NOT NULL,
  user_id UUID NOT NULL,
  content TEXT NOT NULL,
  category VARCHAR(50), -- auto-assigned
  action_items JSONB,   -- extracted tasks
  created_at TIMESTAMP DEFAULT NOW()
);
```

**Estimated Effort:** 2 dev days

---

### Priority 3: D5 - Reminders Bot

**Why:** Zero missed follow-ups = more deals closed.

**Bot_ID:** D5
**UI Location:** Dashboard → Today's Reminders + Property detail
**Trigger:** Scheduled + stage changes

**Features:**
| Feature | Description |
|---------|-------------|
| Stage-based reminders | Auto-reminders when deal stage changes |
| Pending follow-up sequence | Day 3/7/10/15 check-ins for pending deals |
| Custom reminders | AA sets manual follow-ups |
| Daily digest | Morning summary of today's follow-ups |

**Reminder Templates:**
| Deal Stage | Day | Reminder |
|------------|-----|----------|
| Pending | 3 | "Check if deposit posted" |
| Pending | 7 | "Verify contingencies removed" |
| Pending | 15 | "Confirm deal on track" |
| Aged (70+ DOM) | 90 | "Listing expires soon - final push" |

**Estimated Effort:** 2 dev days

---

### Priority 4: D6 - Activity Bot

**Why:** Complete interaction history without manual logging.

**Bot_ID:** D6
**UI Location:** Property detail → Activity tab
**Trigger:** Automatic (call/email/text detection)

**Features:**
| Feature | Description |
|---------|-------------|
| Auto-logging | Calls, emails, texts logged automatically |
| Timeline view | Chronological interaction history |
| Activity search | Filter by type, date, outcome |
| Activity linking | Link activities to notes and reminders |

**Data Model:**
```sql
CREATE TABLE flipiq_activities (
  id UUID PRIMARY KEY,
  property_id UUID NOT NULL,
  user_id UUID NOT NULL,
  activity_type VARCHAR(20), -- call, email, text, meeting
  direction VARCHAR(10),     -- inbound, outbound
  outcome VARCHAR(50),       -- connected, voicemail, no_answer
  duration_seconds INT,
  notes TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);
```

**Estimated Effort:** 2 dev days

---

### Priority 5: D7 - Post Call Bot

**Why:** AI coaching improves call quality over time.

**Bot_ID:** D7
**UI Location:** Property detail → Call History
**Trigger:** After call ends

**Features:**
| Feature | Description |
|---------|-------------|
| Transcription | Call transcribed (NO recording stored) |
| Performance analysis | Script adherence scoring |
| Key moments | AI highlights important parts |
| Coaching suggestions | Specific improvement tips |

**Integration:**
- Transcription service: OpenAI Whisper (recommended) or alternative
- Processing: <2 minutes post-call
- Storage: Transcription text only, no audio files

**Important:** NO CALL RECORDING. Transcription only.

**Estimated Effort:** 3 dev days

---

### Priority 6: D8 - Close Deal Report Bot

**Why:** Learn from wins and losses to improve.

**Bot_ID:** D8
**UI Location:** Closed deal detail
**Trigger:** Deal status → Closed

**Features:**
| Feature | Description |
|---------|-------------|
| Success factors | What worked on this deal |
| Failure patterns | What didn't work (for lost deals) |
| Market comparison | ARV accuracy, days to close vs. average |
| Lessons learned | AI-generated actionable insights |

**Output Report:**
```
DEAL CLOSE REPORT: 123 Main St
================================
Status: CLOSED - Flip
Purchase: $285,000 | ARV: $385,000
Days to Close: 28 (Team Avg: 35)

WHAT WORKED:
- Quick response to price reduction (Day 2)
- Agent had 8 investor transactions - high trust
- Seller motivated by tax delinquency

LESSONS:
- Price reduction + tax distress = high conversion
- Agents with ISC 7+ close faster
```

**Estimated Effort:** 2 dev days

---

### Priority 7: CMaster - Comp Master Bot

**Why:** Orchestrates all comp analysis for accurate ARV.

**Bot_ID:** CMaster
**UI Location:** Property detail → Comps tab
**Trigger:** Property load

**Function:** Coordinates C1, C2, C4 bots to produce comprehensive comp report.

**Output:**
- Map view with comp markers (C1)
- Statistical summary (C2)
- AI-adjusted values (C4)
- Final ARV recommendation

**Estimated Effort:** 2 dev days (orchestration layer)

---

### Priority 8: C1 - Map Review Bot

**Why:** Visual validation catches bad comps.

**Bot_ID:** C1
**UI Location:** Comps tab → Map view
**AI Overlay Position:** Below the map

**Features:**
| Feature | Description |
|---------|-------------|
| Comp plotting | All comps shown on map |
| Boundary overlays | School districts, busy streets |
| Quality indicators | Good/questionable comp markers |
| Distance circles | 0.25mi, 0.5mi, 1mi rings |

**Map Overlays:**
- School district boundaries
- Major road/highway lines
- Flood zones (if available)
- Neighborhood boundaries

**Estimated Effort:** 3 dev days

---

### Priority 9: C2 - Matrix Summary Bot

**Why:** Statistical analysis determines price ranges.

**Bot_ID:** C2
**UI Location:** Comps tab → Matrix view
**AI Overlay Position:** Above the table

**Features:**
| Feature | Description |
|---------|-------------|
| Price/sqft calculation | For all valid comps |
| Outlier detection | Flag comps outside 2 std dev |
| Range determination | Min, median, max, avg |
| Confidence score | Based on comp quality/quantity |

**Output Table:**
| Metric | Value |
|--------|-------|
| Valid Comps | 8 |
| Price/SqFt Range | $185 - $245 |
| Median Price/SqFt | $212 |
| Suggested ARV | $382,000 |
| Confidence | High (8 comps, low variance) |

**Estimated Effort:** 2 dev days

---

### Priority 10: C4 - Comp AI Mapping Bot

**Why:** Photo analysis adjusts for condition differences.

**Bot_ID:** C4
**UI Location:** Comps tab → individual comp cards
**Trigger:** Comp selection

**Features:**
| Feature | Description |
|---------|-------------|
| Photo analysis | AI assesses condition from listing photos |
| Tax data verification | Cross-check sqft, beds, baths |
| Condition scoring | 1-10 scale based on visual analysis |
| Value adjustment | Adjust comp value for condition delta |

**Photo Analysis Points:**
- Kitchen condition (cabinets, counters, appliances)
- Bathroom condition
- Flooring type and condition
- Overall maintenance level
- Curb appeal

**Integration:** Computer vision API (OpenAI Vision or similar)

**Estimated Effort:** 3 dev days

---

### Priority 11: IAMaster - Investment Master Bot

**Why:** Determines if deal fits buy box and calculates returns.

**Bot_ID:** IAMaster
**UI Location:** Property detail → Investment tab
**AI Overlay Position:** Below main content

**Features:**
| Feature | Description |
|---------|-------------|
| Buy box alignment | Property vs. user criteria match % |
| ROI calculation | Based on purchase, rehab, ARV |
| Deal structuring | Flip vs. wholesale recommendation |
| Go/No-Go | Clear recommendation with reasoning |

**Buy Box Fields:**
- Price range (min/max)
- Property type (SFR, multi, condo)
- Year built (range)
- Bed/bath minimums
- Target ROI %
- Max rehab budget

**Output:**
```
INVESTMENT ANALYSIS: 123 Main St
=================================
Buy Box Match: 87% ✓

RETURNS (Flip):
Purchase: $285,000
Rehab Est: $45,000
ARV: $385,000
Gross Profit: $55,000
ROI: 17.3%

RECOMMENDATION: CHASE - Strong flip candidate
- High buy box match
- ROI exceeds 15% target
- Rehab scope manageable
```

**Estimated Effort:** 3 dev days

---

### Priority 12: Com1 - Auto Connect Bot

**Why:** 100% follow-up rate = more conversations = more deals.

**Bot_ID:** Com1
**UI Location:** Background service + Daily Outreach
**Trigger:** After manual call attempt with no answer

**Features:**
| Feature | Description |
|---------|-------------|
| Multi-touch sequences | Auto text/email after no-answer calls |
| Response detection | Stop sequence when agent replies |
| Conversation flagging | Notify AA when conversation starts |
| 100% coverage | Every unanswered call gets follow-up |

**Sequence Template:**
| Day | Channel | Message |
|-----|---------|---------|
| 0 | Text | "Hi [Agent], tried calling about [Address]. Quick question when you have a moment?" |
| 2 | Email | Follow-up email with property details |
| 5 | Text | "Following up on [Address] - still interested in discussing?" |
| 10 | Email | Final outreach |

**Integration:** SMS service (Twilio or similar) + Email

**Estimated Effort:** 2 dev days

---

## What We're NOT Building (December)

| Bot | Reason | Target |
|-----|--------|--------|
| AA0 (Voice/NLP) | Complex - needs OpenAI Realtime tuning | January |
| MGT1-3 (Management) | Operators can use My Stats for now | January |
| M1-M5 (Marketing) | Marketing automation not critical for 7 operators | January |
| C3 (List Grouping) | Phase 3 scope | Q1 2025 |

---

## Development Tracks

### Track A: Deal Context (1-2 devs)
| Week | Bots | Dependencies |
|------|------|--------------|
| Week 1 | D4, D5, D6 | Property overlay DB |
| Week 2 | D7, D8 | Transcription service |

### Track B: Comp Analysis (1-2 devs)
| Week | Bots | Dependencies |
|------|------|--------------|
| Week 1 | C1 | Mapping service, MLS comp data |
| Week 2 | CMaster, C2, C4 | C1 complete, Vision API |

### Track C: Core Features (1 dev)
| Week | Bots | Dependencies |
|------|------|--------------|
| Week 1 | My Stats | Dashboard aggregation |
| Week 2 | IAMaster, Com1 | Buy box config, SMS service |

---

## Database Schema Required

```sql
-- Notes
CREATE TABLE flipiq_notes (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  property_id UUID NOT NULL,
  user_id UUID NOT NULL,
  content TEXT NOT NULL,
  category VARCHAR(50),
  action_items JSONB,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Activities
CREATE TABLE flipiq_activities (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  property_id UUID NOT NULL,
  user_id UUID NOT NULL,
  activity_type VARCHAR(20) NOT NULL,
  direction VARCHAR(10),
  outcome VARCHAR(50),
  duration_seconds INT,
  transcript_id UUID,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Reminders
CREATE TABLE flipiq_reminders (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  property_id UUID,
  user_id UUID NOT NULL,
  reminder_type VARCHAR(50),
  due_date DATE NOT NULL,
  message TEXT,
  is_completed BOOLEAN DEFAULT FALSE,
  completed_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Call Transcripts
CREATE TABLE flipiq_transcripts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  activity_id UUID NOT NULL,
  content TEXT NOT NULL,
  analysis JSONB,
  coaching_suggestions JSONB,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Deal Close Reports
CREATE TABLE flipiq_close_reports (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  property_id UUID NOT NULL,
  outcome VARCHAR(20), -- closed_flip, closed_wholesale, lost
  success_factors JSONB,
  failure_factors JSONB,
  market_comparison JSONB,
  lessons_learned TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Buy Box Configuration (for IAMaster)
CREATE TABLE flipiq_buy_boxes (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL,
  name VARCHAR(100),
  criteria JSONB NOT NULL,
  is_default BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Follow-up Sequences (for Com1)
CREATE TABLE flipiq_sequences (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  property_id UUID NOT NULL,
  agent_id UUID NOT NULL,
  user_id UUID NOT NULL,
  status VARCHAR(20) DEFAULT 'active',
  current_step INT DEFAULT 0,
  started_at TIMESTAMP DEFAULT NOW(),
  completed_at TIMESTAMP,
  response_received BOOLEAN DEFAULT FALSE
);
```

---

## External Services Needed

| Service | Purpose | Priority |
|---------|---------|----------|
| OpenAI API | GPT-4 for AI analysis | Have it |
| OpenAI Whisper | Call transcription (D7) | Need to integrate |
| OpenAI Vision | Photo analysis (C4) | Need to integrate |
| Mapping Service | Comp maps (C1) | Mapbox or Google Maps |
| SMS Service | Auto-follow-up (Com1) | Twilio or similar |
| Email Service | Auto-follow-up (Com1) | SendGrid (have it) |

---

## Questions for Nate

See `decisions-for-nate.md` for full list. Key December decisions:

1. **Transcription service:** Whisper API or alternative?
2. **Mapping service:** Mapbox vs. Google Maps for C1?
3. **SMS service:** Twilio? Already have something?
4. **Computer vision:** OpenAI Vision API acceptable for C4?

---

## Success Criteria (Dec 31)

| Criteria | Measurement |
|----------|-------------|
| My Stats live | All KPIs displaying correctly |
| D4-D8 functional | Notes, reminders, activity logging working |
| Comp analysis working | C1 map, C2 matrix, C4 AI adjustment |
| Investment analysis | IAMaster calculating ROI |
| Auto-follow-up | Com1 sending sequences |
| 7 operators using | No critical bugs blocking usage |

---

## Timeline

| Week | Focus | Deliverable |
|------|-------|-------------|
| Dec 16-22 | Deal Context | My Stats, D4, D5, D6 |
| Dec 23-29 | Deal Intelligence | D7, D8, CMaster, C1, C2 |
| Dec 30-31 | Investment & Comms | C4, IAMaster, Com1 |
| Jan 1+ | Buffer/polish | Bug fixes, AA0, MGT bots |

---

*This document is the primary CTO handoff for December sprint.*
