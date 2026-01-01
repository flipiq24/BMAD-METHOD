# Post-Call & Practice Bot — Training Samples
## Version 1.0 — BMAD iQ
### December 31, 2024

---

## Document Overview

| Field | Value |
|-------|-------|
| **Total Samples** | 12 |
| **Post-Call Samples** | 6 |
| **Practice Mode Samples** | 6 |
| **Difficulty Levels** | EASY, MEDIUM, HARD |
| **Call Types** | New Listing, Aged, Pending, BOM |

---

## Part 1: Post-Call Processing Samples

---

## Sample 1: Good Call — New Listing

### Transcript Input

```
[00:00] AA: Hi, this is Mike from Pacific Residential. Is this Jennifer?
[00:05] AGENT: Yes, this is Jennifer. What can I do for you?
[00:08] AA: I'm calling about your listing on 742 Evergreen Terrace. Beautiful property. I work with investors who close fast with cash. Is there flexibility on the price?
[00:18] AGENT: It just hit the market three days ago. We've had a lot of interest.
[00:24] AA: I understand. My buyers are different — no contingencies, no assignments, we close through First American in 10-14 days. Has your seller mentioned what timeline works for them?
[00:35] AGENT: They're motivated actually. Job relocation to Texas, need to be out by February 15th.
[00:42] AA: That's helpful. If I can get you proof of funds today, would you present a cash offer at 92% of ask?
[00:50] AGENT: Send it over. I'll talk to my clients tonight. Email is jennifer@remax.com.
[00:56] AA: Perfect. I'll have that to you within the hour. What's the best time to follow up tomorrow?
[01:02] AGENT: Call me after 2pm. I should have an answer by then.
[01:06] AA: Great, I'll call you at 2:15. Thanks Jennifer.
```

### Expected Note Output

```
CALL NOTES — 742 Evergreen Terrace
Duration: 1:10 | Quality: Good call ✓

Agent Jennifer receptive despite new listing (3 DOM). Seller
motivated — job relocation to Texas, needs to close by Feb 15th.
Asked to present cash offer at 92% of ask. Sending POF today.

Agent email: jennifer@remax.com

NEXT STEPS:
• Send POF within 1 hour (HIGH PRIORITY)
• Follow-up call tomorrow at 2:15pm
• Prepare formal offer if positive response

CLASSIFICATION: Update Jennifer from Cold → Warm
```

---

## Sample 2: Needs Improvement — Missed Opportunities

### Transcript Input

```
[00:00] AA: Hi, calling about the house on Oak Street. Is the seller flexible?
[00:06] AGENT: Which property? I have three listings on Oak.
[00:10] AA: Um, the one listed at 425.
[00:14] AGENT: 1842 Oak? That's been on for a while. What's your question?
[00:20] AA: Just wondering if they'd take a lower offer.
[00:24] AGENT: How much lower? Are you represented?
[00:28] AA: I work with investors. Maybe like 380?
[00:32] AGENT: That's pretty low. They've already dropped it twice.
[00:38] AA: Oh okay. Well, let me talk to my partner and get back to you.
[00:43] AGENT: Sure. Send me something in writing if you're serious.
[00:47] AA: Okay, thanks.
```

### Expected Note Output

```
CALL NOTES — 1842 Oak Street (assumed)
Duration: 0:50 | Quality: Needs improvement ⚠️

Short call. Agent has multiple Oak St listings — wasn't clear on
which property. Mentioned seller already dropped price twice (motivation signal).
No timeline discussed, no differentiation made, no POF offered.

Verbal offer of $380K mentioned but no commitment from either side.

NEXT STEPS:
• Confirm correct property address
• Prepare written offer with POF
• Call back with specific value proposition

IMPROVEMENT AREAS:
• Research property before calling (know the address!)
• Lead with value prop, not price question
• Offer POF immediately when agent shows interest
• Ask about seller timeline and motivation

CLASSIFICATION: No change (need better engagement first)
```

---

## Sample 3: Aged Listing — Price Discussion

### Transcript Input

