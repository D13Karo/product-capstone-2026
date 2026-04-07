# Final Evidence-Based Problem Statement

**Team Name:** The Merge Conflicters
**Date:** April 6, 2026
**Lab:** Lab 4 - Week 4
**Based on:** 10 customer discovery interviews (Weeks 3-4)

---

## Executive Summary

**One-Sentence Problem Statement:**

KIU students who participate in informal peer-organized sports events consistently miss game time or location changes, arrive unprepared, or show up to cancelled events because schedule updates get buried in the noise of general-purpose group chats and are spread inconsistently across multiple platforms with no single source of truth, resulting in 7 in 10 students missing at least one game last semester, frequent game cancellations for lack of quorum, gradual community dissolution, and organizers spending more time on logistics than on playing.

**Confidence Level:** ⭐⭐⭐⭐⭐ (5/5)

**Why this confidence level:**
All 3 top patterns are validated across 9–10/10 interviews with average pain intensity of 4.0–4.1/5. Quantified impact is present (45–60 min/week organizer overhead, 7/10 missed games, 4/8 players causing a cancelled volleyball game). Every workaround attempted by participants failed, confirming the problem is structural. Two real campus sports groups have already dissolved following this pattern.

---

## Problem Statement Components

---

### Component 1: Specific Segment (WHO)

**WHO exactly experiences this problem?**

KIU students (primarily 3rd year) who participate in informal peer-organized sports events — football, basketball, futsal, volleyball — and rely on Facebook groups or Messenger chats to find out about game times, locations, and schedule changes. This includes both regular participants and the student organizers who coordinate these events.

**ICP Validation from Interviews:**
- 10/10 interviews matched our ICP criteria perfectly
- 0/10 interviews were outside the ICP
- Post-discovery refinement: the organizer (Mamuka, I-05) faces a distinct but related problem on the supply side — both profiles are in scope

**Refinements to ICP Based on Discovery:**
- **Original ICP:** KIU students aged 18–24 who play informal sports
- **Validated ICP:** KIU students who are socially active in peer-organized sports but not necessarily in leadership roles — plus the small number of student organizers who coordinate these groups
- **Key learnings:** The organizer perspective was entirely absent from our v1.0 ICP. Post-interviews, the organizer is actually the most critical user to design for — if they don't adopt a solution, participants can't benefit

**Evidence:**
- Interview #01 (Khatia): Matches ICP — regular football participant, relies on Facebook/Messenger, missed game due to info failure
- Interview #05 (Mamuka): Matches ICP — informal organizer, 45–60 min/week on logistics, considering quitting
- Interview #10 (Cotne): Matches ICP — technically capable participant who built workarounds; both builder and player perspective

---

### Component 2: Specific Problem (WHAT)

**WHAT problem do they experience?**

They regularly miss game time or location changes, arrive unprepared, or show up to events that have been cancelled or moved — because critical schedule updates are buried in high-volume group chats and spread inconsistently across multiple platforms with no single authoritative source.

**Pattern Validation:**
- Mentioned in: 9/10 interviews (P2: Late/missed updates) and 10/10 interviews (P1: Fragmentation)
- Supporting quotes: 30 total across P1 and P2
- Intensity rating: 4.1/5 (P2), 4.0/5 (P1)

**Key Evidence Quotes:**
1. Interview #03 (Giorgi): "Game moved one hour earlier — posted in chat — I didn't see it — team played without me"
2. Interview #06 (Nata): "Venue change posted at 11pm — I saw the screenshot I had, not the update — showed up at wrong place"
3. Interview #01 (Khatia): "The organizer posts updates but they get buried under 40 unrelated messages"
4. Interview #09 (Tiko): "Tournament bracket changed the day before — found out 30 minutes before kickoff"

**Supporting Patterns:**
- Pattern P1: Information Fragmentation — 10/10 interviews
- Pattern P3: Organizer Coordination Burden — 7/10 interviews

**How patterns relate:**
P3 (organizer burden) is the root cause that generates both P1 and P2 — because no purpose-built tool exists, organizers manually broadcast across 3+ platforms, which creates fragmentation (P1) and ensures critical updates get lost in noise (P2). Fix P3 and the other two largely resolve.

---

### Component 3: Context (WHEN/WHERE)

**WHEN/WHERE does this problem occur?**

