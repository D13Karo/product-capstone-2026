# Tech Stack Selection

**Team:** TheMergeConflicters
**Product:** CampusSport
**Date:** 24 April 2026
**Version:** 2.0 — updated 14 May 2026 to reflect Sprint 1 actual build

---

## 1. Decision Summary

Sprint 1 optimises for three things: delivery speed, cross-platform reach from one codebase, and demo reliability. React Native with Expo lets us ship a single codebase that runs as a mobile-first web app (via Expo's web export) and as a native app (if needed later), without maintaining a separate web framework. This is the correct choice for a university sports coordination app where students will primarily access the product on their phones. The backend (Django) is deferred to Sprint 2 — Sprint 1 ships with mock data to eliminate all API integration risk during the first sprint. Usage analytics is read from the Django admin over our own database (we trialled PostHog but switched — see `03-build/analytics/event-schema.md`). No TBD entries remain.

---

## 2. Stack by Layer

| Layer | Selected technology | Why this fits | Alternative considered | Why rejected | Owner |
|------|---------------------|--------------|------------------------|--------------|-------|
| Frontend | React Native + Expo SDK 51 | Single codebase for mobile and web; Expo Router provides file-based navigation; Expo web export gives a deployable static site for Sprint 1 demo | Next.js (web only) | Web-only — cannot become a native mobile app later; university sports coordination is a phone-primary use case | Davit |
| Navigation | Expo Router (file-based) | Mirrors Next.js App Router conventions; deep link support built in; no separate navigation library needed | React Navigation (manual) | More configuration overhead for the same result | Davit |
| Styling | React Native StyleSheet (no external library) | No extra dependency; platform-safe styling; sufficient for Sprint 1 scope | Tailwind CSS (NativeWind) | Adds a build step and dependency; not justified until Sprint 3 | Mariam T. |
| Theme system | React Context (`ThemeContext`) with DarkColors/LightColors | Dark/light mode toggle with zero dependencies; all screens receive colors via `useTheme()` hook | No theme system | Dark mode is a validated user preference from testing | Davit |
| Data (Sprint 1) | Mock data in `lib/mock-data.ts` (TypeScript constants) | Eliminates backend integration risk during Sprint 1; swap to real API in Sprint 2 is a one-file change per screen | Firebase / Supabase from Sprint 1 | Setting up a hosted database adds Sprint 1 scope and integration risk | Davit |
| University auth gate | Custom domain validation in `constants/universities.ts` | Zero dependencies; maps university email domains to university objects; enforces the core product promise at signup | Supabase Auth | Full auth service is Sprint 2 scope; client-side domain validation is sufficient for Sprint 1 demo | Davit |
| Backend (Sprint 2) | Django + Django REST Framework | Team has prior Django experience; DRF generates well-structured REST APIs quickly; PostgreSQL is the natural fit for relational RSVP data | Express.js | Python familiarity advantage; DRF handles serialisation, validation, and auth with less boilerplate | Backend team |
| Database (Sprint 2) | PostgreSQL (hosted on Railway or Render) | Relational model is the right fit for RSVP joins; both offer a free managed Postgres tier | MongoDB | Document model makes RSVP joins more complex; no benefit for this data model | Levan |
| Analytics | Django admin over our own PostgreSQL DB | Reads usage (signups, matches, joins, active users) straight from our backend; no third-party processor; cleanest privacy posture at our scale | PostHog (trialled), Mixpanel | PostHog was trialled then dropped — at one-campus scale the admin gives every count we need without sending data off-platform | Levan |
| Icons | @expo/vector-icons (MaterialCommunityIcons) | Bundled with Expo SDK — zero installation; covers all required sport icons (soccer, basketball, volleyball, handball, tennis-ball, chess-king) and UI icons | Custom SVG (react-native-svg) | Requires a separate npm install not available on university network; MaterialCommunityIcons covers everything needed | Davit |
| Hosting (frontend) | Netlify or Vercel (static web export) | `npx expo export --platform web` produces a static site deployable to either; free tier sufficient for demo scale | Expo Go (QR code only) | Not accessible via URL — cannot satisfy the rubric requirement | Davit |
| Testing | Manual AC verification by PO | Sufficient for Sprint 1; each story AC verified before merge | Jest + React Testing Library | Setup overhead not justified in Sprint 1; deferred to Sprint 3 | Mariam P. |

---

## 3. Approved AI Tools for Sprint 1

| Tool | Approved use | Not for | Review rule | Owner |
|------|--------------|---------|-------------|-------|
| Google Stitch | Generate screen layout and visual hierarchy mockups for match list, detail, confirmation, and auth screens | Generating business logic or data validation code | Review every AC against generated layout | Mariam T., Mariam P. |
| Claude Code | All React Native component code: auth screen, feed screen, match detail, confirmation, theme system, sport icon component, tournament section, mock data structure, router setup | Accepting output for security-sensitive logic without human review | Read every generated line; test all ACs locally; log entry required | Davit |
| GitHub Copilot | Inline completions for TypeScript type definitions, import statements, config boilerplate | Accepting completions for business logic without reading | No tab-to-accept without reading the suggestion | Whole team |

No other AI tools are approved for Sprint 1. Any tool change requires a new entry in `docs/ai-usage-log.md`.

---

## 4. Deployment Target

- **Frontend public URL:** Expo web export deployed on Netlify or Vercel — see README for live link
- **How to build:** `npx expo export --platform web` — outputs to `dist/` folder; drag-and-drop to Netlify or push via CLI
- **Backend (Sprint 2):** Django API on Railway or Render; environment variables for database credentials stored in platform secrets
- **Analytics:** Django admin over our own database (no third-party analytics tool; no analytics API key required)

---

## 5. Rejected Architecture Paths

### Rejected Option 1
- Option: Next.js 14 as full-stack web app (frontend + backend in one repo)
- Why it was attractive: single repo, Vercel native, server actions eliminate a separate API
- Why it was rejected: CampusSport is a mobile-first product; a React Native app with Expo web export is both mobile-native and web-deployable from the same codebase; Next.js would have required a separate React Native effort later

### Rejected Option 2
- Option: Firebase (Firestore + Firebase Auth + Firebase Hosting)
- Why it was attractive: single Google ecosystem, free tier, real-time by default
- Why it was rejected: Firestore's document model makes relational RSVP queries harder than SQL; Django Postgres will be a cleaner fit for Sprint 2

---

## 6. Technical Debt Accepted on Purpose

| Shortcut | Why accepted now | Risk created | When to revisit |
|----------|-----------------|-------------|-----------------|
| Mock data instead of real API | Eliminates all backend integration risk in Sprint 1 | No multi-user consistency; RSVP counts not persisted | Sprint 2: replace mock-data.ts with Django API calls |
| Client-side university email validation | Sufficient for Sprint 1 demo | Could be bypassed in code | Sprint 2: enforce on Django backend |
| No automated tests | Sprint 1 ACs verified manually by PO | Regression risk at Sprint 2 integration | Sprint 3: add Jest unit tests |

---

## 7. Final Stack Lock

- **Frontend:** React Native with Expo SDK 51, Expo Router, deployed as static web app via Expo web export
- **Styling:** React Native StyleSheet; ThemeContext for dark/light mode; MaterialCommunityIcons for vector icons
- **Data (Sprint 1):** Mock data in `lib/mock-data.ts`; swap to Django API in Sprint 2
- **Analytics:** Django admin over our own PostgreSQL database (PostHog trialled, dropped)
- **Backend (Sprint 2):** Django REST Framework + PostgreSQL on Railway
- **Hosting:** Netlify or Vercel for frontend; Railway or Render for Django backend

No TBD entries remain.

---

*Tech Stack | TheMergeConflicters | CS-PD-2026 | Spring 2026*
