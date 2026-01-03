# AM Deal Support Bot — User Stories
## Version 1.1 — BMAD iQ
### January 2, 2025

---

## Document Overview

| Field | Value |
|-------|-------|
| **Total Epics** | 6 |
| **Total User Stories** | 28 |
| **Primary User** | Acquisition Manager (AM) |
| **Secondary Users** | AA (Phase 2) |
| **Dependencies** | NC1, PC1, IAMaster, D2, Dialpad, Google Sheets |

---

## Epic Overview

| # | Epic | Stories | Focus |
|---|------|---------|-------|
| 1 | Core UI & Side Panel | 5 | iQ Button, Deal Review list, property cards |
| 2 | Propensity Score Engine | 5 | Score calculation, explanations |
| 3 | Response Detection & Alerts | 4 | Text/Email indicators, positive response flagging |
| 4 | Checklist & AM Actions | 5 | Verification workflow, task completion |
| 5 | Pipeline Reports & Forecast | 5 | Aggregation, recommendations, AA tracking |
| 6 | Google Sheets Export | 4 | Daily append, historical tracking |

---

## Critical Integration Note

> ⚠️ **AMD1 CONSUMES data from other iQ bots:**
> - NC1: Agent sentiment, engagement tags
> - PC1: Call quality scores
> - IAMaster: ARV, Rehab, Comps
> - D2: Agent profiles, ISC
>
> AMD1 aggregates these outputs for AM-level decision support.

---

## Epic 1: Core UI & Side Panel

**Goal:** Provide AMs with a deal review interface integrated into My Deals.

---

### US-1.1: iQ Button Role Visibility

**As a** System
**I want to** show the iQ Button only to users with AM role
**So that** AAs don't access AM-specific pipeline views prematurely

**Acceptance Criteria:**
- [ ] iQ Button visible only when user.role === 'AM'
- [ ] Button placement: My Active Deals tab, top-right
- [ ] Button style consistent with existing iQ buttons
- [ ] Non-AM users see no button (not disabled, completely hidden)

---

### US-1.2: Side Panel Open

**As an** AM clicking the iQ Button
**I want** a side panel to slide open
**So that** I can review deals without leaving My Deals view

**Acceptance Criteria:**
- [ ] Panel slides from right edge
- [ ] Animation < 300ms
- [ ] Panel width: 400px (desktop)
- [ ] Close button (X) in top-right
- [ ] Clicking outside panel does NOT close it (intentional)
- [ ] ESC key closes panel

---

### US-1.3: Filter Integration

**As an** AM with filters applied on My Deals
**I want** the side panel to respect those filters
**So that** I only see properties matching my current view

**Acceptance Criteria:**
- [ ] Side panel inherits: AA filter, Status filter, Source filter
- [ ] Changing filters in My Deals updates side panel
- [ ] Panel header shows active filters: "Showing: Josh S. | 60%+ | MLS"
- [ ] "Clear Filters" button resets to all properties

---

### US-1.4: Deal Review List

**As an** AM
**I want to** see a prioritized list of properties needing attention
**So that** I focus on high-impact deals first

**Acceptance Criteria:**
- [ ] Properties sorted by priority algorithm (see US-3.4)
- [ ] List shows: Address, Price, AA, Status, Propensity Score
- [ ] List loads in < 2 seconds
- [ ] Pagination for 50+ properties
- [ ] Count shown: "23 properties need review"

---

### US-1.5: Jump to Property

**As an** AM reviewing a property in the side panel
**I want to** quickly jump to that property's full view
**So that** I can see complete details when needed

**Acceptance Criteria:**
- [ ] [Jump to Property] button on each card
- [ ] Click opens property in main view (same tab)
- [ ] Side panel remains open
- [ ] Current scroll position preserved in side panel

---

## Epic 2: Propensity Score Engine

**Goal:** Calculate and display deal closing probability for each property.

---

