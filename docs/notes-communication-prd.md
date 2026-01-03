# NOTES & COMMUNICATION BOT

## Product Requirements Document
### Version 2.0 — January 2, 2025

---

| Field | Value |
|-------|-------|
| **Bot ID** | D3 (Notes and Communication) |
| **Category** | Deal Context / Communication |
| **Priority** | P1 — Core Feature |
| **Primary User** | Acquisition Associate (AA) |
| **UI Location** | PIQ → Property Panel → Notes Tab & Agent Profile → Agent Notes |
| **Trigger Method** | Property open / Agent profile open / Ask iQ query |
| **Integration Points** | Dialpad, Gmail/Outlook, Agent365, D2 (Post-Call Bot), OpenAI/Claude |
| **Handoff To** | Eric (PM) / Nate (CTO) / Faizal (UI) |

---

## CRITICAL: Bot Relationship Clarification

> ⚠️ **D3 and D2 (Post-Call Bot) are COMPLEMENTARY, not competing:**
>
> | Bot | Timing | Purpose |
> |-----|--------|---------|
> | **D2** | AFTER each call | Immediate note extraction from transcript |
> | **D3** | BEFORE calls | Aggregates ALL history, provides patterns and context |
>
> D3 consumes D2's output as one of its data sources. They work together.

---

## 1. EXECUTIVE SUMMARY

### 1.1 Problem Statement

Acquisition Associates manage **75+ active properties** simultaneously. Each property has a unique communication history spanning calls, texts, emails, and internal notes across multiple team members. Critical context gets lost across channels, leading to:

- Embarrassing duplicate outreach (asking questions already answered)
- Missed follow-ups due to lack of time to review all notes
- Inability to connect notes with property variables and agent patterns
- **2-3 hours daily wasted** on context-switching (5-10 min per property × 30+ calls)
- Lost institutional knowledge when AAs don't see other AAs' agent interactions

### 1.2 Solution

An **AI-powered Notes & Communication Bot** that provides **AGENT-FIRST** intelligence, aggregating all communication history across all properties and all AAs to surface patterns, context, and actionable next steps.

### 1.3 Core Capabilities

| Capability | Description |
|------------|-------------|
| Unified Timeline | Aggregates calls, texts, emails, notes per agent across ALL properties |
| Agent Pattern Detection | Identifies communication preferences, behavior patterns, offer preferences |
| Dynamic Context | Shows only what's RELEVANT to the current deal at the current stage |
| Agent 365 Integration | Surfaces buyer agent network, investor relationships, transaction history |
| Ask iQ | Free-form questions about any agent or property |
| Smart Reminders | Prompts status updates without auto-modifying data |

### 1.4 Success Metric

**Each AA closes 2 deals per month** — this bot directly supports that goal by eliminating context-switching overhead and ensuring no follow-up falls through the cracks.

### 1.5 Critical Architecture Insight

> ⚠️ **AGENT-FIRST ARCHITECTURE:** The Agent is the primary entity. All notes, emails, SMS, and patterns are aggregated at the AGENT level across ALL properties and ALL AAs. When viewing any property, NC1 first pulls the agent's complete history, then filters to what's relevant for THIS deal.

---

## 2. USER STORIES

### 2.1 Context Retrieval

**AS A** busy AA about to call an agent
**I WANT TO** see a 30-second summary of all prior interactions with this agent (across all properties and AAs)
**SO THAT** I sound informed, don't repeat questions already answered, and know the key points

### 2.2 Pattern Recognition

**AS AN** AA calling an agent I've never spoken to
**I WANT TO** see how other AAs have interacted with this agent and what worked
**SO THAT** I can leverage institutional knowledge and avoid known pitfalls

### 2.3 Note Capture

**AS AN** AA finishing a call
**I WANT TO** quickly capture notes without typing paragraphs
**SO THAT** I can move to my next call immediately while context is fresh

### 2.4 Follow-up Tracking

**AS AN** AA who promised to "call back Tuesday"
**I WANT** the system to automatically create a reminder
**SO THAT** I never forget commitments made during conversations

### 2.5 Agent Relationship Building

**AS AN** AA reaching out to an agent (not about a specific property)
**I WANT TO** see a full relationship summary and suggested talking points
**SO THAT** I can build relationships strategically based on complete history

### 2.6 Offer Progression

**AS AN** AA who hasn't received a response from an agent
**I WANT** the system to prompt me to send a self-represented offer after 2 days
**SO THAT** no deal is lost due to lack of agent response

---

## 3. FEATURE SPECIFICATIONS

### 3.1 Unified Communication Timeline

**Trigger:** AA opens any property in PIQ OR opens Agent Profile

**Data Sources:**

