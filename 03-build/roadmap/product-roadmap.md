# Product Roadmap

**Team:** TheMergeConflicters  
**Product:** CampusSport  
**Date:** 16 April 2026  
**Version:** 1.0  
**Sprint Arc:** April 24 to June 11 2026 (4 sprints, 8 weeks)

---

## MVP Scope

### What We Are Building

CampusSport is a lightweight, push-based sports coordination tool for KIU students. The organiser posts a match once — sport type, time, location, player limit — and all registered participants receive a direct notification. When anything changes, the notification goes out automatically. There is no re-announcement cycle, no multi-platform duplication, and no information buried in general chat noise. The product directly addresses the validated problem: KIU students consistently miss game time or location changes because schedule updates get buried in general-purpose group chats spread across multiple platforms with no single source of truth (10/10 interviews, average pain intensity 4.0–4.1/5).

### North Star Metric

> Weekly informal sports matches joined per active user

### In Scope (Sprints 1 to 4)

| Feature | Sprint | Interview Evidence |
|---------|--------|--------------------|
| User signup and login (KIU email) | Sprint 1 | Interview #01 (Khatia) — "I text my teammate to get the real information because group chats aren't trustworthy"; a verified campus identity resolves this |
| Match list home screen (browse upcoming matches) | Sprint 1 | Interview #04 (Nia) — "I have 12 unread sports chats and still miss things"; a single feed replaces all of them |
| Match detail screen (sport, time, location, spots) | Sprint 1 | Interview #07 (Misho) — "Just ping me directly — game time, location, done. Why is that so hard?" |
| Join match / RSVP with one tap | Sprint 1 | Interview #03 (Giorgi) — "Game moved one hour earlier — posted in chat — I didn't see it — team played without me"; fast RSVP prevents the hesitation that leads to missed joins |
| Match creation flow for organisers | Sprint 2 | Interview #05 (Mamuka) — "Post on Facebook → repost in Messenger → DM core players. Three steps, every time"; one-post creation removes this |
| Push notifications for match changes | Sprint 2 | Interview #01 (Khatia) — "The delay between posting and seeing can be hours. By then the window to respond is gone." |
| Usage tracking via Django admin (NSM measurement) | Sprint 2 | Required to measure NSM — match joins per active user per week |
| Leave match / cancel RSVP | Sprint 2 | Interview #08 (Nana) — "Four people showed up to volleyball — needed 8 — had to cancel"; RSVP cancellation feeds quorum visibility |
| Quorum visibility (RSVP count vs spots needed) | Sprint 3 | Interview #08 (Nana) — "I want a confirmation the game is happening before I leave home" |
| Organiser cancel match with auto-notification | Sprint 3 | Interview #05 (Mamuka) — "When something changes, I repeat the whole announcement cycle — it never gets shorter" |
| Share / invite match link | Sprint 3 | Interview #09 (Tiko) — "If someone built a proper system, I would switch immediately and make everyone else switch too"; sharing drives referral growth |
| Match result logging | Sprint 3 | Interview #09 (Tiko) — "I had mentally prepared for one opponent. The schedule changed." — post-match records build retention |
| Sport-type filter on home screen | Sprint 3 | Interview #04 (Nia) — broad match list without filters adds cognitive overhead |
| Performance optimisation and Demo Day prep | Sprint 4 | Required for Demo Day — live demo in front of instructor and peers |
| "My Matches" view (upcoming joined matches) | Sprint 4 | Interview #04 (Nia) — "I want a confirmation, not just an announcement. Tell me the game is confirmed." |

### Out of Scope (MVP Phase)

| Feature | Reason Out of Scope |
|---------|---------------------|
| In-app messaging / group chat | Not required to reach activation moment (join a match); replicates what already exists and fails |
| Player ratings and reputation system | No interview evidence of this need; adds complexity without contributing to the NSM |
| League tables and season statistics | Desirable future feature; 0 of 10 interviews cited this as a pain point |
| Multi-sport tournament bracket management | Beyond sprint capacity; only Tiko (I-09) touched on tournament context |
| Payment integration for venue booking | No interview evidence of willingness to pay in MVP phase |
| Social profile pages / player bios | Adds scope without supporting the activation moment |
| Native iOS or Android app | Web-first MVP is sufficient for the sprint arc; native apps are a post-Demo Day consideration |

