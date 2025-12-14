---
stepsCompleted: ['context', 'constraints', 'patterns', 'decisions', 'components', 'integrations', 'data-model', 'validation']
inputDocuments: ['PRD.md', 'FlipIQ Master Technical Specification']
workflowType: 'architecture'
track: 'BMad Method'
---

# Architecture Decision Document - FlipIQ AI Overlay System

**Author:** FlipIQ Architecture Team
**Date:** December 2024
**Version:** 2.0
**Status:** Approved

---

## 1. Executive Summary

FlipIQ is an AI-powered overlay system that enhances the existing Command platform for real estate acquisition teams. The architecture follows an **overlay pattern** - deploying 32 specialized AI bots without modifying the underlying Command database, while integrating multiple external data sources (MLS, PropertyRadar, Agent365, DispoPro).

### 1.1 Key Architectural Decisions

1. **Overlay Pattern**: Zero modifications to Command platform database
2. **Bot Orchestration**: Master bots coordinate specialized sub-bots
3. **Event-Driven**: Real-time data flow through event streaming
4. **API Gateway**: Centralized integration management
5. **Multi-Tenant**: Supports 375+ operators with isolated data

---

## 2. System Context

### 2.1 System Context Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              EXTERNAL SYSTEMS                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌──────────┐   ┌───────────────┐   ┌──────────┐   ┌─────────────┐          │
│  │   MLS    │   │ PropertyRadar │   │ Agent365 │   │  DispoPro   │          │
│  │   API    │   │     API       │   │   API    │   │    API      │          │
│  └────┬─────┘   └───────┬───────┘   └────┬─────┘   └──────┬──────┘          │
│       │                 │                │                 │                 │
│       └────────────────┼────────────────┼─────────────────┘                 │
│                        │                │                                    │
│                        ▼                ▼                                    │
│              ┌─────────────────────────────────────┐                        │
│              │       FlipIQ API Gateway           │                        │
│              └─────────────────┬───────────────────┘                        │
│                                │                                            │
│                                ▼                                            │
│  ┌──────────────────────────────────────────────────────────────────────┐   │
│  │                      FlipIQ OVERLAY SYSTEM                           │   │
│  │  ┌────────────────────────────────────────────────────────────────┐  │   │
│  │  │                    BOT ORCHESTRATION LAYER                     │  │   │
│  │  │  ┌─────────┐ ┌─────────┐ ┌──────────┐ ┌──────────┐ ┌────────┐  │  │   │
│  │  │  │ DMaster │ │MGTMaster│ │ MMaster  │ │ CMaster  │ │IAMaster│  │  │   │
│  │  │  └────┬────┘ └────┬────┘ └────┬─────┘ └────┬─────┘ └───┬────┘  │  │   │
│  │  │       │           │           │            │           │        │  │   │
│  │  │       ▼           ▼           ▼            ▼           ▼        │  │   │
│  │  │  ┌────────────────────────────────────────────────────────────┐ │  │   │
│  │  │  │              SPECIALIZED BOT LAYER (27 Bots)               │ │  │   │
│  │  │  │ AA1-AA4 │ D1-D8 │ PIQ │ MGT2-3 │ M1-M5 │ C1-C4 │ Com1    │ │  │   │
│  │  │  └────────────────────────────────────────────────────────────┘ │  │   │
│  │  └────────────────────────────────────────────────────────────────┘  │   │
│  │                                                                      │   │
│  │  ┌────────────────────────────────────────────────────────────────┐  │   │
│  │  │                    DATA & CACHING LAYER                        │  │   │
│  │  │  ┌────────────┐  ┌────────────┐  ┌────────────────────────┐   │  │   │
│  │  │  │  Redis     │  │   DFI      │  │  Session State         │   │  │   │
│  │  │  │  Cache     │  │  Scores    │  │  Management            │   │  │   │
│  │  │  └────────────┘  └────────────┘  └────────────────────────┘   │  │   │
│  │  └────────────────────────────────────────────────────────────────┘  │   │
│  └──────────────────────────────────────────────────────────────────────┘   │
│                                │                                            │
│                                ▼                                            │
│              ┌─────────────────────────────────────┐                        │
│              │      COMMAND PLATFORM (READ ONLY)   │                        │
│              │      ┌─────────────────────────┐    │                        │
│              │      │     Existing Database   │    │                        │
│              │      │     (NO MODIFICATIONS)  │    │                        │
│              │      └─────────────────────────┘    │                        │
│              └─────────────────────────────────────┘                        │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘

                                     │
                                     ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                               USER INTERFACES                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌──────────────┐  ┌───────────────┐  ┌────────────────┐  ┌──────────────┐  │