| Source | Data Retrieved |
|--------|----------------|
| Dialpad API | Calls + Transcripts + SMS |
| Gmail/Outlook API | Email threads (matched by agent email) |
| Internal Notes | Manual AA entries with property_id links |
| PC1 (Post-Call Bot) | Call summaries, next steps |
| PIQ Bot | DFI, Property Intel, Seller Pain Score |
| D2 Agent Bot | Agent profile, ISC, transaction history |
| IAMaster | Buy box alignment, pricing strategy |
| Agent 365 Report | Full transaction history, buyer network |

**Output Format:**

```
📝 Josh Santos | 123 Oak St | 6 months ago [View Property →]
"Called Barry - he said he doesn't double-end. Wants best & highest only."

📧 Maria Lopez | 456 Elm Ave | 3 months ago [View Property →]
"Texted POF - responded in 2 hours. Prefers text over calls."

📞 Anas Aqeel | 789 Pine Dr | Today [View Property →]
"Finally connected! He's interested. Sending POF now."
```

---

### 3.2 AI Context Summary

**Trigger:** AA clicks "Call" button OR toggles Transcribe button

**Output Structure:**

```
STATUS: [Current deal status - e.g., "Interested - waiting on POF"]

KEY POINTS:
• [Seller pain summary from PropertyRadar]
• [Agent behavior summary from patterns]
• [Questions asked/unanswered]
• [Commitments made by either party]

NEXT STEPS:
□ [Communication action 1]
□ [Communication action 2]

OFFER NEXT STEPS:
□ [Offer progression action]
□ [Fallback if no response]
```

---

### 3.3 Smart Note Capture

**Quick-Select Options:**

```
[Interested] [Not Interested] [Call Back] [Left VM] [Wrong Number]
[Sent Docs] [Scheduled Showing] [Made Offer] [Counter Received]
[Best & Highest] [Double-End Confirmed] [Double-End Rejected]
```

**Processing Logic:**
1. AA speaks or types note
2. LLM parses for: Action items, Follow-up dates, Agent sentiment
3. Auto-creates reminders for action items
4. Prompts AA to update agent status (does NOT auto-update)

---

### 3.4 Action Item Tracker

**Output Format:**

```
📋 OPEN ACTION ITEMS — Barry Tobin

⏰ OVERDUE
□ Send proof of funds (Due: 12/29) ❗

📅 TODAY
□ Call back per his request
□ Confirm double-end decision

📅 THIS WEEK
□ Follow up on counter-offer response
```

---

### 3.5 Agent Pattern Detection (CRITICAL FEATURE)

> 🎯 **This is the CORE feature of NC1.** Pattern Detection aggregates ALL historical data about an agent to surface actionable intelligence.

**Two Levels:**

| Level | Location | Purpose |
|-------|----------|---------|
| AGENT Level | Agent Profile → Agent Notes | Complete agent intelligence |
| PROPERTY Level | Property PIQ → Notes | Only what's relevant for THIS deal |

**Pattern Categories:**

| Pattern Type | What to Detect | Sample Output |
|--------------|----------------|---------------|
| Communication Preference | Which channel gets responses | "Prefers TEXT (2hr avg response)" |
| Response Timing | When do they respond | "Best time: Afternoons (12pm-4pm)" |
| Behavior Pattern | Double-end? Best & highest? | "Does NOT double-end (stated 2x)" |
| Objection Pattern | What do they push back on | "Always negotiates on timeline" |
| Previous AA Context | Who talked to them about what | "Josh talked 6mo ago about 123 Oak" |
| Offer Preferences | How they handle offers | "Always wants best & highest" |

**Agent Classification Variables:**

| Variable | Values | Source |
|----------|--------|--------|
| Works with Investors | YES / RARELY / NO | Agent365 Investor Source Count |
| Value Tier | HIGH / MID / LOW | ISC thresholds (≥10/5-9/<5) |
| One-Off Flag | TRUE / FALSE | Price variance >30% from typical |

**Dynamic Filtering (Property Level):**

| Condition | Action |
|-----------|--------|
| Already confirmed in conversation | HIDE |
| Action already completed | HIDE |
| Agent pattern applies | SHOW |
| New info AA doesn't know | SHOW |

---

### 3.6 Ask iQ Feature

**Purpose:** Allow AA to ask free-form questions about any agent or property.

**Example Queries:**

| User Question | System Response |
|---------------|-----------------|
| "What should I ask about this property?" | "Don't ask double-end. Go to best & highest." |
| "Has he worked with us before?" | "Yes - Josh talked to him 6mo ago about 123 Oak St." |
| "Should I call or text?" | "TEXT - he's ignored 4 calls but responds to texts in 2hr." |

---

### 3.7 Cross-Property Note Linking

**Data Structure:**

