# Growth Strategy

**Team:** TheMergeConflicters
**Product:** KIU Sports Tracker — a push-based match coordination tool that notifies KIU students when match times or locations change, replacing fragmented group chat announcements with one reliable source of truth.
**Document version:** 1.0
**Last updated:** 13 May 2026

---

## 1. Target User (Reference)

Pulled from CP1 problem statement. KIU university students who participate in informal sports matches on campus and consistently miss or show up to wrong matches because schedule updates are buried in multiple, overlapping Messenger and WhatsApp group chats. Pain intensity averaged 4.0–4.1 out of 5 across all 10 interviews.

---

## 2. Activation Metric

**Aha moment:** A KIU student opens the app, sees a live match they want to join, and taps "Join Match" — receiving an immediate in-app confirmation that their spot is reserved.

**Activation metric:** 60% of new signups complete a `match_joined` event within 10 minutes of creating their account.

**Why this is the aha moment:** From our 10 user interviews, the single most cited frustration was uncertainty — not knowing whether their RSVP was real. Interview #04 (Nia): "I want a confirmation, not just an announcement. Tell me the game is confirmed." The moment a student sees "You're in!" with a match they actually care about, the product has replaced the anxiety of group chat coordination with a concrete, reliable signal. A user who never joins a match has never experienced the product's core promise.

---

## 3. Three Acquisition Channels

### Channel 1 -- Direct outreach to KIU sports match organisers

**Type:** Sales (founder-led)

