# FlipIQ Phase 2a - December Sprint Asana Tickets

**Sprint:** December Deal Machine
**Dates:** Dec 16-31, 2024
**Goal:** Ship 12 bots for 7 operators

---

## Team Roles

| Name | Role | Focus |
|------|------|-------|
| **Nate** | CTO | Decisions, architecture review, milestones |
| **Haris** | AI/Bots | Bot logic, AI integrations, LLM prompts |
| **Faisal** | Backend | Database, APIs, sockets, data infrastructure |
| **Anas** | Frontend | UI components, dashboard, user experience |

---

## Section 1: CTO Review & Decisions (Nate)

| Task Name | Assignee | Due Date | Product | Priority | Story Type | Time Estimate |
|-----------|----------|----------|---------|----------|------------|---------------|
| [HANDOFF] Review December Sprint Plan & BMAD Artifacts | Nate | Dec 16 | Command iQ Phase 2 | Urgent | task | 2 Hours |
| [DECISION] Transcription Service Selection (blocks D7) | Nate | Dec 16 | Command iQ Phase 2 | Urgent | decision | 30 Min |
| [DECISION] Mapping Service Selection (blocks C1) | Nate | Dec 16 | Command iQ Phase 2 | Urgent | decision | 30 Min |
| [DECISION] SMS Service Selection (blocks Com1) | Nate | Dec 16 | Command iQ Phase 2 | Urgent | decision | 30 Min |
| [DECISION] Vision API Confirmation (blocks C4) | Nate | Dec 16 | Command iQ Phase 2 | Urgent | decision | 30 Min |
| [DECISION] Command Platform Access Method | Nate | Dec 18 | Command iQ Phase 2 | High | decision | 1 Hour |
| [DECISION] Bot Hierarchy Architecture | Nate | Dec 18 | Command iQ Phase 2 | Mid | decision | 1 Hour |
| [MILESTONE] Week 1 Complete - My Stats, D4, D5, D6, C1 | Nate | Dec 22 | Command iQ Phase 2 | Urgent | milestone | — |
| [MILESTONE] Week 2 Complete - D7, D8, CMaster, C2, IAMaster | Nate | Dec 29 | Command iQ Phase 2 | Urgent | milestone | — |
| [MILESTONE] December Sprint Complete - All 12 bots live | Nate | Dec 31 | Command iQ Phase 2 | Urgent | milestone | — |

---

## Section 2: Backend Infrastructure (Faisal)

| Task Name | Assignee | Due Date | Product | Priority | Story Type | Time Estimate |
|-----------|----------|----------|---------|----------|------------|---------------|
| [INFRA] Create flipiq_notes table | Faisal | Dec 17 | Command iQ Phase 2 | Urgent | task | 2 Hours |
| [INFRA] Create flipiq_activities table | Faisal | Dec 17 | Command iQ Phase 2 | Urgent | task | 2 Hours |
| [INFRA] Create flipiq_reminders table | Faisal | Dec 17 | Command iQ Phase 2 | Urgent | task | 2 Hours |
| [INFRA] Create flipiq_transcripts table | Faisal | Dec 17 | Command iQ Phase 2 | High | task | 1 Hour |
| [INFRA] Create flipiq_close_reports table | Faisal | Dec 17 | Command iQ Phase 2 | High | task | 1 Hour |
| [INFRA] Create flipiq_buy_boxes table | Faisal | Dec 17 | Command iQ Phase 2 | High | task | 1 Hour |
| [INFRA] Create flipiq_sequences table (Com1) | Faisal | Dec 17 | Command iQ Phase 2 | High | task | 1 Hour |
| [API] Notes CRUD endpoints | Faisal | Dec 18 | Command iQ Phase 2 | Urgent | feature | 4 Hours |
| [API] Activities logging endpoints | Faisal | Dec 19 | Command iQ Phase 2 | Urgent | feature | 4 Hours |
| [API] Reminders scheduling endpoints | Faisal | Dec 20 | Command iQ Phase 2 | Urgent | feature | 4 Hours |
| [API] My Stats data aggregation endpoints | Faisal | Dec 18 | Command iQ Phase 2 | Urgent | feature | 1 Day |
| [API] Comp data retrieval endpoints | Faisal | Dec 21 | Command iQ Phase 2 | High | feature | 1 Day |
| [API] Investment analysis calculation endpoints | Faisal | Dec 26 | Command iQ Phase 2 | High | feature | 1 Day |
| [API] Transcription storage endpoints | Faisal | Dec 24 | Command iQ Phase 2 | High | feature | 4 Hours |
| [API] Com1 sequence management endpoints | Faisal | Dec 28 | Command iQ Phase 2 | High | feature | 1 Day |
| [INTEGRATION] OpenAI Whisper transcription setup | Faisal | Dec 23 | Command iQ Phase 2 | Urgent | task | 4 Hours |
| [INTEGRATION] SMS service (Twilio) setup | Faisal | Dec 23 | Command iQ Phase 2 | High | task | 4 Hours |
| [INTEGRATION] OpenAI Vision API setup | Faisal | Dec 28 | Command iQ Phase 2 | High | task | 4 Hours |

