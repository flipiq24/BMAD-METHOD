---
stepsCompleted: ['requirements-inventory', 'epic-grouping', 'story-breakdown', 'ac-definition', 'validation']
inputDocuments: ['PRD.md', 'architecture.md']
workflowType: 'create-epics'
track: 'BMad Method'
---

# FlipIQ AI Overlay System - Epic Breakdown

**Version:** 2.0
**Date:** December 2024
**Status:** Ready for Implementation

---

## Requirements Inventory

### Functional Requirements Coverage

| FR ID | Requirement | Epic | Priority |
|-------|-------------|------|----------|
| FR-001 | Universal Interface Bot (AA0) | Epic 1 | P0 |
| FR-002 | Morning Check-In Bot (AA1) | Epic 2 | P0 ✅ |
| FR-003 | Deal Review Bot (AA2) | Epic 2 | P0 ✅ |
| FR-004 | Daily Outreach Bot (AA3) | Epic 2 | P0 ✅ |
| FR-005 | Agent Relationship Bot (AA4) | Epic 2 | P0 ✅ |
| FR-006 | Property Intelligence Bot (PIQ) | Epic 3 | P0 ✅ |
| FR-007 | MLS Deal Review Bot (D1) | Epic 3 | P0 ✅ |
| FR-008 | Agent Review Bot (D2) | Epic 3 | P0 |
| FR-009 | Propensity Analysis Bot (D3) | Epic 3 | P0 ✅ |
| FR-010 | Notes Bot (D4) | Epic 4 | P1 |
| FR-011 | Reminders Bot (D5) | Epic 4 | P1 |
| FR-012 | Activity Bot (D6) | Epic 4 | P1 |
| FR-013 | Post Call Bot (D7) | Epic 4 | P1 |
| FR-014 | Close Deal Report Bot (D8) | Epic 4 | P2 |
| FR-015 | Management Master Bot (MGTMaster) | Epic 5 | P0 |
| FR-016 | End-of-Day Debrief Bot (MGT2) | Epic 5 | P0 |
| FR-017 | Mailbox Monitor Bot (MGT3) | Epic 5 | P1 |
| FR-018 | Marketing Master Bot (MMaster) | Epic 6 | P1 |
| FR-019 | Honeypot Bot (M1) | Epic 6 | P1 |
| FR-020 | Deal Email Pipeline Bot (M2) | Epic 6 | P1 |
| FR-021 | Retargeting Bot (M3) | Epic 6 | P1 |
| FR-022 | Bi-Monthly Content Bot (M4) | Epic 6 | P2 |
| FR-023 | Digital Encapsulation Bot (M5) | Epic 6 | P2 |
| FR-024 | Comp Master Bot (CMaster) | Epic 7 | P0 |
| FR-025 | Map Review Bot (C1) | Epic 7 | P1 |
| FR-026 | Matrix Summary Bot (C2) | Epic 7 | P1 |
| FR-027 | List Grouping Bot (C3) | Epic 7 | P1 |
| FR-028 | Comp AI Mapping Bot (C4) | Epic 7 | P2 |
| FR-029 | Investment Master Bot (IAMaster) | Epic 8 | P0 |
| FR-030 | Repair Calculator Bot (IARehab) | Epic 8 | P0 |
| FR-031 | Auto Connect Bot (Com1) | Epic 9 | P1 |
| FR-032 | Super Admin Bot (SuperMaster) | Epic 10 | P0 |

### Non-Functional Requirements Coverage

| NFR ID | Requirement | Epic |
|--------|-------------|------|
| NFR-001 | Performance | All Epics |
| NFR-002 | Scalability | Epic 10 |
| NFR-003 | Reliability | All Epics |
| NFR-004 | Security | Epic 11 |
| NFR-005 | Integration | Epic 12 |
| NFR-006 | Usability | All Epics |
| NFR-007 | Compliance | Epic 11 |

---

## Epic List

### Epic 1: Universal Interface (iQ Overlay)

**Goal:** Provide a single conversational interface for all FlipIQ functions through voice/text commands.

**Phase:** 2 (December 2024)
**Priority:** P0
**Estimated Stories:** 5

#### Story 1.1: iQ Overlay UI Framework

**As a** FlipIQ user (AA/AM/Principal)
**I want** an iQ overlay that slides from the right side of my screen
**So that** I can interact with FlipIQ without leaving my current context

**Acceptance Criteria:**
1. Given I am logged into FlipIQ, when I click the iQ icon, then the overlay slides in from the right within 300ms
2. Given the overlay is open, when I click outside or press ESC, then it slides closed
3. Given I am on any page, when I open iQ, then my current context is preserved
4. Given I resize my browser, when the overlay is open, then it remains responsive and usable

**Tasks:**
- [ ] Create overlay component with slide animation
- [ ] Implement open/close triggers
- [ ] Add responsive design for mobile/tablet
- [ ] Integrate with existing navigation state

---

#### Story 1.2: Voice/Text Property Entry

**As an** AA
**I want** to add properties by speaking or typing an address
**So that** I can create property records instantly without navigating

**Acceptance Criteria:**
1. Given I say "Add property 123 Main Street", when processing completes, then a property record is created in <5 seconds
2. Given I type "add 456 Oak Ave", when I press enter, then the property is added with confirmation
3. Given the address is incomplete, when I submit, then I'm prompted for missing information
4. Given a duplicate property exists, when I try to add, then I'm shown the existing record with option to view

**Tasks:**
- [ ] Implement speech-to-text integration
- [ ] Create address parser/normalizer
- [ ] Add property creation API call
- [ ] Handle duplicates and validation errors

---

#### Story 1.3: Wholesale Property Qualifying

**As an** AA
**I want** FlipIQ to ask qualifying questions and auto-populate DispoPro
**So that** wholesale properties are properly categorized for disposition

**Acceptance Criteria:**
1. Given I say "wholesale this property", when prompted, then qualifying questions appear in sequence
2. Given I answer all questions, when complete, then DispoPro receives the qualified data
3. Given I skip a required question, when I try to proceed, then I'm blocked with explanation
4. Given DispoPro integration fails, when submitting, then I see error with retry option

