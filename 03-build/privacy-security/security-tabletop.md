# Security Tabletop

**Product:** CampusSport
**Team:** TheMergeConflicters
**Date:** 12 December 2025
**Audit run date:** 12 December 2025 (see §Dependency Audit for notes on registry access)

---

## Overview

This document applies the STRIDE threat model to the five highest-traffic user flows in CampusSport. For every threat identified, we either name the mitigation in place or explicitly accept the risk with a written rationale. Unexamined threats are not low risks. They are unknown risks.

**Architecture under audit:**
- Frontend: React Native + Expo SDK 51 (`SportActivityApp/frontend`)
- Backend: Django 6.0.5 + DRF 3.17 + djangorestframework-simplejwt 5.5 (`SportActivityApp/backend`)
- Database: PostgreSQL (Railway/Render in production; SQLite in dev — `db.sqlite3` in repo)
- Realtime: Django Channels + channels-redis (`requirements.txt`)
- Push: expo-notifications → Expo Push Service → APNs/FCM
- Email: SendGrid HTTPS API (verification + password reset)
- Auth: JWT access + refresh tokens stored in Expo `AsyncStorage`

**STRIDE reference:**

| Letter | Category | The question |
|--------|----------|-------------|
| S | Spoofing | Can an attacker impersonate a legitimate user or system component? |
| T | Tampering | Can data be modified in transit or at rest without detection? |
| R | Repudiation | Can a user deny performing an action, with no audit trail to prove otherwise? |
| I | Information Disclosure | Can sensitive data be exposed to unauthorised parties? |
| D | Denial of Service | Can this flow be abused to make the service unavailable to legitimate users? |
| E | Elevation of Privilege | Can a user gain capabilities beyond their permission level? |

---

## Five User Flows Selected

| # | Flow name | Why selected |
|---|-----------|-------------|
| 1 | User signup with KIU email + verification code | Entry point for every user. Compromise here breaks the "only your campus's matches" promise. Also handles SendGrid + bcrypt + JWT issuance. |
| 2 | User login (JWT issue) and session refresh | Authentication is the primary attack surface for account takeover. JWT refresh token rotation is the highest-stakes endpoint in the app. |
| 3 | `match_joined` — participant RSVPs to a match (the activation moment) | The North Star Metric event. Highest-traffic write endpoint. Carries match_id, user_id, and triggers push notifications to all other joined participants. |
| 4 | `match_created` — organiser posts a new match | The supply-side flow. An attacker who can create matches at scale can spam every player at a university with push notifications. |
| 5 | Match-change push notification (organiser edits time/venue → backend fans out push to all RSVPed players) | The differentiator feature. Carries match data into Expo Push, which routes to APNs/FCM. Multiple processors touch user data on this path. |

Flows deliberately not in scope today (acknowledged): password reset (lower traffic but high-stakes — covered briefly in Flow 1 notes), `match_left` (mirror of `match_joined` in security profile), profile update (no email change supported yet, so attack surface is small).

---

## Flow 1: User signup with KIU email + verification code

**Description:** User enters email + display name + password + confirm password. Server validates KIU domain, hashes password (Django default PBKDF2), creates inactive user, generates a 6-digit verification code, sends it via SendGrid, then activates the account and issues JWT pair when the code is submitted.

| STRIDE category | Threat identified | Mitigation in place | Status |
|----------------|-------------------|---------------------|--------|
| Spoofing | Attacker could register with someone else's KIU email and gain access to that person's intended account | Email verification code (6 digits, 15-minute TTL, single-use) required before account activation. Unverified accounts cannot log in. | Mitigated |
| Spoofing | Attacker could spoof a non-KIU email by abusing domain validation | KIU domain validated **both** client-side (`constants/universities.ts`) and server-side (Django serializer). Client-only validation is bypassable; server is authoritative. | Mitigated |
| Tampering | Form data tampered in transit between Expo app and Django backend | HTTPS enforced on Vercel/Netlify (frontend) and on Railway/Render (backend). HSTS via Django `SECURE_HSTS_SECONDS` setting. | Mitigated — verify HSTS is set in `settings.py` (action item below) |
| Repudiation | User denies completing signup and claims they never agreed to ToS | `users.created_at` timestamp + IP recorded server-side. ToS version not yet stored — see consent-form.md §6 gap 3. | Partial — action item: add `users.privacy_notice_version` column, owner Davit, target Sprint 4 |
| Information Disclosure | Error message on `Email already in use` enables email enumeration attack | Currently the response distinguishes "already registered" from "invalid email". Known weakness. | **NOT MITIGATED — accepted risk:** low severity at MVP scale (audience is one university, enumeration provides little value to attacker). Action item: unify error to `Unable to create account; if you have an account try logging in` before public launch beyond KIU. Owner: Davit. Target: Sprint 4. |
| Information Disclosure | Verification code intercepted via email | SendGrid uses TLS to recipient mail servers. Code is single-use and expires in 15 minutes. We cannot mitigate end-to-end email security beyond using a reputable transactional provider. | Mitigated to the extent possible at this layer |
| Denial of Service | Attacker submits thousands of signup requests to exhaust DB connections and SendGrid quota | No rate limiting currently on `POST /auth/signup`. SendGrid free tier has a 100/day cap that an attacker could exhaust, blocking real signups. | **NOT MITIGATED — action item:** add `django-ratelimit` to signup and verify endpoints (5/hour per IP), owner: Levan, target: Sprint 4 (before Demo Day) |
| Elevation of Privilege | New user attempts to access admin / staff endpoints | All admin routes protected by Django `IsAdminUser` permission class and `is_staff` flag set server-side at user creation = `False`. JWT carries `user_id` only; role check is per-request from DB. | Mitigated |

