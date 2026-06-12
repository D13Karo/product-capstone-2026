# Sprint 1 Plan

**Team:** TheMergeConflicters  
**Product:** CampusSport  
**Sprint:** 1 of 4  
**Dates:** April 24 to May 7 2026  
**Product Owner:** Davit Karoiani  
**Scrum Master:** Mariam Tskhomelidze  
**Version:** 1.0

---

## Sprint Goal

A KIU student can sign up, browse upcoming sports matches, join a game with one tap, and see a confirmation screen — fully deployed and accessible via a public URL.

---

## Sprint Ceremonies

| Ceremony | When | Where | Who Facilitates |
|----------|------|-------|----------------|
| Sprint Planning | Lab 8, Apr 24/25 | In person | Scrum Master (Mariam T.) |
| Daily Standup | 10:00 every day | Messenger group chat | Async — each member posts |
| Sprint Review | Week 10, May 7 (Google Meet) | Google Meet | Product Owner (Davit) |
| Retrospective | May 7 after Sprint Review | Google Meet | Scrum Master (Mariam T.) |

**Async standup format:**
```
Yesterday: [what I completed — include story ID]
Today: [what I am working on — include story ID]
Blocker: [anything stopping me — or "none"]
AI note: [what AI generated yesterday and whether it was accepted/modified/discarded]
```

**Blocker escalation:** If a blocker is not resolved within 24 hours, Mariam T. pings the full team in the Messenger group with the story ID and specific blocker description.

---

## Definition of Done

A story is Done when all of the following are true:

- [ ] Code reviewed by at least one other team member (not the original author)
- [ ] Pull request merged to `main` via GitHub PR — no direct pushes to main
- [ ] Acceptance criteria confirmed as met by the Product Owner (Davit) — not by the developer who built it
- [ ] If AI-generated: code annotated with inline comments explaining the logic in the reviewer's own words
- [ ] If AI-generated: entry added to `docs/ai-usage-log.md` (tool, task, files changed, review notes)
- [ ] Feature works in the deployed Vercel environment, not just locally
- [ ] No known bugs introduced into the main branch

A story that is "mostly done" is not Done. It stays In Review.

---

## Calibration Anchors

Points represent the full cycle: AI generation + developer review + AC verification + edge case handling + deployment. AI generates the scaffold quickly — the points account for the review, fix, and test time that follows, not just generation.

| Points | What It Looks Like for Our Team | Approx. Hours (incl. AI review) |
|--------|--------------------------------|---------------------------------|
| 1 | A single, obvious UI change or label update. One file. No logic. AI generates in minutes; review takes under 30 min. Example: change button text from "Submit" to "Join Match". | ~1–2 hrs |
| 3 | A complete screen with routing, state, basic validation, and empty-state handling. AI generates the scaffold; review and AC verification take 2–4 hrs. Example: match detail screen fetching live data. | ~6–8 hrs |
| 5 | A feature requiring multiple components, a backend endpoint, a third-party integration, and non-trivial edge cases. AI generates parts; integration and review dominate. Example: push notification trigger on match update. | ~14–18 hrs |
| 8 | A complex feature with significant uncertainty about the full implementation path. If a story reaches 8, split it before committing to the sprint. | 20+ hrs — split first |

---

## Sprint 1 Backlog

### Story S1-01

**User Story:**  
As a new KIU student, I want to sign up with my email and log in so that I can access the sports coordination platform and have my RSVP history tied to my identity.

**Interview Evidence:**  
Source: Interview #01 (Khatia, KIU football participant, March 2026) — "I text my teammate to get the real information because group chats aren't trustworthy." A verified account creates the trusted identity layer that makes a single source of truth possible.  
Source: Interview #10 (Cotne, builder/player, March 2026) — "Any solution needs to require zero extra work from the organizer" — auth must be frictionless so neither side abandons onboarding.

**Story Points:** 3  
**Assignee:** Davit Karoiani  
**AI Tool:** Claude Code  
**AI Tool Rationale:** AI Studio is best for prototyping the auth flow with a backend (user record creation, session management) before wiring it into the main app.

**Acceptance Criteria:**

