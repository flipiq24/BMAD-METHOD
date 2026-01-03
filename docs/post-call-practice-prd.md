# POST-CALL & PRACTICE BOT

## Product Requirements Document
### Version 1.0 — December 31, 2024

---

| Field | Value |
|-------|-------|
| **Bot ID** | D2 (Post Call and Practice) |
| **Category** | Training / Performance |
| **Priority** | P1 — Core Feature |
| **Primary User** | Acquisition Associate (AA) |
| **UI Location** | PiQ → Post-Call Column |
| **Trigger Method** | Dialpad webhook (Post-Call) / iQ Practice button (Practice) |
| **Integration Points** | Dialpad, ElevenLabs, Agent365, OpenAI/Claude |
| **Handoff To** | Eric (PM) / Nate (CTO) |

---

## 1. EXECUTIVE SUMMARY

### 1.1 Problem Statement

Acquisition Associates make 30+ outreach calls daily. During these high-stakes conversations, they must simultaneously follow complex dynamic scripts, listen for key information, take accurate notes, identify next steps, and build rapport. This cognitive overload leads to missed opportunities, incomplete notes, inconsistent follow-up, and no structured way to improve call skills.

Additionally, new AAs have no safe environment to practice calls before going live with real agents, and experienced AAs have no mechanism for continuous improvement feedback.

### 1.2 Solution

An AI-powered Post-Call assistant that:

1. Processes Dialpad call transcripts immediately after calls end
2. Automatically extracts relevant notes mapped to script context
3. Generates intelligent next-step recommendations
4. Provides a Practice Mode with realistic AI-simulated agent conversations

### 1.3 Success Metric

**Each AA closes 2 deals per month** — this bot directly supports that goal by improving call quality and follow-up consistency.

---

## 2. USER STORIES

### 2.1 Acquisition Associate

**AS A** busy Acquisition Associate making 30+ calls daily
**I WANT** an AI assistant that processes my call transcripts and suggests next steps
**SO THAT** I can focus on building rapport and closing deals instead of manual documentation

### 2.2 New AA (Training)

**AS A** new Acquisition Associate
**I WANT TO** practice calls with a realistic AI-simulated agent before going live
**SO THAT** I can build confidence and refine my script delivery

### 2.3 Team Manager

**AS A** team manager
**I WANT TO** see practice summaries and areas where my AAs need improvement
**SO THAT** I can provide targeted coaching and track skill development

---

## 3. FEATURE SPECIFICATIONS

### 3.1 Feature 1: Post-Call Transcript Processing

**Trigger:** Call ends in Dialpad → Transcript pushed via API → Bot processes immediately

**Process Flow:**

1. Dialpad webhook delivers transcript with speaker identification (AA vs Agent)
2. System matches property context from the call (via caller ID or manual selection)
3. LLM analyzes transcript against loaded script template (New Listing, Aged, Pending)
4. Extracts key information and generates summary notes
5. Generates next steps based on call outcome
6. Presents summary to AA for copy/paste to property notes

**Output:** Unstructured, quick-read notes — short but nothing omitted. Includes recommended next steps and agent classification updates.

**Technical Requirement:**

> ⚠️ **BLOCKER: Verify Dialpad API provides immediate transcript delivery with speaker separation**
> - If YES → Consume Dialpad output directly
> - If NO → Build secondary transcription layer using Whisper/OpenAI (+4 hrs)

---

### 3.2 Feature 2: Intelligent Note Extraction

**What the Bot Listens For:**

| Category | Examples |
|----------|----------|
| Availability | Agent's showing schedule, callbacks |
| Motivation | Seller timeline, urgency signals |
| Competition | Other offers, competing buyers |
| Flexibility | Double-end willingness, pricing signals |
| Objections | Concerns raised and how handled |
| Commitments | "I'll call you back Tuesday" |
| Preferences | Title company, closing timeline |

**Note Format:** Free-form, conversational summary. Not structured key-value pairs. Quick to read, nothing omitted, as brief as possible.

**Quality Rating:** Include call quality indicator:
- "Good call — make sure to follow up"
- "Needs improvement — review objection handling"

---

### 3.3 Feature 3: Next Steps Generation

**Categories of Next Steps:**

| Type | Examples |
|------|----------|
| Follow-up | Day 3, Day 10, Day 21 sequences |
| Documents | Proof of funds requests |
| Scheduling | Property visit, callback |
| Status | Offer status changes |
| Classification | Agent warm/cold updates |
| Escalation | Management involvement |

**Automation Level:** MANUAL UPDATES ONLY — Bot suggests, AA executes. Next steps integrate into property reminders.

---

### 3.4 Feature 4: Practice Mode

**Trigger:** AA clicks "iQ Practice" button on any property card (Desktop only)

**AI Persona Generation from Agent365 Data:**

| Data Point | Impact on Persona |
|------------|-------------------|
| Deals/year | Busyness, responsiveness |
| Double-end % | Resistance to double-end ask |
| Last close date | Hunger level, urgency |
| Investor source count | Familiarity with investor calls |
| Avg price point | Conversation context |

**Voice Generation:** ElevenLabs API with male/female selection based on agent first name.

