# Notes & Communication Bot — User Stories
## Version 2.0 — BMAD iQ
### January 2, 2025

---

## Document Overview

| Field | Value |
|-------|-------|
| **Total Epics** | 6 |
| **Total User Stories** | 32 |
| **Primary User** | Acquisition Associate (AA) |
| **Secondary Users** | Team Manager |
| **Dependencies** | Dialpad, Agent365, D2 (Post-Call Bot), OpenAI/Claude |

---

## Epic Overview

| # | Epic | Stories | Focus |
|---|------|---------|-------|
| 1 | Unified Timeline | 6 | Note aggregation, cross-property linking |
| 2 | Agent Pattern Detection | 7 | Communication patterns, behavior analysis |
| 3 | AI Context Summary | 5 | Pre-call intelligence, dynamic filtering |
| 4 | Ask iQ Feature | 4 | Free-form queries |
| 5 | Action Items & Reminders | 5 | Follow-up tracking, status prompts |
| 6 | Note Capture | 5 | Quick-select, PC1 integration |

---

## Critical Integration Note

> ⚠️ **NC1 works WITH PC1 (Post-Call Bot), not competing:**
> - PC1 generates notes AFTER each call
> - NC1 aggregates and analyzes notes BEFORE calls
> - NC1 consumes PC1's output as a data source

---

## Epic 1: Unified Timeline

**Goal:** Aggregate all communication history for an agent across all properties and all AAs.

---

### US-1.1: Agent-Centric Data Model

**As a** System
**I want to** store notes at the agent level with optional property links
**So that** I can aggregate all interactions with an agent regardless of property

**Acceptance Criteria:**
- [ ] Notes table has agent_id as primary key relationship
- [ ] property_id is nullable (agent-only notes allowed)
- [ ] aa_id tracked for every note
- [ ] channel tracked (call, email, sms, note)
- [ ] timestamp for chronological sorting

**Data Structure:**
```typescript
interface AgentNote {
  note_id: string;
  agent_id: string;
  property_id: string | null;
  aa_id: string;
  content: string;
  timestamp: Date;
  type: 'communication' | 'critical' | null;
  channel: 'call' | 'email' | 'sms' | 'note';
  source_bot?: 'PC1' | 'manual' | null;
}
```

---

### US-1.2: Cross-Property Note Query

**As an** AA opening a property
**I want to** see ALL notes for the listing agent (from all properties)
**So that** I have complete context about this agent

**Acceptance Criteria:**
- [ ] Query returns notes for agent_id, not property_id
- [ ] Notes from other AAs are included
- [ ] Notes sorted by timestamp (most recent first)
- [ ] Source property linked for each note
- [ ] PC1 outputs included in timeline

---

### US-1.3: Note Display with Property Context

**As an** AA
**I want to** see which property each note came from
**So that** I can understand the full context of past interactions

**Acceptance Criteria:**
- [ ] Each note shows: AA name, property address, time ago
- [ ] "View Property →" link opens source property
- [ ] Notes without property_id show "Agent-only note"
- [ ] Channel indicator (📞 📧 💬 📝)

**Display Format:**
```
📞 Josh Santos | 123 Oak St | 6 months ago [View Property →]
"Called Barry - he said he doesn't double-end. Wants best & highest only."
```

---

### US-1.4: PC1 Integration

**As a** System
**I want to** pull PC1 (Post-Call Bot) outputs into the timeline
**So that** call summaries are included in agent history

**Acceptance Criteria:**
- [ ] PC1 outputs marked with source_bot = 'PC1'
- [ ] Call duration and quality rating shown
- [ ] Next steps from PC1 integrated
- [ ] No duplicate notes (PC1 vs manual)

---

### US-1.5: Timeline Filtering

**As an** AA
**I want to** filter the timeline by channel or date range
**So that** I can focus on specific types of communication