│  │   iQ Overlay │  │   Dashboard   │  │  Deal Review   │  │   Pipeline   │  │
│  │   (Chat)     │  │   (Metrics)   │  │   (Workflow)   │  │   (Deals)    │  │
│  └──────────────┘  └───────────────┘  └────────────────┘  └──────────────┘  │
│                                                                              │
│  ┌──────────────┐  ┌───────────────┐  ┌────────────────┐  ┌──────────────┐  │
│  │    PIQ       │  │    Agents     │  │  Daily         │  │ Management   │  │
│  │  (Intel)     │  │    (CRM)      │  │  Outreach      │  │  Console     │  │
│  └──────────────┘  └───────────────┘  └────────────────┘  └──────────────┘  │
│                                                                              │
│  USERS: AA (1,500) │ AM (100) │ Principal (375) │ COO (1)                   │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 2.2 AI Overlay Positions by Module

| Module | AI Overlay Position |
|--------|---------------------|
| Comp Map | Below the map |
| Comp Matrix | Above the table |
| Comp List | Sidebar on right |
| Investment Analysis | Below main content |
| Agent Review | Overlay |
| PIQ | Same as Daily Outreach 30 agent call section |

### 2.3 User Roles & Access

| Role | Count | Access Level | Primary Interface |
|------|-------|--------------|-------------------|
| AA (Acquisition Associate) | 1,500 | Property, Agent, Own Metrics | iQ Overlay, Deal Review, PIQ |
| AM (Acquisition Manager) | ~100 | Team Properties, Team Metrics | Management Console |
| Principal/Broker | 375 | Company-wide, Operator Level | Dashboard, EOD Reports |
| COO | 1 | Enterprise-wide, All Operators | National Dashboard |

---

## 3. Technical Constraints

### 3.1 Non-Negotiable Constraints

| Constraint | Rationale |
|------------|-----------|
| **Zero Command DB Modifications** | Business requirement - existing platform must remain unchanged |
| **Real-time Data (<5 sec)** | AAs need current data for agent conversations |
| **Mobile Responsive** | AAs work from field, vehicles |
| **API Rate Limits** | PropertyRadar: 10k/day, MLS: 50k/day |

### 3.2 Technology Stack

| Layer | Technology | Rationale |
|-------|------------|-----------|
| Frontend | React + TypeScript | Command platform standard |
| Overlay UI | Web Components | Portable, framework-agnostic |
| Backend | Node.js + Express | Fast development, async I/O |
| Bot Engine | Python + LangChain | AI/ML ecosystem |
| **LLM** | **OpenAI GPT-4** | **Best quality over cost** |
| **Voice** | **OpenAI Realtime API** | **Real-time voice input** |
| **Transcription** | **TBD (Whisper likely)** | **No call recording - transcription only** |
| Cache | Redis | Sub-ms latency |
| Queue | RabbitMQ | Reliable message delivery |
| Database | PostgreSQL | FlipIQ-specific data only |
| Search | Elasticsearch | Property/agent search |

### 3.3 Platform Constraints

| Constraint | Decision |
|------------|----------|
| Mobile Support | No - Desktop optimized |
| Offline Mode | No - Internet required |
| Keyboard Shortcuts | No - Not needed |
| A/B Testing | No - Not planned |

---

## 4. Component Architecture