```
AC1:
Given I am a new user on the signup page,
When I enter a valid email address and a password of at least 8 characters and submit,
Then my account is created, I am automatically logged in, and I am redirected to the match list home screen.

AC2:
Given I am a registered user on the login page,
When I enter my correct email and password,
Then I am authenticated and redirected to the match list home screen within 2 seconds.

AC3:
Given I enter an email address that is already registered on the signup page,
When I submit the form,
Then the form displays the error "An account with this email already exists" and does not create a duplicate account.

AC4:
Given I am authenticated,
When I reload the app,
Then I remain logged in without being redirected to the login screen (session persists).
```

**Notes:** Do not require KIU domain enforcement in Sprint 1 — generic email auth is acceptable for MVP. KIU SSO can be explored in Sprint 2 if setup is trivial. Password reset flow is out of Sprint 1 scope.

---

### Story S1-02

**User Story:**  
As a registered user, I want to browse upcoming sports matches on a home screen so that I can quickly find a game without checking multiple group chats.

**Interview Evidence:**  
Source: Interview #04 (Nia, KIU student, March 2026) — "I have 12 unread sports chats and still miss things." The home screen replaces all of them with one feed.  
Source: Interview #07 (Misho, low-effort user, March 2026) — "Just ping me directly — game time, location, done. Why is that so hard?" The match card format delivers exactly this.

**Story Points:** 3  
**Assignee:** Mariam Tskhomelidze  
**AI Tool:** Claude Code  
**AI Tool Rationale:** Stitch generates the card-based match list UI from a structured prompt. The core layout was already prototyped in Lab 5 — Mariam T. brings that Stitch output into the codebase and connects it to real data.

**Acceptance Criteria:**

```
AC1:
Given I am authenticated and there is at least one upcoming match in the database,
When I load the home screen,
Then I see a list of match cards each showing: sport type, date, start time, location name, and spots remaining.

AC2:
Given there are multiple upcoming matches,
When I view the home screen,
Then the matches are sorted by soonest start time first.

AC3:
Given there are no upcoming matches in the database,
When I load the home screen,
Then I see an empty state message: "No matches scheduled yet. Check back soon."

AC4:
Given I am authenticated,
When the home screen loads,
Then it renders fully within 3 seconds on a standard mobile connection.
```

**Notes:** Filter by sport type is a Sprint 3 feature — do not build it in Sprint 1. The home screen must be responsive (mobile-first).

---

### Story S1-03

**User Story:**  
As a registered user, I want to view the full details of a match so that I can decide whether to join before committing.

**Interview Evidence:**  
Source: Interview #07 (Misho, March 2026) — "Just ping me directly — game time, location, done." The detail screen is the canonical answer to this request — one screen with everything.  
Source: Interview #03 (Giorgi, March 2026) — "Game moved one hour earlier — posted in chat — I didn't see it." The detail screen is the single source of truth — if it shows the current time, it is the current time.

**Story Points:** 2  
**Assignee:** Mariam Pirtskhalava  
**AI Tool:** Claude Code  
**AI Tool Rationale:** Stitch already produced a match detail screen in the Lab 5 prototype. Mariam P. adapts that output, connects it to the real match record, and verifies all fields render from live data.

**Acceptance Criteria:**

```
AC1:
Given I tap a match card on the home screen,
When the match detail screen loads,
Then I can see: sport type, date, start time, location name, maximum player count, current RSVP count, and a Join Match button.

AC2:
Given a match has zero spots remaining (current RSVPs equal maximum players),
When I view the match detail screen,
Then the Join Match button is disabled and displays "Full" instead of "Join Match".

AC3:
Given I have already joined a match,
When I view that match's detail screen,
Then the Join Match button is replaced with a green "You're in" badge and cannot be tapped again.
```

**Notes:** The detail screen is read-only in Sprint 1. Leave match functionality (S2-04) ships in Sprint 2. Do not build organiser edit controls on this screen yet.

---

### Story S1-04

**User Story:**  
As a registered user, I want to join a match with a single tap and immediately see a confirmation screen so that I know my spot is definitively reserved without any ambiguity.