---

## Flow 2: User login (JWT issue) and session refresh

**Description:** User submits email + password. Server verifies hash, issues JWT access token (short-lived, e.g. 15 min) + refresh token (longer-lived, e.g. 30 days). Refresh endpoint exchanges a refresh token for a new access token without re-prompting the user.

| STRIDE category | Threat identified | Mitigation in place | Status |
|----------------|-------------------|---------------------|--------|
| Spoofing | Credential stuffing — attacker tries leaked password lists against KIU emails | No rate limiting on `POST /auth/login`. No password breach check. Django default PBKDF2 makes hash cracking slow but does not block stuffing at the API. | **NOT MITIGATED — action item:** add `django-ratelimit` (5 attempts per email per 15 minutes), owner: Levan, target: Sprint 4. Accepted risk until then: at current ~0 real users, exposure is theoretical. |
| Spoofing | Stolen refresh token reused after user logs out | `tokens_invalidated_at` column on `users` exists (seen in backend code) and is used to invalidate all tokens issued before logout timestamp. | Mitigated |
| Tampering | JWT signature forged | JWT signed with Django `SECRET_KEY` using HS256 (SimpleJWT default). `SECRET_KEY` is loaded from environment via `python-dotenv` and not committed (verified — see Secrets Check). | Mitigated — conditional on `SECRET_KEY` being long, random, and rotated if leaked |
| Tampering | Token modified at rest on the device (Expo `AsyncStorage` is not encrypted by default on web; secure on iOS Keychain) | On native iOS, expo-secure-store would be a hardening upgrade; on web (current deployment target), AsyncStorage is plain localStorage. XSS would expose tokens. | **NOT MITIGATED — accepted risk:** web target uses localStorage by Expo's default; mitigation is to harden XSS prevention (Content Security Policy, no `dangerouslySetInnerHTML`). React Native's default escaping covers most XSS vectors. Action item: when native builds ship, migrate to `expo-secure-store`. Owner: Davit. Target: post-Demo Day. |
| Repudiation | User denies logging in from a given device | Login attempts logged server-side with IP + user-agent + timestamp. Retention 30 days (Railway/Render log default). | Mitigated for the 30-day window |
| Information Disclosure | Generic error `Invalid credentials` could expose user enumeration if it differs by email existence | Standard response is the same regardless of whether the email exists — confirmed in backend serializer. | Mitigated |
| Denial of Service | Refresh-token endpoint hammered to mint new access tokens | No per-token rate limit. | **NOT MITIGATED — action item:** rate-limit refresh endpoint (10/min per refresh token jti). Owner: Levan. Target: Sprint 4. |
| Elevation of Privilege | Access token from one user used to impersonate another | JWT signed by server-only secret; access token contains user_id and is verified per request by SimpleJWT middleware. | Mitigated |

---

## Flow 3: `match_joined` — RSVP to a match

**Description:** Authenticated user taps Join on a match card. Frontend calls `POST /matches/<id>/join` with JWT in `Authorization` header. Backend verifies the match is in the same university as the user, that spots remain, creates the RSVP row, increments quorum count, and fires the PostHog `match_joined` event.

