# Post-Call & Practice Bot — User Stories
## Version 1.0 — BMAD iQ
### December 31, 2024

---

## Document Overview

| Field | Value |
|-------|-------|
| **Total Epics** | 6 |
| **Total User Stories** | 28 |
| **Primary User** | Acquisition Associate (AA) |
| **Secondary Users** | Team Manager, New AA |
| **Dependencies** | Dialpad, ElevenLabs, Agent365, OpenAI/Claude |

---

## Epic Overview

| # | Epic | Stories | Focus |
|---|------|---------|-------|
| 1 | Post-Call Processing | 6 | Transcript intake, note extraction |
| 2 | Next Steps Generation | 4 | Follow-up recommendations |
| 3 | Practice Mode Setup | 5 | Voice I/O, persona generation |
| 4 | Practice Conversation | 5 | Real-time dialogue, session control |
| 5 | Scoring & Feedback | 4 | Performance analysis |
| 6 | Management Dashboard | 4 | Reporting, visibility |

---

## Epic 1: Post-Call Processing

**Goal:** Automatically process call transcripts and extract actionable notes.

---

### US-1.1: Dialpad Webhook Integration

**As a** System
**I want to** receive call transcripts via Dialpad webhook immediately after calls end
**So that** processing can begin without manual trigger

**Acceptance Criteria:**
- [ ] Webhook endpoint receives Dialpad payload
- [ ] Speaker separation (AA vs Agent) is preserved
- [ ] Call metadata (duration, phone number, timestamp) captured
- [ ] Fallback to manual upload if webhook fails
- [ ] Processing completes within 30 seconds

**Technical Notes:**
```typescript
interface DialpadWebhookPayload {
  call_id: string;
  duration_seconds: number;
  caller_number: string;
  callee_number: string;
  transcript: TranscriptSegment[];
  timestamp: Date;
}

interface TranscriptSegment {
  speaker: 'AA' | 'AGENT';
  text: string;
  start_time: number;
  end_time: number;
}
```

---

### US-1.2: Property Context Matching

**As a** System
**I want to** match incoming calls to the correct property
**So that** notes are associated with the right property record

**Acceptance Criteria:**
- [ ] Match by caller ID to agent phone from Agent365
- [ ] Match by AA's recent activity (last viewed property)
- [ ] Allow manual property selection if no match
- [ ] Display property card context during note generation
- [ ] Handle multi-property agents correctly

---

### US-1.3: Transcript Analysis

**As a** System
**I want to** analyze call transcript against the appropriate script template
**So that** relevant information is identified in context

**Acceptance Criteria:**
- [ ] Identify which script type was used (New Listing, Aged, Pending, BOM)
- [ ] Map conversation segments to script sections
- [ ] Detect script deviations or missed sections
- [ ] Handle off-script conversations gracefully
- [ ] Process transcripts up to 30 minutes in length

---

### US-1.4: Note Extraction

**As an** AA
**I want to** receive automatically generated notes from my call
**So that** I don't have to manually transcribe key information

**Acceptance Criteria:**
- [ ] Notes are free-form, conversational style
- [ ] All key information captured (see extraction categories)
- [ ] Notes are brief but nothing relevant is omitted
- [ ] Call quality indicator included
- [ ] Notes ready for copy/paste to property record

**Extraction Categories:**
| Category | Examples |
|----------|----------|
| Availability | "Agent available Tuesday 2-4pm for showing" |
| Motivation | "Seller needs to close by March 1st, relocating for job" |
| Competition | "Has two other offers, one all-cash" |
| Flexibility | "Agent open to double-end at 4% total" |
| Objections | "Concerned about proof of funds timeline" |
| Commitments | "Will call back Friday after talking to seller" |
| Preferences | "Prefers Chicago Title, close of escrow 21 days" |

---

### US-1.5: Post-Call UI Display

**As an** AA
**I want to** see my generated notes in a clean UI panel
**So that** I can review and use them quickly

**Acceptance Criteria:**
- [ ] Notes panel appears in Post-Call column
- [ ] One-click copy to clipboard
- [ ] Edit capability before saving
- [ ] Property context visible (address, status, price)
- [ ] Timestamp and call duration shown
- [ ] Dismiss/collapse option

---

### US-1.6: Fallback Transcription

