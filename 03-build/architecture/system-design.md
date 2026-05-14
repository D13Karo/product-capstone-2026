# System Design Document

**Team:** TheMergeConflicters
**Product:** CampusSport
**Date:** 24 April 2026
**Version:** 1.0
**Primary author:** Davit Karoiani

---

## 1. Core Sprint 1 Request

```text
A KIU student signs up, browses upcoming sports matches, taps one to view full details, joins it with one tap, and sees a confirmation screen with their spot reserved.
```

**Current Sprint 1 boundary:**
- In scope: user signup, login, session persistence, match list home screen, match detail screen, join match RSVP, confirmation screen, PostHog `match_joined` event, Vercel deployment
- Out of scope: match creation by organisers, push notifications, leave match, quorum visibility, share/invite links, sport-type filters, match result logging, KIU SSO

---

## 2. System Goal

By Sprint 1 review (May 7), CampusSport must support one complete end-to-end user flow on a public Vercel URL. A new student must be able to create an account, browse seeded upcoming matches, view match details, join a match, and see a confirmation screen. The system must persist RSVP records in a database, decrement the spots-remaining counter in real time, and fire the `match_joined` PostHog event with the properties defined in the event schema. The design prioritises delivery speed, mobile-first rendering, and demo reliability over completeness — no features outside the core flow ship in Sprint 1.

---

## 3. Component Breakdown

| Component | Layer | Responsibility | Owner | Technology | AI touchpoint, if any |
|-----------|-------|----------------|-------|------------|-----------------------|
| Web app | Client | Renders signup, login, match list, match detail, join confirmation screens | Mariam T. | Next.js | Stitch used to scaffold match list and detail screens |
| API routes and server actions | Server | Validates requests, coordinates reads and writes, enforces business rules | Davit | Next.js server actions | Claude Code used for join match RSVP logic and server-side validation |
| Authentication | Auth | Account creation, login, session management, session persistence | Davit | Supabase Auth | Google AI Studio used to prototype auth flow before integration |
| Database | Data | Stores users, matches, and RSVP records | Levan | Supabase Postgres | No runtime AI |
| Analytics | Measurement | Records `match_joined`, `user_signup_completed`, and `user_session_started` events | Levan | PostHog | No runtime AI |

---

## 4. Key Data Objects

| Entity | What it represents | Created by | Read by | Stored where |
|--------|--------------------|-----------|---------|-------------|
| User | KIU student account and session identity | Signup flow | Auth check, RSVP creation, match list load | Supabase Auth and users profile table |
| Match | A scheduled informal sports game with sport type, time, location, and player limit | Seed script (Sprint 1); organiser flow (Sprint 2) | Match list screen, match detail screen, join validation | Supabase Postgres — matches table |
| RSVP | Record that a specific user has joined a specific match | Join match action | Match detail screen (spots remaining, "You're in" badge), confirmation screen | Supabase Postgres — rsvps table |
| Event | Product usage signal (match joined, signup, session started) | Frontend and server action callbacks | Team PostHog dashboard | PostHog cloud |

---

## 5. User Request Lifecycle

Describes the join match flow — the core Sprint 1 activation path.

1. Student opens the public CampusSport Vercel URL on mobile browser.
2. Frontend checks for an active Supabase Auth session.
3. If no session exists, the student is redirected to the signup or login screen.
4. Student submits email and password — Supabase Auth creates the account and returns a session token.
5. `user_signup_completed` PostHog event fires with `signup_method: "kiu_email"`.
6. Student lands on the match list home screen — frontend server action reads all upcoming matches from Postgres ordered by `start_time` ascending.
7. Match cards render with sport type, date, start time, location name, and spots remaining.
8. `user_session_started` PostHog event fires on home screen load.
9. Student taps a match card — match detail screen loads the full match record from Postgres.
10. Student taps "Join Match" — frontend sends join request to the server action with `match_id` and authenticated `user_id`.
11. Server action validates: user does not already have an RSVP for this match, spots remaining is greater than zero.
12. Server action writes a new row to the `rsvps` table and decrements `matches.spots_remaining` by 1 in a single transaction.
13. Server action returns the confirmed RSVP and updated match record.
14. `match_joined` PostHog event fires with `match_id`, `sport_type`, `spots_remaining_after_join`, and `time_to_match_hours`.
15. Confirmation screen renders: "You're in!" with match name, sport type, start time, and "Back to matches" button.

---

## 6. Data Flow Notes

- **Data entering from the user:** email address, password, selected match ID, join tap action
- **Data validated:** email format and uniqueness (Supabase Auth), password minimum length (8 chars), RSVP uniqueness (server action), spots remaining before join (server action)
- **Data stored permanently:** user account record, RSVP record, decremented spots count on match row
- **Data temporary or computed:** session token (managed by Supabase), spots remaining display (read from match row at render time), time_to_match_hours (computed at event fire time from match start_time and current timestamp)
- **Data that must never be stored:** raw passwords, email addresses in PostHog event properties, any PII in analytics payloads

---

## 7. APIs and Integrations