**Tasks:**
- [ ] Design qualifying question flow
- [ ] Build question state machine
- [ ] Implement DispoPro API integration
- [ ] Add error handling and retry logic

---

#### Story 1.4: Bug Reporting via Intercom

**As any** FlipIQ user
**I want** to report bugs directly through iQ
**So that** issues are logged without leaving my workflow

**Acceptance Criteria:**
1. Given I say "report a bug", when prompted, then I can describe the issue
2. Given I describe the bug, when submitted, then an Intercom ticket is created
3. Given the ticket is created, when confirmed, then I see ticket number
4. Given I include a screenshot, when submitted, then it's attached to the ticket

**Tasks:**
- [ ] Create bug report dialog
- [ ] Integrate Intercom API
- [ ] Add screenshot capture capability
- [ ] Implement ticket confirmation display

---

#### Story 1.5: Natural Language Navigation

**As an** AA
**I want** to navigate using natural language commands
**So that** I can quickly reach any part of the system

**Acceptance Criteria:**
1. Given I say "show me pending deals", when processed, then Pipeline filters to pending status
2. Given I say "go to agent Tommy Chen", when processed, then Agent detail page opens
3. Given I say an ambiguous command, when processed, then I see clarification options
4. Given the destination doesn't exist, when processed, then I see helpful suggestions

**Tasks:**
- [ ] Build intent recognition model
- [ ] Map intents to navigation routes
- [ ] Handle ambiguous queries
- [ ] Implement suggestion engine

---

### Epic 2: AA Daily Process Bots

**Goal:** Automate the daily workflow for Acquisition Associates to ensure consistent performance.

**Phase:** 1 ✅ (Complete)
**Priority:** P0
**Estimated Stories:** 8

#### Story 2.1: Morning Check-In Trigger (AA1) ✅

**As an** AA
**I want** the check-in bot to activate on my first login
**So that** I start each day with a structured process

**Acceptance Criteria:**
1. Given it's my first login of the day, when the page loads, then AA1 activates within 2 seconds ✅
2. Given I've already checked in today, when I login again, then check-in is skipped ✅
3. Given the check-in flag is set, when I log out/in, then flag persists for 24 hours ✅

**Status:** COMPLETE

---

#### Story 2.2: Availability & Blocker Capture (AA1) ✅

**As an** AA
**I want** to confirm my availability and report blockers
**So that** management knows my capacity

**Acceptance Criteria:**
1. Given check-in starts, when asked about availability, then I can respond yes/no ✅
2. Given I report a blocker, when submitted, then AM receives notification within 30 seconds ✅
3. Given I need help, when I describe it, then help request is routed to AM ✅

**Status:** COMPLETE

---

#### Story 2.3: Deal Priority Sorting (AA2) ✅

**As an** AA
**I want** my deals sorted by priority
**So that** I work on the most important items first

**Acceptance Criteria:**
1. Given check-in completes, when AA2 activates, then deals are sorted Critical → Hot → Warm → Cold → New ✅
2. Given sorting completes, when displayed, then accurate counts show for each category ✅
3. Given a deal's status changes, when I refresh, then it re-sorts correctly ✅

**Status:** COMPLETE

---

#### Story 2.4: Deal Review Navigation (AA2) ✅

**As an** AA
**I want** one-click navigation to Deal Review
**So that** I can start working immediately

**Acceptance Criteria:**
1. Given AA2 displays my action plan, when I click "Go to Deal Review", then Deal Review page opens ✅
2. Given Deal Review opens, when loaded, then first Critical property is highlighted ✅
3. Given no Critical properties, when loaded, then first Hot property is highlighted ✅

**Status:** COMPLETE

---

#### Story 2.5: 30-Property Selection (AA3) ✅

**As an** AA
**I want** 30 high-propensity properties selected for me
**So that** I focus on the best opportunities

**Acceptance Criteria:**
1. Given Deal Review completes, when AA3 activates, then 30 properties are selected ✅
2. Given selection runs, when filtering, then Assigned agents are prioritized (Priority → Hot → Warm → Cold) ✅
3. Given selection runs, when sorting, then properties are sorted by PTFV (lowest first) ✅
4. Given all Assigned properties are processed, when continuing, then Unassigned high-ISC agents are included ✅

**Status:** COMPLETE

---

#### Story 2.6: Personalized Script Generation (AA3) ✅

**As an** AA
**I want** personalized scripts for each property/agent
**So that** I have effective talking points

**Acceptance Criteria:**
1. Given a property is displayed, when script generates, then it includes agent's actual transaction history ✅
2. Given an Aged listing (70+ DOM), when script generates, then it uses the Aged Listing script template ✅
3. Given a New listing, when script generates, then it uses the New Listing script template ✅
4. Given seller distress signals, when script generates, then pain points are included ✅

**Status:** COMPLETE

---

#### Story 2.7: Conversation Tracking (AA3) ✅

**As an** AA
**I want** my conversations (not just calls) tracked
**So that** quality is measured, not just quantity

**Acceptance Criteria:**
1. Given I complete a call with conversation, when I mark complete, then conversation counter increments ✅
2. Given I make a call with no answer, when I log it, then call counter increments but not conversation ✅
3. Given I reach 30 conversations, when complete, then progress shows 30/30 ✅

**Status:** COMPLETE

---

#### Story 2.8: Agent Relationship Dashboard (AA4) ✅

**As an** AA
**I want** to see my progress toward 100 elite agent relationships
**So that** I can track my network building

**Acceptance Criteria:**
1. Given I open AA4, when dashboard loads, then current count shows (e.g., 45/100) ✅
2. Given I upgrade an agent to Priority, when saved, then count updates ✅
3. Given an agent hasn't been contacted in 30 days, when reviewing, then follow-up reminder appears ✅

**Status:** COMPLETE

---