| STRIDE category | Threat identified | Mitigation in place | Status |
|----------------|-------------------|---------------------|--------|
| Spoofing | Attacker forges a JWT to RSVP as another user | JWT signature verification rejects forgery. | Mitigated |
| Spoofing | Authenticated user from University A joins a University B match by guessing match IDs | Backend serializer filters matches by the authenticated user's `university_domain` before resolving the match ID. Cross-university join returns 404. | Mitigated — requires test coverage (action item: add test, owner Levan) |
| Tampering | RSVP request body tampered to claim a spot for someone else | The endpoint takes no `user_id` from the body — the user is derived from the JWT. Body tampering has no effect. | Mitigated by design |
| Tampering | Match `max_players` tampered to bypass spot limit | Backend re-reads `max_players` from the database and checks `current_count < max_players` server-side, independent of client input. | Mitigated |
| Repudiation | User joins a match and later denies it ("I never said I would come") | RSVP row has user_id, match_id, timestamp. `match_joined` PostHog event is independently logged with same identifiers. Two audit trails. | Mitigated |
| Information Disclosure | Other players' identities exposed in match roster | Roster returns display_name only — never email. Verified in `MatchSerializer` (action item: add explicit serializer test). | Mitigated — needs test coverage |
| Denial of Service | Attacker scripts hundreds of join/leave cycles to flood notifications and exhaust DB writes | Currently no rate limit on `POST /matches/<id>/join` or `DELETE /matches/<id>/rsvp`. | **NOT MITIGATED — action item:** rate-limit join/leave to 10/min per user. Owner: Levan. Target: Sprint 4. |
| Elevation of Privilege | RSVP grants the user organiser-level capabilities on the match | Server enforces that only `match.creator` can edit/cancel. RSVP does not change `match.creator`. | Mitigated |

---

## Flow 4: `match_created` — organiser posts a new match

**Description:** Authenticated user submits the create-match form (sport, time, location, max_players). Backend creates the match record with `creator = request.user`, makes it visible only to users with the same `university_domain`, and fires `match_created` in PostHog.

| STRIDE category | Threat identified | Mitigation in place | Status |
|----------------|-------------------|---------------------|--------|
| Spoofing | Attacker creates matches as another organiser | `creator` set from JWT-derived user. Body cannot override. | Mitigated |
| Tampering | Match `university_domain` set to a different university to spam other campuses | `university_domain` is read from the authenticated user's profile server-side; not accepted from the request body. | Mitigated |
| Tampering | Past-date matches created to manipulate analytics | Server-side validation rejects start_time more than 5 minutes in the past. | Mitigated — verify the validator exists (action item: add unit test) |
| Repudiation | Organiser cancels a match and denies they created it | `match.creator` foreign key + `match.created_at` timestamp + corresponding `match_created` PostHog event. | Mitigated |
| Information Disclosure | Match content (location, time) visible to non-KIU users | The list endpoint filters by authenticated user's `university_domain`. Unauthenticated requests are rejected by `IsAuthenticated` permission. | Mitigated |
| Denial of Service | Attacker creates 10,000 matches to flood every player's feed and notification queue | No rate limit on `POST /matches`. A single compromised KIU account could spam the entire university. | **NOT MITIGATED — accepted risk + action item:** at MVP scale (audience ~ one university, account count low) exposure is low. Action item: rate-limit match creation to 20/day per user. Owner: Levan. Target: Sprint 4. |
| Elevation of Privilege | A regular participant gains organiser capabilities by exploiting the create endpoint | There is no separate "organiser" role — any authenticated KIU user can create a match. This is intentional product design (any student can post a pickup game). | Not a threat by design |

---

## Flow 5: Match-change push notification fan-out

**Description:** Organiser edits a match (time, venue, or cancels). Backend computes the diff, identifies all users with an active RSVP, sends them push notifications via Expo Push Service → APNs/FCM. Sends transactional copy via SendGrid in parallel (planned, not all routes yet).