**Acceptance Criteria:**
- [ ] Filter by channel: All, Calls, Emails, SMS, Notes
- [ ] Filter by date: Last 7 days, 30 days, 90 days, All time
- [ ] Filter by AA: All AAs, Just me
- [ ] Filter state persists during session

---

### US-1.6: Timeline Performance

**As a** System
**I want to** load the timeline efficiently
**So that** AAs don't wait when opening properties

**Acceptance Criteria:**
- [ ] Timeline loads in < 2 seconds
- [ ] Pagination for agents with 50+ notes
- [ ] Most recent 20 notes loaded initially
- [ ] "Load more" for historical notes

---

## Epic 2: Agent Pattern Detection

**Goal:** Analyze historical data to surface actionable patterns about agent behavior.

---

### US-2.1: Communication Preference Detection

**As an** AA
**I want to** know which channel the agent prefers
**So that** I can reach them effectively

**Acceptance Criteria:**
- [ ] Analyze response rates by channel (call, text, email)
- [ ] Calculate average response time per channel
- [ ] Identify preferred channel based on response rate
- [ ] Display: "Prefers TEXT (2hr avg response)"

**Detection Logic:**
```
For each channel:
  response_rate = responses / attempts
  avg_response_time = sum(response_times) / responses

preferred_channel = channel with highest response_rate
```

---

### US-2.2: Response Timing Analysis

**As an** AA
**I want to** know when the agent is most responsive
**So that** I can time my outreach optimally

**Acceptance Criteria:**
- [ ] Cluster response timestamps by time of day
- [ ] Identify peak response window
- [ ] Display: "Best time: Afternoons (12pm-4pm)"
- [ ] Minimum 3 data points required

---

### US-2.3: Behavior Pattern Extraction

**As an** AA
**I want to** know the agent's offer preferences
**So that** I don't waste time on approaches they reject

**Acceptance Criteria:**
- [ ] Detect double-end stance from notes
- [ ] Detect offer format preference (best & highest vs direct)
- [ ] Detect POF requirements
- [ ] Display patterns with evidence count: "Does NOT double-end (stated 2x)"

**Keywords to Detect:**
| Pattern | Keywords |
|---------|----------|
| No Double-End | "no double-end", "won't dual agency", "don't do that" |
| Best & Highest | "best and highest", "b&h", "highest and best" |
| POF Required | "proof of funds", "pof", "need to see funds" |

---

### US-2.4: Previous AA Context

**As an** AA calling an agent I've never spoken to
**I want to** see which AAs have talked to this agent before
**So that** I can leverage their experience

**Acceptance Criteria:**
- [ ] List previous AAs with interaction count
- [ ] Show most recent interaction date
- [ ] Link to source properties
- [ ] Display: "Josh talked 6mo ago about 123 Oak - Lost: timing"

---

### US-2.5: Agent Classification

**As an** AA
**I want to** see the agent's classification tier
**So that** I know how much effort to invest

**Acceptance Criteria:**
- [ ] Calculate from Agent365 Investor Source Count
- [ ] Tiers: HIGH (≥10), MID (5-9), LOW (<5)
- [ ] Show "Works with Investors: YES/RARELY/NO"
- [ ] Show "Sells TO Investors: X deals"

---

### US-2.6: One-Off Detection

**As an** AA
**I want to** know if this property is unusual for the agent
**So that** I can adjust my approach accordingly

**Acceptance Criteria:**
- [ ] Calculate price variance from agent's average
- [ ] Flag if variance > 30%
- [ ] Display: "ONE-OFF: This $425K listing is 62% below typical"
- [ ] Explain implication: "May be less experienced with investor deals"

**Logic:**
```
variance = abs(property.price - agent.avg_listing_price) / agent.avg_listing_price
if variance > 0.30:
  flag_one_off = True
```

---

### US-2.7: Dynamic Pattern Filtering

**As a** System
**I want to** filter patterns based on conversation state
**So that** I only show relevant information