**As a** System
**I want to** transcribe calls using Whisper if Dialpad transcript unavailable
**So that** the feature works regardless of Dialpad limitations

**Acceptance Criteria:**
- [ ] Detect when Dialpad transcript is missing/incomplete
- [ ] Upload audio file for Whisper processing
- [ ] Speaker diarization applied to Whisper output
- [ ] Processing time < 2 minutes for 10-minute call
- [ ] Quality comparable to Dialpad native transcription

---

## Epic 2: Next Steps Generation

**Goal:** Generate intelligent follow-up recommendations based on call outcome.

---

### US-2.1: Follow-Up Scheduling

**As an** AA
**I want to** receive suggested follow-up dates based on call outcome
**So that** I never miss a callback commitment

**Acceptance Criteria:**
- [ ] Detect explicit commitments ("I'll call Tuesday")
- [ ] Suggest Day 3/10/21 sequence if no explicit commitment
- [ ] Adjust sequence based on property status (Pending has different cadence)
- [ ] Display suggested date with one-click reminder creation
- [ ] Allow AA to modify before accepting

**Follow-Up Sequences:**
| Status | Day 3 | Day 10 | Day 21 |
|--------|-------|--------|--------|
| New Listing | Check interest | Push for showing | Final attempt |
| Aged | Price discussion | Offer positioning | Close or move on |
| Pending | "Deposit wired?" | "Contingencies removed?" | "Buyer real?" |
| BOM | "What happened?" | Position as backup | Formal offer |

---

### US-2.2: Document Request Detection

**As an** AA
**I want to** know when to send proof of funds or other documents
**So that** I can respond to agent requests immediately

**Acceptance Criteria:**
- [ ] Detect document requests in transcript
- [ ] Identify document type (POF, contract, LOI)
- [ ] Generate next step: "Send POF to agent"
- [ ] Include agent email if available from Agent365
- [ ] Flag urgency if agent mentioned timeline

---

### US-2.3: Agent Classification Update

**As an** AA
**I want to** receive suggestions to update agent classification
**So that** my contact list stays accurate

**Acceptance Criteria:**
- [ ] Suggest Cold → Warm if positive signals detected
- [ ] Suggest Warm → Hot if strong interest expressed
- [ ] Suggest downgrade if agent was dismissive
- [ ] Provide reasoning for classification change
- [ ] One-click update to Agent365

**Classification Signals:**
| Signal | Classification Change |
|--------|----------------------|
| "Send me the info" | Cold → Warm |
| "Let's schedule a call" | Warm → Hot |
| "Not interested" | Warm → Cold |
| "I work with XYZ already" | Note competitor relationship |

---

### US-2.4: Escalation Detection

**As an** AA
**I want to** know when to escalate to management
**So that** complex situations get proper attention

**Acceptance Criteria:**
- [ ] Detect escalation triggers (legal threats, complaints, complex negotiations)
- [ ] Flag for manager review
- [ ] Include context summary for manager
- [ ] Don't auto-escalate — suggest only
- [ ] Track escalation frequency per AA

---

## Epic 3: Practice Mode Setup

**Goal:** Initialize practice sessions with realistic AI agent personas.

---

### US-3.1: Practice Mode Launch

**As an** AA
**I want to** click "iQ Practice" on any property card to start a practice session
**So that** I can practice calls on specific properties

**Acceptance Criteria:**
- [ ] "iQ Practice" button visible on property cards (Desktop only)
- [ ] Click opens practice mode overlay
- [ ] Property context loaded automatically
- [ ] Agent profile loaded from Agent365
- [ ] Microphone permission requested
- [ ] Practice mode indicator visible

---

### US-3.2: Agent Persona Generation

**As a** System
**I want to** generate a realistic AI agent persona from Agent365 data
**So that** practice feels like a real call

**Acceptance Criteria:**
- [ ] Pull agent profile: deals/year, double-end %, last close, investor deals
- [ ] Generate personality traits based on data
- [ ] Create consistent behavior patterns
- [ ] Vary responses based on AA's approach
- [ ] Persona persists throughout session

**Persona Schema:**
```typescript
interface AgentPersona {
  name: string;
  voice_gender: 'male' | 'female';
  busyness: 'low' | 'medium' | 'high';
  double_end_openness: 'resistant' | 'neutral' | 'open';
  hunger_level: 'satisfied' | 'moderate' | 'hungry';
  investor_familiarity: 'low' | 'medium' | 'high';
  personality_traits: string[];
  opening_line: string;
  triggers: PersonaTrigger[];
}
```