### 4.1 Bot Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          BOT ORCHESTRATION ENGINE                           │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  MASTER BOTS (5)                           COORDINATION PATTERNS            │
│  ┌─────────────────┐                      ┌─────────────────────────┐       │
│  │    DMaster      │──────────────────────│  Sequential Pipeline    │       │
│  │  (Deal Support) │                      │  PIQ → D1 → D2 → D3     │       │
│  └─────────────────┘                      └─────────────────────────┘       │
│                                                                              │
│  ┌─────────────────┐                      ┌─────────────────────────┐       │
│  │   MGTMaster     │──────────────────────│  Parallel Aggregation   │       │
│  │  (Management)   │                      │  AA1-4 → Summary        │       │
│  └─────────────────┘                      └─────────────────────────┘       │
│                                                                              │
│  ┌─────────────────┐                      ┌─────────────────────────┐       │
│  │    MMaster      │──────────────────────│  Scheduled Batch        │       │
│  │  (Marketing)    │                      │  M1-M5 (daily/weekly)   │       │
│  └─────────────────┘                      └─────────────────────────┘       │
│                                                                              │
│  ┌─────────────────┐                      ┌─────────────────────────┐       │
│  │    CMaster      │──────────────────────│  Hierarchical Flow      │       │
│  │   (Comps)       │                      │  C1 → C2 → C3 → C4      │       │
│  └─────────────────┘                      └─────────────────────────┘       │
│                                                                              │
│  ┌─────────────────┐                      ┌─────────────────────────┐       │
│  │   IAMaster      │──────────────────────│  Decision Tree          │       │
│  │  (Investment)   │                      │  Buy Box → Rehab → Go   │       │
│  └─────────────────┘                      └─────────────────────────┘       │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Bot Categories & Responsibilities

#### Daily Operations Bots (AA1-AA4)

```
┌─────────────────────────────────────────────────────────────────┐
│                    AA DAILY WORKFLOW                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  LOGIN                                                          │
│    │                                                            │
│    ▼                                                            │
│  ┌──────────┐                                                   │
│  │   AA1    │  Morning Check-In                                 │
│  │ (Check-In)│  • Availability confirmation                     │
│  └────┬─────┘  • Blocker identification                        │
│       │        • AM notification routing                        │
│       ▼                                                         │
│  ┌──────────┐                                                   │
│  │   AA2    │  Deal Review                                      │
│  │ (Review) │  • Priority sorting (Critical→Hot→Warm→Cold)     │
│  └────┬─────┘  • Navigation to Deal Review page                │
│       │        • 5-tab workflow guidance                        │
│       ▼                                                         │
│  ┌──────────┐                                                   │
│  │   AA3    │  Daily Outreach                                   │
│  │(Outreach)│  • 30 high-propensity properties                 │
│  └────┬─────┘  • Script generation                             │
│       │        • Conversation tracking                          │
│       ▼                                                         │
│  ┌──────────┐                                                   │
│  │   AA4    │  Agent Relationships                              │
│  │ (Agents) │  • 100 elite agent tracking                      │
│  └──────────┘  • Tier management                               │
│                • Follow-up scheduling                           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

#### Deal Analysis Bots (DMaster + D1-D8)

```
┌─────────────────────────────────────────────────────────────────┐
│                    DEAL INTELLIGENCE PIPELINE                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Property Selected                                              │
│         │                                                        │
│         ▼                                                        │
│  ┌─────────────┐                                                │
│  │   DMaster   │  Orchestrates all deal intelligence            │
│  └──────┬──────┘                                                │
│         │                                                        │
│         ├──────────────────────────────────────┐                │
│         │                                      │                │
│         ▼                                      ▼                │
│  ┌─────────────┐                      ┌─────────────┐           │
│  │     PIQ     │                      │     D1      │           │
│  │ (Property   │                      │ (MLS Deal   │           │
│  │  Intel)     │                      │  Review)    │           │
│  └──────┬──────┘                      └──────┬──────┘           │
│         │                                    │                   │
│         │  ┌──────────────────────────────────┘                 │
│         │  │                                                     │
│         ▼  ▼                                                     │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐          │
│  │     D2      │    │     D3      │    │   D4-D8     │          │
│  │ (Agent      │    │ (Propensity │    │ (Notes,     │          │
│  │  Review)    │    │  Analysis)  │    │  Reminders, │          │
│  └─────────────┘    └─────────────┘    │  Activity)  │          │
│                                        └─────────────┘          │
│         │                  │                  │                  │
│         └──────────────────┼──────────────────┘                 │
│                            ▼                                     │
│                   ┌─────────────────┐                           │
│                   │   DFI Score     │                           │
│                   │ + Chase/Don't   │                           │
│                   │   Chase         │                           │
│                   └─────────────────┘                           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### 4.3 Data Flow Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           DATA FLOW ARCHITECTURE                            │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  EXTERNAL SOURCES                 TRANSFORMATION                  STORAGE   │
│                                                                              │
│  ┌──────────┐                                                               │
│  │   MLS    │────▶ Property Sync ────▶ ┌──────────────┐                    │
│  │   API    │      (Real-time)         │              │                    │
│  └──────────┘                          │              │                    │
│                                        │   Property   │                    │
│  ┌──────────┐                          │   Cache      │                    │
│  │Property  │────▶ Distress Enrich ──▶│  (Redis)     │                    │
│  │ Radar    │      (Real-time)         │              │                    │
│  └──────────┘                          │              │                    │
│                                        └──────┬───────┘                    │
│  ┌──────────┐                                 │                             │
│  │ Agent365 │────▶ Agent Sync ───────▶ ┌─────┴────────┐                    │
│  │          │      (6hr batch)         │              │                    │
│  └──────────┘                          │   Agent      │                    │
│                                        │   Store      │                    │
│  ┌──────────┐                          │  (Postgres)  │                    │
│  │DispoPro  │────▶ Buyer Sync ──────▶ │              │                    │
│  │          │      (Real-time)         └──────┬───────┘                    │
│  └──────────┘                                 │                             │
│                                               │                             │
│                                               ▼                             │
│                                        ┌─────────────┐                     │
│                                        │   DFI       │                     │
│                                        │ Calculator  │                     │
│                                        └──────┬──────┘                     │
│                                               │                             │
│                                               ▼                             │
│                                        ┌─────────────┐                     │
│                                        │  Bot Engine │                     │
│                                        │  (Context)  │                     │
│                                        └─────────────┘                     │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 5. Integration Architecture

