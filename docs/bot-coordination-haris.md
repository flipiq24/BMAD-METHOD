# Bot Coordination Guide for Haris

## Overview

This document outlines the coordination workflow between you (Haris), Nate (CTO), and Tony for implementing the FlipIQ bot system. The goal is to create intelligent overlay assistants that help users understand and manually update their data.

---

## Team Responsibilities

### Haris (Developer) - Bot Connection & Implementation
- Connect and integrate the bots into the existing page infrastructure
- Implement the IQ button trigger mechanism for each page
- Build the overlay UI components that appear below existing page content
- Ensure overlays are **read-only** (no direct data modifications)

### Nate (CTO) - Logic Review & Architecture
- Review the implementation logic in the [FlipIQ Design Document](https://docs.google.com/document/d/1CqGHHt7mcNGRabSfeALsROebDwlts_zcFtYzISDERH0/edit?pli=1&tab=t.nalnm2ue4fkk#heading=h.ue8r5g3218vk)
- Validate the architecture for each tab integration
- Approve data flow patterns between bots and UI

### Tony - Data Definition & AI Training
- Define the specific **data points** each bot requires
- Create **final training specifications** for the OpenAI assistants
- Provide **sample outputs** for each bot's expected responses
- Document the assistant behavior and response formatting

---

## Bot Architecture

### 1. Comp Bots (Comparables Analysis)
Purpose: Analyze comparable properties and provide intelligent insights

**Target Pages/Tabs:**
| Tab | Overlay Behavior |
|-----|------------------|
| **Comps Map** | Geographic analysis overlay below the map view |
| **Comps Matrix** | Side-by-side comparison insights below the matrix |
| **Comps List** | List analysis and recommendations below the listing |
| **My Stats** | Personal performance metrics and trends |

### 2. Post Call Bots
Purpose: Process and analyze information gathered during calls

**Integration Note:** These bots should be designed for potential combination with Notes Bots to streamline the post-call workflow.

### 3. Notes Bots
Purpose: Intelligent note-taking assistance and organization

**Combination Opportunity:** Consider merging with Post Call Bots for a unified post-interaction experience.

---

## Implementation Pattern

### IQ Button Overlay System

```
+----------------------------------+
|        EXISTING PAGE VIEW        |
|   (Maps / Matrix / List / Stats) |
+----------------------------------+
|         [ IQ Button ]            |
+----------------------------------+
           |
           v (on click)
+----------------------------------+
|     TRAINING OVERLAY PANEL       |
|  - Read-only explanations        |
|  - Guided insights               |
|  - Manual update prompts         |
|  - "Go to [field] to update"     |
+----------------------------------+
```

### Key Principles

1. **Read-Only Overlays**: The overlays do NOT modify any data directly
2. **Training Focus**: Content explains what the data means and how to improve it
3. **Manual Direction**: Outputs guide users to manually update specific fields
4. **Non-Intrusive**: Overlays appear below existing content, not blocking it

---

## Coordination Action Items

### Immediate Next Steps

#### For Haris:
- [ ] Review this document and the Google Doc with Nate
- [ ] Understand the current page structure for each tab
- [ ] Plan the IQ button placement for each page
- [ ] Design the overlay component structure

#### For Nate to Review (Google Doc Tabs):
- [ ] Comps Map - geographic overlay logic
- [ ] Comps Matrix - comparison analysis logic
- [ ] Comps List - list processing logic
- [ ] My Stats - personal metrics logic

#### Waiting on Tony:
- [ ] Data point definitions per bot
- [ ] OpenAI assistant training specifications
- [ ] Sample output formats for each bot response

---

## OpenAI Assistant Structure (Pending Tony's Input)

Each bot will be configured as an OpenAI Assistant with:

```yaml
assistant:
  name: "[Bot Name]"
  model: "gpt-4" # or specified model
  instructions: "[Tony to define]"

expected_inputs:
  - "[Tony to define data points]"

sample_output:
  format: "[Tony to provide]"
  example: "[Tony to provide]"
```

---

## Communication Flow

```
Tony (Data/Training)
      |
      v
   Defines data points & training specs
      |
      v
Nate (Architecture Review) <-----> Haris (Implementation)
      |                                    |
      v                                    v
   Approves logic                    Builds overlays
      |                                    |
      +-----------> Integration <----------+
```

---

## Questions for Nate During Review

When reviewing the Google Doc logic, clarify:

1. What data sources feed into each tab?
2. What are the key metrics the Comp Bots should analyze?
3. How should Post Call and Notes Bots share context?
4. What's the expected response time for bot analysis?
5. Are there any existing API endpoints to leverage?

---

## Document References

- **Primary Design Doc**: [FlipIQ Bot Logic](https://docs.google.com/document/d/1CqGHHt7mcNGRabSfeALsROebDwlts_zcFtYzISDERH0/edit?pli=1&tab=t.nalnm2ue4fkk#heading=h.ue8r5g3218vk)
- **Tabs to Review**: Comps Map, Comps Matrix, Comps List, My Stats

---

## Summary

The goal is simple: **Help users understand their data through intelligent overlays that train them on best practices and direct them to make manual updates.**

Start by coordinating with Nate on the Google Doc logic review, then wait for Tony's data point definitions before finalizing the OpenAI assistant configurations.

---

*Last Updated: 2025-12-19*