### US-2.1: NC1 Sentiment Integration

**As a** System
**I want to** pull agent sentiment data from NC1
**So that** I can include engagement signals in propensity calculation

**Acceptance Criteria:**
- [ ] Query NC1 for: sentiment_score, engagement_tags, conversation_quality
- [ ] Map sentiment to weight: positive=1.0, neutral=0.5, negative=0.0
- [ ] Engagement tags include: "agent_guiding", "agent_responsive", "agent_cold"
- [ ] NC1 weight in propensity: 40%

**Data Retrieved:**
```typescript
interface NC1PropensityInput {
  agent_id: string;
  property_id: string;
  sentiment_score: number;  // -1 to 1
  engagement_tags: string[];
  last_positive_response: Date | null;
  response_rate: number;  // 0 to 1
}
```

---

### US-2.2: PC1 Call Quality Integration

**As a** System
**I want to** pull call quality data from PC1
**So that** I can include conversation progression in propensity

**Acceptance Criteria:**
- [ ] Query PC1 for: call_quality_score, conversation_progression, objection_handling
- [ ] Map quality to weight: good_call=1.0, average=0.5, poor=0.0
- [ ] Include number of quality calls (frequency matters)
- [ ] PC1 weight in propensity: 25%

**Data Retrieved:**
```typescript
interface PC1PropensityInput {
  property_id: string;
  total_calls: number;
  quality_calls: number;  // rated "good_call"
  avg_call_duration: number;  // minutes
  conversation_stage: 'initial' | 'engaged' | 'negotiating' | 'closing';
}
```

---

### US-2.3: Pipeline Status Weighting

**As a** System
**I want to** include offer status in propensity calculation
**So that** deals closer to closing get higher scores

**Acceptance Criteria:**
- [ ] Status weight mapping:
  - 80% Under Contract = 1.0
  - 60% In Negotiations = 0.8
  - 50% Contract Submitted = 0.6
  - 30% Offer Terms Sent = 0.4
  - 20% Continue to Follow = 0.2
  - 10% Initial Contact = 0.1
- [ ] Pipeline weight in propensity: 25%

---

### US-2.4: Propensity Score Calculation

**As a** System
**I want to** combine all inputs into a single propensity score
**So that** AMs have one metric to prioritize deals

**Acceptance Criteria:**
- [ ] Formula: `propensity = (NC1 * 0.40) + (PC1 * 0.25) + (Status * 0.25) + (Recency * 0.10)`
- [ ] Recency: days since last contact (1.0 if <2 days, 0.5 if 2-5 days, 0.0 if >5 days)
- [ ] Score mapped to tier:
  - HIGH 🟢: propensity ≥ 0.70
  - MID 🟡: propensity 0.40-0.69
  - LOW 🔴: propensity < 0.40
- [ ] Score recalculated on panel open (not cached)

**Calculation Example:**
```
Property: 123 Main St

NC1: sentiment=0.8, engagement=high → 0.85
PC1: 3 good calls, negotiating stage → 0.80
Status: 60% In Negotiations → 0.80
Recency: Last contact 1 day ago → 1.0

Propensity = (0.85 * 0.40) + (0.80 * 0.25) + (0.80 * 0.25) + (1.0 * 0.10)
           = 0.34 + 0.20 + 0.20 + 0.10
           = 0.84 → HIGH 🟢
```

---

### US-2.5: Propensity Explanation Generation

**As an** AM viewing a propensity score
**I want to** see WHY the property scored that way
**So that** I understand what's driving the recommendation

**Acceptance Criteria:**
- [ ] LLM generates 1-2 sentence explanation
- [ ] Explanation references specific signals
- [ ] Examples:
  - HIGH: "Agent presenting Friday, guiding on terms, conversation progressing well"
  - MID: "Agent responded positively but no contact in 3 days, needs follow-up"
  - LOW: "5 calls made, no response, agent not returning voicemails"
