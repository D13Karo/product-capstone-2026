# Error Budget

**Product:** CampusSport
**Team:** TheMergeConflicters
**Window:** 12 December 2025 to 11 January 2026 (first 30-day window starting from the Lab 10 audit date — used as the baseline window even though active monitoring begins post-Demo Day)
**Date last updated:** 12 December 2025

---

## Error Budget Summary

| SLO | Target | Window | Budget (minutes) | Consumed | Remaining | Status |
|-----|--------|--------|-----------------|----------|-----------|--------|
| Availability (`/healthz` 2xx) | 99% | 30 days | 432 min | 0 min (no monitoring yet) | 432 min | Green (provisional — monitoring starts Sprint 4) |
| `match_joined` activation success | 98% | 30 days | 864 min | 0 min (no production traffic yet) | 864 min | Green (provisional) |
| Push notification fan-out latency p95 | < 10 s p95 | 7 days | 504 min above threshold | 0 min | 504 min | Green (provisional — `push_metrics` table not yet shipped) |

**Status key:**
- **Green:** more than 50% of budget remaining
- **Amber:** 10% to 50% of budget remaining
- **Red:** less than 10% of budget remaining, or budget exhausted

**Caveat on this first window:** all three SLIs are currently `not yet measured` (see `slo-sheet.md` for the instrumentation action items). The "Green" status is provisional — it reflects "no incidents recorded" rather than "we verified no incidents occurred". Once instrumentation lands in Sprint 4, the next window (12 Jan – 10 Feb 2026) will be the first window with real measurements.

---

## Budget Calculation

All arithmetic shown explicitly so anyone on the team can verify the numbers without a spreadsheet.

### SLO 1: Availability (99% over 30 days)

```
SLO target: 99%
Allowed downtime rate: 1 − 0.99 = 0.01 = 1%

Window in minutes:
30 days × 24 hours × 60 minutes = 43,200 minutes

Error budget:
0.01 × 43,200 = 432 minutes per 30-day window

Equivalent in hours: 432 / 60 = 7.2 hours
Equivalent in days: 7.2 / 24 = 0.30 days
```

**Plain English:** we are allowed up to 7.2 hours of downtime in any rolling 30-day window before we have to stop shipping features and fix reliability.

---

### SLO 2: `match_joined` activation success rate (98% over 30 days)

```
SLO target: 98%
Allowed failure rate: 1 − 0.98 = 0.02 = 2%

Window in minutes:
30 days × 24 hours × 60 minutes = 43,200 minutes

Error budget:
0.02 × 43,200 = 864 minutes per 30-day window

Equivalent in hours: 864 / 60 = 14.4 hours
```

**Plain English:** for every 100 attempts to join a match across all users over 30 days, up to 2 may fail. If we cross that threshold, we are over budget.

**Caveat on units:** for SLO 2 we are measuring a request-success ratio, not downtime minutes. The minute-based budget is calculated for consistency with SLO 1, but in practice we will track this SLO as `(failed joins) / (total joins) × 100` over the rolling window and treat the 2% line as the freeze trigger.

---

### SLO 3: Push notification fan-out latency p95 < 10 s (7-day window)

```
SLO target: 95% of fan-outs complete in under 10 seconds
Allowed slow-fan-out rate: 1 − 0.95 = 0.05 = 5%

Window in minutes:
7 days × 24 hours × 60 minutes = 10,080 minutes

Error budget (time above threshold):
0.05 × 10,080 = 504 minutes per 7-day window

Equivalent in hours: 504 / 60 = 8.4 hours
```

**Plain English:** in a rolling 7-day window, up to 5% of match-edit-to-push-acknowledged times may exceed 10 seconds. If we cross that, we have a latency budget exhaustion and the freeze rules apply.

---

## Incident Log for This Window

Record every incident that consumed error budget. If no incidents occurred, write **"No incidents this window."**

| Incident | Date | Duration | SLOs affected | Budget consumed | Postmortem link |
|----------|------|----------|--------------|----------------|-----------------|
| No incidents this window. | — | — | — | — | — |

**Reason there are no incidents recorded:** active monitoring (UptimeRobot, PostHog funnel, `push_metrics`) has not yet been instrumented as of the audit date. Action items 1–4 in `slo-sheet.md` close this gap by end of Sprint 4 (11 June 2026). The first window with real incident data will start the day after Demo Day.

**Total budget consumed this window:**

| SLO | Budget consumed | Budget remaining | % remaining |
|-----|----------------|-----------------|-------------|
| Availability | 0 min | 432 min | 100% |
| Activation success | 0 min | 864 min | 100% |
| Push latency | 0 min above threshold | 504 min | 100% |

---

## Error Budget Policy

State what actions are triggered at each threshold.

| Budget remaining | Action |
|------------------|--------|
| **More than 50%** | Normal operations. Feature development continues. Sprint scope unchanged. |
| **10% to 50%** | **Amber alert.** Reliability items added to next sprint backlog. No risky deployments (e.g. DB migrations during peak hours) without a documented rollback plan. Mariam T. (SM) notifies the team in the next standup. |
| **Less than 10%** | **Red alert.** Feature freeze in effect. Engineering effort pivots to reliability. On-call review mandatory before any production push. |
| **0% or negative** | **Hard freeze.** No deployments at all. Incident review required for the window. SLO target itself is reviewed — if the budget has been exhausted three windows in a row, the SLO target may be too aggressive and needs revision. |

**Who owns the budget freeze decision:** Mariam Tskhomelidze (Scrum Master).
**Backup:** Davit Karoiani (PO).

The freeze is binary, not negotiable per-feature. If we are in a freeze, no feature ships — including features that "look small". The whole point of the policy is to break the temptation to ship "just one more" while the system is wobbling.

---

## Planned Maintenance

Planned maintenance consumes error budget just like unplanned incidents. Log it here.

| Maintenance activity | Date | Duration | SLOs affected | Budget consumed |
|--------------------|------|----------|--------------|----------------|
| (none planned in this window) | — | — | — | — |

**Anticipated upcoming maintenance (next 60 days):**
- Database migration to add `users.consent` JSONB column + `users.privacy_notice_version` column (per `consent-form.md` §6 gap 2 and 3). Expected duration: <2 minutes downtime if run during off-peak (3am Tbilisi time). Target window: Sprint 4.
- UptimeRobot probe activation against `/healthz` (no downtime — additive only). Target: Sprint 4.
- GitHub Actions CI dependency-audit workflow rollout (no production impact). Target: Sprint 4.

---

## Next Window

**Next window:** 12 January 2026 to 10 February 2026
**Budget resets:** 12 January 2026

Budget does not roll over. A full budget on the first of the next window does not compensate for an exhausted budget last window. Each window is evaluated independently.

**What we will know by the end of the next window that we do not know today:**
1. Whether our 99% availability target is achievable on Railway/Render free tier with real student-organiser traffic.
2. Whether the `match_joined` 2% failure budget is realistic or generous — the spot-race-condition path is the main unknown.
3. Whether the 10-second push latency target is too tight, too loose, or right.

If any of these turn out to be miscalibrated, the SLO target — not the budget arithmetic — gets revised. Lying about how reliable we are by setting an unreachable target is worse than honestly setting a lower one.

---

## Review Log

| Date | Reviewer | Notes |
|------|----------|-------|
| 12 December 2025 | Davit Karoiani (PO), Mariam Tskhomelidze (SM) | First baseline. All SLIs `not yet measured`. Instrumentation tracked as Sprint 4 action items in `slo-sheet.md`. |

---

*Error Budget | TheMergeConflicters | CampusSport | CS-PD-2026 | Spring 2026*