```
[00:00] AA: Hi David, this is Sarah from FlipIQ Acquisitions. Calling about your listing on Maple Drive — the one that's been on market about 85 days.
[00:10] AGENT: Yeah, that one. What about it?
[00:14] AA: I work with investors who close fast. Given the days on market, I'm wondering what it would take to get this done. What's the real number?
[00:24] AGENT: Look, my sellers started at 575. They're at 515 now. They're not going to give it away.
[00:32] AA: I hear you. They've shown flexibility already. Here's what I can do — cash, close in 10 days, no contingencies, buyer pays all closing costs. At 480, we can make this work today.
[00:45] AGENT: 480 is tough. They're already underwater on their mortgage at 495.
[00:52] AA: Understood. Is there a short sale possibility, or are they bringing cash to close?
[00:58] AGENT: They can bring about 20K. It's painful but they need to move on.
[01:04] AA: If I can structure something at 485 that gets them out clean, would that work?
[01:10] AGENT: Get me something in writing at 485. I'll present it.
[01:15] AA: I'll have it to you today. Can you confirm the email on file is correct — david@century21.com?
[01:22] AGENT: That's right. Send it over.
```

### Expected Note Output

```
CALL NOTES — Maple Drive (85 DOM)
Duration: 1:25 | Quality: Good call ✓

Strong negotiation. Agent initially resistant but responded to
specific numbers and speed proposition. Sellers started at $575K,
now at $515K, underwater at $495K but can bring $20K to close.

Verbal acceptance of $485K written offer. Sellers motivated to
"move on" — likely distress situation (underwater mortgage).

Agent email confirmed: david@century21.com

NEXT STEPS:
• Send written offer at $485K TODAY (HIGH PRIORITY)
• Include all terms: cash, 10-day close, no contingencies, seller pays nothing
• Follow up tomorrow AM for presentation status

CLASSIFICATION: Update David from Cold → Warm
```

---

## Sample 4: Pending Property — Backup Position

### Transcript Input

```
[00:00] AA: Hi, I'm calling about 567 Pine Street that went pending last week. Is the deal still on track?
[00:08] AGENT: As far as I know. Why?
[00:12] AA: I represent cash investors. Sometimes these fall through. We'd be interested in a backup position.
[00:20] AGENT: Well, they're supposed to have their deposit wired today. Inspection is Friday.
[00:28] AA: Got it. If for any reason that deal falls apart, we can close in 10 days, all cash, no contingencies. Can I send you our proof of funds so you have it on file?
[00:40] AGENT: Sure, can't hurt to have a backup.
[00:44] AA: Great. I'll send that today. Mind if I check in with you Monday after the inspection?
[00:50] AGENT: That works. Call me after 10am.
```

### Expected Note Output

```
CALL NOTES — 567 Pine Street (PENDING)
Duration: 0:55 | Quality: Good call ✓

Current deal: Deposit due today, inspection Friday.
Positioned as backup buyer — agent agreed to receive POF.
No immediate opportunity but good positioning for fallback.

NEXT STEPS:
• Send POF today (backup position)
• Follow up Monday after 10am (post-inspection)
• Check status of current buyer's deposit

FOLLOW-UP SEQUENCE:
• Day 3 (Monday): "Did deposit wire? How was inspection?"
• Day 10: "Contingencies removed?"
• Day 20: "Still closing? We're ready if anything changes."

CLASSIFICATION: Keep as Cold (pending with another buyer)
```

---

## Sample 5: Back on Market — Deal Rescue

### Transcript Input

```
[00:00] AA: Hi, I noticed your listing on Cedar Lane came back on market yesterday. What happened with the previous deal?
[00:10] AGENT: Buyer couldn't perform. Their financing fell through at the last minute.
[00:16] AA: That's frustrating. How's your seller feeling?
[00:20] AGENT: Frustrated and nervous. They already bought their next house. Carrying two mortgages.
[00:28] AA: I can solve that problem today. We're cash buyers — no financing contingency, no appraisal delays. We closed 12 deals in this zip code last year. What does your seller need to feel good about moving forward?
[00:42] AGENT: They just want certainty at this point. The price is already aggressive.
[00:48] AA: I understand. If I bring you proof of funds and a clean offer at asking price, can we tie this up today?
[00:56] AGENT: At full ask with no contingencies? I'll call them right now.
[01:02] AA: Do that. I'll have the POF and offer to you in 30 minutes. What's the best email?
```

### Expected Note Output

