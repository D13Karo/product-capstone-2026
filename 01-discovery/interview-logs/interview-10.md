# Interview Log — Interview 10

**Interviewee:** Cotne (last name withheld)
**Date:** April 1, 2026
**Duration:** 45 minutes
**Interviewer:** Davit Karoiani
**Location:** His room (dorm), after he finished playing games on his PC

---

## ICP Verification

- [x] KIU student: Yes — 3rd year, Management faculty
- [x] Participates in informal peer-organized sports: Yes — football tournaments, regular participant
- [x] Has experienced missing or showing up to a changed game: Yes — and has tried to build a solution

**ICP match:** Confirmed — plus uniquely valuable technical perspective

---

## The Story

Cotne is a CS student and he did what CS students do: when he encountered the sports coordination problem, he tried to build his way out of it. He created a Telegram bot in January that would read announcements from an admin and forward them to all subscribers. He spent two weekends on it. He was proud of it.

It lasted three weeks. 

"The problem wasn't the bot. The bot worked fine. The problem was that Sandro [the organizer] didn't want to use a different platform. He already had everything in Messenger. Asking him to switch to Telegram to post updates was one extra step he wasn't willing to take." Cotne paused and nodded slowly. "I learned something from that."

What he learned: the solution has to work for the organizer, not just the participants. "You can build the best tool in the world but if the person with the information won't use it, it's useless." He also tried a Google Sheets solution — a shared schedule everyone could view. Same result. The organizer didn't update it, so it went stale within a week and people stopped trusting it.

He has since given up on technical solutions and just texts the organizer directly. "It's the least scalable solution possible. It only works because I know Sandro personally. If I didn't know him, I'd have no reliable way to get information."

Cotne was the longest interview. He had a lot to say. Crucially, he validated the problem from a technical angle and told us what doesn't work and why.

---

## 3 Key Quotes

1. "I built a Telegram bot that worked perfectly. Nobody used it. The organizer wasn't willing to change his workflow for it. That was the real problem."

2. "Any solution has to require zero extra work from the organizer. If it's one more thing they have to do, it won't happen."

3. "I now just text Sandro directly. It's the most un-scalable solution imaginable. It only works because I know him. Most people don't have that."

---

## Pain Intensity

**Rating:** 4 / 5

**Evidence for rating:** He invested two weekends building a solution — that is high commitment. The failure of his own solution is both evidence of the problem's depth and a critical product insight. He remains affected (resorted to lowest-tech workaround) even after attempting a sophisticated fix.

---

## One-Sentence Insight

Cotne's Telegram bot failed not because of bad engineering but because adoption requires the organizer to change behavior — this is the product's core design constraint: any solution must work within the organizer's existing workflow, not alongside it.

---

## Current Workaround

- Texts the organizer (Sandro) directly before every game
- Previously: Telegram bot (abandoned), Google Sheets (abandoned)

**Why it's inadequate:** Completely non-scalable. Requires personal relationship with organizer. Cannot be replicated for other groups or new participants.

---

## Unexpected Findings

- The failed Telegram bot provides direct evidence that the organizer is the adoption bottleneck — this shapes product requirements more than any other single interview
- Two prior technical solutions (bot + spreadsheet) both failed for the same reason: organizer workflow disruption
- He has thought about this problem technically longer than anyone else and his diagnosis ("zero extra work from the organizer") is probably the most actionable constraint in the dataset
- He mentioned that Sandro is graduating in June — which means the group will lose its informal hub, making the problem worse in a few months
