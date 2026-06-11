# Repository Audit Checklist — Lab 12

**Team name:** TheMergeConflicters
**Repository URL:** https://github.com/D13Karo/product-capstone-2026
**Live product:** UniSport — https://unisport-412.pages.dev
**Date of audit:** 10 June 2026 (one day before Checkpoint 4 deadline)
**Audited by:** Davit Karoiani (PO)

---

## Section 1: Critical Requirements

| Requirement | Status | Owner | Deadline |
|-------------|--------|-------|----------|
| Repository visibility is set to **Public** | ⚠️ Verify on github.com | Davit | TODAY |
| Live product URL returns a working page | ✅ Verify https://unisport-412.pages.dev loads | Levan | TODAY |
| `05-fundraising/pitch-deck.pdf` exists | ❌ Missing — folder did not exist, now created. Content in `pitch-deck-content.md`; PDF must be exported from Google Slides | Davit | 11 Jun 23:00 |
| `README.md` contains the live product URL | ✅ Already present ("Live at https://unisport-412.pages.dev") | — | — |
| Open-source licence file (`LICENSE`) exists in repo root | ❌ Missing | Davit | TODAY |

### Critical gaps identified

1. **No `LICENSE` file** in repository root. Auto-deduction risk. Fix below.
2. **No `05-fundraising/pitch-deck.pdf`**. Auto-deduction risk. Content drafted; PDF export pending.
3. **Confirm GitHub repo is Public**. Open https://github.com/D13Karo/product-capstone-2026 in an incognito window — if it loads without login, it's public. If you get a 404, it's private.

---

## Section 2: Required Deliverable Files

| File | Status | Owner | Deadline |
|------|--------|-------|----------|
| `00-foundation/team-contract.md` | ✅ Complete | — | — |
| `00-foundation/team-problem-statement.md` | ✅ Complete | — | — |
| `00-foundation/team-icp.md` | ✅ Complete | — | — |
| `01-discovery/synthesis/patterns-analysis.md` | ✅ Complete | — | — |
| `01-discovery/synthesis/final-problem-statement.md` | ✅ Complete | — | — |
| `01-discovery/synthesis/competitive-landscape-seed.md` | ✅ Complete | — | — |
| `03-build/reliability/slo-sheet.md` | ✅ Complete (Lab 10) | — | — |
| `03-build/reliability/error-budget.md` | ✅ Complete (Lab 10) | — | — |
| `03-build/privacy-security/privacy-notice.md` | ✅ Present at `08-legal/privacy-notice.md` — rubric accepts either location | — | — |
| `04-gtm/financials/unit-economics.md` | ✅ Complete | — | — |
| `04-gtm/financials/12-month-model.xlsx` | ⚠️ Have `04-gtm/growth-projection.xlsx` at wrong path. **Action:** copy or rename to `04-gtm/financials/12-month-model.xlsx` (5 min) | Mariam T. | TODAY |
| `04-gtm/growth-strategy.md` | ✅ Complete | — | — |
| `05-fundraising/pitch-deck.pdf` | ❌ Missing — content drafted, PDF export needed | Davit | 11 Jun 23:00 |
| `05-fundraising/one-pager.pdf` | ❌ Missing — content drafted, PDF export needed | Mariam P. | 11 Jun 23:00 |
| `06-strategy/competitive-analysis.md` | ✅ Complete (Lab 11) | — | — |
| `06-strategy/moat-statement.md` | ✅ Complete (Lab 11, written as Moat Hypothesis) | — | — |
| `06-strategy/ecosystem-map.md` | ✅ Complete (Lab 11) | — | — |
| `07-team/contribution-logs/` (contains entries) | ⚠️ Folder now created but empty. **Action:** each member commits one contribution log entry before deadline. Template below. | All 4 | 11 Jun 20:00 |
| `08-legal/privacy-notice.md` | ✅ Complete (Lab 10) | — | — |
| `09-final/case-study.md` | ✅ Drafted in this session | — | — |
| `09-final/demo-video.md` | ✅ Plan drafted in this session; video shoot pending | Mariam T. + team | 11 Jun 21:00 |

---

## Section 3: README Quality Check

| Element | Present | Notes |
|---------|---------|-------|
| Product name and tagline | ✅ Partial | Has product name; tagline could be sharper. Recommended: "UniSport — Never miss a campus match again." |
| Problem statement (2 sentences) | ✅ Yes | Present and well-written |
| Live product URL (clickable link) | ✅ Yes | https://unisport-412.pages.dev |
| Demo video link | ❌ No | Add after video is shot (placeholder: "Demo video: see `09-final/demo-video.md`") |
| Team member names | ✅ Yes | All 4 listed with GitHub handles |
| Tech stack listed | ⚠️ Partial | Says "React Native + Expo" and "Django REST API" but no detailed tech stack. **Action:** add a "Tech Stack" section referencing `03-build/architecture/tech-stack.md` |
| Setup instructions (prereqs, install, run) | ❌ No | **Action:** add a "Setup" section with `git clone`, `npm install`, `expo start` commands |
| Architecture overview or diagram link | ⚠️ Partial | Should link to `03-build/architecture/architecture-diagram.png` |
| Open-source licence declaration | ❌ No | **Action:** add "Licensed under MIT — see LICENSE file" once LICENSE is added |