### Epic 3: Deal Analysis Core (PIQ + D1-D3)

**Goal:** Provide comprehensive property intelligence through data source integration and AI analysis.

**Phase:** 1 (Mostly Complete) + Phase 2 (D2 completion)
**Priority:** P0
**Estimated Stories:** 7

#### Story 3.1: Data Source Merging (PIQ) ✅

**As an** AA
**I want** MLS, PropertyRadar, and Agent365 data in one view
**So that** I don't need to switch between systems

**Acceptance Criteria:**
1. Given I open a property, when PIQ loads, then all three data sources are displayed ✅
2. Given data loading, when complete, then full report appears in <30 seconds ✅
3. Given one source fails, when others succeed, then partial data displays with warning ✅

**Status:** COMPLETE

---

#### Story 3.2: Deal Focus Index Calculation (PIQ) ✅

**As an** AA
**I want** a DFI score calculated automatically
**So that** I know which deals to prioritize

**Acceptance Criteria:**
1. Given property data loads, when DFI calculates, then score displays with breakdown ✅
2. Given FixerCondition + InventoryStage + SellerPain + AgentBehavior, when combined, then DFI = sum ✅
3. Given DFI changes, when I refresh, then updated score displays ✅

**Status:** COMPLETE

---

#### Story 3.3: MLS Keyword Extraction (D1) ✅

**As an** AA
**I want** distress keywords extracted from MLS remarks
**So that** I can quickly identify opportunity signals

**Acceptance Criteria:**
1. Given MLS remarks contain "as-is", when D1 runs, then keyword is highlighted with weight ✅
2. Given multiple keywords exist, when extracted, then all are listed with relevance scores ✅
3. Given no keywords found, when displayed, then "No distress signals" message appears ✅

**Status:** COMPLETE

---

#### Story 3.4: Chase/Don't Chase Recommendation (D1) ✅

**As an** AA
**I want** a clear recommendation on whether to pursue
**So that** I don't waste time on poor opportunities

**Acceptance Criteria:**
1. Given analysis completes, when D1 finishes, then Chase or Don't Chase displays ✅
2. Given "Chase" recommendation, when displayed, then reasoning is provided ✅
3. Given "Don't Chase" recommendation, when displayed, then specific disqualifiers are listed ✅

**Status:** COMPLETE

---

#### Story 3.5: Agent Transaction History Pull (D2)

**As an** AA
**I want** to see the listing agent's transaction history
**So that** I can tailor my approach

**Acceptance Criteria:**
1. Given property loads, when D2 runs, then last 2 years of transactions display
2. Given agent has investor transactions, when displayed, then investor names and counts show
3. Given agent has no investor history, when displayed, then warning appears

**Tasks:**
- [ ] Complete Agent365 API integration
- [ ] Build transaction history component
- [ ] Add investor relationship mapping
- [ ] Implement investor-friendliness scoring

**Status:** 90% Complete - API integration remaining

---

#### Story 3.6: Agent Approach Strategy (D2)

**As an** AA
**I want** a recommended approach strategy for each agent
**So that** I know how to engage effectively

**Acceptance Criteria:**
1. Given agent history loads, when D2 completes, then tier-specific script is generated
2. Given agent is Tier 1 (High Value), when displayed, then premium approach script shows
3. Given agent is new/unknown, when displayed, then discovery approach script shows

**Tasks:**
- [ ] Build agent tier classification logic
- [ ] Create script templates for each tier
- [ ] Implement dynamic script generation
- [ ] Add buyer ecosystem display

**Status:** Pending (Phase 2)

---

#### Story 3.7: Propensity Score with PropertyRadar (D3) ✅

**As an** AA
**I want** seller distress signals from PropertyRadar
**So that** I understand seller motivation

**Acceptance Criteria:**
1. Given property loads, when D3 queries PropertyRadar, then distress signals display ✅
2. Given NOD exists, when scoring, then +6 points added ✅
3. Given multiple signals exist, when totaled, then combined Pain Score displays ✅
4. Given Pain Score > 10, when displayed, then "EXTREME DISTRESS" label appears ✅

**Status:** COMPLETE

---

### Epic 4: Extended Deal Analysis (D4-D8)

**Goal:** Provide comprehensive deal lifecycle support with notes, reminders, activity tracking, and post-deal analysis.

**Phase:** 2 (December 2024)
**Priority:** P1-P2
**Estimated Stories:** 8

#### Story 4.1: Auto-Categorized Notes (D4)

**As an** AA
**I want** my notes automatically categorized
**So that** I can find relevant information quickly

**Acceptance Criteria:**
1. Given I add a note, when saved, then it's auto-tagged (call summary, price discussion, etc.)
2. Given I search notes, when filtering by category, then only matching notes display
3. Given I view a property, when notes section loads, then most recent notes appear first

**Tasks:**
- [ ] Build note categorization model
- [ ] Create category tag database
- [ ] Implement search/filter functionality
- [ ] Add chronological sorting

---

#### Story 4.2: Action Item Extraction (D4)

**As an** AA
**I want** action items extracted from my notes
**So that** I don't forget follow-up tasks

**Acceptance Criteria:**
1. Given I write "call back on Monday", when saved, then action item is created
2. Given action items exist, when viewing property, then they appear in task list
3. Given action item is completed, when marked done, then it's archived

**Tasks:**
- [ ] Build NLP action item extractor
- [ ] Create action item data model
- [ ] Implement task list component
- [ ] Add completion tracking

---

#### Story 4.3: Stage-Based Reminders (D5)

**As an** AA
**I want** automatic reminders based on deal stage
**So that** I never miss a follow-up

**Acceptance Criteria:**
1. Given a property goes Pending, when Day 3 arrives, then deposit check reminder fires
2. Given a property is Aged (70+ DOM), when Day 90 approaches, then expiration reminder fires
3. Given I dismiss a reminder, when viewing next day, then it doesn't repeat

**Tasks:**
- [ ] Build reminder scheduling engine
- [ ] Create stage-based reminder templates
- [ ] Implement notification delivery
- [ ] Add dismissal tracking