---

## Section 3: AI/Bot Development (Haris)

| Task Name | Assignee | Due Date | Product | Priority | Story Type | Time Estimate |
|-----------|----------|----------|---------|----------|------------|---------------|
| [D4] Notes Bot - Auto-categorization AI logic | Haris | Dec 19 | Command iQ Phase 2 | Urgent | feature | 1 Day |
| [D4] Notes Bot - Action item extraction AI | Haris | Dec 20 | Command iQ Phase 2 | High | feature | 1 Day |
| [D5] Reminders Bot - Stage-based reminder logic | Haris | Dec 21 | Command iQ Phase 2 | Urgent | feature | 1 Day |
| [D5] Reminders Bot - Pending follow-up sequence logic | Haris | Dec 22 | Command iQ Phase 2 | High | feature | 1 Day |
| [D6] Activity Bot - Auto-logging detection logic | Haris | Dec 20 | Command iQ Phase 2 | Urgent | feature | 1 Day |
| [D7] Post Call Bot - Transcription processing | Haris | Dec 26 | Command iQ Phase 2 | Urgent | feature | 1.5 Days |
| [D7] Post Call Bot - Performance analysis AI prompts | Haris | Dec 28 | Command iQ Phase 2 | High | feature | 1 Day |
| [D7] Post Call Bot - Coaching suggestions AI | Haris | Dec 29 | Command iQ Phase 2 | Mid | feature | 0.5 Day |
| [D8] Close Deal Report - Success/failure analysis AI | Haris | Dec 27 | Command iQ Phase 2 | High | feature | 1 Day |
| [D8] Close Deal Report - Lessons learned generation | Haris | Dec 28 | Command iQ Phase 2 | Mid | feature | 0.5 Day |
| [CMaster] Comp orchestration logic | Haris | Dec 26 | Command iQ Phase 2 | Urgent | feature | 1 Day |
| [C2] Matrix Summary - Statistical analysis logic | Haris | Dec 27 | Command iQ Phase 2 | High | feature | 1 Day |
| [C2] Matrix Summary - Outlier detection algorithm | Haris | Dec 28 | Command iQ Phase 2 | Mid | feature | 0.5 Day |
| [C4] AI Mapping - Photo condition analysis prompts | Haris | Dec 30 | Command iQ Phase 2 | High | feature | 1.5 Days |
| [C4] AI Mapping - Condition scoring logic | Haris | Dec 31 | Command iQ Phase 2 | Mid | feature | 0.5 Day |
| [IAMaster] Investment Analysis - Buy box matching logic | Haris | Dec 26 | Command iQ Phase 2 | Urgent | feature | 1 Day |
| [IAMaster] Investment Analysis - ROI/IRR calculation | Haris | Dec 27 | Command iQ Phase 2 | High | feature | 1 Day |
| [IAMaster] Investment Analysis - Go/No-Go AI recommendation | Haris | Dec 28 | Command iQ Phase 2 | Mid | feature | 0.5 Day |
| [Com1] Auto Connect - Sequence trigger logic | Haris | Dec 30 | Command iQ Phase 2 | High | feature | 0.5 Day |
| [Com1] Auto Connect - Response detection logic | Haris | Dec 31 | Command iQ Phase 2 | High | feature | 0.5 Day |