Throughout the academic semester whenever informal sports events are being organized — typically in the 12–48 hours before a game, when last-minute changes are most likely to occur and least likely to be seen by participants scrolling busy group chats.

**Contextual Evidence:**
- Interview #05 (Mamuka): Most changes happen within 24 hours of the event, triggered by weather, venue availability, or player availability
- Interview #03 (Giorgi): Time change posted the evening before; he didn't check the chat until morning and missed it entirely
- Interview #08 (Nana): Venue posted at 11pm; participants who were already asleep missed it

**Frequency:**
- Weekly: 10/10 interviews — every interviewee deals with this on a per-game basis throughout the semester
- The problem is most acute at the point of schedule change — initial announcements are usually seen, but updates to them are not

**Typical Scenarios Where Problem Occurs:**
1. A game time shifts by 1 hour the evening before — posted in chat — buried under casual conversation by morning
2. A venue changes last-minute due to booking conflicts — participants rely on screenshots that are now outdated
3. A tournament bracket changes the day before — organizer posts in one channel; not all participants see it before kickoff

---

### Component 4: Root Cause (WHY)

**WHY does this problem exist at the deepest level?**

Because there is no purpose-built coordination tool for informal student sports — participants and organizers rely entirely on general-purpose social apps (Messenger, Facebook, WhatsApp) that were designed for conversation, not time-sensitive event management. In these apps, critical updates compete with casual conversation for attention with no way to distinguish a game cancellation from a meme.

**Five Whys Chain:**
```
Surface: Players miss game changes posted in group chats
  ↓ Why?
Updates get buried in high-volume chat noise
  ↓ Why?
Critical updates compete with casual messages — no distinction exists
  ↓ Why?
General-purpose chat apps have no concept of "important event update"
  ↓ Why?
These apps were designed for conversation, not event coordination
  ↓ Why?
ROOT CAUSE: No purpose-built coordination tool exists for informal
student sports groups — general-purpose social apps fill the gap by default
```

**Evidence of Root Cause:**
- Interview #06 (Nata): "There is no source of truth. Everyone has a different version of the information."
- Interview #10 (Cotne): "Two solutions I built failed because I couldn't control where the organizer puts information"
- Interview #03 (Giorgi): "I muted the group because it was too noisy. Then I missed the time change. You can't win either way."

**How we validated this is the root cause:**
Every workaround attempted by participants (muting chats, creating sub-groups, building bots, using spreadsheets) failed or created new problems — because participant-side fixes cannot solve an organizer-side infrastructure problem. The root cause explains all three validated patterns simultaneously.

**Surface problems this root cause explains:**
1. Players arriving at wrong venue or missing games entirely (P2)
2. Organizer duplicating announcements across 3+ platforms (P1)
3. Every self-built workaround making fragmentation worse (P4)

---

### Component 5: Measurable Consequences (IMPACT)

**WHAT happens as a result? What is the measurable impact?**

**Time Impact:**
- Organizer overhead: 45–60 minutes per week on logistics for a 60-minute game (1:1 ratio)
- Player wasted time: 30–60 min per incident (wrong venue travel, waiting at cancelled games)
- Evidence: Interview #05 (Mamuka): "I spend more time communicating about the game than actually playing it"

**Emotional/Stress Impact:**
- Emotional language used: "frustrated", "considering quitting", "you can't win either way", "I would rather skip a game than show up stressed"
- Evidence: Interview #05 (Mamuka): "I've considered quitting as organizer. The football is great. The admin is not."

**Quality/Output Impact:**
- Missed games: 7/10 interviewees missed at least one game last semester due to info failure
- Cancelled games: 4 of 8 players showed up to Nana's volleyball game — insufficient quorum — four people wasted their afternoon
- Performance impact: Tiko (I-09) lost preparation time for a tournament opponent when the bracket changed last-minute
- Evidence: Interview #08 (Nana): "Four people showed up to volleyball — needed 8 — had to cancel — four people wasted their afternoon"

**Behavioral Impact:**
- Khatia (I-01) stopped inviting friends to games after passing on wrong information twice
- Giorgi (I-03) muted the noisy chat — which caused him to miss the game change entirely
- Cotne (I-10) spent two weekends building a Telegram bot that failed when the organizer wouldn't switch platforms
- Evidence: Interview #01 (Khatia): "I stopped inviting friends because I'm embarrassed when things change after I've invited them"

**Frequency of Impact:**
- Occurs: Weekly — every game cycle involves some version of this problem
- Mentioned by: 10/10 interviews

