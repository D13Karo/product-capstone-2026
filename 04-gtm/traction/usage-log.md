# Usage Log — UniSport

**Team:** TheMergeConflicters
**Product:** UniSport — https://unisport-412.pages.dev
**Source of these numbers:** the Django admin interface over our own PostgreSQL database (the live backend at https://sportactivityappbackend.onrender.com/admin/). We read counts directly from the admin and record them here.

---

## How we measure usage (and why not a third-party analytics tool)

We trialled PostHog early in the build but moved to reading usage directly from the **Django admin** over our own database. At our scale (one campus, weekly cadence) the admin gives us every number we need — total users, matches created, RSVPs/joins, and recently-active users — without sending any user data to a third-party analytics processor. This keeps all usage data inside our own backend, which is also the cleaner privacy posture (see `08-legal/privacy-notice.md`).

**What counts as our "analytics dashboard" for the course requirement:** the Django admin. Every number below is verifiable by signing into the admin and viewing the relevant model list count. A judge can be shown this live.

### How each number is pulled from the admin

| Number | How to read it in the Django admin | Equivalent SQL |
|--------|-------------------------------------|----------------|
| Total signups | `Users` model → list count | `select count(*) from auth_user;` |
| Total matches created | `Matches` model → list count | `select count(*) from matches_match;` |
| Total joins / RSVPs | `RSVPs` model → list count | `select count(*) from matches_rsvp;` |
| Active users (last 7 days) | `Users` filtered by `last_login` within 7 days | `select count(*) from auth_user where last_login >= now() - interval '7 days';` |

> Model/table names above follow the backend in `SportActivityAppBACKEND`. Confirm the exact app label (`matches_…`) against the live schema before quoting SQL in the pitch.

---

## Usage snapshots

Record a dated row each time someone reads the admin. Keep it honest — small real numbers beat large fake ones (the rubric penalises unverifiable figures).

| Date | Total signups | Matches created | Joins / RSVPs | Active users (7d) | Read by |
|------|---------------|-----------------|---------------|-------------------|---------|
| 2026-06-11 (Demo Day) | ~18 *(confirm exact count in admin)* | [fill from admin] | [fill from admin] | [fill from admin] | [name] |

**Action before Demo Day:** sign into the admin, read the four counts, and replace the bracketed cells + the `~18` with the exact figures. The pitch deck (Slide 6) currently says "~18 real signups" — make this log and the deck agree on the same number.

---

## Context for the numbers

- **Launched ~4 weeks before Demo Day** at a single university (KIU), zero marketing spend.
- **Pre-launch smoke test (13–21 May 2026):** a Carrd landing page distributed in KIU sports group chats; signups captured in a Google Form. Results in `04-gtm/traction/waitlist.md`.
- **10 customer discovery interviews** at an average pain intensity of 4.0/5 (`01-discovery/interview-logs/`).

Honest framing for the pitch: "early activation signal at one campus — every number is real and verifiable in our own admin."

---

*Usage Log | TheMergeConflicters | UniSport | CS-PD-2026 | Spring 2026*
