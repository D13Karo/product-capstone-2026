# System Design Document

**Team:** TheMergeConflicters
**Product:** CampusSport
**Date:** 24 April 2026
**Version:** 2.0 — updated 14 May 2026 to reflect Sprint 1 actual build
**Primary author:** Davit Karoiani

---

## 1. Core Sprint 1 Request

```text
A KIU student signs up with their university email, browses upcoming sports matches
at their university, taps one to view full details, joins it with one tap, and sees
a confirmation screen with their spot reserved.
```

**Sprint 1 boundary:**
- In scope: university email signup, login, match list, match detail, join match RSVP, confirmation screen, recorded `match_joined` action (RSVP persisted in our own DB), public web deployment
- Out of scope: organiser match creation, push notifications, leave match, quorum visibility, share/invite links, sport-type filters, KIU SSO, Django backend (Sprint 2)

---

## 2. System Goal

By Sprint 1 review, CampusSport must support one complete end-to-end user flow on a public URL. A new student must be able to sign up with a university email, browse seeded upcoming matches for their university, view match details, join a match, and see a confirmation screen. Sprint 1 ships with mock data — no backend persistence — to maximise delivery speed. The Django REST API is a Sprint 2 deliverable. Usage is tracked by reading our own database via the Django admin (the `match_joined` action is a persisted RSVP).

---

## 3. Component Breakdown

| Component | Layer | Responsibility | Owner | Technology | AI touchpoint |
|-----------|-------|----------------|-------|------------|----------------|
| Mobile/web app | Client | Renders signup, match list, match detail, join confirmation | Davit, Mariam T. | React Native + Expo (web export) | Stitch used to validate screen layout; Claude Code used for all component code |
| University auth gate | Client | Validates university email domain on signup; filters matches by university domain | Davit | Custom domain validation in `constants/universities.ts` | Claude Code |
| Theme system | Client | Dark/light mode toggle across all screens | Davit | ThemeContext + React context | Claude Code |
| Mock data layer | Data (Sprint 1) | Provides seeded match data; replaced by Django API in Sprint 2 | Davit | `lib/mock-data.ts` (TypeScript constants) | No AI |
| Usage tracking | Measurement | Records signups, joins, sessions as rows in our own DB; counts read via Django admin | Levan | Django admin over our own PostgreSQL DB (PostHog trialled, dropped) | No runtime AI |
| Django REST API | Backend (Sprint 2) | Manages users, matches, RSVPs with full persistence | Backend team | Django + Django REST Framework + PostgreSQL | Planned for Sprint 2 |

---

## 4. Key Data Objects

| Entity | What it represents | Created by | Read by | Stored where |
|--------|--------------------|-----------|---------|--------------|
| User | University student account (email, name, university) | Signup flow | Auth check, match list filter, RSVP creation | AuthContext (AsyncStorage, Sprint 1); Django users table (Sprint 2) |
| Match | Scheduled informal sports game with sport type, time, location, player limit, university domain | Seed data (Sprint 1); organiser flow via API (Sprint 2) | Match list, match detail, join validation | `lib/mock-data.ts` (Sprint 1); Django matches table (Sprint 2) |
| RSVP | Record that a specific user has joined a specific match | Join match action | Match detail, confirmation screen | Local state (Sprint 1); Django rsvps table (Sprint 2) |
| University | Domain-keyed record mapping email domain to university name, city, short name | `constants/universities.ts` | Signup validation, match list filter, header display | Static constant file |
| Usage record | Product usage signal (e.g. an RSVP, a signup) | App on the relevant action | Team via Django admin | Our own PostgreSQL database |

---

## 5. User Request Lifecycle

Join match flow — the core Sprint 1 activation path:

1. Student opens the CampusSport public URL on any device.
2. App checks for an active session via `AuthContext`.
3. If no session, student is redirected to the auth screen.
4. Student enters university email — app validates domain in real time against `constants/universities.ts` and displays the detected university.
5. Student submits email + password — `login()` stores user in `AuthContext` + `AsyncStorage`.
6. The signup is recorded in our own database (counted later via the Django admin).
7. Student lands on the match list home screen — matches are filtered by the student's university domain from `lib/mock-data.ts`.
8. Match cards render with sport icon (MaterialCommunityIcons vector icon), time, location, spots badge.
9. Student taps a match card — match detail screen loads full match record.
10. Student taps "Join Match" — app navigates to confirmation screen.
11. The join is persisted as an RSVP row (`match_id`, `sport_type`, `spots_remaining_after_join`, `time_to_match_hours`), counted later via the Django admin.
12. Confirmation screen renders: "You're in!" with match name, sport type, start time.

---

## 6. Data Flow Notes

- **Data entering from the user:** university email address, password, selected match ID, join tap action
- **Data validated:** university email domain (client-side against known domains), password minimum length (8 chars)
- **Data stored Sprint 1:** user session in AsyncStorage (email, name, university object)
- **Data stored Sprint 2 (planned):** RSVP record, decremented spots count — via Django API
- **Data that must never be stored:** raw passwords; no PII in usage records (users identified by system-generated ID only)

---

## 7. APIs and Integrations

| Service | Why it exists | Request direction | Risk | Fallback |
|---------|---------------|-------------------|------|----------|
| Django REST API (Sprint 2) | Persistent match and RSVP storage; also the source of all usage counts (read via Django admin) | Client to Django API on Railway/Render | API not ready for Sprint 1 | Sprint 1 uses mock data; Sprint 2 replaces mock-data.ts with real API calls |

---

## 8. Deployment Topology

- **Frontend:** React Native app exported as static web app via `npx expo export --platform web`, deployed on Netlify or Vercel
- **Backend:** Django REST API — Sprint 2, to be hosted on Railway or Render
- **Database:** PostgreSQL managed by Railway or Render — Sprint 2
- **Analytics:** Django admin over our own PostgreSQL database (no third-party analytics tool)
- **Auth:** Custom university email validation (Sprint 1); Django JWT auth (Sprint 2)
- **Public URL:** See `README.md` for the live deployment link

---

## 9. AI in the Build and AI in the Product

### AI in the build workflow

| Tool | Used for what | Owner | Review rule |
|------|---------------|-------|-------------|
| Google Stitch | Scaffold screen layout and visual hierarchy for match list, detail, confirm, and auth screens | Mariam T., Mariam P. | Review every AC against generated UI; no merge without AC pass |
| Claude Code | All React Native component code — auth screen, feed, match detail, confirmation, theme system, tournament section, sport icons, mock data structure | Davit | Read every generated line; test all ACs locally; log entry required |
| GitHub Copilot | Inline completions for TypeScript types, imports, config boilerplate | Whole team | Accept line only after reading it |

### AI in the product

No runtime AI feature in Sprint 1. AI tools are used exclusively in the build workflow. The product does not call any AI API during user sessions.

---

## 10. Security, Privacy, and Reliability

- **Email gate:** Only recognised university email domains can sign up — enforced client-side via domain validation; server-side enforcement planned for Sprint 2
- **Sensitive data:** User email stored in AsyncStorage (device-local, not synced to server in Sprint 1); no PII in usage records (users identified by system-generated ID only)
- **Usage tracking has no runtime dependency:** usage is read from our own database via the Django admin, so there is no external analytics service that can fail at runtime
- **Sprint 1 reliability commitment:** Static web export has no server-side failure modes; mock data always loads

---

## 11. Technical Risks

1. **Risk:** Mock data creates a gap between Sprint 1 demo and a real multi-user experience
   - Mitigation: Sprint 2 replaces mock-data.ts with Django API calls; architecture is designed for this swap with a clean data layer abstraction

2. **Risk:** Expo web export may not render correctly across all browsers
   - Mitigation: Test with Chrome, Safari, and Firefox before submission; fix any layout issues before tagging

3. **Risk:** Usage counts not readable from the Django admin before submission
   - Owner: Levan; confirm signup/join counts load in the admin before the `cp2-3-submission` tag

---

## 12. Open Questions for Sprint 2

- Django API authentication: JWT tokens or session cookies?
- Database hosting: Railway vs Render for the Django backend?
- Should the Expo app stay web-first or add a native build via EAS in Sprint 3?

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