### 5.1 API Gateway Pattern

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          FlipIQ API GATEWAY                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  INBOUND                      GATEWAY                       OUTBOUND        │
│                                                                              │
│  ┌──────────┐              ┌─────────────┐                                  │
│  │ iQ UI    │──────────────│             │              ┌──────────┐       │
│  └──────────┘              │   Rate      │──────────────│   MLS    │       │
│                            │   Limiter   │              │   API    │       │
│  ┌──────────┐              │             │              └──────────┘       │
│  │ Dashboard│──────────────│   ┌─────┐   │                                 │
│  └──────────┘              │   │Auth │   │              ┌──────────┐       │
│                            │   │     │   │──────────────│Property  │       │
│  ┌──────────┐              │   └─────┘   │              │ Radar    │       │
│  │ Mobile   │──────────────│             │              └──────────┘       │
│  └──────────┘              │   ┌─────┐   │                                 │
│                            │   │Cache│   │              ┌──────────┐       │
│                            │   │     │   │──────────────│ Agent365 │       │
│                            │   └─────┘   │              └──────────┘       │
│                            │             │                                  │
│                            │   ┌─────┐   │              ┌──────────┐       │
│                            │   │Queue│   │──────────────│ Command  │       │
│                            │   │     │   │              │ Platform │       │
│                            │   └─────┘   │              └──────────┘       │
│                            │             │                                  │
│                            └─────────────┘                                  │
│                                                                              │
│  RATE LIMITS:                                                               │
│  • MLS: 50,000 requests/day                                                 │
│  • PropertyRadar: 10,000 requests/day                                       │
│  • Agent365: 5,000 requests/day                                             │
│  • Internal API: 1,000 requests/minute per user                            │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 5.2 Microservices Architecture

**All external integrations are being built as internal microservices:**

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     FlipIQ MICROSERVICES LAYER                              │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌──────────────────────┐   ┌──────────────────────┐   ┌─────────────────┐  │
│  │   National Data      │   │   Agent Reports      │   │ Command Reader  │  │
│  │   Microservice       │   │   Microservice       │   │ Microservice    │  │
│  │   ──────────────     │   │   ──────────────     │   │ ──────────────  │  │
│  │   PropertyRadar      │   │   DispoPro           │   │ Command DB      │  │
│  │   wrapper            │   │   (Our Product)      │   │ (TBD by Nate)   │  │
│  │                      │   │                      │   │                 │  │
│  │   • Distress signals │   │   • Agent-investor   │   │   • Property    │  │
│  │   • Tax data         │   │     relationships    │   │     data        │  │
│  │   • Foreclosure      │   │   • Transaction      │   │   • User data   │  │
│  │   • Vacancy          │   │     history          │   │   • Settings    │  │
│  │                      │   │   • Lender/Title     │   │                 │  │
│  │   Status: Building   │   │     relationships    │   │   Status: TBD   │  │
│  │                      │   │                      │   │                 │  │
│  │                      │   │   Status: Building   │   │                 │  │
│  └──────────────────────┘   └──────────────────────┘   └─────────────────┘  │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 5.3 Integration Specifications

