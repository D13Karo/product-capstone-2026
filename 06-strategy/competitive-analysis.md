# Competitive Analysis

**Team:** TheMergeConflicters
**Product:** CampusSport — push-based informal sports match coordination for KIU students
**Date:** 10 June 2026
**Version:** 1.0 — Lab 11 submission

---

## Source Document

This analysis builds on the competitive landscape seed compiled in Lab 4:
`01-discovery/synthesis/competitive-landscape-seed.md`

The seed document recorded what real KIU students use today, captured across 10 customer discovery interviews in Weeks 3–4 (March–April 2026). The most frequently mentioned alternatives were: Facebook Messenger group chats (mentioned by 10 of 10 interviewees), Facebook Groups (10 of 10), WhatsApp group chats (3 of 10), a self-built Telegram bot that failed at the organizer adoption layer (Cotne, I-10), and a self-built Google Sheets schedule that the organizer stopped updating after two weeks (Cotne, I-10). Two interviewees described "do nothing" workarounds: muting the chat and stopping to invite friends rather than fixing the coordination problem (Giorgi, I-03; Khatia, I-01).

---

## Competitor Matrix

**Scoring guide:**
- **5** = Excellent. Genuine strength.
- **4** = Good. Above the market average.
- **3** = Adequate. Meets the minimum expected standard.
- **2** = Weak. Present but poorly executed.
- **1** = Minimal. Barely present.
- **0** = Absent. Dimension does not exist in this product.

