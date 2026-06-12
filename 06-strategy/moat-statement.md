# Moat Hypothesis

**Team:** TheMergeConflicters
**Product:** CampusSport
**Date:** 10 June 2026

---

## Note on framing — why this is a Hypothesis, not a Statement

The Lab 11 rubric is explicit: *"A well-written hypothesis earns partial marks. An overclaimed moat earns none."* CampusSport is 13 weeks into development. Sprint 2 (push notifications + organizer flow + analytics instrumentation) just finished on 21 May. Sprint 3 (quorum visibility + share/invite + match management) is in progress. We do not yet have a production retention cohort, a measured switching cost, or signed partnership agreements committed to the repo. We have strong qualitative evidence from 10 interviews that the moat we are building toward is real, and we have a concrete plan to convert that qualitative evidence into the three repository-evidence pieces the rubric requires.

This document is therefore Section B of the template — the Hypothesis form — with the evidence gap and collection plan named honestly.

---

## The Power We Are Building Toward

**Primary Helmer Power:** **Switching Costs** (primary, building) with **Network Effects** as an emerging secondary power on the supply side.

We pick Switching Costs as primary because it is the power our product's structure most directly produces. An organizer who has set up recurring matches, accumulated a player roster, and built a participant history in CampusSport faces a real, growing cost to recreate that history elsewhere. We mark Network Effects as emerging-secondary because once we are running on more than one university, every new organizer makes the product more valuable to every other organizer at the same university (they share the player audience) and very weakly to organizers at other universities (they share the credibility signal). We deliberately do not claim Network Effects as primary — that would require demonstrated cross-side density we do not have.

We deliberately do not claim Branding, Scale Economies, or Process Power. The rubric is correct that these require exceptional evidence at our stage and we cannot provide it.

---

## Why We Believe This Power Is Accessible

The product is built around a single asymmetric design constraint: zero extra work from the organizer (Cotne, I-10). Every feature that wins the organizer over — match templates, recurring schedules, accumulated participant history, the auto-fan-out push notification — is also a feature that creates personal artifacts the organizer would have to rebuild from scratch in any other tool. By Sprint 4, our planned MVP includes match creation, organizer-cancel-with-auto-notify, share/invite links, and match result logging. Each is a unit of organizer history. The user behaviour we are designing for — recurring weekly games organized by the same person across a semester — naturally accumulates state that the organizer values. Switching Costs are not bolted on; they are a byproduct of the core product working as designed.

The Network Effects component is structurally present but currently weak: every new player at KIU makes a CampusSport match more likely to fill quorum, which makes posting a match on CampusSport more attractive to an organizer, which attracts more players. The loop exists. It does not yet spin fast because the player and organizer base are both small.

---

## The Evidence Gap

We do not yet have the three pieces of repository evidence the rubric requires for a full Switching Costs claim. Specifically, we need direct measurements of organizer retention, accumulated history per organizer, and the player-side stickiness that comes from being part of a recurring group.

| Evidence needed | What it would show | How we will collect it |
|------------------|---------------------|-------------------------|
| Organizer 4-week retention cohort (Sprint 2 cohort returning to create a 2nd, 3rd, 4th match) | The organizer is building a repeating-behaviour pattern that has cost to abandon | SQL query / Django-admin export counting distinct `match_created` records per organiser `user_id` over a 4-week window, run on the Sprint 2 cohort once 4 weeks of data exists. Levan exports the result to `03-build/analytics/organizer-retention-cohort.png` by **8 July 2026** |
| Accumulated participant-history depth per organizer | Each organizer has a non-trivial roster of players and a non-trivial number of past matches that they would have to recreate in any other tool | SQL query on Django backend counting `participants_per_organizer` and `past_matches_per_organizer` for organizers active for ≥4 weeks. Davit commits the query and its output as `03-build/analytics/organizer-history-depth.md` by **8 July 2026** |
| Direct player quote demonstrating perceived switching cost or group-stickiness | Players articulate the cost of leaving CampusSport even when they have not built personal history themselves, because they would lose access to their group | Targeted re-interview of 3 of the 10 original interviewees once they have been on the live product for 4+ weeks, asking specifically: *"If a friend told you about a competing app today, would you switch? What would it cost you?"* Mariam Pirtskhalava runs the interviews and commits transcripts to `01-discovery/interview-logs/` by **15 July 2026** |

We already have **strong indirect evidence** that the switching cost mechanism is real for organizers, even before our own data:

- **Cotne (Interview #10):** built a working Telegram bot and a Google Sheets schedule. Both failed because the organizer would not migrate from Messenger. The organizer's investment in Messenger as the place where the group expects information was already a switching cost — a cost so high that two technically-correct alternatives could not overcome it. Documented in `01-discovery/interview-logs/interview-10.md`.
- **Mamuka (Interview #5):** *"If auto-reminders existed, I would switch immediately."* Once an organizer switches to CampusSport, the same switching-cost mechanic that protected Messenger will protect us. Documented in `01-discovery/interview-logs/interview-05.md`.
- **Gega (Interview #2):** the WhatsApp sub-group he created could not displace the original Messenger group because the organizer kept posting in Messenger. Even within Meta's own ecosystem, the established platform's switching cost dominated. Documented in `01-discovery/interview-logs/interview-02.md`.

These three interview citations are strong qualitative evidence for the *mechanism* of the moat but do not yet evidence the moat's *strength inside our product*. The collection plan above closes that gap.

---

## Our Commitment

**Target date for evidence collection:** Three pieces of repository evidence collected and committed by **15 July 2026** — five weeks after Demo Day, in time for Checkpoint 4 / final repository review at Week 15.

**Specific first action and owner:** Levan Kovziridze writes the SQL / Django-admin query for organizer 4-week retention by **20 June 2026** — this is the longest-lead-time evidence piece because it requires four full weeks of post-Sprint-2 data to accumulate, and we want it ready before the Checkpoint 4 deadline rather than the day of.

**Secondary commitment:** if by **15 July 2026** the organizer retention curve shows a 4-week retention rate below 25%, we treat the Switching Costs hypothesis as falsified at the current product stage and re-write this document. In that case the primary moat we would claim instead is **Counter-Positioning** — we are the only purpose-built tool that wins the organizer specifically by refusing to add chat (the exact feature general-purpose competitors cannot remove without contradicting their core identity). Counter-Positioning is an honest fallback because our product strategy is structurally different from the incumbents in a way they cannot easily copy, regardless of whether we accumulate personal switching cost.

---

## Path Forward (60 days)

Even though this is a hypothesis, the rubric also asks what specific actions over the next 60 days would strengthen the moat. Over the next 60 days, three concrete moves: (1) Ship the recurring-match-template feature (currently scoped for post-Demo Day Sprint 5) to deepen the per-organizer history accumulation. (2) Approach KIU Sports & Wellness Office (per `ecosystem-map.md` § Partners) and convert the conversation into a formal pilot during autumn 2026, which would constitute a Cornered Resource signal alongside the Switching Costs hypothesis. (3) Onboard the first organizer at one of Free Uni / ISU / TSU before 11 August 2026, which extends the geographic dimension of the moat to a level no single-university tool could match.

---

*Filed by Davit Karoiani (PO), with input from Mariam Pirtskhalava (Discovery), Mariam Tskhomelidze (Scrum), Levan Kovziridze (Test).*