---

## Section 4: Frontend Development (Anas)

| Task Name | Assignee | Due Date | Product | Priority | Story Type | Time Estimate |
|-----------|----------|----------|---------|----------|------------|---------------|
| [MY STATS] Dashboard - KPI cards with sparklines | Anas | Dec 18 | Command iQ Phase 2 | Urgent | feature | 1 Day |
| [MY STATS] Dashboard - Team leaderboard component | Anas | Dec 19 | Command iQ Phase 2 | Urgent | feature | 1 Day |
| [MY STATS] Dashboard - Daily performance report section | Anas | Dec 20 | Command iQ Phase 2 | High | feature | 1 Day |
| [MY STATS] Dashboard - AI summary panel | Anas | Dec 21 | Command iQ Phase 2 | Mid | feature | 0.5 Day |
| [D4] Notes Bot - Notes UI component | Anas | Dec 19 | Command iQ Phase 2 | Urgent | feature | 1 Day |
| [D4] Notes Bot - Action items list UI | Anas | Dec 20 | Command iQ Phase 2 | High | feature | 0.5 Day |
| [D5] Reminders Bot - Reminders panel UI | Anas | Dec 21 | Command iQ Phase 2 | Urgent | feature | 1 Day |
| [D6] Activity Bot - Timeline view UI | Anas | Dec 22 | Command iQ Phase 2 | High | feature | 1 Day |
| [D7] Post Call Bot - Transcription viewer UI | Anas | Dec 27 | Command iQ Phase 2 | High | feature | 1 Day |
| [D7] Post Call Bot - Coaching suggestions UI | Anas | Dec 28 | Command iQ Phase 2 | Mid | feature | 0.5 Day |
| [D8] Close Deal Report - Report display UI | Anas | Dec 28 | Command iQ Phase 2 | High | feature | 1 Day |
| [C1] Map Review Bot - Comp map component | Anas | Dec 20 | Command iQ Phase 2 | Urgent | feature | 1.5 Days |
| [C1] Map Review Bot - Boundary overlays UI | Anas | Dec 22 | Command iQ Phase 2 | High | feature | 1 Day |
| [C1] Map Review Bot - Comp quality indicators | Anas | Dec 23 | Command iQ Phase 2 | Mid | feature | 0.5 Day |
| [INTEGRATION] Mapping service (Mapbox/Google) frontend | Anas | Dec 18 | Command iQ Phase 2 | Urgent | task | 4 Hours |
| [C2] Matrix Summary - Matrix table UI | Anas | Dec 27 | Command iQ Phase 2 | High | feature | 1 Day |
| [C2] Matrix Summary - AI overlay above table | Anas | Dec 28 | Command iQ Phase 2 | Mid | feature | 0.5 Day |
| [C4] AI Mapping - Photo analysis results UI | Anas | Dec 30 | Command iQ Phase 2 | High | feature | 1 Day |
| [IAMaster] Investment Analysis - Buy box form UI | Anas | Dec 26 | Command iQ Phase 2 | Urgent | feature | 1 Day |
| [IAMaster] Investment Analysis - ROI display component | Anas | Dec 27 | Command iQ Phase 2 | High | feature | 0.5 Day |
| [IAMaster] Investment Analysis - Recommendation panel UI | Anas | Dec 28 | Command iQ Phase 2 | Mid | feature | 0.5 Day |
| [Com1] Auto Connect - Sequence status UI | Anas | Dec 30 | Command iQ Phase 2 | High | feature | 0.5 Day |
| [Com1] Auto Connect - Response notification UI | Anas | Dec 31 | Command iQ Phase 2 | High | feature | 0.5 Day |

---

## Section 5: QA & Testing

