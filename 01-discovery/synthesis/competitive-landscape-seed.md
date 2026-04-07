# Competitive Landscape Seed

**Team:** The Merge Conflicters
**Date:** April 7, 2026
**Purpose:** Compile all current solutions, workarounds, and alternatives mentioned by interviewees. This document becomes the foundation for the full competitive analysis in Week 13.

---

## Instructions

During your interviews, users described how they currently handle the problem you are investigating. They mentioned tools, apps, manual processes, workarounds, and "good enough" solutions. This template captures all of those.

For each entry, include: what it is, who mentioned it, what they said about it, and why it falls short.

---

## Direct Competitors

Products or services that explicitly try to solve the same problem.

| Solution | Type | Mentioned By | What Users Said | Strengths | Weaknesses |
|----------|------|-------------|----------------|-----------|------------|
| Facebook Groups | Social platform / group feed | Interviews #1, 2, 3, 4, 5, 6, 7, 8, 9, 10 | "The Facebook post is 'official' but updates only come in Messenger" (I-02, Gega) | Large existing user base; organizer can post announcements to all members at once | Updates get buried in feed noise; no push notification for changes; not all members check regularly |
| Facebook Messenger (group chats) | Messaging app / group chat | Interviews #1, 2, 3, 4, 5, 6, 7, 8, 9, 10 | "The organizer posts updates but they get buried under 40 unrelated messages" (I-01, Khatia) | Widely adopted; supports media and links; familiar interface | Critical updates compete with casual conversation; no way to distinguish a cancellation from a meme; high message volume causes important info to be missed |
| WhatsApp (group chats) | Messaging app / group chat | Interviews #2, 6, 7 | "There's a Messenger group, a Facebook group, and now a WhatsApp sub-group — three places, one game" (I-02, Gega) | Popular among students; reliable delivery notifications | Added a third fragmented channel instead of solving fragmentation; same structural problem as Messenger — no event-specific prioritization |
| Telegram (bots / groups) | Messaging app with bot support | Interview #10 | "Two solutions I built failed because I couldn't control where the organizer puts information" (I-10, Cotne) | Supports custom bots and automation; cleaner API than Messenger | Requires organizer to migrate to a new platform; adoption barrier killed the solution even when the bot functioned correctly |

---

## Indirect Competitors

Products or services that solve a related problem or serve the same need differently.

| Solution | Type | Mentioned By | What Users Said | How It Relates |
|----------|------|-------------|----------------|---------------|
| Google Sheets / spreadsheets | Collaborative document tool | Interview #10 | "I built a spreadsheet to track the schedule — the organizer stopped updating it after two weeks" (I-10, Cotne) | Partially addresses schedule visibility — participants can see the current schedule in one place, but requires the organizer to keep it updated manually and participants to check it proactively; no push notifications |
| Direct DMs / personal texting | Manual one-to-one messaging | Interviews #1, 5, 7 | "I text my teammate to get the 'real' information because group chats aren't trustworthy" (I-01, Khatia); "Just ping me directly — game time, location, done. Why is that so hard?" (I-07, Misho) | Partially addresses reliability — a direct message is harder to miss than a group post — but does not scale; the organizer cannot DM every participant individually for every update without unsustainable effort |
| Screenshot archiving (offline reference) | Manual workaround using phone camera | Interviews #4, 6 | "I screenshot announcements to have them offline but screenshots become wrong the moment something changes" (I-06, Nata) | Partially addresses the need for a portable, accessible record of game details — but immediately goes stale when any detail changes; no mechanism for update propagation |

---

## Manual Workarounds

Things people do by hand because no good solution exists. These are often the strongest signals of unmet demand.