**Suggested README additions** (copy-paste-ready, see `09-final/readme-additions.md` — file does NOT yet exist; will be created if you ask):

---

## Section 4: Live Product Check

**Live URL:** https://unisport-412.pages.dev

| Question | Answer |
|---|---|
| Does the product load without error? | ⚠️ Verify in incognito window NOW |
| Can a new user sign up without special credentials? | ⚠️ Verify — KIU email domain gate may block external evaluators. Consider creating one demo account with credentials in presenter notes |
| Does the core user flow work end to end right now? | ⚠️ Verify by signing up + creating a match + joining it |
| When did you last successfully deploy? | TBD — check most recent deploy on Cloudflare Pages dashboard |
| Is your analytics dashboard collecting data? | ⚠️ Verify PostHog `match_joined` event is firing on real user actions |
| Current active user count from analytics | TBD — pull this number before pitch (see Slide 6 of `pitch-deck-content.md`) |

**Critical action before Demo Day:** create a **demo account** (e.g. `demo_evaluator@kiu.edu.ge`) and seed it with one upcoming match, so judges can sign in and try the product without needing a real KIU email. Document credentials in presenter notes only (do NOT commit them to the repo).

---

## Section 5: Priority Gap List

| Priority | Gap | Owner | Deadline |
|----------|-----|-------|----------|
| 1 | Add `LICENSE` (MIT) to repo root — auto-deduction risk | Davit | TODAY (5 min — see commands below) |
| 2 | Export `pitch-deck-content.md` to `pitch-deck.pdf` via Google Slides — auto-deduction risk | Davit | 11 Jun 21:00 |
| 3 | Export `one-pager-content.md` to `one-pager.pdf` via Canva or Google Docs | Mariam P. | 11 Jun 21:00 |
| 4 | Shoot the 60-second launch video per `09-final/demo-video.md` script, upload, link in README | Mariam T. + team | 11 Jun 21:00 |
| 5 | Move `04-gtm/growth-projection.xlsx` → `04-gtm/financials/12-month-model.xlsx` so the rubric path matches | Mariam T. | TODAY |
| 6 | Each team member commits one entry to `07-team/contribution-logs/` (Markdown file per person) | All 4 | 11 Jun 20:00 |
| 7 | Verify GitHub repo visibility is Public (incognito-window test) | Davit | TODAY |
| 8 | Update README with Tech Stack, Setup, Architecture link, Licence declaration | Davit | 11 Jun 18:00 |

---

## Quick-fix commands (run from `product-capstone-2026/`)

```powershell
# 1. Add LICENSE
# (LICENSE file content provided separately — copy the MIT text into a file named LICENSE in the root)

# 5. Fix the 12-month-model.xlsx path
Copy-Item 04-gtm\growth-projection.xlsx 04-gtm\financials\12-month-model.xlsx

# 6. Stub contribution logs
@'
# Contribution Log — Davit Karoiani

**Role:** Product Owner

## Sprint 1 (24 Apr – 7 May 2026)
- Story S1-01: User signup and login (3 pts) — shipped
- Drove the LMS → sports product pivot in Lab 2

## Sprint 2 (8 May – 21 May 2026)
- Story S2-01: Organiser creates match (3 pts) — shipped
- Owns PostHog event instrumentation review

## Sprint 3 (22 May – 4 Jun 2026)
- Story S3-01: Quorum visibility on match cards (3 pts) — shipped
- Story S3-05: Sport-type filter (1 pt) — shipped

## Sprint 4 (5 Jun – 11 Jun 2026)
- Story S4-03: Demo Day prep — in progress
- Wrote Lab 10 privacy notice, consent form
- Wrote Lab 11 moat hypothesis
'@ | Out-File -Encoding utf8 07-team\contribution-logs\davit-karoiani.md

# Each team member writes one similar file.

# 7. Verify repo is public
Start-Process "https://github.com/D13Karo/product-capstone-2026"
# Open the same URL in an incognito window. If it loads, it's public.
```

---

## Audit Sign-Off

| Name | Role | Signature |
|------|------|-----------|
| Davit Karoiani | PO | DK 10 Jun 2026 |
| Mariam Pirtskhalava | Discovery Lead | MP 10 Jun 2026 |
| Mariam Tskhomelidze | Scrum Master | MT 10 Jun 2026 |
| Levan Kovziridze | Test Lead | LK 10 Jun 2026 |

---

## One naming inconsistency flagged

The academic docs (Lab 10, 11) use **CampusSport** as the product name. The live deployed product is **UniSport** (see README live URL and the frontend `package.json` `"name": "unisport"`). The pitch deck, one-pager, video, and case study (drafted in this session) all use **UniSport** to match what judges see when they click the live URL — consistency between pitch and live product is more important for Demo Day scoring than backwards-fixing the academic docs.

**If you have 10 minutes after Demo Day:** do a find-replace across `00-foundation/`, `03-build/`, `04-gtm/`, `06-strategy/`, `08-legal/` to standardize on UniSport. Not blocking for Checkpoint 4.

---

*Repository Audit Checklist | TheMergeConflicters | UniSport | Lab 12 | 10 June 2026*