---

### US-3.3: Difficulty Calculation

**As a** System
**I want to** auto-calculate practice difficulty based on agent profile
**So that** AAs face appropriate challenges

**Acceptance Criteria:**
- [ ] Score 0-100 calculated from agent metrics
- [ ] Display difficulty level: EASY / MEDIUM / HARD
- [ ] Show breakdown of scoring factors
- [ ] Allow manual override for training purposes
- [ ] Track difficulty vs performance correlation

**Scoring Formula:**
```
Base Score = 50

Deals/Year:
  <10: -20 points
  10-30: 0 points
  30-50: +15 points
  50+: +30 points

Double-End %:
  >30%: -15 points
  10-30%: 0 points
  <10%: +20 points

Days Since Last Close:
  >60 days: -15 points (hungry)
  30-60: 0 points
  <30: +15 points (satisfied)

Property Status Modifier:
  Aged 70+ DOM: -15 points (easier)
  New 0-7 DOM: +10 points (harder)
```

---

### US-3.4: Voice Setup (TTS)

**As a** System
**I want to** generate natural-sounding agent voice via ElevenLabs
**So that** practice feels realistic

**Acceptance Criteria:**
- [ ] Male/female voice selected based on agent name
- [ ] Voice consistent throughout session
- [ ] Latency < 500ms from text to audio start
- [ ] Volume normalized
- [ ] Fallback voice if ElevenLabs unavailable

---

### US-3.5: Voice Input (STT)

**As a** System
**I want to** transcribe AA speech in real-time
**So that** AI can respond to what AA says

**Acceptance Criteria:**
- [ ] Browser-native STT (Web Speech API) as primary
- [ ] Whisper API as fallback
- [ ] Real-time transcription display (optional)
- [ ] Handle interruptions gracefully
- [ ] Detect end of AA speech to trigger response

---

## Epic 4: Practice Conversation

**Goal:** Enable realistic back-and-forth dialogue with AI agent.

---

### US-4.1: Conversation State Machine

**As a** System
**I want to** manage conversation flow through defined states
**So that** dialogue progresses naturally

**Acceptance Criteria:**
- [ ] States: OPENING → DISCOVERY → OBJECTION → CLOSING → END
- [ ] Transitions based on AA input and time
- [ ] Dead-end detection (conversation going nowhere)
- [ ] Natural topic changes within state
- [ ] State visible to system for scoring

**State Transitions:**
```
OPENING: Agent answers, initial exchange
  → DISCOVERY if AA asks questions
  → OBJECTION if agent pushes back
  → END if poor opening

DISCOVERY: Information exchange
  → CLOSING if AA attempts close
  → OBJECTION if resistance detected
  → END if stalled too long

OBJECTION: Handling resistance
  → CLOSING if overcome
  → END if unresolved

CLOSING: Commitment attempt
  → END (positive, neutral, or negative)
```

---

### US-4.2: Real-Time Conversation Loop

**As an** AA
**I want to** have a natural back-and-forth conversation with AI agent
**So that** practice feels like a real call

**Acceptance Criteria:**
- [ ] AA speaks → STT → LLM → TTS → Agent responds
- [ ] Total latency < 2 seconds per turn
- [ ] Natural pauses and turn-taking
- [ ] Interruption handling
- [ ] Conversation history maintained for context

---

### US-4.3: Session End Logic

**As a** System
**I want to** end sessions appropriately based on AA performance
**So that** practice has realistic consequences

**Acceptance Criteria:**
- [ ] HANG UP: Score <40 after 60s, missed 3+ opportunities
- [ ] CONTINUE: Score ≥60, hit 2+ key points
- [ ] POSITIVE: Score ≥80, successful close attempt
- [ ] Agent delivers appropriate closing line
- [ ] Session cannot exceed 5 minutes

**End Conditions:**
| Outcome | Condition | Agent Line |
|---------|-----------|------------|
| HANG UP | Poor performance | "I've got a lot of calls, gotta run" |
| NEUTRAL | Mediocre | "Send me your info, I'll take a look" |
| POSITIVE | Strong | "Okay, let's set up a time to talk more" |
| TIMEOUT | 5 minutes | "I need to go, call me back tomorrow" |