---

#### Story 4.4: Auto Follow-Up Sequences (D5)

**As an** AA
**I want** automatic follow-up sequences for pending properties
**So that** I stay on top of escrow progress

**Acceptance Criteria:**
1. Given property goes Pending, when Day 3 arrives, then "deposit posted?" reminder fires
2. Given property still Pending, when Day 7 arrives, then "contingencies removed?" reminder fires
3. Given property still Pending, when Day 15 arrives, then "deal on track?" reminder fires

**Tasks:**
- [ ] Design follow-up sequence logic
- [ ] Implement day-based triggers
- [ ] Create follow-up message templates
- [ ] Build sequence tracking

---

#### Story 4.5: Automatic Activity Logging (D6)

**As an** AA
**I want** my calls, emails, and texts logged automatically
**So that** I don't have to manually track everything

**Acceptance Criteria:**
1. Given I make a call through FlipIQ, when call ends, then activity is logged
2. Given I send an email, when sent, then activity is logged with content
3. Given I view activity timeline, when loading, then all interactions appear chronologically

**Tasks:**
- [ ] Integrate call tracking
- [ ] Integrate email tracking
- [ ] Build activity timeline component
- [ ] Add activity type icons and filtering

---

#### Story 4.6: Call Recording & Transcription (D7)

**As an** AA
**I want** my calls recorded and transcribed
**So that** I can review conversations and improve

**Acceptance Criteria:**
1. Given I initiate a call, when consent is given, then recording starts
2. Given call ends, when processing completes in <2 minutes, then transcription is available
3. Given transcription exists, when viewing, then AI highlights key moments

**Tasks:**
- [ ] Implement call recording integration
- [ ] Add consent capture flow
- [ ] Integrate transcription service
- [ ] Build transcription viewer

---

#### Story 4.7: Call Performance Analysis (D7)

**As an** AA
**I want** AI analysis of my call performance
**So that** I can improve my skills

**Acceptance Criteria:**
1. Given transcription exists, when D7 analyzes, then script adherence score displays
2. Given analysis completes, when viewing, then specific improvement suggestions appear
3. Given multiple calls analyzed, when viewing trends, then progress over time shows

**Tasks:**
- [ ] Build script comparison model
- [ ] Create performance scoring algorithm
- [ ] Implement suggestion generator
- [ ] Add trend visualization

---

#### Story 4.8: Post-Close Analysis (D8)

**As an** AA/AM
**I want** analysis of closed deals
**So that** we can learn what works

**Acceptance Criteria:**
1. Given deal closes, when D8 runs, then success factors are identified
2. Given analysis completes, when viewing, then "what worked" and "what didn't" sections appear
3. Given market data exists, when analyzing, then ARV accuracy and days-to-close are compared to averages

**Tasks:**
- [ ] Build close deal trigger
- [ ] Create success factor extraction
- [ ] Implement market comparison
- [ ] Add lessons learned generator

---

### Epic 5: Management Bots (MGT1-3)

**Goal:** Enable exception-based management with real-time visibility and automated reporting.

**Phase:** 2 (December 2024)
**Priority:** P0-P1
**Estimated Stories:** 6

#### Story 5.1: Management Dashboard Aggregation (MGTMaster)

**As a** Principal/AM
**I want** all team data aggregated in one dashboard
**So that** I can see performance at a glance

**Acceptance Criteria:**
1. Given I login as AM, when dashboard loads, then all assigned AA metrics display
2. Given I login as Principal, when dashboard loads, then company-wide metrics display
3. Given data updates, when refreshing, then dashboard updates in <5 seconds

**Tasks:**
- [ ] Build role-based data aggregation
- [ ] Create dashboard layout
- [ ] Implement real-time refresh
- [ ] Add metric cards and charts

---

#### Story 5.2: Exception Highlighting (MGTMaster)

**As a** Principal/AM
**I want** only problems surfaced prominently
**So that** I don't waste time on routine activity

**Acceptance Criteria:**
1. Given an AA misses check-in, when viewing dashboard, then alert appears at top
2. Given an AA is below daily targets, when viewing, then their name is highlighted red
3. Given all AAs are on track, when viewing, then "All systems normal" displays

**Tasks:**
- [ ] Define exception criteria
- [ ] Build exception detection engine
- [ ] Create alert component
- [ ] Implement severity-based styling

---

#### Story 5.3: Automated 5PM EOD Report (MGT2)

**As a** Principal/AM
**I want** an automated end-of-day report at 5PM
**So that** I can review without manual compilation

**Acceptance Criteria:**
1. Given 5PM arrives, when MGT2 triggers, then EOD report is generated
2. Given report generates, when complete, then it's emailed to AM and Principal
3. Given report displays, when viewing, then all KPIs vs targets are shown

**Tasks:**
- [ ] Build scheduled job for 5PM trigger
- [ ] Create report template
- [ ] Implement email delivery
- [ ] Add KPI comparison logic

---

#### Story 5.4: AI Coaching Suggestions (MGT2)

**As a** Principal/AM
**I want** AI-generated coaching suggestions
**So that** I know how to help each AA

**Acceptance Criteria:**
1. Given AA is underperforming, when report generates, then specific coaching tips appear
2. Given AA is excelling, when report generates, then recognition suggestion appears
3. Given AA has pattern issues, when analyzing, then root cause is suggested

**Tasks:**
- [ ] Build performance pattern analysis
- [ ] Create coaching suggestion templates
- [ ] Implement AI recommendation engine
- [ ] Add pattern detection

---

#### Story 5.5: Team Leaderboard (MGT2)

**As an** AA/AM/Principal
**I want** to see team rankings
**So that** healthy competition is fostered

**Acceptance Criteria:**
1. Given EOD report generates, when leaderboard displays, then AAs are ranked by composite score
2. Given I'm an AA, when viewing, then my rank is highlighted
3. Given week completes, when viewing, then weekly rankings appear