```typescript
interface AgentNote {
  note_id: string;
  agent_id: string;
  property_id: string | null;  // Links to source property
  aa_id: string;
  content: string;
  timestamp: Date;
  type: 'communication' | 'critical' | null;
  channel: 'call' | 'email' | 'sms' | 'note';
}
```

---

### 3.8 One-Off Detection Logic

```
IF abs(agent.avg_listing_price - this_property.price) / agent.avg_listing_price > 0.30
THEN flag: "ONE-OFF - This property is [X%] below their typical price range"
```

---

### 3.9 Offer Next Steps Logic

> 🚨 **CRITICAL: Never let no response = no offer**

| Condition | Offer Next Step |
|-----------|-----------------|
| Agent said "no double-end" | "Go straight to best & highest" |
| No response after 2 days | "Send self-represented offer" |
| Agent prefers text | "TEXT the offer terms" |
| Agent has buyer's agent | "Backup: Call [Buyer Agent Name]" |

---

### 3.10 Status Update Reminder System

**Purpose:** NC1 suggests status updates but **NEVER auto-updates**. AA must manually update.

**First Prompt:**

```
💡 SUGGESTED UPDATE
Agent Status is: COLD
Conversation was: Positive engagement
Consider updating to: WARM
[Remind Me Later] [Skip] [I'll Update Now]
```

---

## 4. DATA ARCHITECTURE

### 4.1 Entity Relationship

```
AGENTS (Primary Entity)
├── agent_id (PK)
├── name, email, phone, office
├── relationship_status, rating, basket
├── Agent 365 Report (embedded or linked)
│
├── AGENT_NOTES (1:Many)
│   ├── note_id, agent_id, property_id (nullable)
│   ├── aa_id, content, timestamp, type, channel
│   └── Links to source property if applicable
│
├── AGENT_SMS (1:Many) - via Dialpad
│
├── AGENT_EMAILS (1:Many) - via Gmail/Outlook
│
└── PROPERTIES (Many:Many via listing_agent_id)
```

### 4.2 API Integrations Required

| API | Purpose | Priority |
|-----|---------|----------|
| Dialpad API | Call logs, transcripts, SMS | P0 |
| Gmail API | Email sync (OAuth per AA) | P1 (Phase 2) |
| Outlook API | Email sync (OAuth per AA) | P1 (Phase 2) |
| OpenAI/Claude API | LLM for summarization, patterns | P0 |
| Agent 365 Report | Transaction history, buyer network | P0 |
| PropertyRadar API | Seller distress data | P0 |

---

## 5. SUCCESS METRICS

| Metric | Target | Measurement |
|--------|--------|-------------|
| Context Prep Time | <30 seconds | Time from property click to call initiation |
| Note Capture Rate | 95% of calls | % of calls with notes logged within 5 min |
| Action Item Completion | 90% | % of AI-extracted items marked complete |
| Pattern Detection Accuracy | 80% | User feedback on pattern relevance |
| Offer Submission Rate | 100% | No deal without offer (self-rep if needed) |

---

## 6. DEVELOPMENT PLAN

> **Note:** Timelines reflect BMAD Method + Claude Code (AI-assisted development)

### Phase 0: API Verification — 2 hrs (BLOCKER)

| Task | Hours |
|------|-------|
| 0.1 Dialpad API access confirmation | 0.5 |
| 0.2 Agent365 Report API confirmation | 0.5 |
| 0.3 Database schema review | 0.5 |
| 0.4 PC1 integration point verification | 0.5 |

---

### Epic 1: Notes Aggregation & Timeline — 6 hrs

| Task | Hours |
|------|-------|
| 1.1 Agent-first data model setup | 1 |
| 1.2 Notes query across all properties | 1 |
| 1.3 PC1 (Post-Call Bot) output integration | 1 |
| 1.4 Timeline UI component | 1.5 |
| 1.5 Cross-property linking | 1 |
| 1.6 Note display formatting | 0.5 |

---

### Epic 2: Agent365 Integration — 4 hrs

| Task | Hours |
|------|-------|
| 2.1 Agent365 Report API integration | 1 |
| 2.2 Buyer agent network extraction | 1 |
| 2.3 Transaction history parsing | 1 |
| 2.4 Classification variables calculation | 1 |

---

### Epic 3: Pattern Detection Engine — 8 hrs

| Task | Hours |
|------|-------|
| 3.1 Communication pattern analysis | 1.5 |
| 3.2 Behavior pattern extraction | 1.5 |
| 3.3 One-off detection logic | 1 |
| 3.4 Dynamic filtering rules | 1.5 |
| 3.5 Agent-level pattern aggregation | 1 |
| 3.6 Property-level pattern filtering | 1 |
| 3.7 Pattern UI display | 0.5 |

---

### Epic 4: AI Summary & Ask iQ — 6 hrs