---

### US-4.4: Winning Phrases Detection

**As a** System
**I want to** detect when AA uses effective language
**So that** scoring reflects good technique

**Acceptance Criteria:**
- [ ] Detect key themes regardless of exact wording
- [ ] Track which phrases were used
- [ ] Weight phrases by impact
- [ ] Include in feedback report
- [ ] Learn from variations

**Winning Themes:**
| Theme | Detection Patterns |
|-------|-------------------|
| Flexibility | "creative", "work with you", "find a way" |
| Agent benefit | "profitable for you", "help you close" |
| Closing question | "what do you need", "prove I can close" |
| Differentiation | "no wholesalers", "we close", "not an assignment" |
| Homework | Mentions agent's past deals, partners, preferences |
| Urgency | "tie this up", "today", "move quickly" |

---

### US-4.5: Practice UI

**As an** AA
**I want to** see a clean practice interface
**So that** I can focus on the conversation

**Acceptance Criteria:**
- [ ] Property context card visible
- [ ] Agent profile summary visible
- [ ] Difficulty indicator shown
- [ ] Mute/unmute control
- [ ] End session button
- [ ] Real-time duration counter
- [ ] Optional: live transcript view

---

## Epic 5: Scoring & Feedback

**Goal:** Provide immediate, actionable feedback after practice sessions.

---

### US-5.1: Scoring Algorithm

**As a** System
**I want to** calculate a performance score (1-100)
**So that** AAs can track improvement

**Acceptance Criteria:**
- [ ] Score based on multiple factors (see breakdown)
- [ ] Weighted by importance
- [ ] Consistent across sessions
- [ ] Comparable across difficulty levels
- [ ] Historical tracking per AA

**Scoring Breakdown:**
| Factor | Weight | Criteria |
|--------|--------|----------|
| Key phrases used | 30% | Count of winning themes detected |
| Conversation flow | 25% | Natural progression, no dead ends |
| Objection handling | 20% | Successfully addressed resistance |
| Closing attempt | 15% | Made clear ask |
| Time management | 10% | Efficient use of time |

---

### US-5.2: Performance Analysis

**As a** System
**I want to** analyze what went well and what didn't
**So that** feedback is specific and actionable

**Acceptance Criteria:**
- [ ] Identify 2-3 strengths
- [ ] Identify 2-3 areas for improvement
- [ ] Link to specific transcript moments
- [ ] Suggest script sections to review
- [ ] Compare to previous sessions

---

### US-5.3: Feedback Generation

**As an** AA
**I want to** receive immediate feedback after practice
**So that** I know how to improve

**Acceptance Criteria:**
- [ ] Feedback appears within 5 seconds of session end
- [ ] Score prominently displayed
- [ ] Strengths listed with examples
- [ ] Improvements listed with suggestions
- [ ] "Practice Again" option

**Feedback Format:**
```
SCORE: 72/100 (MEDIUM difficulty)

✓ WHAT YOU DID WELL:
• Strong opening — got agent's attention quickly
• Good use of differentiation ("we're not wholesalers")
• Asked a direct closing question

⚠️ AREAS TO IMPROVE:
• Missed opportunity to mention agent's past investor deals
• Could have addressed price concern more directly
• Consider asking about timeline earlier

📖 REVIEW THESE SCRIPT SECTIONS:
• Objection Handling: Price Concerns
• Closing: The Direct Ask
```

---

### US-5.4: Feedback UI

**As an** AA
**I want to** see my feedback in a clear, organized panel
**So that** I can quickly understand and act on it

**Acceptance Criteria:**
- [ ] Score displayed prominently with color coding
- [ ] Expandable sections for details
- [ ] One-click to replay session (future feature)
- [ ] Share with manager option
- [ ] Close and return to property view

---

## Epic 6: Management Dashboard

**Goal:** Provide managers visibility into team practice and performance.

---

### US-6.1: AA Summary Views

**As a** Manager
**I want to** see practice summaries for each AA
**So that** I can identify coaching opportunities

**Acceptance Criteria:**
- [ ] List of AAs with practice activity
- [ ] Sessions this week/month
- [ ] Average score trend
- [ ] Common improvement areas
- [ ] No raw transcripts visible

---

### US-6.2: Daily/Weekly Reports