### Explicitly Rejected

| Feature | Why Rejected |
|---------|-------------|
| General-purpose in-app chat | Interviews #02, #03, #04 all documented that more chat channels make the fragmentation worse, not better. Adding chat replicates the root cause (P4 — workarounds create new problems). Explicitly contradicts the product direction from patterns analysis. |
| Facebook / Messenger sync integration | Interviews #10 (Cotne) proved that participant-side solutions which depend on the organiser's existing platforms fail when the organiser won't switch. We must own the canonical channel, not bridge to broken ones. |
| Participant-side workaround tools (bots, scrapers) | Cotne (I-10) spent two weekends building a Telegram bot that failed when the organiser wouldn't adopt it. Every participant-side fix in our data made fragmentation worse. This failure mode rules out the entire category. |

---

## Sprint Overview

| Sprint | Dates | Theme | Key Deliverable | Checkpoint |
|--------|-------|-------|-----------------|-----------|
| Sprint 1 | Apr 24 to May 7 | Foundation | A user can sign up, browse matches, join a game, and see confirmation — end to end in a deployed app | Midterm Apr 30 — dev continues async |
| Sprint 2 | May 8 to May 21 | Instrumentation | Organiser can create and update matches; participants receive push notifications; NSM is measurable from the Django admin | Checkpoint 3 May 21 |
| Sprint 3 | May 22 to Jun 4 | Growth | Quorum visibility live, share/invite working, match management complete, growth experiment results in | Peer Assessment Jun 4 |
| Sprint 4 | Jun 5 to Jun 11 | Demo | Pitch-ready product with complete repo, venture packet, and live demo rehearsed | Demo Day Jun 11 |

---

## Sprint 1: Foundation

**Dates:** April 24 to May 7 2026  
**Sprint Goal:** A KIU student can sign up, browse upcoming sports matches, join a game with one tap, and see a confirmation screen — fully deployed and accessible via a public URL.  
**Demo:** Live walkthrough: new user signs up → sees match list → taps a match → joins it → sees "You're in!" confirmation. All steps run in the deployed Vercel app, no localhost.

### Capacity

Capacity is calculated as: available hours ÷ 3.5 hours per story point (includes AI generation + review + testing + deployment). Midterm (Apr 30) reduces Week 1 dev hours by ~40% for all members.

| Team Member | Available Hours (excl. midterm prep) | Story Points Max |
|-------------|--------------------------------------|-----------------|
| Davit Karoiani (PO) | 16 hrs | 4 pts |
| Mariam Pirtskhalava | 18 hrs | 5 pts |
| Mariam Tskhomelidze (SM) | 14 hrs | 4 pts |
| Levan Kovziridze | 18 hrs | 5 pts |
| **Total** | **66 hrs** | **18 pts** |

**Sprint 1 commitment:** 10 story points (56% of maximum — within 60% target)

**Rationale for commitment level:** Sprint 1 overlaps with the midterm on April 30. This is also the first sprint with no historical velocity data. Committing to 56% of theoretical max builds in buffer for estimation errors, mid-sprint blockers, and the reality that AI-generated code requires significant review time that early-sprint inexperience will underestimate.

### Stories Allocated to Sprint 1

| Story ID | Story (summary) | Points | Assignee | AI Tool |
|----------|----------------|--------|----------|---------|
| S1-01 | As a new KIU student, I want to sign up and log in | 3 | Davit | Google AI Studio |
| S1-02 | As a user, I want to browse upcoming matches on a home screen | 3 | Mariam T. | Google Stitch |
| S1-03 | As a user, I want to view match details | 2 | Mariam P. | Google Stitch |
| S1-04 | As a user, I want to join a match and see confirmation | 2 | Levan | Claude Code |
| **Sprint 1 Total** | | **10** | | |

