# Risk Register

**Team:** TheMergeConflicters
**Product:** KIU Sports Tracker
**Date:** 24 April 2026

---

## Top Technical Risks

| Risk ID | Risk statement | Likelihood, Low Medium High | Impact, Low Medium High | Earliest detection point | Mitigation or spike | Owner | Status |
|--------|----------------|-----------|--------|--------------------------|---------------------|-------|--------|
| R1 | RSVP write and spots-remaining decrement race condition allows two users to join a full match simultaneously | Medium | High | When two users tap "Join" on the same match within milliseconds — testable locally with concurrent requests | Wrap RSVP insert and spots_remaining decrement in a single Postgres transaction with a row-level check (`spots_remaining > 0` before insert); return error if check fails | Levan Kovziridze | Open |
| R2 | Supabase Auth email confirmation flow breaks the first-activation path (S1-01 AC1) | High | High | First time Davit runs the signup flow end-to-end locally | Set `SUPABASE_AUTH_EMAIL_AUTOCONFIRM=true` in Vercel environment variables for Sprint 1; add note to system-design.md; test in Vercel preview before raising S1-01 PR | Davit Karoiani | Open |
| R3 | Stitch-generated UI uses hardcoded mock data that conflicts with Next.js server action data contracts | Medium | Medium | When Mariam T. or Mariam P. attempts to wire Stitch output to live Supabase data | Review Stitch export against AC before integrating; replace any static data with live server action calls before PR is raised; verify each AC passes against real data in Vercel preview | Mariam Tskhomelidze | Open |
| R4 | PostHog `match_joined` event property names diverge from event-schema.md spec | Low | Medium | When Levan instruments the event in the join server action | Cross-reference every property name against `03-build/analytics/event-schema.md` before instrumentation; Davit (PO) verifies event fires with correct properties in PostHog dashboard as part of S1-04 AC2 acceptance | Levan Kovziridze | Open |

---

## Notes on the Top 3

### R1 — RSVP race condition
- **Why this matters to Sprint 1:** S1-04 AC1 requires that when the user taps Join, their RSVP is recorded and spots_remaining decrements by 1. If two users hit the endpoint simultaneously, both could pass the `spots_remaining > 0` check before either write completes, resulting in over-capacity RSVPs and a broken confirmation guarantee — the core product promise.
- **What evidence would show the risk is real:** two simultaneous POST requests to the join endpoint both return 200 and both write RSVP rows even when spots_remaining was 1.
- **What we will do first:** Levan writes the server action using a Supabase Postgres transaction. The transaction reads `spots_remaining`, checks `> 0`, inserts the RSVP row, and decrements `spots_remaining` in a single atomic block. If the check fails, the transaction rolls back and the server action returns a 409 with the S1-04 AC4 error message.

### R2 — Supabase Auth email confirmation
- **Why this matters to Sprint 1:** S1-01 AC1 says "I am automatically logged in and redirected to the match list home screen" after signup. If Supabase Auth sends a confirmation email first and blocks session creation until the email is clicked, AC1 fails unconditionally. This affects the Sprint Review demo directly — a new signup on demo day will not work without this fix.
- **What evidence would show the risk is real:** Davit creates a new account locally or in Vercel preview and is sent to an email confirmation page instead of the home screen.
- **What we will do first:** Davit sets `SUPABASE_AUTH_EMAIL_AUTOCONFIRM=true` in the Supabase project dashboard under Auth → Settings, and mirrors the setting in the Vercel environment config. This is done in the first 48 hours of Sprint 1 as part of the deployment pipeline test.

### R3 — Stitch UI / server action integration mismatch
- **Why this matters to Sprint 1:** Both S1-02 (Mariam T.) and S1-03 (Mariam P.) use Stitch to scaffold their screens. If Stitch generates React components with local useState mock data, the developers must replace all mock data with server action calls. If the Stitch component prop contracts do not match the Supabase data shape (e.g., field names `matchName` vs `sport_type`), integration becomes a full rewrite rather than a wire-up.
- **What evidence would show the risk is real:** Mariam T. spends more than 2 hours renaming props or restructuring Stitch output to fit the data model.
- **What we will do first:** Before committing any Stitch output, Mariam T. runs through each AC by hand against the generated UI. Any screen that cannot pass an AC with real data is reworked locally before raising a PR. The PR description must show a screenshot from the Vercel preview URL, not a localhost screenshot.

---

## Spike Plan

| Spike | Question to answer | Timebox | Owner | Output |
|------|--------------------|---------|-------|--------|
| Spike 1 | Does the Supabase Auth + Next.js session setup work end-to-end on Vercel preview? | 4 hours — Day 1 of Sprint 1 | Davit Karoiani | Working signup → home screen redirect on a Vercel preview URL; Vercel env vars confirmed |
| Spike 2 | Does a Postgres transaction in a Next.js server action correctly prevent double-join on the same match? | 3 hours — during S1-04 development | Levan Kovziridze | Local test with two concurrent requests: one succeeds, one receives error response |

---

*Risk Register | TheMergeConflicters | CS-PD-2026 | Spring 2026*