**Tasks:**
- [ ] Build composite scoring algorithm
- [ ] Create leaderboard component
- [ ] Implement ranking logic
- [ ] Add weekly aggregation

---

#### Story 5.6: Email Priority Filtering (MGT3)

**As a** Principal/AM
**I want** critical emails surfaced automatically
**So that** urgent matters don't get buried

**Acceptance Criteria:**
1. Given email arrives with "urgent" or "ASAP", when MGT3 scans, then it's flagged priority
2. Given email mentions specific deal numbers, when scanned, then it's linked to deal
3. Given priority inbox is viewed, when loading, then only flagged emails appear

**Tasks:**
- [ ] Build email scanning integration
- [ ] Create urgency detection rules
- [ ] Implement deal linking
- [ ] Add priority inbox view

---

### Epic 6: Marketing Automation (MMaster + M1-M5)

**Goal:** Automate marketing operations including campaigns, lead intake, and content generation.

**Phase:** 2 (December 2024)
**Priority:** P1-P2
**Estimated Stories:** 8

#### Story 6.1: Marketing Bot Orchestration (MMaster)

**As a** System
**I want** all marketing bots coordinated
**So that** campaigns run smoothly

**Acceptance Criteria:**
1. Given daily schedule runs, when MMaster activates, then M1-M5 tasks are queued
2. Given M1 completes, when reporting, then MMaster logs success
3. Given any bot fails, when detected, then alert is generated

**Tasks:**
- [ ] Build marketing scheduler
- [ ] Create bot coordination layer
- [ ] Implement success/failure logging
- [ ] Add alerting

---

#### Story 6.2: VA Social Media Instructions (M1)

**As a** VA
**I want** daily posting instructions generated
**So that** I know exactly what to post

**Acceptance Criteria:**
1. Given new day starts, when M1 runs, then posting instructions are emailed to VA
2. Given instructions include, when viewing, then copy, images, and platforms are specified
3. Given 10+ leads/week target, when tracking, then lead source is tagged

**Tasks:**
- [ ] Build posting instruction generator
- [ ] Create content templates
- [ ] Implement VA email delivery
- [ ] Add lead tracking

---

#### Story 6.3: Deal Email Auto-Processing (M2)

**As a** System
**I want** deal@ emails automatically processed
**So that** leads enter pipeline without manual entry

**Acceptance Criteria:**
1. Given email arrives at deal@, when M2 processes, then property data is extracted
2. Given property data extracted, when creating, then property card appears in Dashboard
3. Given extraction fails, when logging, then email is flagged for manual review

**Tasks:**
- [ ] Build email parser
- [ ] Create property extraction rules
- [ ] Implement property card creation
- [ ] Add failure handling

---

#### Story 6.4: Agent Segmentation for Campaigns (M3)

**As a** Principal/AM
**I want** agents segmented by tier for targeted campaigns
**So that** messaging is relevant

**Acceptance Criteria:**
1. Given campaign is created, when selecting audience, then tier options display (Priority/Hot/Warm/Cold)
2. Given tier is selected, when previewing, then matching agent count shows
3. Given campaign sends, when tracking, then opens/clicks are recorded per tier

**Tasks:**
- [ ] Build audience segmentation
- [ ] Create tier selection UI
- [ ] Implement campaign tracking
- [ ] Add analytics dashboard

---

#### Story 6.5: Monthly Touch Calendar (M3)

**As a** Principal/AM
**I want** a campaign calendar showing scheduled touches
**So that** I can ensure 2x monthly coverage

**Acceptance Criteria:**
1. Given calendar loads, when viewing, then scheduled campaigns display
2. Given agent hasn't been touched this month, when viewing, then gap is highlighted
3. Given 2x monthly target, when calculating, then coverage % displays

**Tasks:**
- [ ] Build campaign calendar component
- [ ] Create touch tracking
- [ ] Implement gap detection
- [ ] Add coverage metrics

---

#### Story 6.6: Market Content Generation (M4)

**As a** Principal/AM
**I want** market content auto-generated
**So that** I can send value-add content easily

**Acceptance Criteria:**
1. Given bi-monthly schedule, when M4 runs, then market update content is generated
2. Given content includes, when viewing, then local market stats and insights appear
3. Given content is approved, when sending, then it goes to Warm agent segment

**Tasks:**
- [ ] Build content generation engine
- [ ] Integrate market data sources
- [ ] Create approval workflow
- [ ] Implement scheduled sending

---

#### Story 6.7: Open Rate Tracking (M4)

**As a** Principal/AM
**I want** email performance tracked
**So that** I know what content works

**Acceptance Criteria:**
1. Given campaign sends, when emails open, then open rate updates in real-time
2. Given >20% open rate target, when achieved, then success indicator displays
3. Given <20% open rate, when viewing, then suggestions for improvement appear

**Tasks:**
- [ ] Implement email tracking pixels
- [ ] Build analytics dashboard
- [ ] Create performance benchmarks
- [ ] Add improvement suggestions

---

#### Story 6.8: Digital Presence Scorecard (M5)

**As a** Principal/AM
**I want** a digital presence score
**So that** I know where to improve online

**Acceptance Criteria:**
1. Given M5 analyzes presence, when complete, then scorecard displays
2. Given score categories include, when viewing, then website, social, reviews are scored
3. Given improvement needed, when viewing, then specific recommendations appear

**Tasks:**
- [ ] Build presence analysis engine
- [ ] Create scoring algorithm
- [ ] Implement scorecard component
- [ ] Add recommendation generator

---

### Epic 7: Comp Analysis Bots (CMaster + C1-C4)

**Goal:** Provide comprehensive comparable property analysis with visual validation and AI-powered adjustments.

**Phase:** 2-3
**Priority:** P0-P2
**Estimated Stories:** 6

#### Story 7.1: Comp Analysis Orchestration (CMaster)

**As a** System
**I want** comp analysis bots coordinated
**So that** comprehensive reports are generated

