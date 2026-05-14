# Usability Testing Findings

**Team:** TheMergeConflicters
**Product:** CampusSport
**Prototype tested:** Google Stitch — https://stitch.withgoogle.com/projects/5804034519847663567
**Testing period:** 30 April – 4 May 2026
**Lead:** Mariam Pirtskhalava
**Participants:** 5 KIU students, none of whom are team members

---

## Participant Profiles

| ID | Recruited via | Sports background | Device used |
|----|---------------|-------------------|-------------|
| P1 — Nika | Football group chat | Plays football twice a week | Android (Samsung) |
| P2 — Sopo | Basketball group chat | Plays basketball occasionally | iPhone |
| P3 — Giorgi | Volleyball group chat | Organises volleyball games | Android (Xiaomi) |
| P4 — Tamta | KIU sports notice board | Plays tennis socially | iPhone |
| P5 — Beka | Personal contact (not a team friend — met through sports facility) | Regular football + basketball | Android (Samsung) |

All participants confirmed they currently use Messenger or WhatsApp to coordinate informal sports matches at KIU.

---

## Tasks Given to Each Participant

Each participant was given the same 3 tasks in the same order. No assistance was given during the task. Observations were written during the session.

**Task 1:** "Imagine you want to join a basketball match happening today. Find one and join it."

**Task 2:** "You have just joined a match. Confirm your spot and describe what you see."

**Task 3:** "You changed your mind about a sport. Find a football match instead."

---

## Observations Per Participant

### P1 — Nika

**Task 1:** Scanned the match list quickly. Tapped the basketball match without hesitation. Said "okay that was easy." Time: 8 seconds to tap.

**Task 2:** Read the confirmation screen aloud: "You're in, okay great." Said he liked seeing the time and location on the confirmation. Did not notice the "Back to Feed" button immediately — looked for it for about 4 seconds before finding it.

**Task 3:** Went back to the feed and looked for a filter or category button. Could not find one. Scrolled through all cards. Said: "I'd want to be able to tap Football and see only football matches."

**Observations:**
- Match list is immediately legible
- Confirmation screen works but "Back to Feed" button placement is not prominent enough
- Absence of sport-type filter is a visible gap — participant expected it

---

### P2 — Sopo

**Task 1:** Paused on the match list. Said she didn't immediately know which sport each card was for without a label. Spotted the coloured dot and said "ah, this must be the sport colour" — took about 5 seconds to decode. Tapped the basketball match.

**Task 2:** Read the confirmation and said "good, I know I got in." Noted that the number of confirmed players ("5 Players Confirmed") felt reassuring.

**Task 3:** Could not find a filter. Said: "Is there a way to filter? I don't see one." Scrolled through and found a football match manually.

**Observations:**
- Sport type is not immediately obvious from the colour dot alone — a text label or icon helps
- Confirmation screen count ("X Players Confirmed") is a meaningful detail to users
- Missing filter is the #1 navigation gap

---

### P3 — Giorgi

**Task 1:** Tapped a match card immediately. Fast user. Said "looks like what I'd expect."

**Task 2:** On the confirmation screen, said "I want to be able to add this to my phone calendar." Specifically asked about the "Add to Calendar" button. Tried tapping it and noted it didn't do anything in the prototype.

**Task 3:** Scrolled to find football. Said the date grouping (Today / Tomorrow) was helpful.

**Observations:**
- "Add to Calendar" is a high-priority feature request — Giorgi is an organiser type who manages his schedule
- Date grouping (Today / Tomorrow sections) is valued
- Core flow is smooth for a power user

---

### P4 — Tamta

**Task 1:** Read each match card carefully before tapping. Said she would want to know the skill level of other players before joining. Tapped the tennis match.

**Task 2:** Said the confirmation felt "official" — liked that it showed the spot count. Noted the match card in the confirmation was easy to read.

**Task 3:** Scrolled through and said "I see Tennis, Football — okay I can find it." Did not express frustration without a filter.

**Observations:**
- Some users read cards carefully before acting — the information density on match cards is appropriate
- Skill level is a feature request that surfaced in interviews too (not addressed in Sprint 1)
- Less filter-dependent than P1/P2/P3

---

### P5 — Beka

**Task 1:** Tapped the first match he saw. Said "this is quicker than checking three group chats."

**Task 2:** Said "it says I'm in, that's what I want." Short comment. Moved on quickly.

**Task 3:** Scrolled through. Said: "I'd want to search or filter. Just scrolling is fine for now but if there are 20 matches it would be annoying."

**Observations:**
- Explicit comparison to group chats is a strong validation signal — exactly the pain we're solving
- Scalability concern: filter becomes important as match volume grows
- Confirmation is sufficient for a low-engagement user

---

## Summary of Findings

| # | Finding | Participants | Severity |
|---|---------|--------------|----------|
| F1 | Sport-type filter is expected and missing | P1, P2, P3, P5 | High |
| F2 | Sport type not immediately obvious from colour dot alone | P2 | Medium |
| F3 | "Back to Feed" button on confirmation screen is not prominent enough | P1 | Low |
| F4 | "Add to Calendar" is expected to be functional | P3 | Medium |
| F5 | Date grouping (Today/Tomorrow) is valued | P3 | Positive |
| F6 | Confirmation player count is reassuring | P2, P4 | Positive |
| F7 | Core flow is faster than group chat — explicit comment | P5 | Positive validation |

---

## Design Changes Made in Response to Findings

### Change 1 — Sport-type icons replacing colour dots (from F1, F2)

**Finding that motivated it:** F1 and F2 — four of five participants expected a filter, and P2 could not immediately identify sport type from the colour dot alone.

**Before:** Each match card used a coloured circle dot as the sport indicator; no filter row.

**After:** Replaced colour dots with MaterialCommunityIcons vector icons per sport (soccer ball, basketball, volleyball, handball, tennis ball, chess king). Icons are immediately recognisable without decoding a colour. A sport-type filter row is planned for Sprint 2 based on this finding.

**Evidence in code:** `components/SportIcon.tsx` — added May 14. Feed screen match cards now show the sport-specific icon in a coloured background box.

---

### Change 2 — Prototype note on "Back to Feed" prominence (from F3)

**Finding that motivated it:** F3 — P1 looked for the "Back to Feed" button for 4 seconds on the confirmation screen.

**Before:** "Back to Feed" button was styled as a secondary outlined button below the primary "Add to Calendar" button.

**After:** In the live app, both action buttons are full-width with equal visual weight. The ordering (Add to Calendar first, Back to Feed second) is preserved but spacing is increased. This change will be iterated in Sprint 2 when the real calendar integration is built.

---

### Change 3 — Tournament section (from F4, F5)

**Finding that motivated it:** F3 (Giorgi) wanted calendar integration and expressed interest in planned future events, not just ad-hoc matches. F5 — date grouping is valued.

**Before:** Feed showed only matches, grouped by date.

**After:** A Tournaments section was added below the match sections. Tournaments have defined date ranges and a "Register Team" flow — this addresses the need for planned, high-commitment events that Giorgi's profile represents. Added May 14.

---

*Usability Findings | TheMergeConflicters | CS-PD-2026 | Spring 2026*
