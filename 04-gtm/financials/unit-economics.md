# Unit Economics Analysis

**Team:** TheMergeConflicters
**Product:** CampusSport
**Document version:** 1.0
**Last updated:** 13 May 2026

---

## 1. Lifetime Value (LTV)

The product is currently free for students. We use a value proxy because there is no revenue model in the MVP. The proxy reflects the concrete value the product creates for users: time saved on coordination friction and matches attended that would otherwise have been missed.

### Formula

```
LTV = ARPU × Gross Margin × Lifetime
```

### Inputs

**Value proxy per user per month (ARPU):** $2.25

Source: Estimated as follows. An average active KIU sports participant plays 2 matches per week (8 sessions per month). Without the product, each session requires approximately 15 minutes of checking group chats, sending confirmation messages, and monitoring for time or location changes — confirmed across all 10 interviews. With the product, this drops to approximately 1 minute per session. Time saved: 14 minutes × 8 sessions = 112 minutes = 1.87 hours per month.

At a student opportunity cost of $0.85 per hour (regional minimum wage equivalent): 1.87 × $0.85 = $1.59.

Additionally, from our interviews, an estimated 0.5 matches per month per user are disrupted by a missed update (cancelled match, time change not seen). Each disrupted match costs approximately 30 minutes of travel plus 60 minutes on-site: 1.5 hours × $0.85 = $1.28. Expected disruption cost avoided per user per month: 0.5 × $1.28 = $0.64.

Total value proxy: $1.59 + $0.64 = **$2.23**, rounded to **$2.25**.

We acknowledge this is a soft estimate based on interview evidence, not measured user behaviour. We will revisit once we have PostHog session data showing actual match_joined frequency per user.

**Gross Margin:** 95%

Calculation:
- Value proxy per user per month: $2.25
- Cost to serve per user per month: $0.11
  - Vercel hosting (free hobby tier for MVP scale): $0
  - Supabase Postgres (free tier, up to 500MB): $0
  - PostHog Cloud (free tier, up to 1M events/month): $0
  - Allocated infrastructure and tooling cost per user at scale: $0.11 (estimated)
- Gross margin = ($2.25 − $0.11) / $2.25 = 95.1% ≈ **95%**

**Average Lifetime:** 10 months

Source: We have no retention data yet. Estimate based on the KIU academic calendar: a student who adopts the product mid-semester will use it through two full semesters of active sports participation (approximately 10 months of the academic year, excluding summer). We treat this as the upper conservative estimate — a student who loses interest after one semester contributes 5 months. The 10-month estimate reflects a user who becomes a habit adopter. This will be replaced with a real retention curve once we have 12 weeks of cohort data from PostHog.

### Calculation

```
LTV = $2.25 × 95% × 10 months
LTV = $2.14 × 10
LTV = $21.38
```

We use **$21.38** throughout this model.

### If Product Becomes Paid (Future Reference)

If the product introduces a premium tier at $2 per month per organiser (organisers pay for advanced features such as match templates, recurring schedules, and participant history), LTV for the organiser segment would be:
```
LTV (organiser) = $2.00 × 90% margin × 20 months = $36.00
```
Not modelled in Sprint 2 — noted for future reference only.

---

## 2. Customer Acquisition Cost (CAC) per Channel

We value founder and team time at $5 per hour (regional student opportunity cost). This is a conservative but defensible internal rate.

### Channel 1 -- Organiser outreach (direct Messenger contact)

| Item | Value |
|------|-------|
| Ad spend | $0 |
| Founder/team time (3 hrs/week × 2 weeks × 2 people × $5/hr) | $60 |
| Tooling specific to this channel | $0 (existing Messenger accounts) |
| Agency or contractor fees | $0 |
| **Total spend** | **$60** |
| Customers acquired (expected case: 5 organisers × 20 players each) | 100 |
| **CAC** | **$0.60** |

**Source for spend:** Team time at student labour rate; no ad spend required for personal Messenger outreach.