**Summary of Consequences:**
Results in 7/10 students missing at least one game per semester due to information failure, frequent game cancellations for lack of quorum, 45–60 min/week of organizer overhead (equal to game duration), community decay (two campus groups have already dissolved following this pattern), and a near-term succession risk as the primary organizer for one group graduates in June 2026.

---

## FINAL PROBLEM STATEMENT

### Complete Statement

KIU students who participate in informal peer-organized sports events consistently miss game time or location changes, arrive unprepared, or show up to cancelled events because schedule updates get buried in the noise of general-purpose group chats (Messenger, Facebook, WhatsApp) and are spread inconsistently across multiple platforms with no single source of truth. During the academic semester, in the 24–48 hours before each game, critical updates compete with casual conversation for attention — with no way to distinguish a cancellation from a meme. As a result, 7 in 10 students we interviewed missed at least one game last semester due to an information failure; group games are frequently cancelled for lack of quorum; informal sports communities gradually dissolve as participants lose confidence that showing up is worth the effort; and the organizers who hold these groups together spend more time on logistics than on playing, with at least one considering quitting. The current workaround — checking multiple chats, taking screenshots, texting the organizer directly — fails systematically: every workaround we documented either required unsustainable effort or made the fragmentation worse.

**Evidence documented across 10 customer discovery interviews** conducted in Weeks 3–4 (March–April 2026), with strong validation from interviews #1 through #10. Top pain points identified: (1) Information fragmented across multiple platforms — no single source of truth (10/10 interviews), (2) Schedule changes lost in chat noise (9/10 interviews), (3) Organizer coordination burden unsustainable (7/10 interviews). Root cause analysis completed using Five Whys methodology, identifying the absence of a purpose-built sports coordination tool as the underlying structural issue.

---

## Problem Statement Evolution

### Week 1 Hypothesis (Original)

**Original Problem Statement:**
"LMS notifications are unreliable; KIU students miss important academic updates."

**Original Confidence:** ⭐⭐ (2/5) — Based on personal experience and assumptions

---

### Week 4 Validated Statement (Evidence-Based)

**Final Problem Statement:**
See complete statement above.

**Final Confidence:** ⭐⭐⭐⭐⭐ (5/5) — Based on 10 interviews with quantified data

---

### What Changed & Why

**Key Changes from Original to Final:**

1. **Domain pivoted entirely:**
   - Before: Academic LMS notifications
   - After: Informal sports event coordination
   - Why: Pre-interview signals in Week 2 revealed far stronger pain in sports coordination than in LMS; all 10 interviews confirmed the pivot

2. **Quantification added:**
   - Before: No numbers
   - After: 7/10 missed games, 45–60 min/week overhead, 4/8 quorum failure
   - Why: Interviewees provided concrete, specific impact data unprompted

3. **Root cause identified:**
   - Before: "scattered info across Facebook and Messenger"
   - After: General-purpose apps cannot prioritize time-sensitive updates — structural mismatch between tool design and use case
   - Why: Five Whys analysis across 3 patterns converged on the same underlying cause

4. **ICP refined to include organizer:**
   - Before: Passive participants only
   - After: Both participants and organizers — organizer is the primary design target
   - Why: Mamuka (I-05) and Cotne (I-10) revealed that every failed solution broke at the organizer layer

**What Surprised Us:**
Organizer burnout was the most underrated risk — we had no organizer perspective in v1.0. Every failed workaround (bot, sub-group, spreadsheet, mute strategy) made things worse rather than better, confirming that participant-side fixes cannot solve an organizer-side infrastructure problem.

**What Got Invalidated:**
The original LMS hypothesis — zero of our 10 interviews pointed to academic notification problems as a primary pain. The pivot to sports was fully validated.

**What Got Strongly Validated:**
Platform fragmentation and missed updates were present in 9–10/10 interviews, confirming the v2.0 direction. The severity of community decay (two dissolved groups) was stronger than expected.

---

## Quality Validation Checklist