| STRIDE category | Threat identified | Mitigation in place | Status |
|----------------|-------------------|---------------------|--------|
| Spoofing | Attacker triggers a fake notification to all match participants | Edit endpoint requires JWT and `match.creator == request.user`. Non-organisers receive 403. | Mitigated |
| Spoofing | Attacker registers a fake Expo push token for another user to hijack notifications | Token registration endpoint requires JWT; the token is associated with `request.user`. Cross-user token registration is rejected. | Mitigated |
| Tampering | Notification payload modified between backend and Expo Push Service | HTTPS to Expo Push API. Payload signed by Expo to APNs/FCM. | Mitigated end-to-end |
| Repudiation | Organiser claims they never edited the match | Edit logged with `updated_at`, `updated_by_user_id`, and a diff row in `match_history` table (when present — action item below). | Partial — action item: add `match_history` audit table if not yet present. Owner: Davit. Target: post-Demo Day. |
| Information Disclosure | Push notification leaks data to anyone holding the device | Lock-screen previews show match title + new time + new venue (the data the user needs). No PII (no other player names, no email). | Acceptable — user controls lock-screen visibility at OS level |
| Information Disclosure | Expo Push Service / Google Cloud sees notification payload | Disclosed in privacy notice §3. Payload contains match metadata, no PII. | Mitigated by data minimisation |
| Denial of Service | Mass-edit attack to push spam to every joined player | Edit endpoint is rate-limited only by the per-user limits added under Flow 4. An organiser editing one match 1000 times in an hour would fan out 1000 pushes per joined player. | **NOT MITIGATED — action item:** rate-limit match edits to 10/match/hour. Owner: Levan. Target: Sprint 4. |
| Denial of Service | Attacker exhausts the Expo Push quota | Expo Push has generous quotas; would require very high abuse. Per-organiser edit rate limit (above) is the practical mitigation. | Same action item as above |
| Elevation of Privilege | Non-organiser triggers an edit that fans out notifications | `match.creator == request.user` check returns 403 for anyone else. | Mitigated |

---

## Dependency Audit

### Audit run

**Command used:** `npm audit --json` (frontend) and review of `requirements.txt` against PyPI security advisories (backend — `pip-audit` was not installed in the sandbox at audit time).
**Date run:** 12 December 2025
**Working directory:** `SportActivityApp/frontend` for npm; `SportActivityApp/backend` for Python.

### Frontend (`npm audit`)

**Result:** Audit blocked. The npm registry security advisory endpoint returned **403 Forbidden** at the time of audit on the network used. This is most likely a network restriction (the sandboxed shell could not reach `registry.npmjs.org/-/npm/v1/security/advisories/bulk`).

**Status:** `TBD — needs investigation`, per Lab 10 README guidance to use this label when blocked rather than guessing CVEs.

**Remediation:** Re-run `npm audit --json > npm-audit-2025-12-12.json` on a network that allows the npm advisory endpoint (any standard residential network or campus wifi outside the sandbox). Owner: Levan Kovziridze. Target: same-day before commit, or 24 hours after Lab 10.

**Manual qualitative review (pending the formal audit):**

| Package | Version | Notes |
|---------|---------|-------|
| `expo` | `~51.0.0` | Expo SDK 51 is one major behind current (SDK 52). No outstanding critical advisories known to us at audit time, but SDK 52 is the safer baseline. Upgrade planned post-Demo Day. |
| `react-native` | `0.74.1` | One minor behind 0.74.5 (patch fixes available). Recommend bump to 0.74.5 in next dev cycle. |
| `expo-notifications` | `~0.28.19` | Active maintenance; aligns with SDK 51. |
| `@react-native-async-storage/async-storage` | `1.23.1` | Current. Stores JWT in plaintext on web — covered as a STRIDE finding in Flow 2 (Tampering, accepted risk). |
| `@react-navigation/native` | `^6.1.17` | Caret allows minor/patch upgrades. Confirm `package-lock.json` resolved version against CVE list during the formal `npm audit` rerun. |

### Backend (`requirements.txt` review)

`pip-audit` could not be run in the same window (Python venv not activated in the audit shell). Manual review against publicly known advisories on 12 December 2025:

| Package | Version | Notes |
|---------|---------|-------|
| `Django` | `6.0.5` | Django 6.x track. Confirm 6.0.5 is the latest patch for the 6.0 series at time of `pip-audit` rerun. |
| `djangorestframework` | `3.17.1` | Current at audit time. |
| `djangorestframework_simplejwt` | `5.5.1` | Current. Check for any CVE updates at `pip-audit` rerun. |
| `psycopg2-binary` | `2.9.12` | Active. |
| `PyJWT` | `2.12.1` | Active. |
| `channels` `>=4.1,<5` and `channels-redis` `>=4.2,<5` | Constraint-based — will resolve to latest in range | Confirm resolved versions at `pip-audit` rerun. |
| `whitenoise` `>=6.7,<7` | Constraint | Same — confirm resolved version. |

**Action item:** install `pip-audit` and run it in the backend venv. Pin exact versions in `requirements.txt` once audit clears. Owner: Levan. Target: 24 hours after Lab 10.

### Three highest-priority findings (placeholder until formal audit completes)

Per Lab 10 guidance, these are the three highest-priority items at audit time. They will be updated to specific CVE numbers once the formal `npm audit` and `pip-audit` runs are complete.