```
CALL NOTES — Cedar Lane (BACK ON MARKET)
Duration: 1:05 | Quality: Excellent call ★

Previous deal fell through — buyer financing failed. Sellers
carrying two mortgages, desperate for certainty. Agent very
receptive to cash offer at full asking price.

Strong urgency signals — sellers "nervous," already bought next house.
Agent calling sellers immediately to present our offer.

NEXT STEPS:
• Send POF + offer within 30 minutes (CRITICAL)
• Offer at full asking price, no contingencies
• Be available for immediate response

HOT OPPORTUNITY — Move fast!

CLASSIFICATION: Update agent to HOT
```

---

## Sample 6: Poor Call — Agent Dismissive

### Transcript Input

```
[00:00] AA: Hi, calling about your listing on—
[00:03] AGENT: Which one? Make it quick, I'm in a meeting.
[00:06] AA: The one on Broadway. Is there—
[00:09] AGENT: That's under contract. Anything else?
[00:12] AA: Oh, okay. What about backup offers?
[00:15] AGENT: We're good. Multiple backups already. Gotta go.
[00:18] [Call ended]
```

### Expected Note Output

```
CALL NOTES — Broadway property
Duration: 0:18 | Quality: Pass — not a fit ✗

Agent dismissive and rushed. Property under contract with
multiple backup offers. No opportunity identified.
Agent indicated being in a meeting — poor timing.

NEXT STEPS:
• Remove from active call list
• Consider calling in 30 days if still pending
• Try reaching different agent at brokerage if property important

NO FOLLOW-UP RECOMMENDED at this time.

CLASSIFICATION: Keep as Cold
```

---

## Part 2: Practice Mode Samples

---

## Sample 7: EASY Difficulty — Hungry Agent

### Agent Profile

| Field | Value |
|-------|-------|
| Name | Tom Wilson |
| Deals/Year | 8 |
| Double-End % | 42% |
| Last Close | 78 days ago |
| Investor Deals | 3 this year |

### Difficulty Calculation: 28/100 (EASY)

```
Base: 50
Deals/Year (<10): -20
Double-End (>30%): -15
Days Since Close (>60): -15
Status (Active 45 DOM): -5
Total: 28 → EASY
```

### Property Context

- 892 Oak Lane, $385,000, Active 45 days
- 3BR/2BA, needs cosmetic updates
- Seller is estate (probate)

### AI Persona Behavior

- Friendly and engaging
- Happy to get investor call
- Open to discussing double-end
- Looking for a deal to break dry spell

### Sample Conversation

```
[AGENT opens]: "Hey, Tom Wilson here. What can I do for you?"

[If AA mentions property]: "Oh yeah, the Oak Lane probate. Great
property actually. What's your interest?"

[If AA asks about seller]: "Estate sale. The kids just want it
done. They're in different states, so a quick clean close would
be ideal for everyone."

[If AA mentions cash]: "Cash is definitely attractive here. These
probate deals can drag on with financing."

[If AA asks about double-end]: "I mean, I'm open to it. What kind
of commission are we talking about?"

[If AA makes offer]: "That's in the ballpark. Let me run it by the
executor. Can you send me proof of funds?"

[POSITIVE END]: "Alright, send that over. I think we can make
something work here."
```

### Scoring Expectations

- Score ≥70 if: Cash mentioned, asked about timeline, made offer
- Score ≥85 if: Also discussed double-end, mentioned speed
- HANG UP unlikely unless AA completely passive

---

## Sample 8: MEDIUM Difficulty — Standard Agent

### Agent Profile

| Field | Value |
|-------|-------|
| Name | Maria Santos |
| Deals/Year | 24 |
| Double-End % | 18% |
| Last Close | 21 days ago |
| Investor Deals | 6 this year |

### Difficulty Calculation: 52/100 (MEDIUM)

```
Base: 50
Deals/Year (10-30): 0
Double-End (10-30%): 0
Days Since Close (30-60): 0
Status (Aged 72 DOM): -10
Investor Familiarity (medium): +5
Total: 52 → MEDIUM
```

### Property Context

- 1456 Maple Street, $465,000, Active 72 days
- 4BR/2BA, dated but livable
- Price reduced twice (started at $525K)

### AI Persona Behavior

- Professional, somewhat busy
- Will engage if AA is prepared
- Skeptical of lowball offers
- Responds to market data

### Sample Conversation