**Acceptance Criteria:**
1. Given property loads, when CMaster activates, then C1-C4 are triggered in sequence
2. Given all bots complete, when aggregating, then unified comp report displays
3. Given any bot fails, when detected, then partial report with warning displays

**Tasks:**
- [ ] Build comp orchestration layer
- [ ] Create sequential trigger logic
- [ ] Implement report aggregation
- [ ] Add failure handling

---

#### Story 7.2: Map-Based Comp Validation (C1)

**As an** AA
**I want** to see comps on a map with overlays
**So that** I can validate location quality

**Acceptance Criteria:**
1. Given comp report loads, when map displays, then all comps are plotted
2. Given school district overlay is toggled, when viewing, then boundaries appear
3. Given busy street overlay is toggled, when viewing, then traffic patterns show
4. Given comp is on wrong side of boundary, when viewing, then warning indicator appears

**Tasks:**
- [ ] Integrate mapping service
- [ ] Build overlay toggle system
- [ ] Create boundary data integration
- [ ] Implement quality indicators

---

#### Story 7.3: Statistical Comp Analysis (C2)

**As an** AA
**I want** statistical analysis of valid comps
**So that** I understand price ranges

**Acceptance Criteria:**
1. Given comps are filtered, when C2 analyzes, then price/sqft ranges display
2. Given statistical analysis runs, when complete, then min/median/max shows
3. Given outliers exist, when identified, then they're flagged for review

**Tasks:**
- [ ] Build outlier detection
- [ ] Create statistical summary component
- [ ] Implement price/sqft calculations
- [ ] Add visualization charts

---

#### Story 7.4: Condition-Based Grouping (C3)

**As an** AA
**I want** comps grouped by condition
**So that** I can establish value ceiling

**Acceptance Criteria:**
1. Given comps exist, when C3 groups, then High/Mid/Low condition categories appear
2. Given grouping complete, when viewing, then value ceiling with explanation displays
3. Given subject property condition known, when comparing, then appropriate group is highlighted

**Tasks:**
- [ ] Build condition classification model
- [ ] Create grouping algorithm
- [ ] Implement value ceiling calculation
- [ ] Add comparison visualization

---

#### Story 7.5: Photo-Based Condition Assessment (C4)

**As an** AA
**I want** AI to assess comp condition from photos
**So that** manual review is reduced

**Acceptance Criteria:**
1. Given comp has photos, when C4 analyzes, then condition score is generated
2. Given analysis completes, when viewing, then key features are highlighted
3. Given condition differs from listing description, when detected, then warning appears

**Tasks:**
- [ ] Integrate computer vision API
- [ ] Build condition scoring model
- [ ] Create feature extraction
- [ ] Implement discrepancy detection

---

#### Story 7.6: Tax Data Integration (C4)

**As an** AA
**I want** tax records integrated with comp data
**So that** property details are accurate

**Acceptance Criteria:**
1. Given comp loads, when tax data fetches, then sqft/beds/baths are verified
2. Given MLS differs from tax records, when detected, then discrepancy is highlighted
3. Given tax data unavailable, when loading, then MLS data is used with warning

**Tasks:**
- [ ] Build tax data integration
- [ ] Create verification logic
- [ ] Implement discrepancy alerts
- [ ] Add fallback handling

---

### Epic 8: Investment Analysis (IAMaster + IARehab)

**Goal:** Provide buy box alignment and accurate rehab cost estimation for investment decisions.

**Phase:** 2-3
**Priority:** P0
**Estimated Stories:** 5

#### Story 8.1: Buy Box Alignment (IAMaster)

**As an** AA
**I want** properties evaluated against my buy box
**So that** I know if deals fit our criteria

**Acceptance Criteria:**
1. Given property loads, when IAMaster evaluates, then buy box match % displays
2. Given match is <80%, when viewing, then specific mismatches are listed
3. Given match is >90%, when viewing, then "Strong Fit" indicator appears

**Tasks:**
- [ ] Create buy box configuration
- [ ] Build matching algorithm
- [ ] Implement match visualization
- [ ] Add mismatch details

---

#### Story 8.2: ROI/IRR Calculation (IAMaster)

**As an** AA
**I want** automatic return calculations
**So that** I can evaluate investment quality

**Acceptance Criteria:**
1. Given property data loads, when IAMaster calculates, then ROI displays
2. Given holding period estimated, when calculating, then IRR displays
3. Given inputs change, when recalculating, then returns update in real-time

**Tasks:**
- [ ] Build ROI calculation engine
- [ ] Implement IRR calculation
- [ ] Create real-time update logic
- [ ] Add sensitivity analysis

---

#### Story 8.3: Deal Structure Suggestions (IAMaster)

**As an** AA
**I want** optimal deal structure suggested
**So that** I know how to approach the offer

**Acceptance Criteria:**
1. Given analysis completes, when IAMaster suggests, then recommended structure displays
2. Given wholesale is optimal, when viewing, then wholesale strategy is shown
3. Given flip is optimal, when viewing, then flip strategy with exit timeline shows

**Tasks:**
- [ ] Build structure optimization logic
- [ ] Create strategy templates
- [ ] Implement recommendation engine
- [ ] Add timeline visualization

---

#### Story 8.4: Photo-Based Rehab Estimation (IARehab)

**As an** AA
**I want** rehab costs estimated from photos
**So that** I can make offers without visiting

**Acceptance Criteria:**
1. Given property photos exist, when IARehab analyzes, then repair areas are identified
2. Given analysis completes, when viewing, then line-item cost estimate displays
3. Given estimate accuracy target <10%, when compared to actual, then variance is tracked

**Tasks:**
- [ ] Integrate computer vision for damage detection
- [ ] Build cost database by region
- [ ] Create line-item estimate generator
- [ ] Implement accuracy tracking

---

#### Story 8.5: Rehab Cost Database (IARehab)

**As a** System
**I want** market-specific cost models
**So that** estimates are accurate locally

**Acceptance Criteria:**
1. Given property in California, when estimating, then CA labor/material rates are used
2. Given cost database updated, when estimating, then new rates apply immediately
3. Given regional data missing, when estimating, then national averages with warning are used