- [ ] Explanation cached for session (regenerate on data change)

---

## Epic 3: Response Detection & Alerts

**Goal:** Surface positive agent responses and flag unanswered follow-ups.

---

### US-3.1: Text/Email Indicator Badge

**As an** AM viewing My Deals
**I want to** see which properties have agent responses
**So that** I can quickly identify engagement

**Acceptance Criteria:**
- [ ] Badge shows next to Critical/Reminders: "• 1 Text/Email"
- [ ] Count = unread positive responses
- [ ] Clicking badge opens side panel filtered to that property
- [ ] Badge color: orange for unread, gray when addressed

**Display Format:**
```
☀️ Medium | To do: Not set | • 0 Critical • 0 Reminders • 1 Text/Email
```

---

### US-3.2: Positive Response Detection

**As a** System
**I want to** identify positive agent responses
**So that** I only surface meaningful engagement signals

**Acceptance Criteria:**
- [ ] Pull response tags from NC1 (sentiment analysis)
- [ ] Positive triggers: "interested", "willing to present", "guiding on approach", "open to negotiation"
- [ ] Negative triggers excluded: "not interested", "already under contract"
- [ ] Only POSITIVE responses trigger Text/Email indicator

**NC1 Integration:**
```typescript
interface ResponseClassification {
  message_id: string;
  channel: 'sms' | 'email';
  sentiment: 'positive' | 'neutral' | 'negative';
  engagement_signals: string[];
  requires_followup: boolean;
}
```

---

### US-3.3: Unanswered Response Flagging

**As an** AM
**I want to** see when an AA hasn't responded to a positive message
**So that** I can ensure no warm lead goes cold

**Acceptance Criteria:**
- [ ] Flag triggered when: positive response exists AND no AA follow-up in 24-48 hours
- [ ] Alert display: "⚠️ Unanswered response (2 days)"
- [ ] Time calculation uses business hours (M-F 8am-6pm)
- [ ] Flag clears when AA responds OR AM marks addressed

---

### US-3.4: Priority Sorting Logic

**As a** System
**I want to** sort Deal Review properties by closing urgency
**So that** AMs see highest-impact deals first

**Acceptance Criteria:**
- [ ] Priority factors (in order):
  1. Offer Status % (higher = higher priority)
  2. Agent Engagement Level (engaged > stalled > cold)
  3. Unanswered Positive Response (urgent flag)
  4. Propensity Score (HIGH > MID > LOW)
- [ ] Many calls with no response = LOWER priority (agent not engaging)
- [ ] Algorithm documented for AM transparency

**Sorting Algorithm:**
```typescript
function priorityScore(property: Property): number {
  const statusWeight = property.offerStatus / 100;  // 0.0 - 0.8
  const engagementWeight = property.engagement === 'engaged' ? 0.3 :
                           property.engagement === 'stalled' ? 0.15 : 0;
  const urgentWeight = property.hasUnansweredResponse ? 0.25 : 0;
  const propensityWeight = property.propensity * 0.2;

  return statusWeight + engagementWeight + urgentWeight + propensityWeight;
}
```

---

## Epic 4: Checklist & AM Actions

**Goal:** Provide AMs with verification workflows and task completion tracking.

---

### US-4.1: Standard Checklist Display

**As an** AM reviewing a property
**I want to** see a checklist of items to verify
**So that** I systematically review all deal components

**Acceptance Criteria:**
- [ ] Checklist items: Notes, Comps, Rehab, ARV, Wholesale
- [ ] Each item clickable to expand details
- [ ] Status: ✓ verified, ⚠️ needs attention, ○ not reviewed
- [ ] IAMaster data shown for Comps/Rehab/ARV

**Display Format:**
```
📋 VERIFICATION CHECKLIST
├── ✓ Notes — Last updated 1 day ago
├── ⚠️ Comps — Only 2 comps found
├── ○ Rehab — $45,000 estimate
├── ○ ARV — $425,000
└── ○ Wholesale — N/A (not wholesale deal)
```