**Acceptance Criteria:**
- [ ] Hide patterns already confirmed in conversation
- [ ] Hide actions already completed
- [ ] Show new info AA doesn't know
- [ ] Show actionable next steps

**Filtering Rules:**
| Condition | Action |
|-----------|--------|
| Double-end already discussed | Hide "ask about double-end" |
| POF already sent | Hide "send POF" |
| Agent preference unknown | Show pattern |
| Previous AA interaction | Show context |

---

## Epic 3: AI Context Summary

**Goal:** Generate concise, actionable summaries before each interaction.

---

### US-3.1: Pre-Call Context Generation

**As an** AA about to make a call
**I want to** see a 30-second summary of what I need to know
**So that** I sound informed without reading all notes

**Acceptance Criteria:**
- [ ] Trigger on "Call" button click
- [ ] Generate in < 5 seconds
- [ ] Include: status, key points, next steps
- [ ] Max 200 words

**Output Structure:**
```
STATUS: [Current deal status]

KEY POINTS:
• [Seller pain summary]
• [Agent behavior summary]
• [Open questions]

NEXT STEPS:
□ [Action 1]
□ [Action 2]
```

---

### US-3.2: Seller Context Integration

**As an** AA
**I want to** see seller distress signals in the summary
**So that** I can tailor my approach

**Acceptance Criteria:**
- [ ] Pull from PropertyRadar data
- [ ] Include: NOD, tax default, divorce, probate
- [ ] Summarize: "Seller pain: NOD filed 45 days ago"
- [ ] Link to full PropertyRadar report

---

### US-3.3: Agent Behavior Summary

**As an** AA
**I want to** see agent behavior patterns in the summary
**So that** I know how to approach them

**Acceptance Criteria:**
- [ ] Include communication preference
- [ ] Include offer format preference
- [ ] Include known objections
- [ ] One line per pattern

---

### US-3.4: Open Questions Tracking

**As an** AA
**I want to** see questions that haven't been answered yet
**So that** I can follow up on them

**Acceptance Criteria:**
- [ ] Extract questions from notes
- [ ] Track answered vs unanswered
- [ ] Display unanswered questions
- [ ] Clear when answered

---

### US-3.5: Offer Next Steps

**As an** AA
**I want to** always see the next step to progress the offer
**So that** no deal stalls due to unclear next action

**Acceptance Criteria:**
- [ ] Always include offer progression action
- [ ] Include fallback if no response
- [ ] Include deadline if known
- [ ] "No response after 2 days → self-rep offer"

---

## Epic 4: Ask iQ Feature

**Goal:** Allow free-form questions about agents and properties.

---

### US-4.1: Ask iQ Input

**As an** AA
**I want to** type a question about an agent in natural language
**So that** I can get specific information quickly

**Acceptance Criteria:**
- [ ] Chat-style input box
- [ ] Submit with Enter or "Ask iQ" button
- [ ] Question history shown
- [ ] Clear input after submit

---

### US-4.2: Question Processing

**As a** System
**I want to** understand the AA's question and find relevant data
**So that** I can provide accurate answers

**Acceptance Criteria:**
- [ ] Parse question intent
- [ ] Retrieve relevant notes, patterns, Agent365 data
- [ ] Pass context to LLM
- [ ] Generate focused response

**Example Queries:**
| Question | Data Retrieved |
|----------|----------------|
| "Should I call or text?" | Response rates by channel |
| "Has he worked with us before?" | Previous AA interactions |
| "What's his commission preference?" | Notes mentioning commission |

---

### US-4.3: Answer Generation

**As an** AA
**I want to** receive a concise, actionable answer
**So that** I can use the information immediately

**Acceptance Criteria:**
- [ ] Answer in < 3 seconds
- [ ] Max 100 words
- [ ] Cite source when possible
- [ ] Admit when information not available

---

### US-4.4: Question Context Awareness

**As a** System
**I want to** consider the current property context
**So that** answers are relevant to the current deal