**Tasks:**
- [ ] Build regional cost database
- [ ] Create update mechanism
- [ ] Implement rate application logic
- [ ] Add fallback handling

---

### Epic 9: Communication Automation (Com1)

**Goal:** Automate follow-up communication to ensure consistent agent engagement.

**Phase:** 2 (December 2024)
**Priority:** P1
**Estimated Stories:** 3

#### Story 9.1: Multi-Touch Follow-Up Sequences (Com1)

**As an** AA
**I want** automated follow-up after my manual attempts
**So that** I maintain contact without manual tracking

**Acceptance Criteria:**
1. Given I make a call with no answer, when sequence starts, then Day 1 text is queued
2. Given no response after Day 1 text, when Day 3 arrives, then second touch is sent
3. Given response received, when detected, then sequence stops automatically

**Tasks:**
- [ ] Build sequence engine
- [ ] Create touch templates
- [ ] Implement response detection
- [ ] Add sequence management UI

---

#### Story 9.2: Response Detection (Com1)

**As a** System
**I want** agent responses detected automatically
**So that** sequences stop appropriately

**Acceptance Criteria:**
1. Given agent replies to text, when detected, then AA is notified
2. Given agent replies to email, when detected, then sequence pauses
3. Given negative response ("not interested"), when detected, then agent is flagged

**Tasks:**
- [ ] Build response monitoring
- [ ] Create notification triggers
- [ ] Implement sentiment detection
- [ ] Add agent flagging

---

#### Story 9.3: 100% Follow-Up Tracking (Com1)

**As an** AM
**I want** to verify 100% follow-up rate
**So that** no opportunities are lost

**Acceptance Criteria:**
1. Given reporting runs, when viewing, then follow-up completion % displays
2. Given follow-up is missed, when detected, then alert is generated
3. Given 100% achieved, when viewing, then success indicator appears

**Tasks:**
- [ ] Build follow-up tracking
- [ ] Create completion metrics
- [ ] Implement alert generation
- [ ] Add success visualization

---

### Epic 10: Enterprise Scale (SuperMaster)

**Goal:** Enable enterprise-wide performance tracking across 375+ operators.

**Phase:** 3 (Q1 2025)
**Priority:** P0
**Estimated Stories:** 6

#### Story 10.1: Multi-Operator Dashboard (SuperMaster)

**As a** COO
**I want** all 375 operators visible in one dashboard
**So that** I can manage the entire operation

**Acceptance Criteria:**
1. Given I login as COO, when dashboard loads, then all operators display
2. Given operators are listed, when sorting, then I can sort by revenue, deals, health
3. Given dashboard loads, when complete, then it renders in <5 seconds

**Tasks:**
- [ ] Build enterprise data aggregation
- [ ] Create operator list component
- [ ] Implement sorting/filtering
- [ ] Optimize for performance

---

#### Story 10.2: Regional Performance View (SuperMaster)

**As a** COO
**I want** performance broken down by region
**So that** I can identify geographic trends

**Acceptance Criteria:**
1. Given regional view selected, when loading, then West/Texas/Southeast/Midwest/Northeast display
2. Given region selected, when drilling down, then operators in that region display
3. Given regional target exists, when comparing, then variance is shown

**Tasks:**
- [ ] Build regional grouping
- [ ] Create drill-down navigation
- [ ] Implement target comparison
- [ ] Add variance visualization

---

#### Story 10.3: Operator Tier Classification (SuperMaster)

**As a** COO
**I want** operators classified by performance tier
**So that** I can allocate resources appropriately

**Acceptance Criteria:**
1. Given operators are analyzed, when classifying, then Tier 1/2/3/4 labels are assigned
2. Given Tier 4 operators, when viewing, then intervention recommendations appear
3. Given tier changes, when detected, then alert is generated

**Tasks:**
- [ ] Build tier classification algorithm
- [ ] Create tier assignment logic
- [ ] Implement intervention recommendations
- [ ] Add change detection

---

#### Story 10.4: Success Manager Assignment (SuperMaster)

**As a** COO
**I want** to assign success managers to operators
**So that** support is distributed

**Acceptance Criteria:**
1. Given success manager selected, when assigning, then operators are linked
2. Given SM has 25 operators, when viewing their dashboard, then those operators display
3. Given SM's operators underperform, when detected, then SM is alerted

**Tasks:**
- [ ] Build SM assignment system
- [ ] Create SM dashboard
- [ ] Implement alert routing
- [ ] Add workload balancing

---

#### Story 10.5: Revenue Collection Dashboard (SuperMaster)

**As a** COO
**I want** to track revenue collection
**So that** cash flow is managed

**Acceptance Criteria:**
1. Given monthly billing runs, when viewing, then total due vs collected displays
2. Given operator is 60+ days overdue, when viewing, then they're flagged critical
3. Given collection actions, when taking, then they're logged with outcome

**Tasks:**
- [ ] Build revenue tracking
- [ ] Create aging report
- [ ] Implement collection workflow
- [ ] Add action logging

---

#### Story 10.6: Churn Prediction (SuperMaster)

**As a** COO
**I want** at-risk operators identified early
**So that** I can prevent churn

**Acceptance Criteria:**
1. Given operator activity drops, when analyzed, then churn risk score generates
2. Given risk score >70%, when detected, then SM receives intervention alert
3. Given intervention happens, when tracking, then outcome is recorded

**Tasks:**
- [ ] Build churn prediction model
- [ ] Create risk scoring
- [ ] Implement alert system
- [ ] Add outcome tracking

---

### Epic 11: Security & Compliance

**Goal:** Ensure the system meets security and compliance requirements.

**Phase:** 2-3
**Priority:** P0
**Estimated Stories:** 4

#### Story 11.1: Role-Based Access Control

**As a** System Admin
**I want** role-based permissions enforced
**So that** users only access appropriate data