#### MLS Integration (Direct)

| Aspect | Specification |
|--------|---------------|
| Protocol | REST API |
| Auth | OAuth 2.0 |
| Sync | Real-time via webhooks |
| Data | caretslistingstatus, dom/cdom, ptfv, future_value, keywords, price_change |
| Rate Limit | 50k/day |
| Fallback | 15-minute cache |

#### PropertyRadar (via National Data Microservice)

| Aspect | Specification |
|--------|---------------|
| Protocol | Internal REST API |
| Auth | Service-to-service |
| Sync | Real-time lookup |
| Data | inForeclosure, NoticeOfDefault, isTaxDefaulted, AffidavitOfDeath, Bankruptcy, isSiteVacant, EstimatedEquity |
| Rate Limit | Managed by microservice |
| Fallback | 24-hour cache |

#### DispoPro Agent Reports (via Agent Reports Microservice)

| Aspect | Specification |
|--------|---------------|
| Protocol | Internal REST API |
| Auth | Service-to-service |
| Sync | On-demand + 6-hour batch refresh |
| Data | InvestorSourceCount, LastClosingDate, DoubleEndedCount, TransactionHistory, TopInvestorPartners, LenderRelationships, TitleCompanyRelationships |
| Rate Limit | Managed by microservice |
| Fallback | 7-day cache |

**Note:** DispoPro is FlipIQ's own product - provides comprehensive agent intelligence including every agent who has worked with an investor, transaction histories, and relationships with lenders/title companies.

---

## 6. Data Model

### 6.1 FlipIQ Domain Model

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       FlipIQ DATA MODEL (Overlay Store)                     │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │                          OPERATOR                                    │    │
│  │  id, name, subscription_tier, created_at                            │    │
│  │  success_manager_id, monthly_revenue_target                         │    │
│  └────────────────────────────────┬────────────────────────────────────┘    │
│                                   │ 1:N                                     │
│                                   ▼                                         │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │                            USER                                      │    │
│  │  id, operator_id, role (AA|AM|Principal), name, email               │    │
│  │  check_in_status, daily_metrics, relationship_count                  │    │
│  └────────────────────────────────┬────────────────────────────────────┘    │
│                                   │ 1:N                                     │
│                                   ▼                                         │
│  ┌──────────────────────────────────────────────────────────────────────┐   │
│  │                      PROPERTY_OVERLAY                                 │   │
│  │  id, mls_id (FK to Command), user_id                                 │   │
│  │  offer_status, dfi_score, propensity_score                           │   │
│  │  temperature (Critical|Hot|Warm|Cold|New)                            │   │
│  │  last_action_date, next_followup_date                                │   │
│  └───────────────────────┬──────────────────────────────────────────────┘   │
│                          │                                                   │
│         ┌────────────────┼────────────────────────┐                         │
│         │ 1:N            │ 1:N                    │ 1:1                      │
│         ▼                ▼                        ▼                         │
│  ┌─────────────┐  ┌─────────────────┐  ┌────────────────────┐              │
│  │    NOTE     │  │    ACTIVITY     │  │  PROPENSITY_DATA   │              │
│  │  id         │  │  id             │  │  property_id       │              │
│  │  property_id│  │  property_id    │  │  nod_date          │              │
│  │  content    │  │  type           │  │  nots_date         │              │
│  │  category   │  │  (call|email|   │  │  tax_delinquency   │              │
│  │  created_by │  │   text|meeting) │  │  affidavit_death   │              │
│  │  created_at │  │  outcome        │  │  bankruptcy        │              │
│  └─────────────┘  │  created_at     │  │  is_vacant         │              │
│                   └─────────────────┘  │  pain_score        │              │
│                                        │  updated_at        │              │
│                                        └────────────────────┘              │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐   │
│  │                         AGENT_OVERLAY                                 │   │
│  │  id, agent365_id, mls_agent_id                                       │   │
│  │  assigned_user_id, relationship_status (Priority|Hot|Warm|Cold)      │   │
│  │  investor_source_count, tier (Whale|Dolphin|Fish)                    │   │
│  │  last_communication, next_followup, basket                           │   │
│  └───────────────────────────────────────────────────────────────────────┘   │
│                                   │ 1:N                                     │
│                                   ▼                                         │
│  ┌──────────────────────────────────────────────────────────────────────┐   │
│  │                       AGENT_TRANSACTION                               │   │
│  │  id, agent_id, investor_name, transaction_date                       │   │
│  │  price, property_type, title_company, lender                         │   │
│  └──────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐   │
│  │                         CHECK_IN_LOG                                  │   │
│  │  id, user_id, check_in_time, available_today                         │   │
│  │  help_requested, blockers, routed_to_am                              │   │
│  └──────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐   │
│  │                         DAILY_METRICS                                 │   │
│  │  id, user_id, date                                                   │   │
│  │  offers_sent, conversations_completed, calls_made                    │   │
│  │  relationships_built, properties_reviewed                            │   │
│  └──────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 6.2 DFI Calculation Model

