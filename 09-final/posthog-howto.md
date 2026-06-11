# How to pull PostHog numbers for the pitch (5 minutes)

You need 4 numbers for Slide 6 + the one-pager + the demo video. Here's exactly how to get them.

## Step 1 — Log in

1. Go to **https://eu.posthog.com** (or **https://app.posthog.com** if you're on the US tenant)
2. Sign in with the team account
3. Select project **UniSport** (or whichever name matches `EXPO_PUBLIC_POSTHOG_KEY`)

> Don't remember which tenant? Check `SportActivityApp/frontend/.env` for `EXPO_PUBLIC_POSTHOG_HOST`. If it has `eu.posthog.com` → EU tenant. Otherwise US.

## Step 2 — Pull the 4 numbers

### Number 1 — Total signups since launch

1. Left sidebar → **Activity** → **Events**
2. Filter: `Event = user_signup_completed`
3. Filter: `Date = Since May 7, 2026`
4. **Number to write down: total count** (top of page)

### Number 2 — Weekly Active Users (last 7 days)

1. Left sidebar → **Insights** → **New insight** → **Trends**
2. Series: `Unique users` of `user_session_started`
3. Date range: **Last 7 days**
4. **Number to write down: total unique users** shown on the chart

### Number 3 — `match_joined` events (the activation event, NSM)

1. Left sidebar → **Activity** → **Events**
2. Filter: `Event = match_joined`
3. Filter: `Date = Since May 7, 2026`
4. **Number to write down: total count**

### Number 4 — Smoke test signups

This one is NOT in PostHog — it's in the Google Form responses from the May 13–21 smoke test.

1. Open the Google Drive folder for the team
2. Find the Google Form titled "UniSport early access" (or similar)
3. Open the **Responses** tab
4. **Number to write down: total submissions**

## Step 3 — Calculate the activation rate

```
Activation rate = (Number 3 / Number 1) × 100
```

Example: 11 `match_joined` events / 23 signups = 48% activation rate.

## Step 4 — Write the numbers into 3 places

| Where | What to update |
|---|---|
| `09-final/pitch-script.md` Slide 6 | Replace `[N]`, `[X]`, `[%]`, `[Y]` with your actual numbers |
| `05-fundraising/one-pager-content.md` Traction table | Same 4 numbers |
| `09-final/demo-video.md` Section 4 (if you shoot the video) | Number 3 + the activation rate |

## If PostHog is broken or empty

- If a number is **zero** because instrumentation didn't fire: say "we instrumented but haven't collected production traffic yet, here's what we have from the smoke test instead" — pivot to smoke test signups only
- If PostHog dashboard is **down**: pull from the Django admin (`select count(*) from auth_user` for signups, `select count(*) from matches_rsvp` for joins)
- If **everything is broken**: be honest in the pitch. "We have the schema live, we shipped Sprint 2's instrumentation on May 21, we're sharing the smoke-test signal we have." Judges respect honesty over fake numbers.

## Honest framing for small numbers

- 23 signups in 33 days at one university with no marketing = **"early activation signal at one campus"** ✓
- 200 signups when you have 23 = **lie**, rubric penalizes
- The teacher's grading rubric explicitly says: *"Numbers must be verifiable in your analytics dashboard."*

**Pull the numbers now. Write them into the script. Move on.**