```
[AGENT opens]: "Maria Santos, how can I help you?"

[If AA mentions property]: "Maple Street, yes. We've had some
interest but nothing that stuck. What's your angle?"

[If vague or unprepared]: "Look, I get a lot of these calls. Do
you have something specific in mind?"

[If AA mentions comps/data]: "Okay, you've done your homework.
What are you thinking?"

[If AA lowballs]: "That's pretty far from where we are. They've
already come down 60K."

[If AA explains value prop]: "The cash and quick close does help.
But I need to be able to present something reasonable."

[If AA asks about seller motivation]: "Between us, they're tired.
It's been a long few months. But they're not desperate."

[NEUTRAL END]: "Send me what you've got. I'll present it, but no
promises. Email is maria@compass.com."
```

### Scoring Expectations

- Score ≥60 if: Specific offer made, POF offered, followed up professionally
- Score ≥80 if: Also showed market knowledge, built rapport
- HANG UP if: No clear value prop within 90 seconds

---

## Sample 9: HARD Difficulty — Busy Top Producer

### Agent Profile

| Field | Value |
|-------|-------|
| Name | Sarah Martinez |
| Deals/Year | 62 |
| Double-End % | 4% |
| Last Close | 3 days ago |
| Investor Deals | 8 this year |

### Difficulty Calculation: 82/100 (HARD)

```
Base: 50
Deals/Year (50+): +30
Double-End (<10%): +20
Days Since Close (<30): +15
Status (Aged 85 DOM): -15
Investor Familiarity (high): -5
Total: 82 → HARD
```

### Property Context

- 456 Oak Street, $525,000, Active 85 days
- AS-IS sale, NOD filed 45 days ago
- Price reduced from $575K

### AI Persona Behavior

- Curt and time-pressed
- Will hang up if not impressed quickly
- Resists double-end conversation
- Responds only to data and certainty

### Sample Conversation

```
[AGENT opens]: "Sarah Martinez. Make it quick, I'm between showings."

[If slow start]: "I get 20 calls a day on this one. What's your offer?"

[If AA mentions investors]: "Which investors? What's their track
record in this area?"

[If AA asks about seller]: "Motivated. Obviously. It's an NOD
situation. What else do you need to know?"

[If AA asks double-end]: "I don't do that. Bring your best offer
and I'll present it. That's the deal."

[If AA lowballs]: "That's not going to work. Next."

[If AA shows value]: "Okay, you're not wasting my time. What's
your timeline to get me something in writing?"

[If AA mentions past deals]: "Wait, you worked with Mike Chen at
Pacific? Okay, that's different. He's solid."

[POSITIVE END]: "Alright fine. Send the proof of funds. I'll look
at it tonight. But don't waste my time with a low offer."

[HANG UP]: "Look, I've got to go. Call me back when you have
something real." [click]
```

### Scoring Expectations

- Score ≥50 if: Made it past opening, got to offer discussion
- Score ≥70 if: Made specific offer, showed market knowledge
- Score ≥85 if: Mentioned mutual contact, demonstrated track record
- HANG UP likely if: No differentiation within 60 seconds

---

## Sample 10: Practice Feedback — High Score

### Session Summary

| Field | Value |
|-------|-------|
| Property | 742 Evergreen Terrace |
| Agent Difficulty | MEDIUM (55) |
| Duration | 2:45 |
| Outcome | POSITIVE |
| Score | 84/100 |

### Transcript Highlights

```
[0:08] AA: "We're not wholesalers — we close every deal we start."
[0:32] AA: "Your seller's been on market 58 days. What's it going
to take to tie this up today?"
[1:15] AA: "I saw you worked with Pacific Coast on the Maple deal.
We're partners with them."
[1:48] AA: "Our goal is to make this profitable for you too. What
terms would make that work?"
```

### Expected Feedback Output

```
PRACTICE SCORE: 84/100 ★★★★☆
Difficulty: MEDIUM | Duration: 2:45 | Outcome: POSITIVE

✓ WHAT YOU DID WELL:
• Strong differentiation — "not wholesalers" early in call
• Direct closing question — "tie this up today"
• Homework demonstrated — mentioned agent's past deal
• Agent benefit positioning — "profitable for you too"

⚠️ AREAS TO IMPROVE:
• Could have asked about seller timeline sooner
• Price discussion came a bit late (1:48)
• Consider leading with proof of funds offer

📖 REVIEW THESE SCRIPT SECTIONS:
• Opening: The First 30 Seconds
• Closing: Creating Urgency

COMPARED TO LAST SESSION: +8 points improvement
Keep up the strong openings!
```