**Note:** DFI is calculated ON-DEMAND when property is viewed, NOT as a nightly batch job.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    DEAL FOCUS INDEX (DFI) CALCULATION                       │
│                           (Calculated On-Demand)                            │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  DFI = [FixerCondition] + [InventoryStage] + [SellerPainLevel] + [Agent]   │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │  FIXER CONDITION (from MLS keywords)                                │    │
│  │  ─────────────────────────────────────                              │    │
│  │  Heavy Fixer: "as-is", "TLC", "handyman", "investor"   → +3        │    │
│  │  Moderate Fixer: "needs work", "potential"             → +2        │    │
│  │  Light Fixer: "cosmetic", "updating"                   → +1        │    │
│  │  Turnkey: none of above                                → +0        │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │  INVENTORY STAGE (from MLS DOM/Status)                              │    │
│  │  ─────────────────────────────────────                              │    │
│  │  New Listing (0-7 days)                                → +1        │    │
│  │  Active (8-30 days)                                    → +2        │    │
│  │  Aging (31-70 days)                                    → +3        │    │
│  │  Aged (70+ days)                                       → +4        │    │
│  │  Pending/Backup                                        → +2        │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │  SELLER PAIN LEVEL (from PropertyRadar)                             │    │
│  │  ────────────────────────────────────────                           │    │
│  │  Notice of Trustee Sale (NOTS)                         → +8        │    │
│  │  Notice of Default (NOD)                               → +6        │    │
│  │  Tax Delinquency                                       → +5        │    │
│  │  Affidavit of Death                                    → +5        │    │
│  │  Bankruptcy / Judgment                                 → +4        │    │
│  │  High Mortgage / Debt                                  → +3        │    │
│  │  Vacant Property                                       → +3        │    │
│  │                                                                     │    │
│  │  PAIN SCORE = Sum of all applicable signals                        │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │  AGENT BEHAVIOR (from Agent365)                                     │    │
│  │  ───────────────────────────────                                    │    │
│  │  High ISC (7+ investors)                               → +3        │    │
│  │  Medium ISC (3-6 investors)                            → +2        │    │
│  │  Low ISC (1-2 investors)                               → +1        │    │
│  │  No investor history                                   → +0        │    │
│  │                                                                     │    │
│  │  Hungry Agent (>180 days since close)                  → +2        │    │
│  │  Double-end willingness                                → +1        │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  FINAL DFI RANGES:                                                          │
│  • 0-5: Low Priority (Cold)                                                 │
│  • 6-10: Medium Priority (Warm)                                             │
│  • 11-15: High Priority (Hot)                                               │
│  • 16+: Critical (Immediate Action)                                         │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 7. Security Architecture