**Interview Evidence:**  
Source: Interview #04 (Nia, March 2026) — "I want a confirmation, not just an announcement. Tell me the game is confirmed." The confirmation screen directly answers this.  
Source: Interview #06 (Nata, March 2026) — "I screenshot announcements to have them offline but screenshots become wrong the moment something changes." A persistent confirmation in-app replaces the screenshot workaround with a live source.

**Story Points:** 2  
**Assignee:** Levan Kovziridze  
**AI Tool:** Claude Code  
**AI Tool Rationale:** The join action involves backend logic — RSVP record creation, spots-remaining decrement, and the `match_joined` event firing. Claude Code handles multi-file backend logic better than Stitch, which is UI-only.

**Acceptance Criteria:**

```
AC1:
Given I am on a match detail screen for a match with available spots,
When I tap the Join Match button,
Then my RSVP is recorded in the database, the spots remaining decrements by 1, and I am shown a confirmation screen displaying the match name, sport type, start time, and "You're in!" message.

AC2:
Given I tap Join Match and the RSVP is confirmed,
When the confirmation screen renders,
Then the match_joined action is recorded as an RSVP row in our own database with: match_id, sport_type, spots_remaining_after_join, and time_to_match_hours.

AC3:
Given I am on the confirmation screen,
When I tap "Back to matches",
Then I am returned to the home screen and the match card I just joined shows the updated (decremented) spots remaining count.

AC4:
Given the server returns an error when I tap Join Match (e.g. match became full between load and tap),
When the error occurs,
Then I see an inline error message "This match just filled up — try another one" and am not shown a false confirmation screen.
```

**Notes:** This story depends on S1-01 (auth) and S1-03 (detail screen) being merged first. Levan should pull S1-04 only after S1-03 is In Review. The `match_joined` action and the data we record for it are defined in `03-build/analytics/event-schema.md` — use that spec exactly.

---

## Sprint 1 Summary

| Story ID | Summary | Points | Assignee | AI Tool | Status |
|----------|---------|--------|----------|---------|--------|
| S1-01 | User signup and login | 3 | Davit Karoiani | Claude Code | Done |
| S1-02 | Match list home screen | 3 | Mariam Tskhomelidze | Claude Code | Done |
| S1-03 | Match detail view | 2 | Mariam Pirtskhalava | Claude Code | Done |
| S1-04 | Join match + confirmation | 2 | Levan Kovziridze | Claude Code | Done |
| **Total** | | **10** | | | |

**Capacity check:** 10 points committed out of approximately 18 maximum (56% — within the 60% target)

---

## Sprint Review Criteria

At Sprint Review (May 7, Google Meet), the team will demo:

1. New user signs up with a fresh email — account is created, lands on home screen
2. Home screen shows at least 2 real match cards with live data
3. User taps a match card, sees full detail, taps Join Match
4. Confirmation screen appears with correct match details
5. The Django admin (or a DB query) shows the new `match_joined` RSVP row

The demo must be live in the deployed Vercel app. No localhost. No screenshots. No pre-recorded video. Product Owner (Davit) accepts or rejects each story against its AC during the review.

---

## AI Usage Log Reference

All AI-assisted work in Sprint 1 must be logged in `docs/ai-usage-log.md`.

Entry format:
```
---
Date: YYYY-MM-DD
Story: [Story ID] — [Story summary]
Tool: [Google Stitch / Claude Code / Google AI Studio / GitHub Copilot]
Task: [What AI was asked to generate or assist with]
Prompt summary: [Brief description of the prompt used]
Files changed: [List of files the AI output touched]
Result: Accepted / Modified / Discarded
Review notes: [What was checked. What was changed from AI output. Any errors or hallucinations found.]
Reviewer: [Name]
---
```

---

## Change Log

| Date | Changes | Author |
|------|---------|--------|
| 16 April 2026 | Sprint 1 plan created | Davit Karoiani |
| 14 May 2026 | Sprint 1 statuses updated — all stories Done | Davit Karoiani |

---

*Sprint 1 Plan | TheMergeConflicters | CS-PD-2026 | Spring 2026*