### Sprint 1 Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| Auth setup takes longer than estimated due to KIU email domain restrictions | M | H | Fall back to generic email + password auth if KIU SSO is not achievable by Day 3 of sprint |
| Vercel deployment blocked by config issues | M | H | Davit tests deployment in the first 48 hours of Sprint 1 so blockers surface early |
| Midterm week reduces velocity below projection | H | M | Sprint 1 is deliberately under-committed at 67% — buffer absorbs one person being unavailable for 2–3 days |
| Stitch-generated UI requires excessive rework to integrate backend | M | M | Mariam T. and Mariam P. review Stitch output against AC before committing any generated code |

---

## Sprint 2: Instrumentation

**Dates:** May 8 to May 21 2026  
**Sprint Goal:** An organiser can create a match and push a change notification to all participants; the NSM (`match_joined` per active user per week) is live and measurable from the Django admin.  
**Demo:** Organiser creates a match → participant receives push notification → participant joins → the join appears as a new RSVP in the Django admin. All live.  
**Checkpoint 3 due:** May 21 at 23:59

### Capacity

Theoretical max: ~20 pts (4 members × 18 hrs ÷ 3.5 hrs/pt — no midterm in Sprint 2)  
**Sprint 2 commitment:** 12 story points (60% of max — to be confirmed at Sprint 2 Planning after Sprint 1 velocity is measured)

### Stories Allocated to Sprint 2

| Story ID | Story (summary) | Points | Assignee | AI Tool |
|----------|----------------|--------|----------|---------|
| S2-01 | As an organiser, I want to create a match with sport, time, location, and player limit | 3 | Davit | Claude Code |
| S2-02 | As a participant, I want to receive a push notification when a joined match changes | 5 | Mariam T. | Google AI Studio |
| S2-03 | As a registered user, I want core actions recorded in our own database so the NSM is measurable from the Django admin | 2 | Levan | Claude Code |
| S2-04 | As a user, I want to leave a match I previously joined | 2 | Mariam P. | Claude Code |
| **Sprint 2 Total** | | **12** | | |

### Sprint 2 Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| Push notification service (web push or FCM) has complex setup | H | H | Spike in Week 1 of Sprint 2 — if web push is infeasible, fall back to email notifications for Checkpoint 3 |
| Usage metrics in the admin don't match the event-schema spec | M | M | Use the committed event-schema.md as the spec for what we count; Levan cross-references every metric name against the DB models |

---

## Sprint 3: Growth

**Dates:** May 22 to June 4 2026  
**Sprint Goal:** Users can see real-time quorum status before leaving for a game, organisers can cancel with one tap, and the share/invite flow drives at least 3 new signups during the sprint experiment window.  
**Demo:** Organiser cancels a match → all participants see "Cancelled" notification in real time → quorum counter visible on home screen cards → share link tested live.

### Capacity

Theoretical max: ~20 pts (4 members × 18 hrs ÷ 3.5 hrs/pt)  
**Sprint 3 commitment:** 11 story points (55% of max — to be confirmed at Sprint 3 Planning after Sprint 2 velocity)

### Stories Allocated to Sprint 3

| Story ID | Story (summary) | Points | Assignee | AI Tool |
|----------|----------------|--------|----------|---------|
| S3-01 | As a participant, I want to see RSVP count vs quorum on each match card | 3 | Davit | Google Stitch |
| S3-02 | As an organiser, I want to cancel a match and auto-notify all participants | 3 | Mariam T. | Claude Code |
| S3-03 | As a user, I want to share a match invite link | 2 | Mariam P. | Claude Code |
| S3-04 | As an organiser, I want to log the final score after a match | 2 | Levan | GitHub Copilot |
| S3-05 | As a user, I want to filter the match list by sport type | 1 | Davit | Google Stitch |
| **Sprint 3 Total** | | **11** | | |

### Sprint 3 Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| Share link referral flow requires deep-link handling that breaks on mobile browsers | M | M | Test on both desktop and mobile Chrome in first iteration; fallback to simple URL copy |
| Real-time quorum counter requires websocket or polling — adds infrastructure complexity | M | H | Use polling every 30s for MVP; websocket is a post-Demo Day optimisation |

---

## Sprint 4: Demo