**Acceptance Criteria:**
1. Given AA role, when accessing, then only assigned properties/agents are visible
2. Given AM role, when accessing, then only team data is visible
3. Given Principal role, when accessing, then company-wide data is visible
4. Given unauthorized access attempt, when detected, then it's logged and blocked

**Tasks:**
- [ ] Implement RBAC framework
- [ ] Create permission definitions
- [ ] Build access enforcement
- [ ] Add audit logging

---

#### Story 11.2: Call Recording Consent

**As an** AA
**I want** proper consent captured before recording
**So that** we comply with state laws

**Acceptance Criteria:**
1. Given call initiated in two-party consent state, when starting, then consent prompt plays
2. Given consent is given, when recording, then it proceeds
3. Given consent is refused, when detected, then recording is disabled for that call
4. Given call recording exists, when accessing, then consent status is displayed

**Tasks:**
- [ ] Build state-specific consent rules
- [ ] Create consent capture flow
- [ ] Implement consent tracking
- [ ] Add compliance reporting

---

#### Story 11.3: Data Encryption

**As a** System
**I want** all data encrypted
**So that** sensitive information is protected

**Acceptance Criteria:**
1. Given data at rest, when stored, then AES-256 encryption is applied
2. Given data in transit, when transmitted, then TLS 1.3 is used
3. Given encryption keys, when managed, then rotation occurs every 90 days

**Tasks:**
- [ ] Implement at-rest encryption
- [ ] Configure TLS
- [ ] Build key rotation system
- [ ] Add encryption monitoring

---

#### Story 11.4: Audit Trail

**As a** System Admin
**I want** complete audit trail of actions
**So that** we can investigate issues

**Acceptance Criteria:**
1. Given any user action, when performed, then it's logged with user/timestamp
2. Given bot decision made, when executing, then reasoning is logged
3. Given audit query, when searching, then results return within 5 seconds

**Tasks:**
- [ ] Build audit logging framework
- [ ] Create log storage
- [ ] Implement search interface
- [ ] Add retention policies

---

### Epic 12: Data Integrations

**Goal:** Establish robust integrations with external data sources.

**Phase:** 1-2
**Priority:** P0
**Estimated Stories:** 5

#### Story 12.1: MLS Real-Time Sync ✅

**As a** System
**I want** MLS data synced in real-time
**So that** property information is current

**Acceptance Criteria:**
1. Given MLS listing updates, when webhook fires, then FlipIQ data updates in <30 seconds ✅
2. Given new listing appears, when syncing, then property card is created ✅
3. Given listing status changes, when syncing, then status updates immediately ✅

**Status:** COMPLETE

---

#### Story 12.2: PropertyRadar Integration

**As a** System
**I want** PropertyRadar data available on demand
**So that** distress signals are current

**Acceptance Criteria:**
1. Given property is viewed, when data requested, then PropertyRadar response returns in <5 seconds
2. Given API rate limit approaching, when detected, then requests are queued
3. Given data returned, when caching, then it's stored for 24 hours

**Tasks:**
- [ ] Complete PropertyRadar API integration
- [ ] Implement rate limit management
- [ ] Build caching layer
- [ ] Add error handling

---

#### Story 12.3: Agent365 Batch Sync

**As a** System
**I want** Agent365 data synced every 6 hours
**So that** agent information stays current

**Acceptance Criteria:**
1. Given 6 hours elapse, when sync runs, then all agent data is updated
2. Given sync completes, when logging, then record count and duration are recorded
3. Given sync fails, when detecting, then alert is generated and retry is scheduled

**Tasks:**
- [ ] Build batch sync job
- [ ] Implement differential sync
- [ ] Add monitoring and alerting
- [ ] Create retry logic

---

#### Story 12.4: DispoPro Integration

**As a** System
**I want** DispoPro connected for wholesale disposition
**So that** buyer matching is automated

**Acceptance Criteria:**
1. Given wholesale property qualified, when submitted, then DispoPro receives data
2. Given DispoPro returns matches, when displaying, then buyer list appears
3. Given buyer selected, when routing, then disposition process continues

**Tasks:**
- [ ] Build DispoPro API integration
- [ ] Create data transformation layer
- [ ] Implement buyer match display
- [ ] Add disposition workflow

---

#### Story 12.5: Command Platform Read-Only Access

**As a** System
**I want** read-only access to Command platform
**So that** existing data is leveraged without modification

**Acceptance Criteria:**
1. Given property query, when executing, then Command data returns
2. Given write attempt, when blocked, then error is logged
3. Given connection fails, when detecting, then cache is used with warning

**Tasks:**
- [ ] Establish read-only connection
- [ ] Implement write blocking
- [ ] Build failover to cache
- [ ] Add monitoring

---

## Summary

| Epic | Stories | Phase | Priority | Status |
|------|---------|-------|----------|--------|
| Epic 1: Universal Interface | 5 | 2 | P0 | Ready |
| Epic 2: AA Daily Process | 8 | 1 | P0 | Complete ✅ |
| Epic 3: Deal Analysis Core | 7 | 1-2 | P0 | 90% Complete |
| Epic 4: Extended Deal Analysis | 8 | 2 | P1-P2 | Ready |
| Epic 5: Management Bots | 6 | 2 | P0-P1 | Ready |
| Epic 6: Marketing Automation | 8 | 2 | P1-P2 | Ready |
| Epic 7: Comp Analysis | 6 | 2-3 | P0-P2 | Ready |
| Epic 8: Investment Analysis | 5 | 2-3 | P0 | Ready |
| Epic 9: Communication | 3 | 2 | P1 | Ready |
| Epic 10: Enterprise Scale | 6 | 3 | P0 | Ready |
| Epic 11: Security & Compliance | 4 | 2-3 | P0 | Ready |
| Epic 12: Data Integrations | 5 | 1-2 | P0 | 80% Complete |

**Total Stories:** 71
**Complete:** ~15 stories
**Ready for Phase 2:** ~45 stories
**Ready for Phase 3:** ~11 stories

---

*Document generated using BMAD Method v6*