| Service or API | Why it exists | Request direction | Risk | Fallback plan |
|----------------|---------------|------------------|------|---------------|
| Supabase Auth | Email account creation and session management — fastest credible auth for MVP | Frontend to Supabase Auth service | Email confirmation flow could block first login | Disable email confirmation for Sprint 1 demo accounts; enable for production |
| Supabase Postgres | Persistent match and RSVP storage with real-time row-level queries | Server action to Supabase Postgres | Connection pool exhaustion under load | Not a Sprint 1 risk at demo scale; monitor in Sprint 3 |
| PostHog | Event tracking for NSM measurement — `match_joined` is the activation event | Frontend and server action to PostHog Cloud | Misconfigured event property names misalign with schema | Cross-reference every event name against `03-build/analytics/event-schema.md` before instrumentation |

No external mapping, payment, or notification APIs in Sprint 1.

---

## 8. Deployment Topology

- Frontend hosted on: Vercel (automatic deploy on merge to `main`)
- Backend hosted on: Next.js server actions on Vercel (same deployment as frontend)
- Database hosted on: Supabase managed Postgres (free tier)
- Domain or public URL: Vercel-generated URL (e.g. `kiu-sports-tracker.vercel.app`)
- Analytics platform: PostHog Cloud (free tier, 1M events/month)
- Auth provider: Supabase Auth
- File storage, if any: not needed in Sprint 1

---

## 9. AI in the Build and AI in the Product

### AI in the build workflow

| Tool | Used for what | Owner | Review rule |
|------|---------------|-------|-------------|
| Google Stitch | Scaffold match list and match detail UI screens from structured prompt | Mariam T., Mariam P. | Review every AC against generated UI; no merge without AC pass confirmed |
| Claude Code | Join match server action, RSVP write logic, spots-remaining decrement, race condition handling | Levan | Read every generated line; annotate with inline comments; PR reviewer checks annotations |
| Google AI Studio | Prototype auth flow (signup, login, session persistence) before wiring into Next.js | Davit | Test all four S1-01 ACs locally before raising PR; log entry required |
| GitHub Copilot | Inline completions for boilerplate (imports, types, config) | Whole team | Accept line only after reading it; no tab-to-accept without review |

### AI in the product, if applicable

No runtime AI feature exists in Sprint 1. AI tools are used exclusively in the build workflow to generate and review code. The product itself does not call any AI API during user sessions.

---

## 10. Security, Privacy, and Reliability Basics

- **Auth risks:** Supabase email confirmation could add friction to first-time activation — disable confirmation in Sprint 1 demo config and document the decision
- **Sensitive data handled:** user email (stored by Supabase Auth only), RSVP records linked to user UUID — no PII enters PostHog events
- **Failure mode if Supabase goes down:** match list and join flow become unavailable — acceptable for Sprint 1 demo; Sprint 3 will add seed data fallback for Demo Day
- **Logging and monitoring plan for Sprint 1:** PostHog event stream confirms activation events are firing; Vercel deployment logs surface server action errors; no custom alerting in Sprint 1
- **One thing we will not promise yet:** real-time spots-remaining counter without page reload — polling or real-time subscriptions are a Sprint 3 optimisation

---

## 11. Technical Risks and Spikes

1. **Risk:** RSVP write and spots-remaining decrement may allow race condition double-joins if two users tap simultaneously
   - Why it matters: a user sees "You're in!" but the match is over capacity — breaks the confirmation guarantee that is the core product promise
   - Mitigation or spike: use a Supabase Postgres transaction with a row-level check (`spots_remaining > 0`) before insert; test with two simultaneous requests in the Levan's local environment
   - Owner: Levan Kovziridze

2. **Risk:** Supabase Auth email confirmation blocks new user activation on the demo day
   - Why it matters: S1-01 AC1 requires the user to land on the home screen immediately after signup — a confirmation email step breaks this
   - Mitigation or spike: set `SUPABASE_AUTH_EMAIL_AUTOCONFIRM=true` in the Vercel environment config for Sprint 1; document the setting; revisit for production
   - Owner: Davit Karoiani

3. **Risk:** Stitch-generated UI requires excessive rework to wire up to real Supabase data
   - Why it matters: if Stitch output uses hardcoded mock data or component patterns that conflict with Next.js server actions, integration time expands beyond estimate
   - Mitigation or spike: Mariam T. and Mariam P. review Stitch export against AC before committing any generated code; hardcoded data is replaced with live data fetches before PR is raised
   - Owner: Mariam Tskhomelidze, Mariam Pirtskhalava

---

## 12. Open Questions

- Should the Vercel preview URL be shared with the Product Owner before Sprint Review for early AC verification?
- Does seeded match data need a minimum of 5 matches for a convincing demo, or is 2 (per AC) sufficient?
- Should the `match_joined` event fire from the server action (after DB write confirms) or from the frontend (on confirmation screen render)?

---

## 13. Final Readiness Check

- [x] Every component has one clear job
- [x] Core request lifecycle is written end to end
- [x] Stack in this file matches `tech-stack.md`
- [x] Top technical risks are named
- [x] Out of scope items are explicit
- [x] Another developer could start work from this document

---

*System Design | TheMergeConflicters | CS-PD-2026 | Spring 2026*
