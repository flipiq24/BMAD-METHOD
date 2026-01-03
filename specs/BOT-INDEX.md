# FlipIQ Bot Ecosystem v3.0

> **Source of Truth** — January 2026
> >
> > > ⚠️ **IMPORTANT: 14 Bots Total (NOT 32)**
> > > >
> > > > > The previous 32-bot structure (AA1-AA4, DMaster, D1-D8, etc.) is **DEPRECATED**.
> > > > >
> > > > > ---
> > > > >
> > > > > ## Current Architecture
> > > > >
> > > > > | Phase | Bot Count | Status | Target |
> > > > > |-------|-----------|--------|--------|
> > > > > | Phase 1 | 2 Workflows | ✅ COMPLETE - Production | — |
> > > > > | Phase 2 (v2) | 8 Bots | 🔧 BUILD - Current Focus | Now |
> > > > > | Phase 3 (v3) | 4 Master Bots | 📋 FUTURE | End Feb 2026 |
> > > > > | **TOTAL** | **14** | | |
> > > > >
> > > > > ---
> > > > >
> > > > > ## Phase 1: COMPLETE ✅ (2 Workflows)
> > > > >
> > > > > | Bot_ID | Name | Primary Function | Status |
> > > > > |--------|------|------------------|--------|
> > > > > | iQ-1 | Morning Check-In | Daily kickoff, availability, blockers → routes to AM | ✅ Production |
> > > > > | iQ-2 | Deal Outreach/PIQ/Agent | Property intel, agent scripts, daily outreach, 30 contacts | ✅ Production |
> > > > >
> > > > > **Spec Location:** `specs/phase-1-complete/`
> > > > >
> > > > > ---
> > > > >
> > > > > ## Phase 2: BUILD 🔧 (8 Bots) - Current Focus
> > > > >
> > > > > ### Build Priority Order
> > > > > 1. C2 (Matrix) — P0 Critical Path
> > > > > 2. 2. D1 (Investment Analysis) — P0 Critical Path
> > > > >    3. 3. C3 (List) — P1 Core Feature
> > > > >       4. 4. C1 (Map) — P1
> > > > >          5. 5. MGT1 (My Stats) — P1
> > > > >             6. 6. MGT2 (AM Deal Support) — P1
> > > > >                7. 7. D2 (Post Call) — P2
> > > > >                   8. 8. D3 (Notes) — P2
> > > > >                     
> > > > >                      9. | Bot_ID | Name | Category | Priority | Primary User | Status |
> > > > >                      10. |--------|------|----------|----------|--------------|--------|
> > > > >                      11. | C1 | Comps > Map | Comps/Analysis | P1 | AA | 📋 SPEC NEEDED |
> > > > >                      12. | C2 | Comps > Matrix | Comps/Analysis | P0 | AA | ✅ PRD COMPLETE |
> > > > >                      13. | C3 | Comps > List | Comps/Analysis | P1 | AA | ✅ PRD COMPLETE |
> > > > >                      14. | D1 | Investment Analysis | Deal Approach | P0 | AA | 🔧 IN PROGRESS |
> > > > >                      15. | D2 | Post Call and Practice | Deal Approach | P2 | AA | 📋 CONCEPT |
> > > > >                      16. | D3 | Notes and Communication | Deal Approach | P2 | AA | 📋 CONCEPT |
> > > > >                      17. | MGT1 | My Stats | Management | P1 | AM | 📋 SPEC NEEDED |
> > > > >                      18. | MGT2 | AM Deal Support | Management | P1 | AM | 📋 SPEC NEEDED |
> > > > >                     
> > > > >                      19. **Spec Location:** `specs/phase-2-build/`
> > > > >                     
> > > > >                      20. ---
> > > > >
> > > > > ## Phase 3: FUTURE 📋 (4 Master Bots) - End Feb 2026
> > > > >
> > > > > | Bot_ID | Name | Category | Primary User | Target |
> > > > > |--------|------|----------|--------------|--------|
> > > > > | AA0 | iQ Universal Interface | Universal Interface | All Users | Feb 2026 |
> > > > > | MMaster | Marketing Master | Marketing | Principal/COO | Feb 2026 |
> > > > > | MGTMaster | Management Master | Management | AM/Principal | Feb 2026 |
> > > > > | FUM | FlipIQ User Master | Central Layer | System | Feb 2026 |
> > > > >
> > > > > **Spec Location:** `specs/phase-3-future/`
> > > > >
> > > > > ---
> > > > >
> > > > > ## FUM Integration Requirements
> > > > >
> > > > > Every bot MUST integrate with FUM (FlipIQ User Master):
> > > > >
> > > > > | Operation | Description |
> > > > > |-----------|-------------|
> > > > > | READS | Fields the bot reads from FUM |
> > > > > | WRITES | Fields the bot writes to FUM |
> > > > > | RECEIVES | Events/data routed from other bots via FUM |
> > > > >
> > > > > ---
> > > > >
> > > > > ## Known Blockers
> > > > >
> > > > > 1. **PropertyRadar API connection** (Owner: Haris) - Critical path
> > > > > 2. 2. **Propensity Score showing N/A** - Needs Tax data feed
> > > > >    3. 3. **Agent Data fields not in PIQ database**
> > > > >       4. 4. **FUM service architecture decision** - Microservice vs embedded
> > > > >         
> > > > >          5. ---
> > > > >         
> > > > >          6. *Last Updated: January 2, 2026*
> > > > >          7. *Maintained by: BMAD Validation Agent*
