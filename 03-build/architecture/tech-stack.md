# Tech Stack Selection

**Team:** TheMergeConflicters
**Product:** CampusSport
**Date:** 24 April 2026
**Version:** 1.0

---

## 1. Decision Summary

Sprint 1 optimises for three things in order: deployment speed, team familiarity, and demo reliability. The team has no prior velocity data, is overlapping with midterm week, and must ship a live Vercel URL by May 7. Every stack choice is made to eliminate setup friction — not to build the most scalable system. Next.js as a single-framework frontend and backend removes the overhead of maintaining two separate services. Supabase gives us a hosted Postgres database, Auth, and a real-time layer from one free-tier account without DevOps configuration. PostHog was already committed in the event schema — it is not re-evaluated here. The only thing deferred on purpose is native app distribution; the web-first choice is correct for MVP and was validated against interview data (no interviewee requested a native app). No TBD entries remain in this document.

---

## 2. Stack by Layer

| Layer | Selected technology | Why this fits | Alternative considered | Why rejected | Owner |
|------|---------------------|--------------|------------------------|--------------|------|
| Frontend | Next.js 14 (App Router) | React-based, server-side rendering for mobile performance, first-class Vercel deployment, large community for debugging | Vite + React (SPA) | No server-side rendering — slower first load on mobile connections; no built-in routing | Mariam T. |
| Backend | Next.js server actions (same repo) | Eliminates a separate Express or FastAPI service; server actions run on Vercel as serverless functions; team stays in one language and repo | Express.js REST API | Requires a separate server deployment, separate repo, and CORS configuration — too much overhead for 4-person team in Sprint 1 | Davit |
| Database | Supabase Postgres | Hosted, managed, free tier generous enough for demo scale, row-level security built in, works natively with Supabase Auth, real-time subscriptions available for Sprint 3 | Firebase Firestore | NoSQL document model makes relational RSVP joins (user ↔ match ↔ RSVP) more complex; Firestore pricing model is harder to reason about at zero cost | Levan |
| Authentication | Supabase Auth | Ships with the Supabase account — no separate service; supports email/password, session management, and JWT out of the box; Google AI Studio was used to prototype the flow | Firebase Auth | Would require switching the database to Firebase as well — Supabase Auth + Supabase Postgres is a tighter integration | Davit |
| Analytics | PostHog Cloud | Committed in event-schema.md; open-source, privacy-first, 1M events/month free tier, self-hostable post-Demo Day, purpose-built for product analytics | Mixpanel | Also valid, but PostHog was already selected and documented — switching creates unnecessary churn; PostHog's free tier is more generous | Levan |
| Hosting | Vercel | Committed in sprint plan and roadmap; zero-config Next.js deployment, preview URLs on every PR, free hobby tier covers demo scale | Railway | Valid for Express backends but adds no value here since Next.js server actions deploy natively on Vercel | Davit |
| Testing | Jest + React Testing Library | Standard Next.js test setup; sufficient for unit and component tests on acceptance criteria | Playwright (E2E only) | E2E tests alone do not catch component-level regressions; Jest covers unit logic; Playwright is Sprint 4 scope | Mariam P. |
| Diagramming | Mermaid (in Markdown) | Committed to version control alongside the code; renders on GitHub without external tools; no licensing cost | Lucidchart / draw.io | External tools not in version control; diagram diverges from code over time; Mermaid is sufficient for Sprint 1 | Davit |

---

## 3. Approved AI Tools for Sprint 1

| Tool | Approved use | Not for | Review rule | Owner |
|------|--------------|---------|-------------|------|
| Google Stitch | Generate match list, match detail, and signup/login UI screen scaffolds from structured prompt | Generating server-side logic, database queries, or auth configuration | Developer reviews every AC against generated UI before raising PR; annotate AI-generated blocks with inline comments | Mariam T., Mariam P. |
| Claude Code | Multi-file backend logic: join match server action, RSVP write, spots-remaining decrement, race condition validation | Generating Supabase schema migrations without human review | Read every generated line before accepting; annotate with inline comments; PR reviewer verifies annotations; log entry in `docs/ai-usage-log.md` required | Levan |
| Google AI Studio | Prototype auth flow (signup, session persistence, login redirect) before integration | Generating production-ready code without review | Test all S1-01 ACs locally; any output that fails an AC is modified not accepted; log entry required | Davit |
| GitHub Copilot | Ambient completions for imports, TypeScript types, config boilerplate, repetitive patterns | Accepting suggestions for business logic without reading the output | No tab-to-accept without reading the line; any Copilot-assisted logic block must be annotated if non-obvious | Whole team |