- ☑ **Specificity Test:** KIU students, peer-organized sports, Messenger/Facebook/WhatsApp — specific enough that a stranger would know exactly who we mean
- ☑ **Evidence Test:** Every claim cites 7–10 interviews; full citation index below
- ☑ **Root Cause Test:** Five Whys completed for all 3 top patterns; root cause is structural, not behavioral
- ☑ **Quantification Test:** 7/10 missed games, 45–60 min/week, 4/8 quorum failure, 2 dissolved groups
- ☑ **Actionability Test:** Problem statement directly implies push notifications, single source of truth, low organizer friction
- ☑ **ICP Alignment Test:** All 10 interviewees match validated ICP
- ☑ **Pain Intensity Test:** Average 4.0–4.1/5 across top patterns; organizers at 4.5/5
- ☑ **Feasibility Test:** Core solution (single source + push notifications) is buildable in 8–10 weeks
- ☑ **Scope Test:** Focused on one primary problem — sports event coordination failure
- ☑ **Truth Test:** ✅ — all four team members agree this accurately represents what we heard

**All boxes checked:** ✅ Problem statement is ready

---

## Evidence Summary

### Interview Citation Index

**WHO (ICP):**
- Strong matches: Interviews #1, 2, 3, 4, 5, 6, 7, 8, 9, 10

**WHAT (Problem):**
- Primary pattern (P2 — late updates): Interviews #1, 2, 3, 4, 5, 6, 7, 8, 9
- Secondary pattern (P1 — fragmentation): Interviews #1, 2, 3, 4, 5, 6, 7, 8, 9, 10
- Tertiary pattern (P3 — organizer burden): Interviews #1, 5, 6, 9, 10

**WHEN/WHERE (Context):**
- Described context: Interviews #1, 2, 3, 4, 5, 6, 7, 8, 9, 10

**WHY (Root Cause):**
- Root cause evidence: Interviews #3, 5, 6, 8, 10

**IMPACT (Consequences):**
- Missed games: Interviews #1, 2, 3, 5, 6, 7, 9
- Organizer time waste: Interview #5
- Cancelled games / quorum failure: Interview #8
- Community decay: Interview #8
- Social embarrassment / behavior change: Interviews #1, 7
- Failed workarounds: Interviews #2, 3, 4, 10

---

## Next Phase: Design Direction

### Implications for Solution Design

**Based on this validated problem, our solution MUST:**

1. **Create a single source of truth for each sports group's schedule**
   - Why: P1 (fragmentation) is present in 100% of interviews; every failed workaround broke because there was no canonical channel
   - Evidence: I-06 (Nata): "There is no source of truth. Everyone has a different version of the information."

2. **Push critical updates automatically to all participants**
   - Why: P2 (late updates) has the highest average pain (4.1/5); pull-based solutions require participants to check in, which the data shows they won't reliably do
   - Evidence: I-01 (Khatia): "The delay between posting and seeing can be hours. By then the window to respond is gone."

3. **Require near-zero extra effort from the organizer**
   - Why: Every failed workaround broke at the organizer layer; Cotne (I-10) proved even a working tool fails if the organizer must change behavior
   - Evidence: I-10 (Cotne): "Any solution needs to require zero extra work from the organizer"

**Based on this validated problem, our solution SHOULD:**
- Support basic RSVP / attendance confirmation so organizers know if quorum is viable before it's too late
- Distinguish confirmed events from tentative ones — remove ambiguous "maybe cancel?" situations entirely

**Based on this validated problem, our solution MUST NOT:**
- Require participants to check a new platform proactively — push is mandatory, pull will fail
- Require the organizer to post in multiple places — this is the exact behavior causing the problem

**Initial Solution Direction (High-Level):**
A lightweight, push-based sports coordination tool where the organizer posts once and all registered participants receive a direct notification. When anything changes, the notification goes out automatically — no re-announcement cycle required. This is simpler than a full scheduling app and more purpose-built than any existing general-purpose chat solution.

---

## Team Sign-Off

**We, the undersigned team members, affirm that:**

1. ✅ This problem statement accurately reflects our interview findings
2. ✅ We have 10 interviews with documented evidence supporting every claim
3. ✅ We are confident this problem is real, painful, and worth solving
4. ✅ We have identified the root cause, not just symptoms
5. ✅ We commit to building a solution addressing this validated problem
6. ✅ We understand that if new evidence contradicts this, we will pivot
7. ✅ We are ready to proceed to the Design phase (Week 5+)

