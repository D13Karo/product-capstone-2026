# How to pull usage numbers for the pitch (5 minutes)

You need a few real numbers for Slide 6 (Traction) + the one-pager + the demo video. Here's exactly how to get them from the **Django admin** over our own database. (We trialled PostHog but track usage this way — see `04-gtm/traction/usage-log.md`.)

## Step 1 — Open the admin

1. Go to **https://sportactivityappbackend.onrender.com/admin/**
2. Sign in with the team admin account
3. You'll see the model list (Users, Matches, RSVPs, …)

## Step 2 — Read the numbers

### Number 1 — Total signups since launch
- Open the **Users** model → the list header shows the total count.
- SQL equivalent: `select count(*) from auth_user;`

### Number 2 — Active users (last 7 days)
- Open **Users**, filter by `last login` within the last 7 days → read the filtered count.
- SQL equivalent: `select count(*) from auth_user where last_login >= now() - interval '7 days';`

### Number 3 — Total joins (the activation metric, NSM)
- Open the **RSVPs** model → list header count.
- SQL equivalent: `select count(*) from matches_rsvp;`

### Number 4 — Matches created
- Open the **Matches** model → list header count.
- SQL equivalent: `select count(*) from matches_match;`

### Number 5 — Smoke-test signups
- Not in the app DB — it's in the Google Form responses from the 13–21 May smoke test. Open the form's **Responses** tab → total submissions. (Also recorded in `04-gtm/traction/waitlist.md`.)

> Confirm the exact app label / table names against the live backend (`SportActivityAppBACKEND`) — the `matches_…` prefixes depend on the Django app name.

## Step 3 — Calculate the activation rate

```
Activation rate = (Number 3 joins / Number 1 signups) × 100
```

Example: 11 joins / 18 signups = 61% activation.

## Step 4 — Write the numbers into the committed log + the 3 deliverables

| Where | What to update |
|---|---|
| `04-gtm/traction/usage-log.md` | The dated snapshot row — this is the source of truth |
| `09-final/pitch-script.md` / pitch deck Slide 6 | The signup + activation numbers |
| `05-fundraising/one-pager.pdf` Traction | The same numbers |
| `09-final/demo-video.md` Section 4 | One number + the date you read it |

## If the admin is empty or a count is zero

- If a count is **zero**: say "we launched [N] weeks ago at one campus; here's what we have, plus the pre-launch smoke-test signal" — pivot to smoke-test signups.
- Be honest. Judges respect a real small number over a fake big one. The teacher's rubric: *"Numbers must be verifiable in your analytics dashboard."* Your admin **is** that dashboard — you can show it live in Q&A.

## Honest framing for small numbers

- ~18 signups in 4 weeks at one university with no marketing = **"early activation signal at one campus"** ✓
- Inflating to 200 when you have 18 = **a lie the rubric penalises.**

**Pull the numbers now. Write them into the usage log + script. Move on.**