---

### US-4.2: Status-Based Checklist Variations

**As a** System
**I want to** show different checklist items based on offer status
**So that** AMs see relevant verification for each stage

**Acceptance Criteria:**
- [ ] 10% Initial Contact: Has agent been called? Is property qualified?
- [ ] 30-50% Offer Sent: Notes, Comps, ARV, Rehab, Wholesale numbers
- [ ] 60% In Negotiations: Notes, Counter terms, Final numbers, Agent guidance
- [ ] 80% Under Contract: Contract terms, Escrow status, Contingencies

---

### US-4.3: Copy to Notes Function

**As an** AM completing a review
**I want to** copy my review to the property Notes
**So that** AAs can see what was addressed

**Acceptance Criteria:**
- [ ] [Copy to Notes] button on property card
- [ ] Copies: Propensity score, explanation, checklist status, AM comments field
- [ ] Format matches existing Notes style
- [ ] Timestamp and AM name auto-added
- [ ] One-click action (no confirmation modal)

**Output Format:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[AM REVIEW — 01/02/2025 — Bob Martinez]
Propensity: HIGH 🟢
Agent engaged, presenting Friday

Verified: ✓Notes ✓Comps ✓ARV ✓Rehab

AM Comments: ARV was $15K high. Adjusted
with Josh. Rehab estimate solid. Good deal,
push for close.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### US-4.4: AM Comments Input

**As an** AM
**I want to** add comments to my review
**So that** I can explain what was addressed and outcomes

**Acceptance Criteria:**
- [ ] Text area below checklist
- [ ] Placeholder: "Add your comments..."
- [ ] Character limit: 500
- [ ] Auto-saved on blur (no submit button needed)
- [ ] Comments included in Copy to Notes

---

### US-4.5: Review Completion Tracking

**As a** System
**I want to** track which properties have been reviewed by AM
**So that** I can measure AM engagement and coverage