**Source for customers acquired:** Each converted organiser posts at least 1 match in the first 2 weeks. Their existing player group (15–30 people per organiser, confirmed from interview data — Interview #05 Mamuka manages ~25 regular players) receives the match link and a portion signs up. Expected case: 5 organisers × 20 player signups each = 100 total. This channel compounds: an organiser who posts weekly continues generating player signups beyond the initial 2-week window.

---

### Channel 2 -- KIU sports group chats (Messenger and WhatsApp)

| Item | Value |
|------|-------|
| Ad spend | $0 |
| Founder/team time (2 hrs/week × 3 weeks × 1 person × $5/hr) | $30 |
| Tooling | $0 (existing accounts) |
| Agency or contractor fees | $0 |
| **Total spend** | **$30** |
| Customers acquired (expected case) | 60 |
| **CAC** | **$0.50** |

**Source for spend:** One team member (Mariam T., Scrum Master) manages group chat posting as part of her experiment monitoring role. 2 hours per week covers posting, responding to questions, and tracking UTM data.

**Source for customers acquired:** Our smoke test (May 13–21) targets 50+ unique visitors with a 25%+ conversion target. Extrapolating to 3 weeks of active posting across 5–8 groups, we estimate 200–300 unique link views and 20–25% signup conversion = 40–75 signups. Expected case: 60 signups. Source: our own experiment data + Mailchimp 2024 industry benchmark of 20–30% conversion for high-relevance community link shares.

---

### Channel 3 -- QR code posters at KIU sports facilities

| Item | Value |
|------|-------|
| Print cost (20 A4 colour posters at $1.00 each at campus print shop) | $20 |
| Founder time (1 hr installation + 0.5 hr/week refresh × 4 weeks × $5/hr) | $15 |
| Tooling | $0 |
| Agency or contractor fees | $0 |
| **Total spend** | **$35** |
| Customers acquired (expected case) | 35 |
| **CAC** | **$1.00** |

**Source for spend:** Campus print shop rate confirmed. One team member installs posters in ~1 hour across 5 locations.

**Source for customers acquired:** 5 poster locations. Estimated 10–15 QR code scans per week across all locations (conservative — sports facilities are used daily but not all passers-by scan). Over 4 weeks: ~50 scans. Scan-to-signup conversion estimated at 60–70% due to high in-context intent (user is physically at the sports facility). Expected: 50 scans × 65% = 32 → rounded to 35. Source: estimate only; UTM tracking from the QR code will give us real data within one week of posting.

---

### Blended CAC

```
Blended CAC = total spend / total customers acquired
Blended CAC = ($60 + $30 + $35) / (100 + 60 + 35)
Blended CAC = $125 / 195
Blended CAC = $0.64
```

---

## 3. LTV to CAC Ratio

### Per Channel

| Channel | LTV | CAC | Ratio | Interpretation |
|---------|-----|-----|-------|----------------|
| Organiser outreach | $21.38 | $0.60 | 35.6x | Healthy; LTV is a proxy estimate not measured revenue |
| Group chats | $21.38 | $0.50 | 42.8x | Healthiest; almost entirely founder time, no ad spend |
| QR posters | $21.38 | $1.00 | 21.4x | Healthy; lower than digital channels but captures high-intent users |

### Blended

```
LTV : Blended CAC = $21.38 : $0.64 = 33.4x
```

### Interpretation

The ratios look strong because all three channels are organic or near-zero-cost, and our LTV is based on a value proxy rather than real revenue. If we replace the value proxy with the lower-bound estimate of $1.00 per user per month (the minimum a student would consciously feel the product is "worth"), LTV becomes $9.50 and the blended ratio drops to 14.8x. Still comfortably above the 3:1 healthy threshold, but a useful calibration exercise.

The more important constraint is not the ratio — it is reach. The total addressable market at KIU is approximately 300–500 active sports participants. All three channels combined will exhaust that audience within 2–3 sprints. Before Sprint 4, we need a plan to expand beyond KIU to other Georgian universities, otherwise growth flatlines at the KIU ceiling.

---

## 4. Payback Period

```
Payback = CAC / (Value proxy × Gross Margin per month)
Monthly gross margin per user = $2.25 × 95% = $2.14
```

### Per Channel

| Channel | CAC | Monthly margin per user | Payback (months) |
|---------|-----|-------------------------|------------------|
| Organiser outreach | $0.60 | $2.14 | 0.28 |
| Group chats | $0.50 | $2.14 | 0.23 |
| QR posters | $1.00 | $2.14 | 0.47 |

### Target

For a consumer product, under 6 months is the standard target. Our payback period is under 1 month across all channels, which is effectively instant. This metric becomes more meaningful when we introduce a paid tier for organisers — payback will extend to 2–3 months at that point and is worth re-running this model then.

### Interpretation

Payback periods under 1 month mean we recover acquisition cost almost immediately. The business model is not constrained by payback at MVP scale. The binding constraint is reach (see Section 3 Interpretation) and supply-side adoption (organiser onboarding).

---

## 5. Assumptions and Sources

| Assumption | Value | Source | Confidence |
|------------|-------|--------|------------|
| Average sessions per user per month | 8 (2×/week) | 10/10 interviews confirmed regular weekly sports participation | Medium |
| Coordination time per session without product | 15 minutes | Interview triangulation: Nia (I-04) "12 unread chats", Misho (I-07) "game time, location, done — why is that so hard?" | Medium |
| Coordination time per session with product | 1 minute | Internal product design assumption | Low |
| Student time value (hourly) | $0.85 | Georgian regional minimum wage equivalent | Medium |
| Disrupted matches per user per month | 0.5 | Interview #03 (Giorgi) — missed game due to unread update; estimated 1 per 2 months | Low |
| Value proxy per user per month | $2.25 | Internal calculation from above | Low |
| Gross margin | 95% | Free-tier infrastructure confirmed: Vercel, Supabase, PostHog all within free limits at MVP scale | High |
| Average lifetime | 10 months | Academic calendar estimate; no retention data yet | Low |
| Group chat signup conversion | 20–25% | Own smoke test target + Mailchimp 2024 community link benchmark | Low |
| Organiser player group size | 15–30 per organiser | Interview #05 (Mamuka): manages ~25 regular players | Medium |
| Poster scan-to-signup conversion | 65% | Estimate; high intent assumed for users at sports facility | Low |

---

## 6. Sensitivity Analysis

**Riskiest assumption:** Average lifetime of 10 months.

**Current value:** 10 months

**If it is half what we expect (5 months):** LTV = $2.25 × 95% × 5 = $10.69. Blended LTV:CAC = $10.69 / $0.64 = 16.7x. Still healthy; no strategic change required.

**If it is double what we expect (20 months):** LTV = $2.25 × 95% × 20 = $42.75. Blended ratio = 66.8x. Same conclusion with more confidence.

**Why this is the riskiest assumption:** Most students may use the product for one sports season (one semester) and stop when their playing group stops coordinating actively, or when the academic year ends. We have no evidence about whether a student returns to the app the following year. A 5-month average lifetime is entirely plausible and would halve our LTV estimate.

**What we will do to validate this assumption in Sprint 2:** Begin tracking D30 retention from the first sign-up cohort in May. PostHog `user_session_started` events will give us a weekly active user curve. At week 8, run a cohort analysis to see how many of the Sprint 1 cohort are still returning.

---

## 7. What We Will Refine and When

| Number | Currently | Replace by | How |
|--------|-----------|------------|-----|
| Group chat signup conversion rate | 20–25% (smoke test target) | May 21 | Carrd + Google Form experiment data after 8 days live |
| Organiser player group size | 15–30 per organiser (interview estimate) | June 4 | Direct count from first 5 organiser onboardings |
| Average lifetime | 10 months (calendar estimate) | End of Sprint 3 | PostHog D30 and D60 retention from May cohort |
| Value proxy ($2.25/month) | Internal estimate | If monetisation is introduced | Fine as proxy for free product model |
| Poster scan rate | Estimate | May 27 | One week of UTM data after posters are installed |

---

**Filed by:** Davit Karoiani, Mariam Tskhomelidze, Mariam Pirtskhalava, Levan Kovziridze