| Dimension | Facebook Messenger | Facebook Groups | WhatsApp | Discord | Telegram Bots (Cotne's failed bot) | Google Sheets | CampusSport |
|-----------|--------------------|-----------------|----------|---------|------------------------------------|---------------|-------------|
| Core feature: schedules informal sports matches | 2 | 2 | 2 | 2 | 3 | 1 | 5 |
| Push notification specifically for match changes | 1 | 1 | 1 | 2 | 4 | 0 | 5 |
| Single source of truth for the schedule | 1 | 2 | 1 | 3 | 3 | 4 | 5 |
| Zero extra effort required from the organizer | 5 | 5 | 5 | 2 | 1 | 2 | 4 |
| Quorum visibility (will the game actually happen?) | 0 | 0 | 0 | 1 | 1 | 2 | 4 |
| University-scoped audience (KIU only) | 1 | 2 | 1 | 2 | 1 | 1 | 5 |
| Mobile experience quality | 5 | 4 | 5 | 4 | 3 | 2 | 4 |
| Existing user adoption on KIU campus | 5 | 5 | 4 | 2 | 0 | 1 | 1 |
| Switching cost / user lock-in | 4 | 4 | 4 | 3 | 0 | 1 | 2 |
| Zero cost to user | 5 | 5 | 5 | 5 | 5 | 5 | 5 |

**Note on the "Existing user adoption on KIU campus" row:** CampusSport scoring 1 here is correct and intentional. We are pre-launch. Scoring this honestly is critical to the rubric — an entry of 5 would not be credible. This row is the single biggest force pulling against us and we name it openly.

---

## Competitor Profiles

### Facebook Messenger (group chats)

**Type:** Indirect competitor — fills the coordination gap by default but is not designed for it.
**Description:** The general-purpose group chat that every KIU sports group uses as their primary coordination channel. Treats a match cancellation and a meme as identical message types.
**Primary strengths:** Universal adoption among KIU students. Excellent mobile UX. Zero onboarding friction. Familiar.
**Primary weaknesses:** No event-specific prioritization. Critical updates compete with casual conversation. No push distinction between "the game moved" and "haha". Information buries within hours.
**Why users choose them:** "The organizer posts updates but they get buried under 40 unrelated messages" (Khatia, I-01). They don't choose Messenger — they're trapped in it because the organizer chose it.

---

### Facebook Groups

**Type:** Indirect competitor — used as the official announcement channel.
**Description:** The "official" feed where new matches are announced. Gega (I-02) said the Facebook post is treated as the announcement of record, but updates only come in Messenger. Two-platform fragmentation by design.
**Primary strengths:** Searchable, persistent posts. Larger audience reach than a chat. Implicit "official" status.
**Primary weaknesses:** No push notifications for replies or edits. Algorithmic feed buries time-sensitive updates. Users don't check it daily. Disconnected from the update channel (Messenger).
**Why users choose them:** Inherited by default from the organizer. Mentioned in 10/10 interviews as the announcement-of-record platform paired with Messenger.

---

### WhatsApp (group chats)

**Type:** Indirect competitor — emerged as a workaround that made fragmentation worse.
**Description:** A third channel that some organizers and players adopt to escape Messenger noise. Almost always adds to the platform stack instead of replacing it.
**Primary strengths:** Reliable delivery, end-to-end encryption, broad mobile adoption.
**Primary weaknesses:** Same structural problem as Messenger — no event-specific prioritization. Adding it makes the player check three apps instead of two.
**Why users choose them:** "There's a Messenger group, a Facebook group, and now a WhatsApp sub-group — three places, one game" (Gega, I-02). The workaround that became its own problem.

---

### Discord

**Type:** Direct competitor — purpose-built for community coordination, has event scheduling features.
**Description:** Has scheduled events, role-based notifications, and a clear separation between announcement channels and chat channels. The only competitor that structurally solves the "important vs casual" message problem.
**Primary strengths:** Distinct announcement channels. Event RSVPs with native UI. Strong on PC, acceptable on mobile. Free.
**Primary weaknesses:** Requires the organizer to set up and moderate a server (this is the killer — it's significant work). Almost no adoption among KIU informal sports groups (0/10 interviews mentioned Discord as a current solution). The organizer-side setup friction is exactly the barrier our research shows kills adoption.
**Why users choose them:** Almost no one in our target segment does. Discord adoption is concentrated in gaming and tech communities, not pickup-football organizers at KIU.

---

### Telegram bots (Cotne's failed bot, Interview #10)

**Type:** Direct competitor — same structural solution, real-world failure case in our market.
**Description:** Cotne (I-10) built a Telegram bot for his football group that handled scheduling and announcements. Technically functional. Failed because the organizer wouldn't migrate from Messenger.
**Primary strengths:** Custom logic via bots, clean API, push notifications work, no platform fees.
**Primary weaknesses:** Requires the organizer to switch platforms. In our market, the organizer almost never switches. The bot's technical correctness was irrelevant.
**Why users choose them:** They don't. Cotne's bot is the documented evidence that participant-built solutions fail when they depend on organizer migration. *"Two solutions I built failed because I couldn't control where the organizer puts information"* (Cotne, I-10).

---

### Google Sheets (Cotne's spreadsheet, also Interview #10)

**Type:** Substitute — a manual workaround using a general-purpose tool.
**Description:** A shared spreadsheet with the match schedule. The organizer maintained it for two weeks then stopped.
**Primary strengths:** A real single source of truth while it's maintained. Free. Zero learning curve. Visible to everyone.
**Primary weaknesses:** No push notifications — entirely pull-based. Organizer maintenance is unsustainable. Goes stale silently. No quorum or RSVP capability.
**Why users choose them:** A small minority of technical users build this themselves; it always decays. Cotne abandoned his sheet after the organizer stopped updating.

---

## Synthesis

The greatest Porter force threat to CampusSport is **the bargaining power of suppliers**, where the "supplier" in our two-sided market is the match organizer. We have one supply-side actor per sports group, and the entire downstream value of the product collapses if that organizer chooses not to post their match in our app. Every workaround documented in our seed broke at this exact point. Cotne (I-10) built a working Telegram bot and his football organizer wouldn't switch; he then built a Google Sheet and the organizer stopped updating it after two weeks. Gega (I-02) tried to create a WhatsApp sub-group to fix fragmentation and the organizer kept posting in the original channels. The organizer is a near-monopoly supplier of match information for their group, they cost us nothing to convert, but they cost us everything if they refuse. The threat of substitutes (sticking with Messenger) is real but secondary: substitutes win when supply is unreliable, and substitutes lose the moment a single organizer adopts our tool and their player group stops getting late updates.

The two dimensions where we create the most defensible gap are **push notification specifically for match changes** (we score 5, every general-purpose chat scores 1, Discord scores 2, Telegram bots score 4 but have no organizer adoption) and **quorum visibility** (we score 4, every competitor scores 0–2). Push for match changes matters to our segment because the failure mode every interviewee described — missing a game because the time changed at 11pm and was buried in chat — is solved structurally, not behaviorally, by sending the player a notification they did not have to ask for. Quorum visibility matters because Nana's (I-08) volleyball game cancelled when 4 of 8 players showed up — wasting four people's afternoon — is a problem no chat-based competitor can solve because none of them know who is actually planning to attend. Closing the quorum gap requires every match's RSVP state to be visible in one place, which structurally cannot happen on a chat platform.

For this gap to close, two things would have to happen simultaneously: Meta would have to add event-level RSVP-and-push primitives to Messenger or WhatsApp at a level of granularity they currently don't sell at (Messenger's existing event feature is a calendar entry, not an RSVP loop with quorum thresholds), **and** Meta would have to do this specifically for informal university-segmented audiences rather than as a general feature. Both are individually possible. The combination is unlikely on a 12–24 month horizon because Meta's revenue strategy does not direct engineering attention at this granular audience. The more realistic threat is institutional: a university or its IT department could build their own version. We address that threat directly in `ecosystem-map.md` § Threats.
