# North Star Metric

**Team:** TheMergeConflicters  
**Product:** CampusSport  
**Date:** 09 April 2026  
**Version:** 1.0

---

## Our North Star Metric

```
[Number] of [action] per [user/team] per [time period]
```

**Written out:**

> Weekly informal sports matches joined per active user

---

## Why This Metric

**Question 1: What is the core action a user takes that proves they got value from our product?**

Our core value proposition is eliminating the chaos of coordinating sports via Messenger and Facebook groups. The moment a user gets actual value from our app is when they successfully lock in their spot to play. Therefore, the core action is joining (RSVPing to) an informal sports match.

**Question 2: Can we measure it? Is it a discrete, countable event?**

Yes, it is a highly discrete and countable event. Every time a user taps the "Join" button on a match, it will trigger a specific event in our event schema called `match_joined`. We can count how many of these events occur per user over a 7-day period.

**Question 3: Does it change when our product gets better or worse?**

Yes. If we ship a bad release where the schedule UI is confusing or the match creation flow breaks, students won't be able to find or join games, and this metric will drop instantly. Conversely, if we improve the app (like adding smart push notifications for games they like), the number of matches joined per user will increase.

---

## What Our NSM Is Not

| Alternative Metric | Why We Rejected It |
|-------------------|--------------------|
| Total Signups | Measures acquisition, not value. A student can download the app and sign up, but if they never actually play a sport, we haven't solved their problem. |
| Match Details Viewed | This is an activity metric, not an outcome metric. Scrolling through games is nice, but the value is in actually committing to play. |
| Messages Sent | We actively want to *reduce* chat clutter. If this number goes up, it might mean our UI is failing to clearly communicate match details (time, location, players). |

---

## Connection to AARRR

- [ ] Acquisition
- [x] Activation (most NSMs live here)
- [ ] Retention
- [ ] Referral
- [ ] Revenue

**Stage:** Activation

**Why:** The "aha" moment for our user is the first time they seamlessly find and secure their spot in a game without having to read through 50 disorganized Messenger texts.

---

## Connection to Prototype

**Screen name:** Match Details & RSVP Screen

**What the user does on that screen:** Taps the "Join Match" button.

**Event that fires:** `match_joined`

**How that event feeds the NSM:** Each `match_joined` event increments the weekly match participation count for that specific `user_id`.

---

## Team Sign-Off

All team members reviewed and agreed on this NSM:

| Name | Role | Agreement |
|------|------|-----------|
| Davit Karoiani | Program Lead | ☑ Agreed |
| Mariam Pirtskhalava | Discovery Lead | ☑ Agreed |
| Mariam Tskhomelidze | Tech Lead | ☑ Agreed |
| Levan Kovziridze | Test Lead | ☑ Agreed |

**Date agreed:** 09 April 2026

---

## Change Log

| Date | Change | Reason |
|------|--------|--------|
| 09 April 2026 | Initial definition | Lab 5 |

---

*North Star Metric | TheMergeConflicters | CS-PD-2026 | Spring 2026*