### 7.1 Authentication & Authorization

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       SECURITY ARCHITECTURE                                  │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  AUTHENTICATION                           AUTHORIZATION                      │
│  ┌────────────────────────┐              ┌────────────────────────────────┐ │
│  │  OAuth 2.0 + OIDC      │              │  ROLE-BASED ACCESS CONTROL     │ │
│  │  ──────────────────    │              │  ──────────────────────────    │ │
│  │  • Command SSO         │              │                                │ │
│  │  • JWT tokens          │              │  COO:                          │ │
│  │  • 24hr expiry         │              │  • All operators              │ │
│  │  • Refresh tokens      │              │  • All regions                │ │
│  │                        │              │  • Financial data             │ │
│  │  MFA (Optional):       │              │                                │ │
│  │  • TOTP                │              │  Principal:                    │ │
│  │  • SMS fallback        │              │  • Own operator               │ │
│  │                        │              │  • All team AAs               │ │
│  └────────────────────────┘              │  • Company metrics            │ │
│                                          │                                │ │
│                                          │  AM:                           │ │
│                                          │  • Assigned AAs               │ │
│                                          │  • Team metrics               │ │
│                                          │                                │ │
│                                          │  AA:                           │ │
│                                          │  • Assigned properties        │ │
│                                          │  • Assigned agents            │ │
│                                          │  • Own metrics                │ │
│                                          └────────────────────────────────┘ │
│                                                                              │
│  DATA SECURITY                            AUDIT                             │
│  ┌────────────────────────┐              ┌────────────────────────────────┐ │
│  │  Encryption            │              │  Logging                       │ │
│  │  ──────────            │              │  ───────                       │ │
│  │  At Rest: AES-256      │              │  • All API calls              │ │
│  │  In Transit: TLS 1.3   │              │  • User actions               │ │
│  │                        │              │  • Bot decisions              │ │
│  │  PII Handling:         │              │  • Data access                │ │
│  │  • Phone numbers       │              │                                │ │
│  │  • Email addresses     │              │  Retention:                    │ │
│  │  • Financial data      │              │  • 7 years (transactions)     │ │
│  │                        │              │  • 90 days (logs)             │ │
│  └────────────────────────┘              └────────────────────────────────┘ │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 7.2 API Security

| Control | Implementation |
|---------|----------------|
| Rate Limiting | Per-user limits, burst protection |
| Input Validation | Schema validation, SQL injection prevention |
| CORS | Whitelist allowed origins |
| API Keys | Rotation every 90 days |
| Request Signing | HMAC for sensitive operations |

---

## 8. Scalability & Performance

### 8.1 Scale Targets

| Metric | Current | Target | Architecture Support |
|--------|---------|--------|---------------------|
| Operators | 375 | 500+ | Horizontal pod scaling |
| AAs | 1,500 | 2,000+ | Connection pooling |
| Properties/Operator | 50 | 100 | Sharded database |
| Concurrent Users | 500 | 1,000 | Load balancer |
| API Requests/Second | 100 | 500 | Cache layer |

### 8.2 Performance Targets

| Operation | Target | Implementation |
|-----------|--------|----------------|
| Page Load | <2 sec | CDN, lazy loading |
| Bot Response | <3 sec | Pre-computed scores |
| API Response | <500ms | Redis cache |
| DFI Calculation | <1 sec | Cached components |
| Search | <500ms | Elasticsearch |

### 8.3 Caching Strategy

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          CACHING ARCHITECTURE                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  L1: Browser Cache                    L2: CDN (CloudFlare)                  │
│  ┌────────────────────────┐          ┌────────────────────────┐             │
│  │  • Static assets       │          │  • Static assets       │             │
│  │  • User preferences    │          │  • API responses       │             │
│  │  • Session state       │          │  • Edge caching        │             │
│  │  TTL: Session          │          │  TTL: 5 minutes        │             │
│  └────────────────────────┘          └────────────────────────┘             │
│                                                                              │
│  L3: Redis (Application)              L4: PostgreSQL Query Cache            │
│  ┌────────────────────────┐          ┌────────────────────────┐             │
│  │  • Property data       │          │  • Complex aggregations│             │
│  │  • Agent data          │          │  • Historical reports  │             │
│  │  • DFI scores          │          │  • Materialized views  │             │
│  │  • Session data        │          │  TTL: 1 hour           │             │
│  │  TTL: 15 minutes       │          └────────────────────────┘             │
│  └────────────────────────┘                                                 │
│                                                                              │
│  CACHE INVALIDATION:                                                        │
│  • Property update → Invalidate property cache                              │
│  • Agent update → Invalidate agent cache                                    │
│  • User action → Invalidate user-specific cache                             │
│  • Daily at midnight → Refresh all DFI scores                               │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 9. Deployment Architecture