**Difficulty Levels (Auto-Calculated):**

| Level | Score | Agent Profile |
|-------|-------|---------------|
| EASY | 0-33 | <10 deals/yr, >30% double-end, hungry |
| MEDIUM | 34-66 | 10-30 deals/yr, 10-30% double-end |
| HARD | 67-100 | 50+ deals/yr, <10% double-end, just closed |

**Session End Logic:**

| Outcome | Trigger | Agent Response |
|---------|---------|----------------|
| HANG UP | Score <40 after 60s, missed 3+ opportunities | "I've got a lot of calls, gotta run" |
| CONTINUE | Score ≥60, hit 2+ key points | Conversation continues |
| POSITIVE | Score ≥80 | "Send me the info" or "Let me think about it" |

**Winning Phrases to Detect:**

| Theme | Example Variations |
|-------|-------------------|
| Flexibility | "We are very creative" |
| Agent benefit | "Our goal is to make it profitable for you" |
| Closing question | "What terms do you need to prove I can close?" |
| Differentiation | "No wholesalers, no assignments" |
| Homework | Using agent's investor partners by name |
| Urgency | "What's it going to take to tie this up today?" |

**Feedback Generation:**

- Overall score (1-100)
- What they did well
- Areas to improve
- Specific script sections to review

---

## 4. MANAGEMENT VISIBILITY

### 4.1 What Management Sees

Management receives SUMMARIES, not raw transcripts:

- What each AA is struggling with
- What they need to practice
- What they're doing well
- Suggested scripts for personalized coaching

### 4.2 Reporting Frequency

| Report | Timing |
|--------|--------|
| Real-time | Practice scores immediately after session |
| Daily Digest | End-of-day summary of all AA activity |
| Weekly Report | Trend analysis and improvement tracking |
| On-Demand | Manager pulls individual AA reports |

### 4.3 Data Retention

Practice results tracked by property and rolled up per AA. Historical data retained for trend analysis.

---

## 5. SUCCESS METRICS

| Metric | Target | Measurement |
|--------|--------|-------------|
| Note Quality | 80% no-edit | % of notes AA uses without modification |
| Next Steps Relevance | 90% approved | % of suggested next steps AA accepts |
| Practice Engagement | 3+ sessions/week | Avg practice sessions per AA per week |
| Skill Improvement | 15% in 30 days | Practice score improvement over time |
| Deal Correlation | 20% more deals | Deal close rate for AAs using practice mode |

---

## 6. TECHNICAL REQUIREMENTS

### 6.1 API Integrations

| API | Purpose | Priority |
|-----|---------|----------|
| Dialpad | Transcript retrieval with speaker separation | P0 |
| ElevenLabs | Text-to-speech for practice mode | P0 |
| OpenAI/Claude | LLM for transcript analysis, persona, conversation | P0 |
| Agent365 | Agent profile data for persona creation | P0 |
| Whisper | Fallback transcription if Dialpad insufficient | P1 |

### 6.2 Data Sources

| Source | Data |
|--------|------|
| MLS/Property Radar | Property context |
| Agent365 | Agent intelligence |
| Dynamic Scripts | Script templates by property status |
| Dialpad | Call transcripts |

### 6.3 Platform

- **Practice Mode:** Desktop only (browser-based voice I/O)
- **Post-Call Processing:** Any device Dialpad supports

---

## 7. DEVELOPMENT PLAN

> **Note:** Timelines reflect BMAD Method + Claude Code (AI-assisted development)

### Phase 0: API Verification — 2 hrs (BLOCKER)

| Task | Hours |
|------|-------|
| 0.1 Dialpad API capability verification | 1 |
| 0.2 ElevenLabs latency testing | 0.5 |
| 0.3 Browser STT compatibility check | 0.5 |

> **If Dialpad fails:** Add 4 hrs to build Whisper-based transcription

---

### Epic 1: Post-Call Processing — 6 hrs

| Task | Hours |
|------|-------|
| 1.1 Dialpad webhook integration | 1 |
| 1.2 Property context matching | 0.5 |
| 1.3 Transcript analysis prompt engineering | 1 |
| 1.4 Note extraction logic | 1 |
| 1.5 Next steps generation | 1 |
| 1.6 Post-call UI panel | 1 |
| 1.7 Error handling | 0.5 |

---

### Epic 2: Practice Mode Core — 12 hrs

| Task | Hours |
|------|-------|
| 2.1 ElevenLabs TTS integration | 1.5 |
| 2.2 Browser speech-to-text setup | 1.5 |
| 2.3 Agent persona generator from Agent365 | 1.5 |
| 2.4 Difficulty calculation logic | 0.5 |
| 2.5 Conversation state machine | 2 |
| 2.6 Real-time conversation loop | 2 |
| 2.7 Session end logic (hang up/continue/positive) | 1 |
| 2.8 Winning phrases detection | 1 |
| 2.9 Practice mode UI | 1 |

---

### Epic 3: Scoring & Feedback — 4 hrs

| Task | Hours |
|------|-------|
| 3.1 Scoring algorithm (1-100) | 1 |
| 3.2 Performance analysis engine | 1 |
| 3.3 Feedback generation | 1 |
| 3.4 Feedback UI panel | 1 |

