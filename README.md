# TheMergeConflicters — Product Capstone 2026

| Field | Details |
|---|---|
| **Course** | CS-PD-2026 Product Development for Software Engineers |
| **Semester** | Spring 2026 |
| **Institution** | Kutaisi International University |

---

## The Problem We're Solving

KIU students who participate in informal peer-organized sports events consistently miss game time or location changes, arrive unprepared, or show up to cancelled events because schedule updates get buried in the noise of general-purpose group chats (Messenger, Facebook, WhatsApp) spread across multiple platforms with no single source of truth.

**Evidence base:** 10 interviews · 68 affinity insights · 3 validated patterns · Average pain intensity 4.0/5

---

## The Product

**UniSport** — Live at https://unisport-412.pages.dev — a platform that connects university students to informal sports matches on campus. Students sign up with their university email, see only their university's matches, and get notified when match times or locations change.

| Resource | URL |
|---|---|
| **Live app** | https://unisport-412.pages.dev |
| **Backend API** | https://sportactivityappbackend.onrender.com |
| **Admin panel** | https://sportactivityappbackend.onrender.com/admin/ |

| Repo | Description |
|---|---|
| [Frontend](https://github.com/D13Karo/SportActivityAppFRONTEND) | React Native + Expo mobile app |
| [Backend](https://github.com/D13Karo/SportActivityAppBACKEND) | Django REST API |

---

## Team

| Name | Role | GitHub |
|---|---|---|
| Davit Karoiani | Product Owner | [@D13Karo](https://github.com/D13Karo) |
| Mariam Pirtskhalava | Discovery Lead | [@pircxo](https://github.com/pircxo) |
| Mariam Tskhomelidze | Scrum Master | [@ZONDROK](https://github.com/ZONDROK) |
| Levan Kovziridze | Test Lead | [@Leo-21-K](https://github.com/Leo-21-K) |

---

## Current Phase

**Sprint 2** (May 8 – May 21) — Organiser match creation, push notifications, usage tracking via the Django admin. Checkpoint 3 due May 21.

---

## Repository Structure

```
00-foundation/         Team contract, problem statement, ICP profiles
01-discovery/          Interview scripts, logs, synthesis, competitive landscape
02-design/             Prototypes (Stitch), usability findings
03-build/              Roadmap, sprint plans, architecture, analytics, experiments,
                       privacy-security (consent, security tabletop), reliability (SLO, error budget)
04-gtm/                Growth strategy, unit economics, loops and moats, traction
05-fundraising/        Pitch deck, one-pager (PDFs for Demo Day)
06-strategy/           Competitive analysis, strategy canvas, ecosystem map, moat statement
07-team/               Contribution logs per team member
08-legal/              Privacy notice (GDPR-aligned)
09-final/              Case study, demo video plan, repo audit
docs/                  AI usage log, standup log
milestones/            Weekly milestone tracking
```

---

## Tech Stack

| Layer | Choice | Source |
|---|---|---|
| Frontend | React Native + Expo SDK 51 | `03-build/architecture/tech-stack.md` |
| Navigation | Expo Router (file-based) | tech-stack.md |
| Backend | Django 6.0.5 + DRF + SimpleJWT | `SportActivityApp/backend/requirements.txt` |
| Database | PostgreSQL (managed: Railway/Render) | tech-stack.md |
| Push notifications | Expo Push Service → APNs/FCM | tech-stack.md |
| Email (transactional) | SendGrid HTTPS API | tech-stack.md |
| Analytics | Django admin over our own PostgreSQL DB (PostHog trialled, dropped) | `04-gtm/traction/usage-log.md` |
| Frontend host | Cloudflare Pages | live URL above |
| Backend host | Railway / Render | tech-stack.md |

Full architecture diagram in [`03-build/architecture/architecture-diagram.png`](03-build/architecture/architecture-diagram.png).

---

## Setup (for reviewers running locally)

The deployed product at https://unisport-412.pages.dev is the source of truth for evaluation. To run locally:

```bash
# Frontend (React Native / Expo)
git clone https://github.com/D13Karo/SportActivityAppFRONTEND
cd SportActivityAppFRONTEND
npm install
npx expo start --web    # opens at http://localhost:8081

# Backend (Django REST API)
git clone https://github.com/D13Karo/SportActivityAppBACKEND
cd SportActivityAppBACKEND
python -m venv venv
.\venv\Scripts\Activate.ps1     # Windows
# or: source venv/bin/activate  # macOS/Linux
pip install -r requirements.txt
cp .env.example .env             # then fill in DJANGO_SECRET_KEY, DATABASE_URL, SENDGRID_API_KEY, etc.
python manage.py migrate
python manage.py runserver
```

---

## Sprint Overview

| Sprint | Dates | Theme | Status |
|--------|-------|-------|--------|
| Sprint 1 | Apr 24 – May 7 | Foundation — core user flow end to end | ✅ Complete |
| Sprint 2 | May 8 – May 21 | Instrumentation — organiser flow, push notifications, analytics | ✅ Complete |
| Sprint 3 | May 22 – Jun 4 | Growth — quorum, share/invite, match management | ✅ Complete |
| Sprint 4 | Jun 5 – Jun 11 | Demo Day prep | In progress |

---

## Demo Day Materials

- **Pitch deck:** [`05-fundraising/pitch-deck.pdf`](05-fundraising/pitch-deck.pdf)
- **One-pager:** [`05-fundraising/one-pager.pdf`](05-fundraising/one-pager.pdf)
- **Demo video plan:** [`09-final/demo-video.md`](09-final/demo-video.md)
- **Case study:** [`09-final/case-study.md`](09-final/case-study.md)

---

## License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.