No other AI tools are approved for Sprint 1. Any tool change must be noted in `docs/ai-usage-log.md` with a reason.

---

## 4. Deployment Target

- Public deployment target: Vercel (production branch = `main`; preview on every PR)
- Database region or environment: Supabase free tier — `eu-central-1` region (closest to Georgia) if available, otherwise default
- How local and production differ: local uses `.env.local` with Supabase dev project credentials; production uses Vercel environment variables set in the Vercel project dashboard; `SUPABASE_AUTH_EMAIL_AUTOCONFIRM=true` is set in production for Sprint 1 to remove email confirmation friction
- What gets deployed first: Davit deploys a bare Next.js + Supabase project to Vercel in the first 48 hours of Sprint 1 to confirm the pipeline works before stories are built
- What will stay local for now: database seed script runs locally via `npm run seed` — no automated seeding in CI for Sprint 1

---

## 5. Rejected Architecture Paths

### Rejected Option 1
- Option: Full-stack Django + React (Python backend, separate JS frontend)
- Why it was attractive: Davit has prior Django experience; Python is familiar for data work
- Why it was rejected now: Two separate services and two languages doubles the deployment complexity; Vercel does not host Python servers natively; team would need a second hosting provider (Render or Railway) for the backend; integration surface is wider and harder to manage during midterm week

### Rejected Option 2
- Option: Firebase (Firestore + Firebase Auth + Firebase Hosting)
- Why it was attractive: single Google product ecosystem, free tier, real-time by default
- Why it was rejected now: Firestore's document model makes relational RSVP queries (user joined which matches, how many RSVPs per match) harder to write and reason about than SQL; Firebase pricing model unpredictability at higher usage is a concern for post-Demo Day; Supabase Postgres gives the team SQL familiarity and schema clarity

---

## 6. Technical Debt You Are Accepting on Purpose

| Shortcut | Why accepted now | Risk created | When to revisit |
|----------|------------------|-------------|-----------------|
| Email confirmation disabled in Supabase Auth | Removes signup friction for Sprint 1 demo; KIU SSO integration is deferred | Any email address can create an account; no verification that user is a real KIU student | Sprint 2: evaluate KIU SSO; Sprint 3 at latest: enable email confirmation or replace with SSO |
| No automated end-to-end tests | Sprint 1 stories are verified manually against AC by PO; E2E test setup takes 4–6 hours that are not in sprint capacity | A regression in the join flow may not be caught before the next PR merges | Sprint 4: add Playwright smoke test for core flow before Demo Day |
| Database seed script runs manually | Automated seeding not needed for demo scale; seeding is deterministic and repeatable | Demo could fail if seed is not run after a schema migration | Sprint 2: add `npm run seed` to the deployment pipeline |
| No rate limiting on join match endpoint | Traffic at demo scale (< 10 concurrent users) does not require rate limiting | Theoretical abuse vector; not a real Sprint 1 risk | Sprint 4 or post-Demo Day |

---

## 7. Final Stack Lock

- Frontend: Next.js 14 with App Router, deployed on Vercel, mobile-first styling with Tailwind CSS
- Backend: Next.js server actions running as Vercel serverless functions — no separate server
- Database: Supabase managed Postgres with row-level security; schema has tables for users, matches, and rsvps
- Auth: Supabase Auth with email and password; sessions managed by Supabase JWT; email confirmation disabled for Sprint 1
- Analytics: PostHog Cloud; events fired from both frontend (signup, session start) and server action (match joined)
- Hosting: Vercel on the free hobby tier; production deploys automatically on merge to `main`

No TBD entries remain.

---

*Tech Stack | TheMergeConflicters | CS-PD-2026 | Spring 2026*
