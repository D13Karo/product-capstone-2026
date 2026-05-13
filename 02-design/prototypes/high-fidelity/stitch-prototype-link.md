# High-Fidelity Prototype: Stitch

**Team:** TheMergeConflicters  
**Product:** KIU Sports Tracker  
**Tool:** Google Stitch (https://stitch.withgoogle.com)  
**Created:** 09 April 2026  
**Status:** Draft (Lab 5) / Final (by April 23)

---

## Prototype Link

**Stitch shareable link:**  
https://stitch.withgoogle.com/projects/5804034519847663567

**Tested in incognito window:** ☑ Yes ☐ No (must be Yes before submission)

---

## What This Prototype Covers

**Core user flow prototyped:**  
A KIU student browses available informal sports matches, views match details, and joins a game with one tap to confirm their spot.

**Screens included:**

| Screen | Purpose | Activation Event Fired |
|--------|---------|----------------------|
| Home / Match List | User browses all upcoming informal matches filtered by sport type | None |
| Match Details & RSVP | User sees match info (sport, time, location, spots left) and taps Join | None |
| Join Confirmation | System confirms the user's spot is reserved | `match_joined` |

**Activation moment screen:** Join Confirmation  
**What the user does at activation:** Taps the "Join Match" button on the Match Details screen  
**NSM connection:** Each confirmed join fires `match_joined`, which increments the weekly match participation count per `user_id` — the team's North Star Metric.

---

## Stitch Brief Used

```
Product name: KIU Sports Tracker

Primary user: A KIU university student who wants to find and join
informal sports matches on campus without the chaos of Messenger
group chats.

Most important flow: Browse available matches → View match details
→ Confirm joining the match

Screens required:
1. Home / Match List — shows upcoming matches with sport icon,
   time, location, and spots remaining badge
2. Match Details & RSVP — shows full match info and a prominent
   "Join Match" button
3. Join Confirmation — confirms the user's spot with match name,
   time, and a success message

Activation moment: User taps "Join Match" and sees confirmation
that their spot is reserved.

Visual style: Clean, mobile-first, sport-inspired with green and
white colour scheme. Card-based layout for match listings.
```

---

## Key Prompts Used

**Initial prompt:**
```
Build a mobile-first web app called KIU Sports Tracker. The primary
user is a university student who wants to find and join informal
sports matches on campus without having to scroll through Messenger
group chats. Screen 1: a home screen showing a list of upcoming
matches as cards, each with a sport type icon, match time, location
name, number of spots remaining, and a View button. Screen 2: a
match details screen showing all match info and a large green "Join
Match" button. Screen 3: a confirmation screen showing "You're in!"
with the match name, time, and a button to view all joined matches.
Use a clean, minimal design with a green and white colour scheme.
```

**Iteration prompts (if any):**
```
Add a sport-type filter row at the top of the home screen with
pill buttons for All, Football, Basketball, Tennis, and Volleyball.
Make the active filter highlighted in green.
```

---

## Design Decisions

**Decision 1:**  
Prominent "spots remaining" badge on every match card, because interviews revealed that students' biggest frustration was showing up to a match that was already full — they need to see availability at a glance before investing time clicking through.

**Decision 2:**  
Single large "Join Match" CTA with no intermediate steps, because interviews showed that the friction of multi-step RSVP flows (as in WhatsApp threads) caused users to delay and ultimately miss their window to join.

**Decision 3:**  
Explicit confirmation screen ("You're in!") with match details, because users reported anxiety about whether their RSVP was actually registered when coordinating over chat. A clear system confirmation eliminates that uncertainty.

---

## What Lab 6 Will Add

This prototype is the design blueprint. Lab 6 adds:

- Backend logic (user authentication, data storage)
- Event schema instrumentation (actual tracking code firing `match_joined` and other events)
- Real data persistence (RSVPs and match records actually save)
- Vercel deployment (public URL for real user testing)

**Live app URL (completed after Lab 6):**  
[Paste Vercel deployment URL here after Lab 6]

---

## Export (if available)

**Export format:** HTML/CSS/JS  
**Export file location in repo:** `02-design/prototypes/high-fidelity/stitch-export/` (to be added after Lab 6)

---

*Stitch Prototype | TheMergeConflicters | CS-PD-2026 | Spring 2026*
