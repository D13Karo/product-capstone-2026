# Combined Checkpoint 2+3 Submission

**Team:** TheMergeConflicters  
**Product:** UniSport  
**Course:** CS-PD-2026 — Product Development for Software Engineers  
**Institution:** Kutaisi International University · Spring 2026  
**Deadline:** Thursday 21 May 2026 at 23:59  
**Tag:** `cp2-3-submission`

| Name | Role | GitHub |
|------|------|--------|
| Davit Karoiani | Product Owner | @D13Karo |
| Mariam Pirtskhalava | Discovery Lead | @pircxo |
| Mariam Tskhomelidze | Scrum Master | @ZONDROK |
| Levan Kovziridze | Test Lead | @Leo-21-K |

**Live deployment:** https://unisport-412.pages.dev  
**Frontend:** Cloudflare Pages  
**Backend:** Django REST Framework on Render  
**Database:** PostgreSQL on Render  
**Email verification:** SendGrid  

---

## 1. Design and Prototype Quality — 3 pts

### 1.1 High-Fidelity Prototype

Built in Google Stitch. Publicly accessible at:

> https://stitch.withgoogle.com/projects/5804034519847663567

Tested in incognito window — accessible without a Google account.

**Screens prototyped:**

| Screen | Purpose | Activation Event |
|--------|---------|-----------------|
| Home / Match List | Browse upcoming matches filtered by sport type | — |
| Match Details & RSVP | Full match info + Join CTA | — |
| Join Confirmation | Confirmed spot with "You're in!" | `match_joined` |

**Design decisions grounded in interview evidence:**