### 9.1 Environment Strategy

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         DEPLOYMENT ENVIRONMENTS                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  DEVELOPMENT                  STAGING                     PRODUCTION        │
│  ┌──────────────────┐        ┌──────────────────┐        ┌──────────────────┐│
│  │  Single instance │        │  Scaled replica  │        │  Full HA cluster ││
│  │  Mock APIs       │        │  Test APIs       │        │  Live APIs       ││
│  │  Local DB        │        │  Snapshot DB     │        │  Replicated DB   ││
│  │                  │        │                  │        │                  ││
│  │  CI: On commit   │        │  CI: On PR merge │        │  CD: Blue/Green  ││
│  └──────────────────┘        └──────────────────┘        └──────────────────┘│
│                                                                              │
│  PRODUCTION TOPOLOGY:                                                        │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │                        LOAD BALANCER (AWS ALB)                       │    │
│  └────────────────────────────────┬────────────────────────────────────┘    │
│                                   │                                         │
│         ┌─────────────────────────┼─────────────────────────┐               │
│         │                         │                         │               │
│         ▼                         ▼                         ▼               │
│  ┌─────────────┐           ┌─────────────┐           ┌─────────────┐        │
│  │   Web Pod   │           │   Web Pod   │           │   Web Pod   │        │
│  │   (x3)      │           │   Bot Pod   │           │   Worker    │        │
│  │             │           │   (x2)      │           │   (x2)      │        │
│  └─────────────┘           └─────────────┘           └─────────────┘        │
│         │                         │                         │               │
│         └─────────────────────────┼─────────────────────────┘               │
│                                   │                                         │
│                                   ▼                                         │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │                         DATA LAYER                                   │    │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐               │    │
│  │  │   Redis      │  │  PostgreSQL  │  │ Elasticsearch│               │    │
│  │  │  Cluster     │  │  (Primary +  │  │   Cluster    │               │    │
│  │  │             │  │   Replica)   │  │              │               │    │
│  │  └──────────────┘  └──────────────┘  └──────────────┘               │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 9.2 CI/CD Pipeline

| Stage | Trigger | Actions |
|-------|---------|---------|
| Build | PR Created | Lint, Unit Tests, Build |
| Test | PR Approved | Integration Tests, E2E |
| Stage | Merge to main | Deploy to Staging |
| Prod | Manual approval | Blue/Green Deploy |
| Rollback | Auto/Manual | Instant traffic switch |

---

## 10. Monitoring & Observability

### 10.1 Monitoring Stack

| Layer | Tool | Purpose |
|-------|------|---------|
| Application | DataDog APM | Request tracing, performance |
| Infrastructure | CloudWatch | AWS metrics, alerts |
| Logs | ELK Stack | Centralized logging |
| Uptime | PagerDuty | Incident management |
| User Analytics | Mixpanel | Feature usage, funnel |

### 10.2 Key Metrics & Alerts

| Metric | Threshold | Alert |
|--------|-----------|-------|
| API Error Rate | >1% | PagerDuty |
| Response Time P95 | >3s | Slack |
| Bot Failure Rate | >5% | PagerDuty |
| DB Connection Pool | >80% | Slack |
| Cache Hit Rate | <90% | Slack |
| Daily Check-in Rate | <95% | Email to AM |

---

## 11. Implementation Phases

### Phase 1 (COMPLETE)
- AA1-AA4 Daily Process Bots
- DMaster, PIQ, D1-D3 Deal Analysis
- Core data integration (MLS, PropertyRadar)

### Phase 2 (December 2024)
- AA0 Universal Interface
- D4-D8 Extended Deal Analysis
- MGT1-3 Management Bots
- M1-M5 Marketing Bots
- C1-C4 Comp Bots (except C3)
- IAMaster Investment Analysis
- Com1 Communication Bot

### Phase 3 (Q1 2025)
- C3 List Grouping Bot
- IARehab Repair Calculator
- SuperMaster Enterprise Bot
- Scale to 500 operators

---

## 12. Decision Log

| ID | Decision | Rationale | Alternatives Considered |
|----|----------|-----------|------------------------|
| ADR-001 | Overlay pattern (no Command DB changes) | Business requirement, faster deployment | Direct integration |
| ADR-002 | Master/Sub-bot orchestration | Maintainability, single responsibility | Monolithic bot |
| ADR-003 | Redis for caching | Sub-ms latency, session support | Memcached |
| ADR-004 | PostgreSQL for FlipIQ data | ACID, complex queries | MongoDB |
| ADR-005 | Event-driven bot triggers | Real-time, decoupled | Polling |
| ADR-006 | Python/LangChain for bots | AI ecosystem, rapid development | Node.js |

---

## 13. Appendix

### A. API Specifications

See separate API documentation for:
- FlipIQ Internal API
- MLS Integration API
- PropertyRadar Integration API
- Agent365 Integration API

### B. Bot Specifications

See PRD.md for complete bot specifications (FR-001 through FR-032).

### C. Database Schema

See `schema/flipiq-overlay.sql` for complete DDL.

---

*Document generated using BMAD Method v6*