**Acceptance Criteria:**
- [ ] Include property context in LLM prompt
- [ ] Differentiate agent-level vs property-level questions
- [ ] Consider deal stage in response
- [ ] Reference current property when relevant

---

## Epic 5: Action Items & Reminders

**Goal:** Extract and track follow-up commitments automatically.

---

### US-5.1: Commitment Detection

**As a** System
**I want to** detect commitments in notes and conversations
**So that** action items are created automatically

**Acceptance Criteria:**
- [ ] Detect AA commitments: "I'll send...", "I'll call..."
- [ ] Detect agent requests: "Call me back...", "Send me..."
- [ ] Extract due dates when mentioned
- [ ] Create action items with reasonable defaults

**Detection Patterns:**
| Pattern | Action Item |
|---------|-------------|
| "I'll send POF" | "Send POF to agent" |
| "Call me Tuesday" | "Call back" due Tuesday |
| "Best & highest by Friday" | "Submit offer" due Friday |

---

### US-5.2: Action Item Display

**As an** AA
**I want to** see my open action items organized by urgency
**So that** I know what to do next

**Acceptance Criteria:**
- [ ] Group: Overdue, Today, This Week, Later
- [ ] Show agent name and property
- [ ] Checkbox to mark complete
- [ ] Overdue items highlighted

---

### US-5.3: Reminder Creation

**As a** System
**I want to** create reminders for action items
**So that** AAs are notified at the right time

**Acceptance Criteria:**
- [ ] Auto-create reminder for due date
- [ ] Morning reminder for "today" items
- [ ] Notification in UI
- [ ] Optional email/SMS reminder (Phase 2)

---

### US-5.4: Status Update Prompts

**As a** System
**I want to** suggest status updates after conversations
**So that** agent data stays current

**Acceptance Criteria:**
- [ ] Detect sentiment change in notes
- [ ] Suggest status update (Cold → Warm, etc.)
- [ ] Display prompt with options: Update, Skip, Remind Later
- [ ] NEVER auto-update — AA must confirm

**Prompt Format:**
```
💡 SUGGESTED UPDATE
Agent Status is: COLD
Conversation was: Positive
Consider updating to: WARM
[Remind Later] [Skip] [Update Now]
```

---

### US-5.5: Offer Progression Tracking

**As an** AA
**I want to** see offer next steps always visible
**So that** deals never stall

**Acceptance Criteria:**
- [ ] Track current offer stage
- [ ] Show next action to progress
- [ ] Show fallback if no response
- [ ] "No response 2 days → self-rep offer"

---

## Epic 6: Note Capture

**Goal:** Make it easy to capture notes quickly after calls.

---

### US-6.1: Quick-Select Outcomes

**As an** AA finishing a call
**I want to** select common outcomes with one click
**So that** I can capture notes quickly

**Acceptance Criteria:**
- [ ] Buttons for common outcomes
- [ ] Click adds to note
- [ ] Multiple selections allowed
- [ ] Custom text still available

**Quick-Select Options:**
```
[Interested] [Not Interested] [Call Back] [Left VM] [Wrong Number]
[Sent Docs] [Scheduled Showing] [Made Offer] [Counter Received]
[Best & Highest] [Double-End Confirmed] [Double-End Rejected]
```

---

### US-6.2: PC1 Output Integration

**As an** AA
**I want to** see PC1-generated notes in the note input
**So that** I can review and edit before saving

**Acceptance Criteria:**
- [ ] PC1 output pre-populates note field
- [ ] AA can edit before saving
- [ ] Source marked as "PC1" if unchanged
- [ ] Source marked as "manual" if edited

---

### US-6.3: Property Linking

**As an** AA
**I want to** link notes to specific properties
**So that** context is preserved

**Acceptance Criteria:**
- [ ] Default to current property if in PIQ
- [ ] Allow agent-only notes (no property)
- [ ] Property selector dropdown
- [ ] Link visible in timeline

---

### US-6.4: Note Type Classification