| Workaround | Mentioned By | What They Do | Time/Effort Cost | Why They Tolerate It |
|-----------|-------------|-------------|-----------------|---------------------|
| Multi-platform re-announcement cycle | Interviews #5, 1, 6 | Organizer posts the same announcement on Facebook group, Messenger chat, and WhatsApp, then DMs core players individually to confirm coverage | 45–60 min/week for a 60-min game | "I spend more time communicating about the game than actually playing it. But if I stop, people miss games." (I-05, Mamuka) |
| Checking multiple chats before every game | Interviews #2, 4, 6 | Participants manually open Facebook, Messenger, and WhatsApp before each game to cross-reference which platform has the latest information | Estimated 10–20 min per game cycle | "I have 12 unread sports chats and still miss things. There's no better option." (I-04, Nia) |
| Screenshotting announcements for offline reference | Interviews #4, 6 | Participants screenshot the game details post immediately after it's published as a personal record | Minimal effort per screenshot, but screenshot goes stale instantly on any change | "I take a screenshot so I have the info even without internet — but then I showed up at the wrong place because the venue changed." (I-06, Nata) |
| Creating a sub-group to filter noise | Interview #2 | Gega created a separate WhatsApp group with only serious players to reduce casual conversation | Time to set up and manage membership; created a third fragmented channel | "I thought a smaller group would be cleaner. Now I have three places to check instead of two." (I-02, Gega) |
| Muting the group chat | Interview #3 | Giorgi muted the high-volume group chat to stop notification fatigue | Zero effort — but caused a missed game | "I muted the group because it was too noisy. Then I missed the time change. You can't win either way." (I-03, Giorgi) |
| Texting the organizer directly | Interviews #1, 7 | Participants bypass the group and DM the organizer directly to get confirmed, up-to-date information | Low effort per message; high effort for the organizer at scale | "I just text Sandro directly. He always knows. But I'm sure everyone does this and it drives him crazy." (I-01, Khatia) |

---

## "Do Nothing" Behavior

Users who have given up trying to solve the problem. Why did they stop trying?

| Behavior | Mentioned By | Why They Gave Up |
|----------|-------------|-----------------|
| Stopped inviting friends to games | Interview #1 | "I stopped inviting friends because I'm embarrassed when things change after I've invited them. It happened twice." (I-01, Khatia) |
| Skipping games rather than showing up stressed | Interview #2 | "I would rather skip a game than show up stressed and not knowing what's happening." (I-02, Gega) |
| Abandoned self-built coordination tools | Interview #10 | "I built a Telegram bot that worked. The organizer didn't use it. I wasted two weekends. I won't try again unless the organizer commits first." (I-10, Cotne) |
| Considered quitting as organizer | Interview #5 | "I've considered quitting as organizer. The football is great. The admin is not." (I-05, Mamuka) |
| Entire sports communities dissolved | Interview #8 | "I've seen two sports groups completely collapse because logistics kept failing. People just stopped showing up." (I-08, Nana) |

---

## Key Takeaways

1. **Most common current solution:** Facebook Messenger group chats — mentioned in all 10 interviews as the primary coordination channel, almost always combined with Facebook Groups and, in several cases, a WhatsApp group as well.

2. **Biggest gap in existing solutions:** No existing tool can distinguish a time-sensitive event update from a casual message. All current solutions treat a game cancellation the same as a meme — buried in the same feed, with no priority, no push mechanism, and no single source of truth.

3. **Strongest workaround signal:** The organizer's multi-platform re-announcement cycle (Interview #5, Mamuka) — spending 45–60 minutes per week on logistics for a 60-minute game, across Facebook, Messenger, WhatsApp, and individual DMs, with no guarantee of full coverage. The fact that a dedicated organizer tolerates a 1:1 admin-to-play ratio rather than quit entirely is the clearest signal that no acceptable alternative exists.

4. **Your opportunity:** A lightweight, push-based coordination tool where the organizer posts once and all participants receive a direct notification automatically. The tool must require near-zero extra effort from the organizer (or adoption will fail, as Cotne's Telegram bot proved), deliver updates via push rather than pull (or participants will miss them, as Giorgi's mute strategy proved), and serve as a single source of truth rather than another channel in the fragmented stack.

---

## Notes for Week 13

This is a seed document. In Week 13, you will expand this into a full competitive analysis with 5+ competitors, Porter's Five Forces, moat identification, and counter-strategies. Save this document; you will build on it directly.

**File location:** `/01-discovery/synthesis/competitive-landscape-seed.md`