| Task | Hours |
|------|-------|
| 4.1 Context summary prompt engineering | 1.5 |
| 4.2 Summary generation pipeline | 1 |
| 4.3 Ask iQ query handling | 1.5 |
| 4.4 Response generation | 1 |
| 4.5 UI integration | 1 |

---

### Epic 5: Action Items & Reminders — 4 hrs

| Task | Hours |
|------|-------|
| 5.1 Action item extraction logic | 1 |
| 5.2 Reminder creation system | 1 |
| 5.3 Status update prompts | 1 |
| 5.4 Offer next steps logic | 1 |

---

### Epic 6: UI Integration & Polish — 4 hrs

| Task | Hours |
|------|-------|
| 6.1 Notes tab restructure | 1 |
| 6.2 Agent Pattern tab | 1 |
| 6.3 Quick-select note buttons | 0.5 |
| 6.4 Status update modals | 0.5 |
| 6.5 Testing & edge cases | 1 |

---

### Timeline Summary

| Phase | Hours | Deliverable |
|-------|-------|-------------|
| Phase 0: API Verification | 2 hrs | Go/no-go decision |
| Epic 1: Notes Aggregation | 6 hrs | Unified timeline live |
| Epic 2: Agent365 Integration | 4 hrs | Agent data enriched |
| Epic 3: Pattern Detection | 8 hrs | Patterns surfaced |
| Epic 4: AI Summary & Ask iQ | 6 hrs | Context summaries live |
| Epic 5: Action Items | 4 hrs | Reminders working |
| Epic 6: UI Integration | 4 hrs | Full UI complete |
| **TOTAL** | **34 hrs (~4-5 days)** | **Bot fully deployed** |

> **Note:** Original PRD stated 44 hours / 7 days. Optimized to 34 hours (~4-5 days) with BMAD + Claude Code efficiency. Epic 3 (Pattern Detection) is the critical path.

---

## 7. RISKS & MITIGATIONS

| Risk | Impact | Mitigation |
|------|--------|------------|
| Dialpad API access | HIGH | Confirm credentials Day 0; fallback to manual notes |
| Email OAuth complexity | MEDIUM | Start with notes-only MVP, add email Phase 2 |
| LLM hallucinations | MEDIUM | Add "View Full History" escape hatch |
| Pattern detection inaccuracy | MEDIUM | Require 2+ data points for patterns |
| PC1 integration issues | MEDIUM | Define clear data contract with PC1 |

---

## 8. ACCEPTANCE CRITERIA

- [ ] AC1: Timeline shows notes from ALL AAs for an agent
- [ ] AC2: Notes link back to source property
- [ ] AC3: Pattern detection identifies communication preferences
- [ ] AC4: One-off listings flagged correctly
- [ ] AC5: Ask iQ returns relevant answers
- [ ] AC6: Action items extracted from notes
- [ ] AC7: Reminders created for commitments
- [ ] AC8: Status update prompts appear (but don't auto-update)
- [ ] AC9: Offer next steps always present
- [ ] AC10: Context summary < 30 seconds to generate
- [ ] AC11: PC1 output integrated into timeline
- [ ] AC12: Agent365 data displayed in patterns

---

## 9. INTEGRATION POINTS

### 9.1 Consumes From:

| Bot/System | Data Consumed |
|------------|---------------|
| PC1 (Post-Call Bot) | Call summaries, next steps from transcripts |
| D2 (Agent Bot) | Agent profile, ISC scores |
| IAMaster (Investment) | Buy box alignment, pricing strategy |
| PIQ Bot | Property intel, seller distress |
| Agent365 | Transaction history, buyer network |
| Dialpad | Call logs, SMS |

### 9.2 Provides To:

| Consumer | Data Provided |
|----------|---------------|
| AA UI | Context summaries, patterns, action items |
| Management Dashboard | Communication metrics, follow-up adherence |

---

## HANDOFF SUMMARY

### For Eric (PM)

| Item | Detail |
|------|--------|
| Problem | AAs waste 2-3 hrs daily on context-switching |
| Solution | Agent-first pattern detection + unified timeline |
| Key Feature | Pattern Detection across all properties/AAs |
| Integration | Works WITH PC1 (Post-Call), not competing |
| **Timeline** | **34 hrs (~4-5 days) with BMAD + Claude Code** |

### For Nate (CTO)

| Item | Detail |
|------|--------|
| Architecture | Agent-first: aggregate at agent level, filter at property level |
| APIs | Dialpad, Agent365, OpenAI/Claude (Gmail/Outlook Phase 2) |
| Critical Path | Epic 3 (Pattern Detection) — 8 hrs |
| PC1 Integration | NC1 consumes PC1 output for call summaries |
| **Timeline** | **34 hrs (~4-5 days) with BMAD + Claude Code** |

---

**Document prepared for FlipIQ Engineering**
**Version 2.0 — January 2, 2025**