---

### Epic 4: Management Dashboard — 4 hrs

| Task | Hours |
|------|-------|
| 4.1 AA summary views | 1 |
| 4.2 Daily/weekly report generation | 1.5 |
| 4.3 Trend analysis queries | 1 |
| 4.4 Dashboard UI | 0.5 |

---

### Timeline Summary

| Phase | Hours | Deliverable |
|-------|-------|-------------|
| Phase 0: API Verification | 2 hrs | Go/no-go decision |
| Epic 1: Post-Call Processing | 6 hrs | Notes + next steps live |
| Epic 2: Practice Mode Core | 12 hrs | Voice practice working |
| Epic 3: Scoring & Feedback | 4 hrs | Feedback loop complete |
| Epic 4: Management Dashboard | 4 hrs | Manager visibility |
| **TOTAL** | **28 hrs (~4 days)** | **Full bot deployed** |

> **Note:** Epic 2 (Practice Mode) is the critical path. Real-time voice conversation loop requires careful latency management. If ElevenLabs latency is too high, evaluate OpenAI TTS or PlayHT as alternatives.

---

## 8. RISKS AND MITIGATIONS

| Risk | Impact | Mitigation |
|------|--------|------------|
| Dialpad doesn't provide immediate transcripts | Delays feature | Build Whisper-based transcription (+4 hrs) |
| Practice mode feels unrealistic | Low adoption | Calibrate with real agent data + user feedback |
| AAs resist "being monitored" | Low adoption | Position as helper; management sees summaries only |
| ElevenLabs latency too high | Poor practice UX | Test OpenAI TTS, PlayHT alternatives |
| Browser STT inconsistent | Poor practice UX | Fallback to Whisper API |

---

## 9. ACCEPTANCE CRITERIA

- [ ] AC1: Post-call notes generate within 30 seconds of call end
- [ ] AC2: Notes include all key information from transcript
- [ ] AC3: Next steps are relevant to call outcome
- [ ] AC4: Practice mode launches from property card
- [ ] AC5: AI agent voice sounds natural (ElevenLabs quality)
- [ ] AC6: Conversation latency < 2 seconds per turn
- [ ] AC7: Difficulty auto-calculates from Agent365 data
- [ ] AC8: Session ends appropriately based on AA performance
- [ ] AC9: Winning phrases detected correctly
- [ ] AC10: Score and feedback generate immediately after session
- [ ] AC11: Management dashboard shows AA summaries
- [ ] AC12: Raw transcripts NOT visible to management

---

## 10. APPENDIX: SAMPLE PRACTICE SCENARIO

### Property Context

| Field | Value |
|-------|-------|
| Address | 456 Oak Street |
| Status | Active — 85 Days on Market |
| Price | $525,000 (reduced from $575,000) |
| Condition | AS-IS |
| Seller Distress | NOD filed 45 days ago |

### Agent Profile (from Agent365)

| Field | Value |
|-------|-------|
| Name | Sarah Martinez |
| Deals/Year | 62 |
| Double-End % | 4% (rarely) |
| Last Close | 3 days ago |
| Investor Deals | 8 this year |

### Calculated Difficulty: 82/100 (HARD)

| Factor | Points |
|--------|--------|
| High volume (62 deals) | +31 |
| Rarely double-ends (4%) | +20 |
| Just closed (satisfied) | +16 |
| Aged listing (85 DOM) | -15 |
| **Total** | **82** |

### AI Persona Behavior

- Will be curt and time-pressed
- Will resist double-end conversation
- Will focus on "what's your offer?"
- May hang up if value not proven quickly
- Will respond to data (investor partners, speed, certainty)

### Sample AI Responses

| Situation | Response |
|-----------|----------|
| Opening | "Sarah Martinez. Make it quick, I'm between showings." |
| If too slow | "Look, I get 20 calls a day on this one. What's your offer?" |
| If mentions investors | "Which investors? What's their track record here?" |
| If asks double-end | "I don't do that. Bring your best offer." |
| If proves value | "Okay, fine. Send the proof of funds. I'll look at it tonight." |

---

## HANDOFF SUMMARY

### For Eric (PM)

| Item | Detail |
|------|--------|
| Problem | AAs overwhelmed with note-taking and no practice environment |
| Solution | AI post-call notes + voice practice mode |
| Success | 80% no-edit notes, 3+ practice sessions/week |
| Priority | P1 Core Feature |
| **Timeline** | **28 hrs (~4 days) with BMAD + Claude Code** |

### For Nate (CTO)

| Item | Detail |
|------|--------|
| APIs | Dialpad, ElevenLabs, OpenAI/Claude, Agent365 |
| **Blocker** | Verify Dialpad API first (2 hrs) |
| Critical Path | Epic 2 (Practice Mode) — voice latency sensitive |
| Platform | Desktop only for Practice Mode |
| **Timeline** | **28 hrs (~4 days) with BMAD + Claude Code** |

---

**Document prepared for FlipIQ Engineering**
**Version 1.0 — December 31, 2024**
