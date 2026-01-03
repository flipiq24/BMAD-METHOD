# [Bot_ID] - [Bot Name]

> **Version:** 1.0
> > **Status:** [CONCEPT | SPEC NEEDED | IN PROGRESS | PRD COMPLETE | PRODUCTION]
> > > **Last Updated:** [Date]
> > >
> > > ---
> > >
> > > ## Identity
> > >
> > > | Field | Value |
> > > |-------|-------|
> > > | Bot_ID | [iQ-1 / C1 / D1 / MGT1 / AA0 / etc.] |
> > > | Bot_Category | [Daily Ops / Comps / Deal / Management / Universal / Marketing] |
> > > | Bot_Priority | [P0 / P1 / P2] |
> > > | Phase | [Phase 1 ✅ / Phase 2 🔧 / Phase 3 📋] |
> > > | Primary_User | [AA / AM / Principal / COO / All Users] |
> > > | Status | [COMPLETE / SPEC NEEDED / PRD COMPLETE / CONCEPT ONLY] |
> > >
> > > ---
> > >
> > > ## UI Location
> > >
> > > | Field | Value |
> > > |-------|-------|
> > > | Primary_UI_Location | [Dashboard / PIQ / etc.] |
> > > | Specific_Path | [e.g., PIQ → Comps → Map View] |
> > > | Trigger_Method | [Auto / Click / Voice / Alert-based] |
> > > | Trigger_Condition | [What activates this bot] |
> > >
> > > ---
> > >
> > > ## FUM Integration (REQUIRED FOR ALL BOTS)
> > >
> > > | Operation | Fields |
> > > |-----------|--------|
> > > | READS | [FUM fields this bot reads from] |
> > > | WRITES | [FUM fields this bot writes to] |
> > > | RECEIVES | [Events/data from other bots via FUM] |
> > >
> > > ---
> > >
> > > ## Success Metrics
> > >
> > > | Metric | Target | Measurement |
> > > |--------|--------|-------------|
> > > | [metric] | [target] | [how measured] |
> > > | Time Saved | [X min → Y sec] | Before vs After |
> > >
> > > ---
> > >
> > > ## Data Inputs
> > >
> > > ### User Provided
> > > | Field | Type | Required | Example |
> > > |-------|------|----------|---------|
> > > | [field] | [type] | [Y/N] | [example] |
> > >
> > > ### System Retrieved
> > > | Field | Source | Table/Endpoint | Example |
> > > |-------|--------|----------------|---------|
> > > | [field] | [MLS / PropertyRadar / Agent365 / PIQ] | [specific field] | [example] |
> > >
> > > ---
> > >
> > > ## Processing Logic
> > >
> > > **TRIGGER:** [What activates this bot]
> > > **REQUIRES:** [FUM prerequisites]
> > >
> > > **STEP 1:** [Action]
> > > - Input: [data from FUM or user]
> > > - - Process: [logic]
> > >   - - Output: [result]
> > >    
> > >     - **STEP 2:** [Action]
> > >     - - IF [condition]: THEN [action]
> > >       - - ELSE: [alternative]
> > >        
> > >         - **WRITES TO FUM:** [What gets written back]
> > >         - **OUTPUT:** [Final result → destination]
> > >        
> > >         - ---
> > >
> > > ## Outputs
> > >
> > > | Field | Type | Destination | Example |
> > > |-------|------|-------------|---------|
> > > | [field] | [type] | [UI location] | [example] |
> > >
> > > ---
> > >
> > > ## Error Handling
> > >
> > > | Error | Response | User Message |
> > > |-------|----------|--------------|
> > > | [error] | [behavior] | "[message]" |
> > >
> > > ---
> > >
> > > ## Non-Goals (Hard Guardrails)
> > >
> > > The system MUST NOT:
> > > | Forbidden | Why |
> > > |-----------|-----|
> > > | [action] | [reason] |
> > >
> > > ---
> > >
> > > ## Related Epics/Stories
> > >
> > > - [epic-id/story-id]: [title]
> > >
> > > - ---
> > >
> > > ## Changelog
> > >
> > > | Version | Date | Changes |
> > > |---------|------|---------|
> > > | 1.0 | [Date] | Initial spec |