---

## Sample 11: Practice Feedback — Low Score (Hang Up)

### Session Summary

| Field | Value |
|-------|-------|
| Property | 456 Oak Street |
| Agent Difficulty | HARD (82) |
| Duration | 0:58 |
| Outcome | HANG UP |
| Score | 31/100 |

### Transcript Highlights

```
[0:05] AA: "Hi, calling about the Oak Street property..."
[0:18] AA: "Um, well, I work with investors..."
[0:35] AA: "What's the lowest they would take?"
[0:48] AGENT: "Look, I've got to go. Call me back when you have
something real." [HANG UP]
```

### Expected Feedback Output

```
PRACTICE SCORE: 31/100 ★☆☆☆☆
Difficulty: HARD | Duration: 0:58 | Outcome: HANG UP

✗ WHAT HAPPENED:
The agent ended the call because no value proposition was
established. With a HARD difficulty agent, you have about
30 seconds to prove you're different.

⚠️ KEY ISSUES:
• No differentiation statement in opening
• Asked "lowest price" question — triggers dismissal
• Hesitation ("um, well") signals lack of preparation
• No mention of cash, speed, or certainty

📖 MUST REVIEW:
• Opening: The Direct Value Prop
• Script: What NOT to Say
• Objection Handling: The Busy Agent

💡 TIP FOR HARD DIFFICULTY:
Lead with your strongest differentiator immediately:
"I'm not calling to lowball you. I close 100% of my offers
with cash in 10 days. I saw your seller has an NOD — let's
talk about what it takes to get this done today."

TRY AGAIN with an EASY agent to rebuild confidence.
```

---

## Sample 12: Practice Feedback — Medium Score (Neutral Outcome)

### Session Summary

| Field | Value |
|-------|-------|
| Property | 1456 Maple Street |
| Agent Difficulty | MEDIUM (52) |
| Duration | 2:12 |
| Outcome | NEUTRAL |
| Score | 62/100 |

### Transcript Highlights

```
[0:12] AA: "I work with investors looking for properties in this area."
[0:45] AA: "We can close quickly with cash, no contingencies."
[1:20] AA: "Would they consider 420?"
[1:55] AGENT: "Send me your info. I'll take a look, but I can't
promise anything."
```

### Expected Feedback Output

```
PRACTICE SCORE: 62/100 ★★★☆☆
Difficulty: MEDIUM | Duration: 2:12 | Outcome: NEUTRAL

✓ WHAT YOU DID WELL:
• Mentioned cash and quick close (key differentiators)
• Made a specific offer ($420K)
• Got agent to agree to receive information

⚠️ AREAS TO IMPROVE:
• Opening was generic — "investors looking for properties"
• Didn't ask about seller motivation/timeline
• No urgency created — agent gave lukewarm response
• Missed opportunity to mention agent benefit (commission)

📖 REVIEW THESE SCRIPT SECTIONS:
• Opening: Specificity Matters
• Discovery: The Timeline Question
• Closing: Creating Urgency

💡 TO IMPROVE TO 80+:
1. Open with specific property knowledge
2. Ask "What does your seller need?" early
3. Create urgency: "What would it take to tie this up today?"
4. Mention mutual benefit: "Let's make this profitable for both of us"

NEXT PRACTICE: Try same property with HARD difficulty
to challenge yourself.
```

---

## Winning Phrase Reference

| Theme | Examples to Detect |
|-------|-------------------|
| Differentiation | "not wholesalers", "we close every deal", "no assignments" |
| Speed/Certainty | "10 days", "cash", "no contingencies", "First American" |
| Urgency | "tie this up today", "move quickly", "get this done" |
| Homework | Agent names, past deals, mutual contacts, market data |
| Agent Benefit | "profitable for you", "make it work for everyone", "your commission" |
| Closing Questions | "what would it take", "what terms do you need", "prove I can close" |

---

## Difficulty Calibration Reference

| Score Range | Label | Characteristics |
|-------------|-------|-----------------|
| 0-33 | EASY | Hungry agent, few deals, open to double-end, aged listing |
| 34-66 | MEDIUM | Standard agent, moderate activity, professional, will engage |
| 67-100 | HARD | Top producer, busy, skeptical, requires proof fast |

---

**Document Version:** 1.0
**Last Updated:** December 31, 2024
**Status:** Ready for Training