**Dates:** June 5 to June 11 2026  
**Sprint Goal:** The product is pitch-ready — polished core flow, complete repo, all Checkpoint 3 materials submitted, 7-minute demo rehearsed and stress-tested.  
**Demo Day:** June 11 2026

### Capacity

Theoretical max: ~13 pts (4 members × 12 hrs ÷ 3.5 hrs/pt — 1-week sprint, Demo Day prep reduces dev hours)  
**Sprint 4 commitment:** 7 story points (54% of max)

### Stories Allocated to Sprint 4

| Story ID | Story (summary) | Points | Assignee | AI Tool |
|----------|----------------|--------|----------|---------|
| S4-01 | As a user, I want a "My Matches" view showing my upcoming joined matches | 2 | Levan | Google Stitch |
| S4-02 | Performance pass — home screen loads in under 2 seconds | 3 | Mariam T. | None |
| S4-03 | Demo Day prep — script, backup static demo, Q&A rehearsal | 2 | Davit (all) | None |
| **Sprint 4 Total** | | **7** | | |

### Sprint 4 Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| Live demo fails due to network issues on Demo Day | M | H | Prepare a local offline backup demo with pre-seeded data; test the demo script on the venue wifi the day before |
| Unresolved Sprint 3 stories carry into Sprint 4 and consume Demo Day prep time | M | H | PO freezes Sprint 4 scope on Jun 5 — any Sprint 3 carry-over is triaged immediately and non-critical stories are cut rather than delaying Demo Day prep |

---

## Full Backlog (All Stories)

| Story ID | Story (summary) | Sprint | Points | Interview Evidence |
|----------|----------------|--------|--------|--------------------|
| S1-01 | User signup and login | 1 | 3 | Interview #01, #10 |
| S1-02 | Match list home screen | 1 | 3 | Interview #04, #07 |
| S1-03 | Match detail view | 1 | 2 | Interview #07, #03 |
| S1-04 | Join match + confirmation | 1 | 2 | Interview #03, #04 |
| S2-01 | Organiser creates match | 2 | 3 | Interview #05 |
| S2-02 | Push notification on match change | 2 | 5 | Interview #01, #06 |
| S2-03 | Usage tracking via Django admin | 2 | 2 | NSM requirement |
| S2-04 | Leave match / cancel RSVP | 2 | 2 | Interview #08 |
| S3-01 | Quorum visibility on match cards | 3 | 3 | Interview #08, #04 |
| S3-02 | Organiser cancel match + auto-notify | 3 | 3 | Interview #05 |
| S3-03 | Share / invite match link | 3 | 2 | Interview #09 |
| S3-04 | Log match result / score | 3 | 2 | Interview #09 |
| S3-05 | Sport-type filter on home screen | 3 | 1 | Interview #04 |
| S4-01 | My Matches view | 4 | 2 | Interview #04 |
| S4-02 | Performance pass | 4 | 3 | Demo Day requirement |
| S4-03 | Demo Day prep | 4 | 2 | Demo Day requirement |

**Total story points across all sprints:** 40  
**Unallocated backlog points:** 0

---

## Milestone Alignment

| Milestone | Date | What Your Product Must Be Able to Do |
|-----------|------|--------------------------------------|
| Checkpoint 2 | Wed 22 Apr | Prototype testable (Stitch link live), event schema committed, roadmap submitted |
| Sprint 1 Review | Week 10 (May 7) | Core user flow working end to end — signup, browse, join, confirm — in deployed Vercel app |
| Checkpoint 3 | Thu 21 May | MVP functional with organiser flow and push notifications, usage measurable from the Django admin, financial model complete |
| Sprint 3 Review | Week 14 (Jun 4) | Quorum, cancellation, share/invite working; growth experiment results documented |
| Demo Day | Thu 11 Jun | 7-minute pitch, 5-minute live demo, Q&A ready |

---

## Change Log

| Date | Version | Changes | Author |
|------|---------|---------|--------|
| 16 April 2026 | 1.0 | Initial roadmap | Davit Karoiani |

---

*Product Roadmap | TheMergeConflicters | CS-PD-2026 | Spring 2026*