**Finding 1 (currently provisional High):**
- Package: `expo` 51.x (entire SDK)
- CVE: TBD on `npm audit` rerun
- Vulnerability: Behind latest major (SDK 52). Generic risk: any advisory addressed in 52.x is unmitigated on 51.x.
- Remediation: Upgrade plan for SDK 52 to be drafted post-Demo Day. Sprint 4 risk-accept until Demo.
- Owner: Davit Karoiani
- Target date: 1 July 2026 (post-Demo)
- Status: Accepted with rationale — Demo Day freeze in effect through 11 June 2026; SDK upgrade after Demo to avoid demo-week regression risk.

**Finding 2 (provisional):**
- Package: `react-native` `0.74.1`
- CVE: TBD
- Vulnerability: One patch behind `0.74.5`.
- Remediation: `npm install react-native@0.74.5` and re-run E2E smoke test.
- Owner: Mariam Tskhomelidze
- Target date: 18 December 2025
- Status: In progress

**Finding 3 (provisional — process gap, not a CVE):**
- Package: Project-wide
- CVE: N/A
- Vulnerability: No automated dependency audit in CI. Lab 10 is the first formal audit run. Without CI integration, this is a one-off rather than continuous monitoring.
- Remediation: Add a GitHub Actions workflow that runs `npm audit --audit-level=high` and `pip-audit` on every PR. Fail the build on High or Critical findings.
- Owner: Levan Kovziridze
- Target date: End of Sprint 4 (11 June 2026)
- Status: Action item logged

---

## Secrets Check

### Commands run

```bash
# In SportActivityApp/backend
git log --all --full-history -- ".env" "**/.env"
git log -p --all | grep -iE "(api_key|secret_key|password|token|SECRET=)"

# In SportActivityApp/frontend
git log --all --full-history -- ".env" "**/.env"
git log -p --all | grep -iE "(api_key|secret|password|token)"
```

### Result

**Status: CLEAN.**

- `git log --all --full-history -- "**/.env"` produced **no commits** in either repo — `.env` has never been committed.
- The grep over `git log -p --all` produced only **code references** (e.g. `os.environ.get("SENDGRID_API_KEY", "")`, password serializer class names, "Your CampusSport password reset code" string literal). No literal secret values were committed.

**Current `.env` status (verified against `.gitignore` in `SportActivityApp/backend`):**

- [x] `.env` file exists locally for development (not in repo)
- [x] `.env` is listed in `.gitignore` (both `.env` and `.env.local` confirmed)
- [x] `venv/` is listed in `.gitignore`
- [ ] `.env.example` exists in the repo with placeholder values only — **GAP**: confirm `.env.example` exists; if not, create it.
- [x] No `.env` file has ever been committed (verified by secrets check above)

**Action item:** if `.env.example` is missing, create one in both `backend/` and `frontend/` with placeholder keys: `DJANGO_SECRET_KEY=`, `DATABASE_URL=`, `SENDGRID_API_KEY=`, `REDIS_URL=`, `EXPO_PUBLIC_POSTHOG_KEY=`, `EXPO_PUBLIC_API_BASE_URL=`. Owner: Davit. Target: 24 hours.

---

## Top Three Vulnerabilities Summary

After STRIDE + dependency audit + secrets check, our three highest-priority items entering Sprint 4:

| Priority | Vulnerability | Flow or component | Mitigation or action | Owner | Date |
|----------|--------------|-------------------|---------------------|-------|------|
| 1 | No rate limiting on auth and write endpoints (login, signup, refresh, match_join, match_leave, match_create, match_edit) | Flows 1, 2, 3, 4, 5 | Add `django-ratelimit` to all named endpoints with calibrated thresholds. Add an integration test that confirms a 6th request inside the window is rejected. | Levan Kovziridze | End of Sprint 4 (11 June 2026) |
| 2 | Dependency audit not run in CI; one-off audit could miss future CVEs | Build/CI | Add GitHub Actions workflow running `npm audit --audit-level=high` and `pip-audit` on every PR; fail on High or Critical. | Levan Kovziridze | End of Sprint 4 |
| 3 | Email enumeration on signup (`Email already in use`) | Flow 1 | Unify the response to `Unable to create account; if you have an account try logging in.` Remove the differentiating branch in the signup serializer. | Davit Karoiani | Sprint 4 |

Lower-priority items captured but not in the top 3: HSTS confirmation (Flow 1), `expo-secure-store` migration for native builds (Flow 2), match_history audit table (Flow 5), explicit serializer tests for cross-university leakage (Flow 3).
