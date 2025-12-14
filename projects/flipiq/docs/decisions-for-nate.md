# Decisions for Nate - FlipIQ CTO Handoff

**Created:** December 2024
**Updated:** December 14, 2024
**Purpose:** Track decisions, UI proposals, and technical questions requiring Nate's input

---

## December Sprint Summary

**Goal:** Ship the Deal Machine for 7 operators by Dec 31

**Handoff Documents:**
- `docs/december-sprint-plan.md` - Complete sprint plan with specs
- `implementation-artifacts/sprint-status.yaml` - Dev tracking
- `planning-artifacts/PRD.md` - Updated Phase 2a/2b breakdown

**December Scope (12 bots):**
- My Stats, D4-D8, CMaster, C1, C2, C4, IAMaster, Com1

**Deferred to January:**
- AA0 (Voice), MGT1-3 (Management), M1-M5 (Marketing)

---

## BLOCKING DECISIONS (Need by Dec 16)

These 4 decisions block December development:

| # | Decision | Blocks | Recommendation |
|---|----------|--------|----------------|
| 1 | Transcription service | D7 (Post Call) | OpenAI Whisper - already have OpenAI |
| 2 | Mapping service | C1 (Map Review) | Mapbox or Google Maps |
| 3 | SMS service | Com1 (Auto Connect) | Twilio or existing? |
| 4 | Vision API | C4 (AI Mapping) | OpenAI Vision - already have OpenAI |

---

## High Priority (Blocking)

### 1. Command Platform Access

**Question:** How do we access the Command platform code and database?

**Context:** FlipIQ is an overlay that reads from Command but doesn't modify it.

**Options:**
- A) Direct database replica (read-only connection)
- B) API wrapper service
- C) Existing SDK/library

**Decision:** _Pending Nate's input_

---

### 2. Bot Hierarchy Architecture

**Question:** How should we structure bot hierarchy based on data connection and relevancy?

**Context:** 32 bots need to be organized. Some bots depend on others. Data flows between them.

**Considerations:**
- Master bots (DMaster, MGTMaster, etc.) coordinate sub-bots
- Some bots share data sources (PIQ, D1-D3 all need MLS + PropertyRadar)
- Real-time vs. batch processing needs
- Failure isolation - if one bot fails, others should continue

**Proposed Structure:**
```
Level 1: Universal (AA0) - Entry point
Level 2: Daily Ops (AA1-AA4) - User workflow
Level 3: Intelligence (PIQ, DMaster) - Data aggregation
Level 4: Analysis (D1-D8, CMaster, IAMaster) - Specific analysis
Level 5: Actions (Com1, Marketing) - Outbound
```

**Decision:** _Pending Nate's input_

---

## UI Layouts to Propose

### 3. Management Dashboard (MGTMaster)

**Needed:** Layout proposal for exception-based management dashboard.

**Must Include:**
- Team compliance overview
- Exception highlighting (missed check-ins, below targets)
- Real-time refresh (<5 seconds)
- Role-based views (AM vs Principal)

**Reference:** My Stats screenshot provided shows KPI card pattern that could be reused.

**Decision:** _Nate to propose_

---

### 4. Personalization Dashboard (Phase 3)

**Needed:** Layout proposal for user AI personalization settings.

**Must Include:**
- Buy Box configuration
- Bot output style settings
- Custom script editor
- Investment criteria defaults
- Reset to baseline option

**Decision:** _Nate to propose_

---

### 5. EOD Report Template (MGT2)

**Needed:** Email template design for 5PM automated report.

**Must Include:**
- All KPIs vs targets
- Team scorecards
- AI coaching suggestions
- Exception highlights

**Decision:** _Nate to propose_

---

## Technical Decisions

### 6. Push Notification Strategy

**Question:** How should we handle notifications for:
- Reminders (D5)
- Blocker routing to AM
- EOD reports
- Exception alerts

**Options:**
- A) Web push only
- B) SMS only
- C) Email only
- D) Combination (user preference)
- E) None - in-app only

**Decision:** _Pending Nate's input_

---

### 7. Transcription Service

**Context:** D7 (Post Call Bot) needs call transcription (NO recording).

**Options:**
- A) OpenAI Whisper API
- B) AssemblyAI
- C) Deepgram
- D) Google Speech-to-Text

**Consideration:** Already using OpenAI for LLM - Whisper may integrate better.

**Decision:** _Pending Nate's input_

---

### 8. Security & Compliance

**Items for Review:**
- [ ] Two-party consent state handling for transcription
- [ ] CCPA data subject request flow
- [ ] SOC 2 audit requirements (if any)
- [ ] Data encryption standards

**Decision:** _Nate to review_

---

### 9. Cost Model

**Items Requiring Budget Guidance:**
- LLM cost per AA per month (GPT-4 at scale)
- PropertyRadar API usage at 500 operators
- Infrastructure (AWS/GCP) budget
- Third-party transcription service

**Decision:** _Nate to provide guidance_

---

## Already Decided (For Reference)

| Item | Decision | Notes |
|------|----------|-------|
| LLM Provider | OpenAI GPT-4 | Quality over cost |
| Voice Input | OpenAI Realtime API | ChatGPT already in use |
| Call Recording | NO | Transcription only |
| DFI Calculation | On-demand | Not nightly batch |
| Mobile Support | No (desktop only) | Maybe phone calls later |
| Offline Mode | No | Internet required |
| Keyboard Shortcuts | No | Not needed |
| A/B Testing | No | Not planned |
| Data Retention | Keep all | Long-term value |
| Personalization | Per user | Not per operator |

---

## Comp UI Locations (Confirmed)

| View | AI Overlay Position |
|------|---------------------|
| Map | Below the map |
| Matrix | Above the table |
| List | Sidebar on right |
| Investment Analysis | Below |
| Agent | Overlay |

---

## Microservices Being Built

| Service | Status | Notes |
|---------|--------|-------|
| National Data | In Progress | PropertyRadar wrapper |
| Agent Reports | In Progress | DispoPro (our product) |
| Command Reader | TBD | Needs Nate input on access |

---

## Action Items Summary

| # | Item | Type | Priority |
|---|------|------|----------|
| 1 | Command platform access | Question | High |
| 2 | Bot hierarchy architecture | Decision | High |
| 3 | Management Dashboard layout | UI Proposal | Medium |
| 4 | Personalization Dashboard layout | UI Proposal | Medium (Phase 3) |
| 5 | EOD Report template | UI Proposal | Medium |
| 6 | Push notification strategy | Decision | Medium |
| 7 | Transcription service | Decision | Medium |
| 8 | Security & compliance review | Review | Medium |
| 9 | Cost model guidance | Budget | Low (7 operators now) |

---

*Update this document as decisions are made.*