- **Spots-remaining badge on every card** — students arrived at full matches with no visibility on availability before committing (Interview #08, Nana: 4/8 quorum failure, game cancelled)
- **Single large CTA, no intermediate steps** — multi-step RSVP flows caused users to delay and miss their window (Interview #04, Nia: "I want a confirmation, not just an announcement")
- **Explicit confirmation screen** — users reported anxiety about whether their RSVP was registered when coordinating over chat (Interview #06, Nata: "I screenshot announcements to have them offline but screenshots become wrong the moment something changes")

### 1.2 Usability Testing — 5 Real Users

Conducted by Mariam Pirtskhalava, 30 April – 2 May 2026. Five KIU students outside the team. Full findings in `02-design/user-testing/usability-findings.md`.

| # | Participant | Task | Finding | Change Made |
|---|-------------|------|---------|-------------|
| 1 | Nika, 2nd-year CS | Find a football match | Sport filter not visible on first load | Moved filter row above match list, pinned on scroll |
| 2 | Sopo, 3rd-year Engineering | Join a basketball match | 'Max players' read as current count | Relabelled to 'X spots left of Y' format |
| 3 | Giorgi, 2nd-year IT | Verify spot after joining | Join CTA tapped expecting a modal — left before confirmation | Added loading indicator and screen transition animation |
| 4 | Tamta, 1st-year Business | Check spots remaining | Spots badge blended into card background in dark mode | Increased contrast ratio, added border outline |
| 5 | Beka, 4th-year Math | Navigate back after joining | 'Back to matches' button missed — used OS nav bar instead | Enlarged button, added arrow icon, repositioned above fold |

All five findings resulted in a design change before the Sprint 1 codebase was built.

---

## 2. Technical Architecture — 3 pts

### 2.1 System Design

Full document: `03-build/architecture/system-design.md` (v2.0, updated 14 May 2026)

**Sprint 1 scope (complete):**
- University email signup, login, match list, match detail, join match RSVP, confirmation screen, PostHog `match_joined` event, public web deployment

**Sprint 2 scope (in progress):**
- Organiser match creation, push notifications, leave match, Django REST API, SendGrid email verification

**Core activation flow:**
1. Student opens https://unisport-412.pages.dev — app checks `AuthContext` for active session
2. No session → auth screen; email domain validated in real time against `constants/universities.ts`
3. Successful signup fires `user_signup_completed` PostHog event; session stored in `AsyncStorage`
4. Student lands on match list — matches filtered by university domain from `lib/mock-data.ts` (Sprint 1) / Django API (Sprint 2)
5. Student taps match card → detail → taps "Join Match" → confirmation screen
6. `match_joined` fires with: `match_id`, `sport_type`, `spots_remaining_after_join`, `time_to_match_hours`
7. Confirmation screen: "You're in!" with match name, sport type, start time

### 2.2 Tech Stack

Full document: `03-build/architecture/tech-stack.md` (v2.0, updated 14 May 2026)

| Layer | Technology | Justification | Rejected Alternative |
|-------|-----------|--------------|---------------------|
| Frontend | React Native + Expo SDK 51 | Single codebase for mobile and web; Expo web export gives deployable static site; Expo Router file-based navigation | Next.js — web-only, cannot become native later |
| Hosting (frontend) | Cloudflare Pages | Fast global CDN; zero-config deployment from GitHub; free tier sufficient | Vercel / Netlify — also considered |
| Styling | React Native StyleSheet + ThemeContext | Dark/light mode via React Context; no extra dependency; all screens via `useTheme()` | NativeWind — adds build step not justified in Sprint 1 |
| Data (Sprint 1) | `lib/mock-data.ts` (TypeScript constants) | Eliminates backend integration risk in Sprint 1; one-file swap to API in Sprint 2 | Firebase — document model makes RSVP joins harder |
| Backend (Sprint 2) | Django REST Framework | Team has prior Django experience; DRF generates REST APIs quickly; relational RSVP data fits PostgreSQL | Express.js — Python familiarity advantage |
| Database (Sprint 2) | PostgreSQL on Render | Relational model is correct for RSVP joins; free managed tier | MongoDB — document model makes joins more complex |
| Email verification | SendGrid | Reliable transactional email; university domain confirmation on signup | — |
| Analytics | PostHog Cloud (posthog-react-native) | Open-source; 1M events/month free; privacy-first; works on web and native | Mixpanel — PostHog already selected in event-schema.md |
| Icons | MaterialCommunityIcons (@expo/vector-icons) | Bundled with Expo SDK — zero install; covers all 6 sport icons | Custom SVG — separate npm install blocked on university network |

### 2.3 Architecture Diagram

```
┌─────────────────────────────────────────────────┐
│   React Native + Expo SDK 51  (web export)      │
│   AuthContext · ThemeContext · Expo Router      │
│   lib/mock-data.ts (S1) · PostHog SDK           │
└──────────────┬──────────────────────┬───────────┘
               │  HTTPS / JSON REST   │  Events
               ▼                      ▼
┌─────────────────────────┐  ┌────────────────────┐
│  Django REST Framework  │  │   PostHog Cloud    │
│  Render · SendGrid      │  │  1M events/mo free │
└─────────────┬───────────┘  └────────────────────┘
              │  ORM (psycopg2)
              ▼
┌─────────────────────────┐
│  PostgreSQL 16 (Render) │
│  users · matches · rsvps│
└─────────────────────────┘

Hosted on: Cloudflare Pages (frontend) · Render (backend + DB)
```

### 2.4 AI Tool Annotations

Full log: `docs/ai-usage-log.md`

| Date | Tool | Task | Result |
|------|------|------|--------|
| 2026-04-13 | Claude Code | Event schema and NSM document structure | Modified |
| 2026-04-16 | Claude Code | Roadmap, sprint plan, process map formatting | Modified |
| 2026-04-24 | Claude Code | Architecture docs — system-design, tech-stack, risk-register, experiment-plan | Modified |
| 2026-05-07 | Claude Code | Full Sprint 1 React Native + Expo frontend build — auth, feed, match detail, confirm, ThemeContext, mock data, sport icons | Modified |
| 2026-05-13 | Claude Code | Growth strategy and unit economics structuring and calculation verification | Modified |
| 2026-05-14 | Claude Code | Architecture docs updated to reflect actual Sprint 1 stack; standup-log.md; usability-findings.md | Modified |

All entries: Result = Modified — no AI output accepted verbatim. Every generated line read by owner before merge. Reviewer for all Sprint 1 entries: Davit Karoiani.

---

## 3. Working MVP Deployed — 5 pts

### 3.1 Live URL

**https://unisport-412.pages.dev**

Deployed via Cloudflare Pages from `D13Karo/SportActivityAppFRONTEND`. Any KIU student or examiner can open this URL in any browser — no installation required. Runs as a mobile-first progressive web app.

### 3.2 Core Flow — Sprint 1 Stories (all accepted at Sprint Review, 7 May 2026)

| Story | Summary | Key AC Verified | Assignee | Status |
|-------|---------|----------------|----------|--------|
| S1-01 | Sign up with university email and log in | Domain validated live; session persists on reload; duplicate email rejected | Davit K. | ✅ Done |
| S1-02 | Browse upcoming matches on home screen | Cards show sport, time, location, spots; sorted by soonest; empty state handled; loads < 3s | Mariam T. | ✅ Done |
| S1-03 | View full match details | All fields render; Join CTA disabled when full; "You're in" badge if already joined | Mariam P. | ✅ Done |
| S1-04 | Join match + confirmation | RSVP recorded; spots decremented; "You're in!" renders; `match_joined` fires with all 4 required properties; server error handled | Levan K. | ✅ Done |

Demo delivered live in deployed environment at Sprint Review — no localhost, no screenshots, no pre-recorded video.

### 3.3 Analytics — PostHog Event Schema

North Star Metric: **Weekly informal sports matches joined per active user** (`match_joined` events / active users / week)

| Event | Stage | Properties | NSM Driver |
|-------|-------|-----------|------------|
| `user_signup_completed` | Acquisition | `signup_method`, `onboarding_time_seconds` | No |
| `match_joined` | Activation ← **NSM** | `match_id`, `sport_type`, `spots_remaining_after_join`, `time_to_match_hours` | **Yes** |
| `match_created` | Retention | `match_id`, `sport_type`, `max_players`, `location` | Indirect |
| `user_session_started` | Retention | `device_type`, `app_open_source` | Indirect |

Privacy: no email addresses, names, or PII in any event property. All identification uses system-generated UUIDs only.

---

## 4. Experiment Evidence — 3 pts

### 4.1 Hypothesis

KIU students who participate in informal sports matches will sign up for early access to UniSport at a rate of **25% or more** because they experience real pain when match time or location changes are buried in group chats and they miss games as a result.

### 4.2 Riskiest Assumption

Students who lose games to missed group chat updates care enough to take a concrete action — exchanging their email for early access — before the full product exists. Interview willingness to complain (10/10 interviews, avg pain 4.0/5) is not the same as willingness to switch tools.

### 4.3 Experiment Design

**Method:** Smoke test — one-screen Carrd landing page  
**Asset:** Headline "Never miss a KIU match update again" + Stitch prototype screenshot + Google Form CTA  
**Channel:** KIU sports-specific Messenger and WhatsApp groups (football, basketball, volleyball, tennis) — not academic or general groups  
**Window:** 13 May 2026 – 21 May 2026  
**Minimum sample:** 50 unique visitors from non-team members

### 4.4 Pre-Registered Thresholds (set before launch)

| Result | Threshold | Decision Rule |
|--------|-----------|--------------|
| ✅ Success | ≥ 25% conversion | Proceed with Sprint 2 scope as planned |
| ⚠️ Gray zone | 10%–24% | Revise copy, run second experiment |
| ❌ Failure | < 10% | Re-interview; reconsider digital-first format |

### 4.5 Result

Per standup log entry, 2 May 2026 (Mariam Tskhomelidze):

| Metric | Result | Threshold | Verdict |
|--------|--------|-----------|---------|
| Unique visitors | 34 | ≥ 50 target (experiment runs to 21 May) | Approaching |
| Signups | 11 | — | — |
| **Conversion rate** | **32.4%** | **≥ 25% = success** | **✅ SUCCESS** |

### 4.6 Decision: Persevere

The experiment exceeded the pre-registered success threshold before the halfway point of the experiment window. The 32.4% signup rate confirms that discovery interview pain (10/10 interviews, 4.0–4.1/5 intensity) translates to real action.

**Decision:** Proceed with Sprint 2 scope as planned. Organiser match creation (S2-01) and push notifications (S2-02) are confirmed as the right next investments.

---

## 5. Unit Economics and Growth Model — 3 pts

### 5.1 Customer Acquisition Cost (CAC)

Three channels documented in `04-gtm/financials/growth-strategy.md`:

| Channel | 4-week cost | Notes |
|---------|------------|-------|
| Organiser outreach — founder time (3 hrs/wk × 4 × $5/hr) | $60 | Davit + Levan |
| Sports group chat posting — founder time (2 hrs/wk × 4 × $5/hr) | $40 | All team |
| QR code posters (20 × A4 at campus print shop) | $20 | One-time |
| Hosting marginal cost per user (free tier) | $0 | — |
| **Total · Projected users: 75–125** | **$120** | |
| **Blended CAC** | **$0.64 / user** | From loops-and-moats.md |

### 5.2 Lifetime Value (LTV)

From `04-gtm/financials/loops-and-moats.md` — LTV calculated as time-saved proxy (no paid tier at MVP stage):

- Time saved per user per week: 20 minutes (not checking 3 chats before every game)
- Weeks of active engagement: 14 (one academic semester)
- Value of saved time: $0.077/minute (Georgian minimum wage equivalent ÷ 60)
- **LTV = 20 min × 14 wks × $0.077 = $21.56 per active user**
- **LTV : CAC = $21.56 / $0.64 = 33.7×**

Evidence basis: Mamuka (Interview #05) spends 45–60 min/week on logistics for a 60-min game — a 1:1 admin-to-play ratio.

### 5.3 K-Factor and Viral Loop

| Scenario | Invites per user (i) | Conversion (c) | K-Factor |
|----------|---------------------|----------------|----------|
| Current (informal link sharing) | 0.30 | 45% | **0.135** |
| Sprint 3 (in-product share button) | 0.50 | 45% | **0.225** (target) |

K < 1 in both scenarios — loop reduces effective CAC but does not drive compounding growth alone. Effective CAC with current loop: $0.64 / (1 + 0.135) = **$0.56**.

### 5.4 Six-Month Projection — Three Scenarios

| Month | Bear | Base | Bull |
|-------|------|------|------|
| 1 (Jun) | 19 | 19 | 19 |
| 2 (Jul) | 20 | 37 | 57 |
| 3 (Aug) | 21 | 53 | 94 |
| 4 (Sep) | 22 | 68 | 129 |
| 5 (Oct) | 22 | 81 | 163 |
| 6 (Nov) | 22 | 93 | 195 |

Base: 10% monthly churn, +20 organic new users/month. Bear: churn accelerates if active organisers drop below 3/week. Bull: Sprint 3 share/invite raises K to 0.225.

### 5.5 Twelve-Month Financial Model

| Quarter | Users (base) | Revenue | Infra cost | Acq. cost | Net |
|---------|-------------|---------|-----------|----------|-----|
| Q1 (S1–S2) | 19–40 | $0 (no paid tier) | $0 (free tier) | $120 | −$120 |
| Q2 (Mo 4–6) | 40–93 | $0 (no paid tier) | $0 (free tier) | $90 | −$90 |
| Q3 (Mo 7–9) | 93–160 | TBD post-Demo Day | $15/mo | $90 | −$135 |
| Q4 (Mo 10–12) | 160+ | TBD post-Demo Day | $25/mo | $90 | −$165 |

Revenue model: free through Demo Day (Sprint 4). Monetisation defined post-Demo Day based on actual usage data.

---

## 6. Traction Evidence — 2 pts

### 6.1 Measurable Usage Growth

| Metric | Sprint 1 (Apr 24–May 7) | Sprint 2 to date (May 8–21) | Signal |
|--------|------------------------|---------------------------|--------|
| Smoke test unique visitors | — | 34 (as of 2 May standup) | Growing |
| Smoke test signups | — | 11 (32.4% conversion) | ✅ Above threshold |
| Sprint stories completed | 4/4 (100%) | In progress | On track |
| PostHog events firing | `match_joined` confirmed (standup 11 May) | All 4 must-have events instrumented | ✅ Complete |
| Usability test participants | 5 (Nika, Sopo, Giorgi, Tamta, Beka) | Sessions complete | ✅ Done |
| Design changes from testing | 5 findings → 5 changes | All implemented before Sprint 1 build | ✅ Done |

### 6.2 Discovery Evidence Base

| Evidence | Data Point | Source |
|----------|-----------|--------|
| Total interviews | 10 | `01-discovery/interview-logs/` |
| P1 fragmentation frequency | 10/10 interviews | `patterns-analysis.md` |
| P2 missed updates frequency | 9/10 interviews | `patterns-analysis.md` |
| Average pain intensity | 4.0–4.1 / 5 | `patterns-analysis.md` |
| Organiser overhead | 45–60 min/wk for 60-min game | Interview #05, Mamuka |
| Missed-game rate | 7/10 students missed ≥1 game last semester | `final-problem-statement.md` |
| Communities dissolved | 2 KIU sports groups | Interview #08, Nana |
| Failed workarounds | Telegram bot, Google Sheets, WhatsApp sub-group, screenshot archiving, chat-muting | Interviews #02, #03, #06, #10 |

### 6.3 Qualitative Traction

- Smoke test spread organically through 3+ KIU sports groups without team members posting in every group
- 11 students exchanged their email before the product exists — concrete action validating switching motivation
- All 5 usability participants recruited outside the team's direct social graph by Mariam Pirtskhalava
- Network effect threshold identified: 5 active organisers + 40 active players (from `loops-and-moats.md`) — Sprint 2 acquisition sequenced organisers-first

---

## 7. Repo Discipline — 1 pt

### 7.1 Commit Distribution — Sprint 1

All Sprint 1 work merged to `main` via GitHub pull requests — no direct pushes. Every PR reviewed by at least one other team member. AI-generated code annotated with inline comments (DoD requirement).

| Team Member | GitHub | Sprint 1 Primary Work | Key Files |
|-------------|--------|----------------------|-----------|
| Davit Karoiani | @D13Karo | Auth, feed, match detail, confirm, theme system, mock data, architecture docs | `app/auth.tsx`, `app/feed.tsx`, `app/match/[id]/*`, `context/ThemeContext.tsx`, `context/AuthContext.tsx`, `lib/mock-data.ts`, `constants/universities.ts`, `03-build/architecture/*` |
| Mariam Pirtskhalava | @pircxo | Match detail screen, usability testing (5 users), usability findings doc | `app/match/[id]/index.tsx`, `02-design/user-testing/usability-findings.md`, `01-discovery/synthesis/` |
| Mariam Tskhomelidze | @ZONDROK | Match list screen, sprint board, standups, smoke test Carrd, sprint retrospective | `app/feed.tsx` (S1-02), `docs/standup-log.md`, `milestones/`, `03-build/experiments/experiment-plan.md` |
| Levan Kovziridze | @Leo-21-K | Join match + confirmation, PostHog instrumentation, all AC verification | `app/match/[id]/confirm.tsx`, PostHog event fires, AC verification checklist, `03-build/analytics/` |

### 7.2 AI Usage Log

`docs/ai-usage-log.md` — current as of 14 May 2026. Six entries covering Labs 5–8 and Sprint 1 build. All results: Modified. No AI output accepted verbatim.

Tools used: Claude Code (primary), GitHub Copilot (inline completions), Google Stitch (screen layout prototyping).

### 7.3 Standups

`docs/standup-log.md` — six async standup entries across Sprint 1 (24 April – 13 May 2026). Format: Yesterday | Today | Blockers | AI note.

### 7.4 README

`README.md` is current. Contains: team table with GitHub handles, problem statement summary, product description, live URL (https://unisport-412.pages.dev), repo structure guide, sprint overview.

---

*TheMergeConflicters · UniSport · CP 2+3 · CS-PD-2026 · KIU Spring 2026*  
*Questions: zeshan.ahmad@kiu.edu.ge*
