# Strategy Canvas

**Team:** TheMergeConflicters
**Product:** CampusSport
**Date:** 10 June 2026

---

## Competitive Factors

The factors below come from the competitive matrix in `competitive-analysis.md`. The Industry Average is the mean of the six competitor scores (Facebook Messenger, Facebook Groups, WhatsApp, Discord, Telegram bots, Google Sheets) — not including our own score.

| Factor | Industry Average | Our Product Score |
|--------|------------------|-------------------|
| Core feature: schedules informal sports matches | 2.0 | 5 |
| Push notification specifically for match changes | 1.5 | 5 |
| Single source of truth for the schedule | 2.3 | 5 |
| Zero extra effort required from the organizer | 3.3 | 4 |
| Quorum visibility before leaving home | 0.7 | 4 |
| University-scoped audience (KIU only) | 1.3 | 5 |
| Mobile experience quality | 3.8 | 4 |
| Existing user adoption on KIU campus | 2.8 | 1 |
| Switching cost / user lock-in | 2.7 | 2 |
| General-purpose chat conversation | 4.5 | 0 |

The last row — *general-purpose chat conversation* — is added intentionally as a competitive factor that the existing market invests in heavily and that we structurally remove. This is essential to the ERRC framework: without naming a factor we Eliminate, there is no Blue Ocean move.

---

## ERRC Framework

Each factor is assigned to exactly one category. Two factors are Eliminated, one is Reduced, four are Raised, and three are Created.

| Factor | Action | Rationale |
|--------|--------|-----------|
| General-purpose chat conversation | **Eliminate** | Adding chat is the exact behaviour that creates the problem (P1 fragmentation). Every workaround in our research that added a chat channel made fragmentation worse — Gega's WhatsApp sub-group (I-02), Cotne's two failed solutions (I-10). We refuse this category outright. |
| Existing user adoption / installed base on KIU campus | **Eliminate** | We cannot compete with Messenger on installed base today. We deliberately do not try; we trade installed base for purpose-built coordination, on the bet that one converted organizer will pull their 15–25 players with them (validated by Mamuka's 25-player group, I-05). |
| Switching cost / user lock-in via long history | **Reduce** | We do not (yet) build deep personalization that creates switching cost through accumulated history. Reduced because at MVP stage we focus on the activation moment (match_joined), not on history accumulation. Switching cost will grow naturally as organizers build participant history; we do not engineer it as a primary feature. |
| Core feature: schedules informal sports matches | **Raise** | Industry average is 2.0 (chat platforms can post a match but don't structure it). We score 5 with a dedicated match object (sport, time, venue, max_players, quorum, RSVPs) that every competitor lacks. |
| Mobile experience quality | **Raise** | Industry average is 3.8 — mature competitors already do this well. We need to be at least at parity (we score 4). Not a differentiator but a hygiene factor; below this we lose users on first launch. |
| Zero extra effort required from the organizer | **Raise** | Industry average is 3.3 (chat platforms feel zero-effort to the organizer because they're already there; that's why the organizer keeps posting there). We score 4 because we add a small step (one post in our app) but eliminate three steps (the re-announcement cycle Mamuka described in I-05). Net effort is lower; perceived effort is the design challenge. |
| Single source of truth for the schedule | **Raise** | Industry average is 2.3. We score 5 because the organizer's post in our app IS the canonical schedule — no parallel chat thread can shift the truth. Nata (I-06) said *"there is no source of truth — everyone has a different version of the information"*; we structurally close that gap. |
| Push notification specifically for match changes | **Create** | Industry average is 1.5 and Discord at 2 is the highest non-bot competitor. We create push notifications scoped to a specific event ("the 6 PM football match moved to 7 PM"), automatically fired on every match edit. No general-purpose chat creates this category. Telegram bots can reach 4 here but have zero KIU adoption. |
| Quorum visibility before leaving home | **Create** | Industry average is 0.7. We create the ability to see "5 of 10 players confirmed" before the player commits the travel time. This directly addresses Nana (I-08), whose volleyball game cancelled when 4 of 8 showed up. No existing competitor — direct or substitute — exposes RSVP state in advance. |
| University-scoped audience (KIU only) | **Create** | Industry average is 1.3. We create a university-domain-gated audience where every match the player sees is from their own campus. Reduces noise, increases relevance, and is the structural reason CampusSport feels different from a generic event app like Meetup. No competitor structurally enforces this. |

**Counts:** Eliminate ×2, Reduce ×1, Raise ×4, Create ×3. Rubric requirement met (≥2 Eliminate/Reduce, ≥1 Create with a genuinely new dimension).

---

## Blue Ocean Narrative

**What we stopped competing on.** We stopped competing on general-purpose chat conversation and on existing installed base. The instinct in our market is to add chat — every workaround our interviewees built (Gega's WhatsApp sub-group I-02, Cotne's Telegram bot I-10) added a chat channel and made the fragmentation worse. We refuse this. CampusSport has no in-app chat at all. We also stopped trying to win on installed base. Messenger has every KIU student already; CampusSport has almost none. Pretending otherwise would not be credible and is exactly the over-claiming the rubric warns against. We trade installed base for purpose-built coordination, on the bet that one converted organizer pulls a 15–25-player group with them — validated directly by Mamuka's group structure (I-05).

**What we introduced.** We introduced three dimensions that no chat-based competitor structurally provides: a push notification scoped to a specific match event, real-time quorum visibility before the player leaves home, and a university-domain-gated audience. These three together convert "is the game still on?" from a question a player has to ask in chat into a fact they already know from one screen. This is the structural change behind Nana's volleyball story (I-08), Giorgi's missed time change (I-03), and Mamuka's 45–60 min/week organizer overhead (I-05).

**Why this combination is right for our specific users.** Our segment is informal peer-organized sports at a single university — small player groups, one organizer per group, weekly cadence, time-sensitive changes within 24–48 hours of kickoff. This is too narrow for a Meta-level platform feature and too transient for a Discord-server setup. It demands a tool that an organizer can adopt in under two minutes and a player can use without learning anything new. Cotne (I-10) said the constraint outright: *"Any solution needs to require zero extra work from the organizer."* Our Blue Ocean position is the only one that respects that constraint while still creating the three dimensions that solve the actual pain.