| Task Name | Assignee | Due Date | Product | Priority | Story Type | Time Estimate |
|-----------|----------|----------|---------|----------|------------|---------------|
| [QA] Backend API testing - Notes, Activities, Reminders | Faisal | Dec 29 | Command iQ Phase 2 | High | task | 4 Hours |
| [QA] Bot logic testing - D4, D5, D6, D7, D8 | Haris | Dec 30 | Command iQ Phase 2 | High | task | 4 Hours |
| [QA] Frontend E2E testing - My Stats, Deal flow | Anas | Dec 30 | Command iQ Phase 2 | High | task | 4 Hours |
| [QA] Integration testing - Comp analysis flow | Haris | Dec 31 | Command iQ Phase 2 | High | task | 4 Hours |
| [QA] Integration testing - Investment analysis | Faisal | Dec 31 | Command iQ Phase 2 | High | task | 4 Hours |

---

## Summary by Assignee

| Assignee | Role | Tickets | Primary Focus |
|----------|------|---------|---------------|
| **Nate** | CTO | 10 | Decisions (4 blocking), architecture, milestones |
| **Faisal** | Backend | 18 | DB schema (7 tables), APIs, integrations |
| **Haris** | AI/Bots | 21 | Bot logic, AI prompts, LLM integrations |
| **Anas** | Frontend | 23 | UI components, dashboards, user experience |

**Total Tickets: 72**

---

## Dependencies Flow

```
Nate Decisions (Dec 16)
    │
    ├── Transcription ──→ Faisal (setup) ──→ Haris (D7 bot) ──→ Anas (D7 UI)
    ├── Mapping ────────→ Anas (C1 frontend)
    ├── SMS ────────────→ Faisal (setup) ──→ Haris (Com1 bot) ──→ Anas (Com1 UI)
    └── Vision ─────────→ Faisal (setup) ──→ Haris (C4 bot) ──→ Anas (C4 UI)

Faisal DB Schema (Dec 17)
    │
    └── All feature development depends on tables being ready

Week 1: Backend APIs + Bot Logic + Frontend UI (parallel tracks)
Week 2: Advanced bots + AI integrations
Week 3: Polish, testing, integration
```

---

## Week-by-Week Plan

### Week 1 (Dec 16-22)

| Faisal (Backend) | Haris (AI/Bots) | Anas (Frontend) |
|------------------|-----------------|-----------------|
| DB schema (7 tables) | D4 categorization | My Stats dashboard |
| Notes API | D4 action extraction | Notes UI |
| Activities API | D5 reminders logic | Reminders UI |
| Reminders API | D6 auto-logging | Activity timeline UI |
| My Stats aggregation | — | C1 map component |
| Comp data API | — | Map overlays |

### Week 2 (Dec 23-29)

| Faisal (Backend) | Haris (AI/Bots) | Anas (Frontend) |
|------------------|-----------------|-----------------|
| Whisper setup | D7 transcription | D7 transcript viewer |
| Transcription API | D7 coaching AI | D8 report UI |
| Investment API | D8 analysis AI | C2 matrix UI |
| SMS/Twilio setup | CMaster orchestration | IAMaster form UI |
| Vision API setup | C2 statistics | IAMaster display |
| — | IAMaster logic | — |

### Week 3 (Dec 30-31)

| Faisal (Backend) | Haris (AI/Bots) | Anas (Frontend) |
|------------------|-----------------|-----------------|
| Com1 sequence API | C4 photo analysis | C4 results UI |
| API testing | Com1 logic | Com1 status UI |
| Integration QA | Bot testing | E2E testing |

---

## Ticket Naming Convention

```
[TYPE] Bot/Feature Name - Specific Task

Types by Owner:
- Nate: [HANDOFF], [DECISION], [MILESTONE]
- Faisal: [INFRA], [API], [INTEGRATION]
- Haris: [D4]-[D8], [CMaster], [C2], [C4], [IAMaster], [Com1] (bot logic)
- Anas: [MY STATS], [D4]-[D8], [C1], [C2], [C4], [IAMaster], [Com1] (UI)
- All: [QA]
```