**Why this channel:**
- **Fit:** Organisers are the supply side of the product. No matches posted = no players to acquire. Our interviews identified 2–3 active organisers (Interview #05 Mamuka, Interview #01 Khatia) who each run recurring games with 15–30 regular participants. Converting one organiser to post their match on the app immediately activates their entire player group. This is the highest-leverage unit of acquisition available to us.
- **Speed:** Can start today. Davit and Levan personally know at least 3 active organisers from campus sports. First outreach message takes 15 minutes.
- **Cost:** Founder time only. Approximately 3 hours per week for two weeks of outreach and follow-up. No ad spend. Internal opportunity cost valued at $5/hour.

**What we will do in Sprint 2:**
1. Identify 8–10 active match organisers at KIU across football, basketball, volleyball, and tennis.
2. Reach out via Messenger individually — not in group chats — with a personalised message showing how posting their match takes less than 2 minutes.
3. Offer to post their first match for them ("I'll set it up, you just confirm the details").
4. Track which signups originate from organiser-led activation using a UTM source `organiser_outreach`.
5. After one match is posted, ask each organiser to share the match link in their existing player group.

**What success looks like in 4 weeks:** 5 active organisers each posting at least 1 match per week. Each organiser's player group generates 15–25 player signups. Total estimated: 75–125 player signups via this channel.

---

### Channel 2 -- KIU sports group chats (Messenger and WhatsApp)

**Type:** Organic

**Why this channel:**
- **Fit:** Every single interviewee (10 out of 10) coordinates sports via Messenger or WhatsApp group chats. This is exactly where our target user is already looking for match information. Sharing a KIU Sports Tracker match link in the group chat replaces the announcement they would have received anyway — with a link to join instead of a wall of text to parse.
- **Speed:** Immediate. Our smoke test experiment (launched May 13) is already running in sports group chats and producing conversion signal. The infrastructure for this channel is already active.
- **Cost:** Founder time only. Approximately 2 hours per week to post in groups, respond to questions, and track link performance. No tooling cost beyond existing accounts.

**What we will do in Sprint 2:**
1. Post match announcements in 5–8 KIU sports-specific group chats using a UTM-tracked link.
2. Message group admins requesting permission to share match coordination links regularly (not spam).
3. When a match from the app is posted in the group, reply to confusion or questions in-thread to reduce friction.
4. Monitor the smoke test Google Form signups by referral source to validate which groups convert best.
5. Double down on the 2–3 highest-converting groups; deprioritise others.

**What success looks like in 4 weeks:** 50–80 signups from group chat links. Visit-to-signup conversion rate of 20%+ (higher than typical organic because these users are already motivated — they want to know about matches).

---

### Channel 3 -- QR code posters at KIU sports facilities

**Type:** Paid (minimal budget) / Organic

**Why this channel:**
- **Fit:** A student standing at the KIU football pitch waiting for a match to start is the highest-intent user imaginable. They are physically present at the exact moment of need. A poster at the facility entrance with one sentence and a QR code catches them in context, not in a random browse session.
- **Speed:** Print today, post tomorrow. Design takes 30 minutes. Print at the KIU campus print shop.
- **Cost:** Approximately $20 for 20 A4 colour posters at the campus print shop. Plus 1 hour of founder time to put them up and 30 minutes per week to refresh. Total under $45 over 4 weeks.

**What we will do in Sprint 2:**
1. Design a minimal poster: KIU Sports Tracker logo, one sentence ("Never miss a match update again"), large QR code, link text below the code.
2. Print 20 copies.
3. Post at: KIU football pitch entrance, basketball court area, volleyball court, main sports hall entrance, student cafeteria near the sports facilities.
4. Use a unique UTM parameter in the QR code URL to track scans separately from digital channels.
5. Refresh posters weekly — damaged or removed posters are replaced.

**What success looks like in 4 weeks:** 15–25 scans per week across all poster locations. 50%+ scan-to-signup conversion (high intent users at the point of need). 30–40 total signups via this channel.

---

## 4. Channel Ranking and Rationale

| Rank | Channel | One-sentence rationale |
|------|---------|------------------------|
| 1 | Organiser outreach | One converted organiser activates 15–25 players automatically, making this the highest-leverage unit of acquisition |
| 2 | Sports group chats | Direct access to the entire target audience at zero cost; already running and generating signal |
| 3 | QR code posters at facilities | Lowest reach of the three but highest activation intent; worth $45 for the high-quality signups it generates |

Channel 1 is ranked first not because of volume but because of mechanism: organisers create supply (matches), and without supply the product is empty. Channels 2 and 3 bring demand (players) but those players churn immediately if they open the app and see no matches. Organiser acquisition is a prerequisite for player acquisition to stick.

---

## 5. Channels We Considered and Rejected

**Paid social media ads (Meta / Instagram)** — Rejected. Our addressable market is KIU students who play sports, which is a narrow demographic inside one university. Paid social targeting cannot reliably isolate this group without a significant budget and experimentation runway we do not have. The organic channels listed above reach the same users at near-zero cost.

**General KIU university social media pages** — Rejected. KIU's official accounts and general student pages reach a broad student audience, the majority of whom do not play informal sports. Posts in general channels attract noise and low-quality signups from users who will never activate. Interview #04 (Nia) explicitly said "I have 12 unread sports chats already" — adding another general announcement channel recreates the problem we are trying to solve.

**Word of mouth referral programme** — Deferred, not rejected. Without enough activated users yet, a formal referral programme has no base to compound from. We will revisit in Sprint 3 once we have 100+ activated users and the share/invite feature (S3-03) is live.

---

## 6. Open Questions

What we do not yet know that we need to answer in Sprint 2:

1. What is the real conversion rate from group chat link shares? Our smoke test (May 13–21) uses a landing page, not the live product. Conversion from a real match link shared in a group may be meaningfully different.
2. Will organisers adopt the app independently, or do they require a concierge-style onboarding where a team member posts their first match for them? The answer determines how much founder time Channel 1 requires at scale.
3. At what point does the QR poster channel saturate? KIU sports facilities are a fixed, finite environment — there is a ceiling on poster reach that we have not measured yet.

---

**Filed by:** Davit Karoiani, Mariam Tskhomelidze, Mariam Pirtskhalava, Levan Kovziridze
