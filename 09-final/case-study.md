# Case Study — UniSport

**Team:** TheMergeConflicters
**Product:** UniSport — https://unisport-412.pages.dev
**Course:** CS-PD-2026 Product Development for Software Engineers
**Semester:** Spring 2026
**Date:** 10 June 2026

> **Note on this document:** the factual / historical sections (timeline, sprint outputs, evidence references) are drawn from the existing repo. The two "team reflection" sections at the bottom — **What we learned** and **What we'd do differently** — are placeholders for the team to fill in together. Don't pitch them; they belong to you.

---

## What we set out to do

**Week 1 hypothesis (v1.0 problem statement, 25 March 2026):**

> "LMS notifications are unreliable; KIU students miss important academic updates."

We picked this because it was visible to us as students. Within two weeks of interviewing, the hypothesis was dead and we had pivoted to informal sports coordination.

---

## What happened, by phase

### Phase 1: Discovery and the pivot (Weeks 2–4, March–April 2026)

The LMS hypothesis collapsed within the first three interviews. Interviewees described missing pickup football games and chasing schedule changes across Messenger, Facebook, and WhatsApp — not LMS. We re-committed to a sports-coordination problem on **27 March 2026** (v2.0).

By Week 4 we had 10 customer discovery interviews with three validated pain patterns:

| Pattern | Validation | Avg pain |
|---|---|---|
| Information fragmentation across multiple platforms | 10/10 interviews | 4.0 / 5 |
| Schedule changes lost in chat noise | 9/10 interviews | 4.1 / 5 |
| Organizer coordination burden | 7/10 interviews | 4.5 / 5 |

Two findings re-shaped the product:

1. **The organizer is the primary user.** Every workaround documented in the interviews broke at the organizer layer — Cotne (I-10), Gega (I-02). We made "zero extra work from the organizer" a hard design constraint, sourced from Cotne (I-10): *"Any solution needs to require zero extra work from the organizer."*
2. **Push, never pull.** Giorgi (I-03) muted the noisy group to fix the noise and missed the game as a result. Any solution had to push, not require checking.

Full evidence trail: `01-discovery/interview-logs/`, `01-discovery/synthesis/final-problem-statement.md`.

### Phase 2: Design and Build (Sprints 1–3, April–June 2026)

| Sprint | Dates | Theme | Key output |
|---|---|---|---|
| Sprint 1 | 24 Apr – 7 May | Foundation | End-to-end user flow deployed: KIU email signup, match list, match detail, one-tap Join. Vercel/Cloudflare deployment live by 7 May. |
| Sprint 2 | 8 May – 21 May | Instrumentation | Organizer match creation, push notifications via Expo Push Service, usage tracking around the NSM (`match_joined`) read from the Django admin. |
| Sprint 3 | 22 May – 4 Jun | Growth | Quorum visibility on match list, share/invite, organizer cancel-with-auto-notify, match result logging, sport filter. |
| Sprint 4 | 5 Jun – 11 Jun | Demo prep | Pitch, one-pager, video, repo audit. |

Sprint 2 also ran our **smoke test** — a Carrd landing page with the value proposition, distributed in KIU sports group chats from 13–21 May. Pre-set success threshold: 25% conversion. Results in `04-gtm/traction/`.

### Phase 3: Strategy and Compliance (Labs 9–11, May–June 2026)

In parallel with build:

- **Lab 9 — Growth & unit economics:** `04-gtm/financials/unit-economics.md` (LTV $21.38, blended CAC $0.64, LTV:CAC 33.4:1 with documented sensitivity analysis), `04-gtm/growth-strategy.md` (3 channels with experiment-derived CACs).
- **Lab 10 — Compliance, security, reliability audit:** GDPR-aligned privacy notice (`08-legal/privacy-notice.md`), consent flow design with named gaps (`03-build/privacy-security/consent-form.md`), STRIDE applied to 5 highest-traffic flows (`03-build/privacy-security/security-tabletop.md`), SLO + error budget (`03-build/reliability/`).
- **Lab 11 — Strategic analysis:** competitive matrix with 6 named competitors × 10 dimensions (`06-strategy/competitive-analysis.md`), Blue Ocean strategy canvas with ERRC framework (`06-strategy/strategy-canvas.md`), ecosystem map naming KIU Sports & Wellness as priority partner and the KIU IT in-house build as the primary institutional threat (`06-strategy/ecosystem-map.md`), Moat **Hypothesis** (Switching Costs primary, Network Effects emerging) with explicit evidence-collection plan (`06-strategy/moat-statement.md`).