**As a** Manager
**I want to** receive automated reports on team practice
**So that** I stay informed without manual checking

**Acceptance Criteria:**
- [ ] Daily digest: Who practiced, scores, notable items
- [ ] Weekly report: Trends, top performers, struggling AAs
- [ ] Email delivery option
- [ ] Dashboard view option
- [ ] Customizable report content

---

### US-6.3: Trend Analysis

**As a** Manager
**I want to** see improvement trends over time
**So that** I can measure training effectiveness

**Acceptance Criteria:**
- [ ] Score trends by AA (line chart)
- [ ] Team average over time
- [ ] Before/after comparisons
- [ ] Correlation with deal closings (future)
- [ ] Filter by date range, AA, difficulty

---

### US-6.4: Coaching Suggestions

**As a** Manager
**I want to** receive AI-generated coaching suggestions
**So that** I can provide targeted help

**Acceptance Criteria:**
- [ ] Per-AA coaching recommendations
- [ ] Based on repeated improvement areas
- [ ] Specific script sections to review with AA
- [ ] Prioritized by impact
- [ ] One-click to schedule coaching session (future)

---

## Data Schemas

### Post-Call Output Schema

```typescript
interface PostCallOutput {
  call_id: string;
  property_id: string;
  aa_id: string;
  agent_id: string;
  call_duration_seconds: number;
  timestamp: Date;

  notes: string;  // Free-form, conversational
  quality_rating: 'good' | 'needs_improvement';
  quality_reason: string;

  next_steps: NextStep[];
  classification_update?: ClassificationUpdate;
  escalation?: EscalationFlag;

  created_at: Date;
}

interface NextStep {
  type: 'follow_up' | 'document' | 'schedule' | 'status_change' | 'escalation';
  description: string;
  due_date?: Date;
  priority: 'high' | 'medium' | 'low';
  accepted: boolean;
}

interface ClassificationUpdate {
  from: 'cold' | 'warm' | 'hot';
  to: 'cold' | 'warm' | 'hot';
  reason: string;
}
```

### Practice Session Schema

```typescript
interface PracticeSession {
  session_id: string;
  aa_id: string;
  property_id: string;
  agent_persona: AgentPersona;

  difficulty_score: number;  // 0-100
  difficulty_level: 'EASY' | 'MEDIUM' | 'HARD';

  duration_seconds: number;
  end_reason: 'hang_up' | 'neutral' | 'positive' | 'timeout' | 'user_ended';

  transcript: ConversationTurn[];
  winning_phrases_detected: string[];

  score: number;  // 1-100
  score_breakdown: ScoreBreakdown;
  feedback: SessionFeedback;

  created_at: Date;
}

interface ConversationTurn {
  speaker: 'AA' | 'AGENT';
  text: string;
  timestamp: number;
  state: 'OPENING' | 'DISCOVERY' | 'OBJECTION' | 'CLOSING' | 'END';
}

interface SessionFeedback {
  strengths: FeedbackItem[];
  improvements: FeedbackItem[];
  script_sections_to_review: string[];
}
```

---

## Story Dependencies

```
US-1.1 → US-1.2 → US-1.3 → US-1.4 → US-1.5 (Post-call pipeline)
US-1.1 ↔ US-1.6 (Fallback path)

US-3.1 → US-3.2 → US-3.3 (Practice setup)
US-3.4 + US-3.5 → US-4.2 (Voice I/O to conversation)

US-4.1 → US-4.2 → US-4.3 (Conversation flow)
US-4.4 → US-5.1 (Phrases to scoring)

US-5.1 → US-5.2 → US-5.3 → US-5.4 (Feedback pipeline)
US-5.* → US-6.* (Session data to management)
```

---

## Acceptance Testing Checklist

- [ ] Post-call notes generate within 30 seconds
- [ ] Notes are accurate and complete
- [ ] Next steps are relevant and actionable
- [ ] Practice mode launches successfully
- [ ] AI agent voice sounds natural
- [ ] Conversation latency < 2 seconds
- [ ] Difficulty calculation is accurate
- [ ] Session ends appropriately
- [ ] Winning phrases detected correctly
- [ ] Score and feedback generate immediately
- [ ] Management dashboard shows summaries
- [ ] Raw transcripts NOT visible to management

---

**Document Version:** 1.0
**Last Updated:** December 31, 2024
**Status:** Ready for Development