| Name | Role | Signature | Date | Confidence (1-5) |
|------|------|-----------|------|------------------|
| Davit Karoiani | Program Lead | DK | April 6, 2026 | ⭐⭐⭐⭐⭐ |
| Mariam Pirtskhalava | Discovery Lead | MP | April 6, 2026 | ⭐⭐⭐⭐⭐ |
| Mariam Tskhomelidze | Tech Lead | MT | April 6, 2026 | ⭐⭐⭐⭐⭐ |
| Levan Kovziridze | Test Lead | LK | April 6, 2026 | ⭐⭐⭐⭐⭐ |

---

## Instructor Review Section

**Instructor Feedback:**

**Strengths:**
- [What's strong about this problem statement]
- [What's well-evidenced]

**Areas for Improvement:**
- [Suggestions for refinement]
- [Questions or concerns]

**Approval Status:**
☐ Approved - Ready for Design Phase
☐ Approved with minor revisions
☐ Needs significant revision

**Instructor Signature:** _______________ **Date:** ___________

---

## Appendix: Full Interview Evidence

### Interview #01 — Khatia
- **ICP Match:** ✅ Yes
- **Key Contribution:** Showed up at wrong field; stopped inviting friends after passing on wrong info; primary organizer (Sandro) named as single point of failure
- **Top Quote:** "I stopped inviting friends because I'm embarrassed when things change after I've invited them"

### Interview #02 — Gega
- **ICP Match:** ✅ Yes
- **Key Contribution:** Created a WhatsApp sub-group to fix fragmentation — added a third channel instead; highest pain scores (5/5) on fragmentation and late updates
- **Top Quote:** "There's a Messenger group, a Facebook group, and now a WhatsApp sub-group — three places, one game"

### Interview #03 — Giorgi
- **ICP Match:** ✅ Yes
- **Key Contribution:** Muted noisy chat → missed time change → missed entire game; articulated the "you can't win either way" dilemma most clearly
- **Top Quote:** "I muted the group because it was too noisy. Then I missed the time change. You can't win either way."

### Interview #04 — Nia
- **ICP Match:** ✅ Yes
- **Key Contribution:** "I have 12 unread sports chats and still miss things"; wants confirmation not just announcement; checking itself has become a time cost
- **Top Quote:** "I want a confirmation, not just an announcement. Tell me the game is confirmed."

### Interview #05 — Mamuka
- **ICP Match:** ✅ Yes (organizer perspective)
- **Key Contribution:** Richest organizer data — 45–60 min/week logistics, three-step announcement cycle, considering quitting; if auto-reminders existed he'd switch immediately
- **Top Quote:** "I spend more time communicating about the game than actually playing it"

### Interview #06 — Nata
- **ICP Match:** ✅ Yes
- **Key Contribution:** Uses screenshot system that goes stale the moment info changes; showed up at wrong venue; articulated "no source of truth" most clearly
- **Top Quote:** "There is no source of truth. Everyone has a different version of the information."

### Interview #07 — Misho
- **ICP Match:** ✅ Yes (low-effort user)
- **Key Contribution:** Even the lowest-friction user has missed games and has a clear solution model ("just ping me directly"); validates that the problem affects even the most tolerant users
- **Top Quote:** "Just ping me directly — game time, location, done. Why is that so hard?"

### Interview #08 — Nana
- **ICP Match:** ✅ Yes
- **Key Contribution:** Community decay perspective — watched two groups dissolve; quorum failure example (4/8 showed up); "maybe cancel?" ambiguity case
- **Top Quote:** "I've seen two sports groups completely collapse because logistics kept failing"

### Interview #09 — Tiko
- **ICP Match:** ✅ Yes (tournament/competitive context)
- **Key Contribution:** Tournament bracket changes affect preparation, not just attendance; problem is normalized ("nobody cared"); highest pain on late updates (5/5)
- **Top Quote:** "If someone built a proper system, I would switch immediately and make everyone else switch too"

### Interview #10 — Cotne
- **ICP Match:** ✅ Yes (builder/technical user)
- **Key Contribution:** Built two failed solutions (Telegram bot, Google Sheets); both broke at organizer layer; articulated the organizer-first design constraint most clearly; Sandro graduation risk
- **Top Quote:** "Two solutions I built failed because I couldn't control where the organizer puts information"

---

**Document Status:** ☑ Complete ☐ Draft ☑ Team Approved ☐ Instructor Approved
**Version:** 3.0
**Last Updated:** April 6, 2026
**Authors:** Davit Karoiani, Mariam Pirtskhalava, Mariam Tskhomelidze, Levan Kovziridze

---

**File Location:** `/01-discovery/synthesis/final-problem-statement.md`