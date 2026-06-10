# SLO Sheet

**Product:** CampusSport
**Team:** TheMergeConflicters
**Date:** 12 December 2025
**Review cadence:** Monthly, and after every SEV1 or SEV2 incident

---

## Overview

This document defines our Service Level Indicators, Service Level Objectives, and severity definitions for CampusSport. These are internal commitments, not customer-facing SLAs. They exist to make reliability visible to the team and to give us a principled way to decide when to stop shipping features and invest in stability instead.

The error-budget arithmetic backing these SLOs lives in `error-budget.md`.

---

## Glossary

**SLI (Service Level Indicator):** A specific metric we measure. The raw number.
**SLO (Service Level Objective):** The target we set for an SLI over a time window. An internal commitment.
**SLA (Service Level Agreement):** A contractual commitment to a customer with consequences for breach. CampusSport has none — students do not pay and have not signed an SLA.
**Error budget:** The amount of unreliability the SLO allows. Budget = (1 − SLO target) × time window.

---

## SLI and SLO Definitions

### SLO 1: Availability of the deployed app

**SLI definition:**
- Metric: Percentage of probe requests to `GET /healthz` on the Django backend that return a 2xx response within 5 seconds.
- Formula: `successful_health_probes / total_health_probes × 100`
- Measured by: An external uptime monitor (UptimeRobot free tier, 5-minute interval) — the simplest tool that does not depend on the host being healthy to measure host health.
- Measurement frequency: Every 5 minutes.
- Current measured value: **not yet measured** — UptimeRobot integration is an action item below.

**SLO target:**
- Target: **99% availability**
- Time window: Rolling 30 days
- Why this target is achievable: Railway and Render both publish ≥99.5% availability for their managed Postgres + Django containers on the free / hobby tier. Vercel and Netlify both target ≥99.9% for static frontend hosting. Setting our SLO at 99% leaves headroom for our own deploys, migrations, and student-team incident response time (we are not a 24/7 on-call team). 99.99% would be a lie at our team capacity.

**Error budget:** 432 minutes per 30-day window. Full arithmetic in `error-budget.md` §SLO 1.

---

### SLO 2: `match_joined` activation success rate

**SLI definition:**
- Metric: Percentage of `POST /matches/<id>/join` requests that complete with a 2xx response and result in a `match_joined` PostHog event firing within the same session.
- Formula: `(POST /join 2xx responses with subsequent match_joined event) / (total POST /join attempts) × 100`
- Measured by: PostHog funnel — `POST /matches/<id>/join attempted` → `match_joined`. The `attempted` event is added on the client right before the network call; the `match_joined` event fires on server confirmation.
- Measurement frequency: Calculated daily from event data; reviewed weekly in standup.
- Current measured value: **not yet measured at production scale** — pre-launch smoke testing shows >99% on a single-user happy path.

**SLO target:**
- Target: **98% activation success rate**
- Time window: Rolling 30 days
- Why this target is achievable: The activation flow is one POST endpoint, one DB write, one PostHog event. Failure modes are network drop, backend 5xx, or DB constraint (spot taken between client view and click). The "spot taken" race condition is a legitimate failure that contributes to the 2% budget; we do not need to push it to 0%. 100% would mean no error budget, which is a lie (see §Anti-pattern note below).

**Error budget:** 864 minutes per 30-day window. Full arithmetic in `error-budget.md` §SLO 2.

---

### SLO 3 (optional, recommended): Push notification delivery latency

**SLI definition:**
- Metric: Time from an organiser-edit save (`match.updated_at`) to the corresponding push notification being acknowledged as received by Expo Push Service (`expo.send` returns success).
- Formula: `expo_push_send_completed_at − match_updated_at` per fan-out, measured in milliseconds, taken as p95 over the window.
- Measured by: Custom server timing — backend records the start time on edit save and the success time after `expo.send` returns. Stored as a row in a lightweight `push_metrics` table.
- Measurement frequency: Continuous; p95 computed daily.
- Current measured value: **not yet measured** — push_metrics table not yet created.

**SLO target:**
- Target: **p95 below 10 seconds from match edit to Expo Push Service acknowledgement**
- Time window: Rolling 7 days
- Why this target is achievable: Expo Push Service typically acks within 1–3 seconds. Our backend processing (fetch RSVPs, build payloads, batch call) is the variable. 10 seconds is conservative and leaves room for the first push notification a player receives — which is the moment that matters for the product promise of "you find out about the change in time".

**Error budget:** Threshold SLO — no more than 5% of fan-outs in the 7-day window may exceed 10 seconds. Full arithmetic in `error-budget.md` §SLO 3.

---

## Anti-pattern note: why no SLO is set to 100%

An SLO of 100% means an error budget of 0 minutes. An error budget of 0 means we cannot ship anything without violating the SLO — every deploy, every migration, every backend redeploy creates the risk of a tiny outage. A 100% SLO is not a target; it is a commitment to never ship again. We refuse this anti-pattern. 99% (availability) and 98% (activation) are the highest honest targets we can hold ourselves to.

---

## Error Budget Policy

When any SLO error budget is exhausted in a given window:

1. **No new feature deployments** until the window resets or the budget is partially restored through reliability work.
2. **Engineering effort in the next sprint pivots** to reliability improvement, not feature work. The product backlog is paused for that week.
3. **An incident review is mandatory** before the next production push, even if no single incident caused the budget exhaustion (sometimes it is a thousand small failures rather than one big one).
4. **Notification:** the team is informed in standup the morning after budget exhaustion is detected.

**Who owns the error budget decision:** Mariam Tskhomelidze (Scrum Master). She calls the freeze, communicates it to the team, and signs off on lifting it. This is the same role she holds for sprint scope, so it is a natural fit.

**Backup decision-maker:** Davit Karoiani (PO), in case Mariam T. is unreachable for >24 hours during an exhausted-budget window.

---

## Severity Definitions

Calibrated to CampusSport. A booking app has different criteria than a content platform.

### SEV1: Core flow completely down

**Definition for CampusSport:**
- No user can complete a `match_joined` action (the activation moment) for ≥10 minutes across any university, **OR**
- The backend API is returning 5xx on ≥50% of all requests for ≥5 minutes, **OR**
- The deployed frontend (Vercel/Netlify) is unreachable globally, **OR**
- A confirmed data breach affecting personal data (see privacy-notice.md §6).

**Response:** All four team members notified immediately. Whoever is on-call this week starts mitigation; others stop what they are doing and join the channel.

**Communication:** Post in team Discord `#incidents` channel within 5 minutes of detection. Pin the post.

**Target time to acknowledge:** 15 minutes
**Target time to mitigate (restore service, even if not yet root-caused):** 2 hours
**Target time to publish a written postmortem:** 7 days

---

### SEV2: Degraded experience or partial outage

**Definition for CampusSport:**
- Push notifications delayed by >5 minutes for any cohort of users for ≥30 minutes (the differentiator feature is silently failing — players are not getting the updates they joined for), **OR**
- `match_joined` error rate above 5% for ≥30 minutes (the activation flow is unreliable), **OR**
- A non-core feature is broken (e.g. share-link, match-result logging) but join + view still work, **OR**
- An SLO is in Amber on the error-budget dashboard (10–50% remaining).

**Response:** On-call team member investigates. Others notified but not interrupted from their current sprint work.

**Communication:** Post in team Discord `#incidents` within 30 minutes of detection.

**Target time to acknowledge:** 30 minutes
**Target time to mitigate:** 8 hours
**Target time to publish a written postmortem:** 14 days

---

### SEV3: Minor issue, no or minimal user impact

**Definition for CampusSport:**
- Cosmetic UI bug (misaligned icon, wrong colour, typo), **OR**
- A single rare error path that affects <1% of users (e.g. one specific browser version), **OR**
- A monitoring or analytics gap (e.g. one event not firing) that does not block users.

**Response:** Logged and scheduled for the next working session. No interruption to anyone.

**Communication:** GitHub issue created with `SEV3` label.

**Target time to acknowledge:** Next working day
**Target time to fix:** Next sprint
**Postmortem required:** No

---

## On-Call Rotation

Even as a four-person student team, we assign an on-call week per person. This distributes the burden and ensures every team member understands the operational side of what they have built.

**Rotation start: 5 January 2026 (post-Demo Day production operations).**

| Week of | On-call | Backup |
|---------|---------|--------|
| 5 Jan 2026 | Davit Karoiani | Mariam Tskhomelidze |
| 12 Jan 2026 | Mariam Tskhomelidze | Levan Kovziridze |
| 19 Jan 2026 | Levan Kovziridze | Mariam Pirtskhalava |
| 26 Jan 2026 | Mariam Pirtskhalava | Davit Karoiani |
| 2 Feb 2026 | Davit Karoiani | Mariam Tskhomelidze |
| (continues rotating, same order) | | |

**On-call responsibilities:**
1. Check the deployment URL once per day (one minute task — load the app and tap Join on a test match).
2. Check the UptimeRobot dashboard once per day.
3. Respond to any SEV1 or SEV2 alert within the target times above.
4. Create a GitHub issue for any alert that fires, even if it resolves on its own.
5. At the end of the week, write a one-paragraph handover note for the next on-call person.

---

## Action Items from This Audit

| # | Action | Owner | Target |
|---|--------|-------|--------|
| 1 | Add `GET /healthz` endpoint to Django backend returning 200 with DB connection check | Davit Karoiani | Sprint 4 |
| 2 | Set up UptimeRobot 5-minute probe against `/healthz` and against the frontend URL | Levan Kovziridze | Sprint 4 |
| 3 | Add the `attempted` PostHog event right before the `POST /join` network call so the funnel denominator is measurable | Davit Karoiani | Sprint 4 |
| 4 | Create `push_metrics` table and instrument start/end timestamps in the fan-out worker | Mariam Tskhomelidze | Sprint 4 |
| 5 | Spin up the error-budget dashboard (lightweight Looker Studio over PostHog export, or a manual weekly review note in `error-budget.md`) | Mariam Tskhomelidze | Sprint 4 |
| 6 | Confirm the on-call rotation start date with all team members | Mariam Tskhomelidze | This week |

---

*SLO Sheet | TheMergeConflicters | CampusSport | CS-PD-2026 | Spring 2026*