**As an** AA
**I want to** mark notes as "Communication" or "Critical"
**So that** important notes stand out

**Acceptance Criteria:**
- [ ] Checkbox for "Communication"
- [ ] Checkbox for "Critical"
- [ ] Critical notes highlighted in timeline
- [ ] Filter by type available

---

### US-6.5: Voice-to-Text (Phase 2)

**As an** AA
**I want to** dictate notes by voice
**So that** I can capture notes hands-free

**Acceptance Criteria:**
- [ ] Microphone button in note input
- [ ] Real-time transcription
- [ ] Edit before saving
- [ ] Works on desktop and mobile

---

## Data Schemas

### Note Schema

```typescript
interface AgentNote {
  note_id: string;
  agent_id: string;
  property_id: string | null;
  aa_id: string;
  content: string;
  timestamp: Date;
  type: 'communication' | 'critical' | null;
  channel: 'call' | 'email' | 'sms' | 'note';
  source_bot: 'PC1' | 'manual' | null;
  quick_select_tags: string[];
}
```

### Pattern Schema

```typescript
interface AgentPattern {
  agent_id: string;

  // Communication
  preferred_channel: 'call' | 'text' | 'email';
  avg_response_time_text: number | null;  // minutes
  avg_response_time_call: number | null;
  best_time_of_day: string | null;  // "Afternoons (12pm-4pm)"
  response_rate_by_channel: Record<string, number>;

  // Behavior
  double_end: 'yes' | 'no' | 'sometimes' | 'unknown';
  offer_format: 'best_highest' | 'direct' | 'unknown';
  pof_requirement: 'before_showing' | 'with_offer' | 'unknown';

  // Classification
  value_tier: 'HIGH' | 'MID' | 'LOW';
  works_with_investors: 'yes' | 'rarely' | 'no';
  sells_to_investors: number;

  // Context
  previous_aa_interactions: AAInteraction[];
  one_off_flag: boolean;
  one_off_reason: string | null;

  last_updated: Date;
}

interface AAInteraction {
  aa_id: string;
  aa_name: string;
  property_id: string;
  property_address: string;
  last_interaction: Date;
  outcome: string;
}
```

### Action Item Schema

```typescript
interface ActionItem {
  item_id: string;
  agent_id: string;
  property_id: string | null;
  aa_id: string;

  description: string;
  due_date: Date;
  status: 'pending' | 'completed' | 'overdue';

  source: 'detected' | 'manual';
  source_note_id: string | null;

  created_at: Date;
  completed_at: Date | null;
}
```

---

## Story Dependencies

```
US-1.1 → US-1.2 → US-1.3 (Data model to query to display)
US-1.4 → US-1.2 (PC1 integration feeds timeline)

US-2.1 + US-2.2 + US-2.3 → US-2.7 (Patterns feed filtering)
US-2.5 + US-2.6 → US-3.3 (Classification feeds summary)

US-3.1 uses US-2.* (Summary uses patterns)
US-4.2 uses US-1.* + US-2.* (Ask iQ uses timeline + patterns)

US-5.1 → US-5.2 → US-5.3 (Detection to display to reminder)
US-6.2 requires PC1 integration (US-1.4)
```

---

## Acceptance Testing Checklist

- [ ] Timeline shows notes from all AAs for an agent
- [ ] Notes link to source property correctly
- [ ] Communication preference detected with 3+ data points
- [ ] One-off listings flagged when price variance > 30%
- [ ] Ask iQ returns relevant answers in < 3 seconds
- [ ] Action items extracted from "I'll..." and "Call me..." patterns
- [ ] Status update prompts appear but don't auto-update
- [ ] Quick-select buttons create proper notes
- [ ] PC1 output integrates into timeline
- [ ] Context summary generates in < 5 seconds
- [ ] Offer next steps always visible

---

**Document Version:** 2.0
**Last Updated:** January 2, 2025
**Status:** Ready for Development