---

## Where we are at Demo Day (11 June 2026)

| Dimension | Status |
|---|---|
| Live product at https://unisport-412.pages.dev | ✅ Deployed and reachable |
| 10 validated customer interviews | ✅ Documented in `01-discovery/interview-logs/` |
| North Star Metric measurable from the Django admin | ✅ `match_joined` (joins) per active user per week |
| Smoke test results | ✅ Carrd + Google Form, May 13–21 |
| GDPR-aligned privacy notice | ✅ `08-legal/privacy-notice.md` |
| STRIDE security tabletop | ✅ `03-build/privacy-security/security-tabletop.md` |
| SLO + error budget defined | ✅ 99% / 98% / p95 < 10s with policy |
| Competitive analysis + moat hypothesis | ✅ `06-strategy/` (4 files) |
| Unit economics with sensitivity check | ✅ `04-gtm/financials/unit-economics.md` |
| Growth strategy with 3 channels and CACs | ✅ `04-gtm/growth-strategy.md` |
| Multi-university expansion plan | ✅ Free Uni / ISU / TSU per `06-strategy/ecosystem-map.md` |

---

## What's next (post-Demo-Day)

The roadmap after Demo Day, drawn from existing repo documents:

**Track A — close the moat evidence gap (`06-strategy/moat-statement.md`):**
Organizer 4-week retention cohort from the Django admin / a SQL query by 8 July. Organizer history-depth SQL by 8 July. Re-interview 3 of the original 10 interviewees by 15 July. If retention > 25%, Switching Costs is confirmed as primary. If < 25%, fallback to Counter-Positioning is named in the same document.

**Track B — convert the KIU institutional relationship (`06-strategy/ecosystem-map.md`):**
KIU Sports & Wellness Office pilot for autumn 2026 — first action: introduction email by 17 June.

**Track C — multi-university expansion:**
Identify named sports coordinators at Free Uni, ISU, TSU by 15 July. Convert one organizer per university by 11 August. The first non-KIU `match_joined` event is the cross-institution milestone.

---

## What we learned the hard way

*[Draft — edit in your own words. These are grounded in what actually happened; make them yours before Demo Day.]*

- **The first idea was wrong, and interviews killed it fast.** Our Week-1 hypothesis was unreliable LMS notifications. Three interviews in, nobody cared about the LMS — they cared about missing pickup games. We learned to let evidence kill an idea early instead of defending it for weeks.
- **The organiser is the whole product, not the players.** Every workaround our interviewees built failed at the same point: the organiser wouldn't move (Cotne's working Telegram bot, I-10). Once we designed for the one person who posts the match, the rest of the product fell into place.
- **Simpler beat impressive on analytics.** We set up PostHog, then realised we were maintaining an event pipeline we didn't need — reading our own Django admin gave us every number at our scale. Cutting it was the right call and a cleaner privacy story.
- **Small real numbers beat big fake ones.** ~18 signups felt embarrassing to put on a slide, but it's verifiable in our own admin — and that's exactly what the rubric (and a real investor) rewards.

---

## What we'd do differently

*[Draft — edit in your own words.]*

- **Seed both sides before launching.** We brought on players faster than organisers, so early feeds looked empty. Next time we'd line up ~5 organisers first, then open the doors to players.
- **Pick the analytics approach once, on day one.** We churned between PostHog and the Django admin and paid for the rework. We'd choose the admin from the start and spend the time on the product.
- **Lock the product name immediately.** We shipped as UniSport but wrote half the academic docs as CampusSport, which created avoidable cleanup.
- **Instrument the activation metric earlier.** If we'd tracked joins-per-user from Sprint 1, we'd have a real retention curve at Demo Day instead of just totals.

---

## Final reflection

*[Draft — make it the team's own voice before Demo Day.]*

Standing at Demo Day, the thing we're proudest of isn't the size of the numbers — it's that they're real. A handful of actual KIU students signed up to a product we deployed, used it to join real matches, and we can show every one of those numbers live in our own admin. We started the semester with the wrong idea and let the interviews drag us to the right one. The one thing we want a judge to walk away with: we found a genuinely validated pain, built the simplest thing that solves it, and we never inflated a single figure to make it look better than it is.

---

*Case Study | UniSport | TheMergeConflicters | CS-PD-2026 | Spring 2026*