**Acceptance Criteria:**
- [ ] Property marked "reviewed" when Copy to Notes executed
- [ ] Reviewed properties show ✅ badge in Deal Review list
- [ ] AM can re-review (adds new note, doesn't overwrite)
- [ ] Dashboard shows: "18 HIGH propensity, 15 reviewed (83%)"

---

## Epic 5: Pipeline Reports & Forecast

**Goal:** Provide AMs with pipeline intelligence and monthly projections.

---

### US-5.1: Pipeline Summary Generation

**As an** AM clicking [Run Pipeline Report]
**I want to** see a summary of my team's pipeline
**So that** I understand current state at a glance

**Acceptance Criteria:**
- [ ] Total active properties count
- [ ] Breakdown by status (10%, 20%, 30%, 50%, 60%, 80%)
- [ ] Breakdown by propensity (HIGH, MID, LOW)
- [ ] Breakdown by source (MLS, Off-Market, Wholesaler, Seller Direct)
- [ ] Generates in < 5 seconds

---

### US-5.2: Monthly Forecast Calculation

**As an** AM
**I want to** see expected closes for the current month
**So that** I can identify gaps before month-end

**Acceptance Criteria:**
- [ ] Forecast based on historical conversion rates by status
- [ ] Default conversion rates:
  - 80% Under Contract → 85% close
  - 60% In Negotiations → 15% close
  - HIGH Propensity → 40% close
- [ ] Show range: "Expected: 6-8 deals"
- [ ] Compare to goal: "GOAL: 8 deals | GAP: 0-2"

**Forecast Formula:**
```
Expected = (UnderContract * 0.85) + (Negotiations * 0.15) + (HIGH * 0.40)
         = (7 * 0.85) + (15 * 0.15) + (18 * 0.40)
         = 5.95 + 2.25 + 7.2
         = 15.4 (capped at unique properties)
         ≈ 6-8 (with overlap adjustment)
```

---

### US-5.3: Source Analysis & Recommendations

**As an** AM
**I want to** see recommendations for pipeline balance
**So that** I can optimize lead sources

**Acceptance Criteria:**
- [ ] Show current source distribution vs targets
- [ ] Targets: 45% MLS, 25% Off-Market, 20% Wholesaler, 10% Seller Direct
- [ ] Flag imbalances: "MLS overweighted (61% vs 45% target)"
- [ ] Generate recommendations:
  - "NEED MORE OFF-MARKET DEALS: Current 17%, Target 25%"
  - "Assign to: Kevin R. (excels at off-market)"

---

### US-5.4: AA Pipeline Detail

**As an** AM
**I want to** see each AA's individual pipeline stats
**So that** I can identify coaching needs

**Acceptance Criteria:**
- [ ] For each AA show: Active count, HIGH count, Sources breakdown
- [ ] Show AA strength (from expertise tracking)
- [ ] Show individual forecast
- [ ] Flag low performers: "⚠️ Low HIGH propensity - needs coaching"

---

### US-5.5: AA Expertise Tracking

**As a** System
**I want to** identify AA strengths by deal source
**So that** AMs can optimize task assignments

**Acceptance Criteria:**
- [ ] Calculate close rate by AA by source
- [ ] Identify top source for each AA
- [ ] Store as: `AA.strength = { source: 'MLS', evidence: '8/10 closes' }`
- [ ] Show recommendation: "Assign MLS leads to Josh S."
- [ ] Update monthly based on closes

**Expertise Schema:**
```typescript
interface AAExpertise {
  aa_id: string;
  aa_name: string;
  primary_strength: 'MLS' | 'Off-Market' | 'Wholesaler' | 'Seller Direct' | 'Developing';
  strength_evidence: string;
  close_rate_by_source: Record<string, number>;
  total_closes_ytd: number;
}
```

---

## Epic 6: Google Sheets Export

**Goal:** Enable historical data tracking via daily Google Sheets append.

---

### US-6.1: Google Sheets API Integration

**As a** System
**I want to** connect to Google Sheets API
**So that** I can export pipeline data

**Acceptance Criteria:**
- [ ] OAuth2 credentials configured
- [ ] Specific sheet ID stored in config
- [ ] Service account with write permissions
- [ ] Connection tested on deployment

---

### US-6.2: Daily Append Export

**As a** System
**I want to** append a new row daily at 6 PM
**So that** pipeline history is tracked automatically

**Acceptance Criteria:**
- [ ] Scheduled job at 6 PM local time
- [ ] Appends to existing sheet (not overwrite)
- [ ] Row includes date stamp
- [ ] All metrics from US-5.1 included
- [ ] Error notification if append fails

**Row Structure:**
```
| Date | Total | HIGH | MID | LOW | 10% | 30% | 50% | 60% | 80% | MLS | Off | Whsl | SD | Fcst | MTD |
| 2025-01-02 | 127 | 18 | 45 | 64 | 42 | 35 | 28 | 15 | 7 | 78 | 22 | 19 | 8 | 6-8 | 2 |
```

---

### US-6.3: Manual Export Button

**As an** AM
**I want to** manually trigger a Google Sheets export
**So that** I can capture data at any time

**Acceptance Criteria:**
- [ ] [Export to Sheets] button in Pipeline Report view
- [ ] Confirms export with toast: "Exported to FlipIQ Pipeline Tracker"
- [ ] Links to sheet: "View Sheet →"
- [ ] Rate limited: max 1 manual export per hour

---

### US-6.4: Export Error Handling

**As a** System
**I want to** handle export failures gracefully
**So that** data isn't lost

**Acceptance Criteria:**
- [ ] Retry logic: 3 attempts with exponential backoff
- [ ] On failure: queue for next scheduled run
- [ ] Notify AM via UI: "Export failed, will retry at 6 PM"
- [ ] Log errors for debugging
- [ ] No duplicate rows on retry success

---

## Data Schemas

### Propensity Score Schema

```typescript
interface PropensityScore {
  property_id: string;
  score: number;  // 0.0 - 1.0
  tier: 'HIGH' | 'MID' | 'LOW';
  explanation: string;

  components: {
    nc1_sentiment: number;
    pc1_quality: number;
    status_weight: number;
    recency_weight: number;
  };

  calculated_at: Date;
}
```

### Deal Review Item Schema

```typescript
interface DealReviewItem {
  property_id: string;
  address: string;
  price: number;
  source: 'MLS' | 'Off-Market' | 'Wholesaler' | 'Seller Direct';

  aa_id: string;
  aa_name: string;

  offer_status: number;  // 10, 20, 30, 50, 60, 80
  propensity: PropensityScore;

  alerts: {
    unanswered_response: boolean;
    days_since_response: number | null;
    needs_attention_flag: boolean;
  };

  am_review: {
    reviewed: boolean;
    reviewed_at: Date | null;
    reviewed_by: string | null;
  };

  priority_score: number;  // For sorting
}
```

### Pipeline Report Schema

```typescript
interface PipelineReport {
  generated_at: Date;
  team_size: number;

  totals: {
    active: number;
    by_status: Record<string, number>;
    by_propensity: Record<string, number>;
    by_source: Record<string, number>;
  };

  forecast: {
    expected_closes: string;  // "6-8"
    goal: number;
    gap: string;  // "0-2"
  };

  source_analysis: {
    current_distribution: Record<string, number>;
    target_distribution: Record<string, number>;
    recommendations: string[];
  };

  aa_details: AADetail[];

  action_items: {
    urgent: string[];
    this_week: string[];
  };
}

interface AADetail {
  aa_id: string;
  aa_name: string;
  active_count: number;
  high_count: number;
  source_breakdown: Record<string, number>;
  strength: string;
  forecast: string;
  needs_coaching: boolean;
}
```

---

## Story Dependencies

```
US-1.1 → US-1.2 → US-1.3 → US-1.4 (Role → Panel → Filters → List)
US-2.1 + US-2.2 + US-2.3 → US-2.4 → US-2.5 (Inputs → Calculation → Explanation)

US-3.1 + US-3.2 → US-3.3 (Badge + Detection → Flagging)
US-1.4 + US-3.4 → Deal Review sorting

US-4.1 + US-4.2 → US-4.3 (Checklist → Copy to Notes)
US-4.4 → US-4.3 (Comments into Copy)
US-4.3 → US-4.5 (Copy triggers completion)

US-5.1 → US-5.2 + US-5.3 + US-5.4 (Summary → Forecast/Analysis/AA)
US-5.5 → US-5.4 (Expertise → AA detail)

US-6.1 → US-6.2 + US-6.3 (API → Scheduled + Manual)
US-6.4 applies to US-6.2 + US-6.3
```

---

## Acceptance Testing Checklist

- [ ] iQ Button visible only to AM role
- [ ] Side panel opens with Deal Review list
- [ ] Filters in My Deals apply to side panel
- [ ] Propensity scores calculated correctly (spot check 5 properties)
- [ ] Explanations reference specific signals
- [ ] Text/Email badge appears for positive responses
- [ ] Unanswered responses flagged after 24-48 hours
- [ ] Priority sorting places engaged deals first
- [ ] Checklist varies by offer status
- [ ] Copy to Notes creates formatted entry
- [ ] Pipeline Report generates in < 5 seconds
- [ ] Forecast shows expected closes with gap analysis
- [ ] AA expertise tracked and displayed
- [ ] Google Sheets export appends new row
- [ ] Manual export button works

---

**Document Version:** 1.1
**Last Updated:** January 2, 2025
**Status:** Ready for Development
