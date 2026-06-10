# Consent Flow Design

**Product:** CampusSport
**Team:** TheMergeConflicters
**Date:** 12 December 2025
**Related file:** `08-legal/privacy-notice.md`

---

## Overview

This document describes how CampusSport obtains, records, and allows withdrawal of user consent under GDPR. Consent under GDPR must satisfy six requirements: freely given, specific, informed, unambiguous, withdrawable, and documented.

**Current state:** Our consent mechanism is **partially implemented**. Gaps are explicitly noted in each section.

Specifically:
- Terms of Service acceptance (contract lawful basis, not "consent" in the GDPR sense) is implemented on the signup screen.
- Marketing-email consent is **not yet implemented** — we do not currently send marketing emails, so the absence of consent UI matches the absence of the underlying processing. If we add a marketing email channel, the consent UI must ship in the same release.
- PostHog analytics currently operate under legitimate interest with disclosure in the privacy notice. Explicit opt-in consent UI is scoped for Sprint 4 (5–11 June 2026).
- Push notifications use the OS-level system prompt (handled by `expo-notifications`) — this is industry-standard and meets the "specific, unambiguous, withdrawable" bar at the OS level.

---

## 1. What Requires Consent

Not all data processing requires consent. CampusSport's processing breaks down as follows:

| Processing activity | Lawful basis | Requires consent UI? |
|--------------------|--------------|----------------------|
| Account creation, login, JWT session management | Contract | No |
| Storing and displaying match RSVPs you make | Contract | No |
| Posting matches as an organiser | Contract | No |
| Sending you push notifications about matches you joined | Contract (this is the core service) — plus OS-level permission prompt | OS-level consent only (handled by `expo-notifications`) |
| Sending transactional email (signup verification, password reset) | Contract (necessary to authenticate you) | No |
| PostHog product analytics on event schema | Legitimate interest (currently) | **Planned: opt-in consent UI in Sprint 4** |
| Marketing emails ("here's a tournament you might like") | Consent (required by GDPR + ePrivacy) | Not yet built — and not yet needed because we send no marketing email |
| Sharing data with third-party processors (Railway, Vercel, PostHog, SendGrid, Expo Push) | Same basis as the underlying processing — disclosed in privacy notice §3 | No separate consent required when basis is contract or legitimate interest |

**Summary:** at MVP scale, CampusSport processes data under contract and legitimate interest. There is one consent gap — the opt-in for PostHog analytics — which is a known item, not a hidden one. We choose to disclose this honestly rather than pretend it does not exist.

---

## 2. Where Consent Is Obtained: The Consent Moment

### 2.1 Terms of Service acceptance — on the signup screen (implemented)

**Location in the flow:** Signup screen, after the user has filled in email and password, before the `Create Account` button is enabled.

**What the user sees:**

```
+------------------------------------------------------------+
| Create your CampusSport account                            |
|                                                            |
|  University email:  [_____________________@kiu.edu.ge]     |
|  Display name:      [_____________________________________]|
|  Password:          [_____________________________________]|
|  Confirm password:  [_____________________________________]|
|                                                            |
|  By creating an account, you agree to our                  |
|  [Terms of Service] and acknowledge our                    |
|  [Privacy Notice]. We process your data to provide         |
|  CampusSport (Contract). For analytics, see Privacy        |
|  Notice §2.2.                                              |
|                                                            |
|                                  [ Create Account ]        |
+------------------------------------------------------------+
```

**Key requirements verified for the signup screen:**

- [x] No pre-ticked checkbox bundles ToS and marketing — there is no marketing checkbox to pre-tick.
- [x] The Terms of Service link and Privacy Notice link are visible and reachable from the signup screen.
- [x] The user can read the privacy notice before submitting the form.
- [x] University email domain validation runs client-side (`constants/universities.ts`) and server-side (Django serializer) — same data, two enforcement points.

### 2.2 Push notification permission — on first arrival on the home screen (implemented)

**Location:** First time the authenticated home screen loads after signup, `expo-notifications.requestPermissionsAsync()` is called.

**What the user sees:** The native OS prompt (iOS: "CampusSport Would Like to Send You Notifications"; Android 13+: "Allow CampusSport to send you notifications?"). This is a one-tap Allow/Deny that the OS records.

**Key requirements verified:**

- [x] Permission is requested at a moment the user understands — after they have seen the match list and the value of the product.
- [x] Denial does not prevent any other product functionality. The user can browse and join matches; they just will not get push notifications about changes.
- [x] Withdrawal is OS-level (Settings → Notifications → CampusSport) and as easy as granting was.

### 2.3 PostHog analytics consent — **not yet implemented (gap)**

**Planned location:** On first authenticated home-screen load, an inline banner above the match list:

```
+------------------------------------------------------------+
| Help us improve CampusSport                                |
|                                                            |
| We use PostHog (a self-hostable analytics tool) to         |
| understand which features matter most. Events are tied     |
| to an anonymous ID, never your email or name. See          |
| [Privacy Notice §2.2] for the full list.                   |
|                                                            |
|  [  Allow analytics  ]   [  No thanks  ]                   |
+------------------------------------------------------------+
```

**Key requirements that the gap closure must satisfy:**

- [ ] Neither button is the default — both are visually equal weight.
- [ ] Declining ("No thanks") disables PostHog event firing for that account; signup, login, and all core flows continue to work.
- [ ] Declined accounts have their choice recorded so the banner does not reappear.
- [ ] A user can change their mind in Settings → Privacy at any time, with one tap.

**Owner:** Davit Karoiani. **Target:** Sprint 4 (5 to 11 June 2026).

---

## 3. Consent Categories

### Category 1: Terms of Service acceptance (contract, not consent in the GDPR sense)

**Purpose:** Bind the user to the terms under which we provide the service.
**Is it optional?** No — without ToS acceptance, no account is created.
**Default state:** Not pre-accepted. The Create Account action implies acceptance only after the user clicks it, having seen the linked ToS and Privacy Notice.
**UI element:** Inline statement above the Create Account button (not a checkbox — implied by the button click and the visible link to ToS and Privacy Notice).
**What happens if the user declines:** They do not click Create Account; no account is created; no data is stored.

### Category 2: Push notifications (OS-level consent)

**Purpose:** Deliver match change notifications — the core value of the product.
**Is it optional?** Yes — the user can decline at the OS prompt.
**Default state:** Not granted until the user taps Allow on the OS prompt.
**UI element:** OS-native prompt via `expo-notifications.requestPermissionsAsync()`.
**What happens if the user declines:** Account works normally; match list, joining, leaving, and viewing all function. The user simply does not receive push notifications when a match they joined changes — they will see updates only when they open the app.

### Category 3: PostHog product analytics (**planned, Sprint 4**)

**Purpose:** Understand which features users engage with so we can improve the product.
**Is it optional?** Yes.
**Default state:** Unchecked / off until the user actively grants consent via the inline banner described in §2.3.
**UI element:** Inline banner on first authenticated home screen, plus a toggle in Settings → Privacy that is always available.
**What happens if the user declines:** PostHog event firing is disabled for that user. The product continues to work fully. We lose the ability to measure their funnel, but that is the user's right.

### Category 4: Marketing communications

**Status:** Not applicable to the current product. CampusSport sends no marketing emails. Transactional emails (signup verification, password reset) are sent under the contract basis and do not require consent.

If a marketing channel is added later, this section will be expanded with the consent UI and behaviour spec, and the consent UI must ship in the same release as the marketing feature itself.

---

## 4. Withdrawal Mechanism

Consent must be as easy to withdraw as it was to give. Status per category:

### Terms of Service
**Withdrawal:** The user deletes their account (email request to karoiani.davit@gmail.com under §5 of the privacy notice). On the post-Demo Day roadmap: in-product Settings → Delete Account button — currently a documented gap.

### Push notifications
**Withdrawal:** OS Settings → Notifications → CampusSport → toggle off. Two taps. Same number of taps as granting.
**Server side:** When a push attempt fails because the OS revoked permission, the backend marks the token invalid within 24 hours.

### PostHog analytics (planned for Sprint 4)
**Withdrawal:** Settings → Privacy → Analytics toggle. One tap to off.
**Steps:** Two screens, one tap. Same as the inline banner that granted it.
**What happens to data already collected?** Events already sent to PostHog remain there subject to PostHog's 12-month rolling retention. No new events will be sent for that user. A user can additionally email karoiani.davit@gmail.com to request deletion of their PostHog events under the right to erasure.

---

## 5. Consent Storage Record

You must be able to prove consent was given, when, to what version of the privacy notice, and what action the user took.

### Current implementation

| Consent | Where stored | Schema | State |
|---------|--------------|--------|-------|
| ToS acceptance at signup | `users` table (PostgreSQL) — `created_at` timestamp implies acceptance at that moment | `users.created_at`, `users.privacy_notice_version` (gap — column does not yet exist) | Partial — the timestamp proves the user reached the Create Account button on a date when v1.0 of the privacy notice was live, but the explicit `privacy_notice_version` column is a gap |
| Push notification permission | Stored client-side by the OS; server records only the resulting Expo push token | `users.expo_push_token`, `users.expo_push_token_updated_at` | Implemented |
| PostHog analytics opt-in | **Not yet implemented** | Planned: `users.consent` JSONB column (see schema below) | Gap |

### Planned consent record schema (Sprint 4)

To be added to the `users` table in PostgreSQL:

```
consent: JSONB
{
  "analytics_posthog": {
    "given": true,
    "timestamp": "2026-06-08T14:23:45Z",
    "privacy_notice_version": "1.0",
    "method": "inline_banner_home_screen"
  },
  "tos_v1": {
    "given": true,
    "timestamp": "2026-05-12T09:01:22Z",
    "privacy_notice_version": "1.0",
    "method": "implicit_on_create_account_click"
  }
}
```

The accompanying migration is on the Sprint 4 backlog. Davit owns the migration; Levan owns the test that proves the value persists across a session.

### Retention of consent records

Consent records are retained for the life of the account plus 24 months after account deletion, to handle any disputes about whether consent was given for a given period. After 24 months, the consent record is purged along with any remaining audit log.

---

## 6. Gaps and Remediation Plan

Listed in priority order. An empty table here would mean either full compliance or not looking carefully enough. We chose to look carefully.

| # | Gap | Owner | Target completion date |
|---|-----|-------|-----------------------|
| 1 | PostHog analytics opt-in UI not yet built — currently operating under legitimate interest with notice. Compliant under GDPR but a stronger consent posture is desired before public marketing launch. | Davit Karoiani | End of Sprint 4 (11 June 2026) |
| 2 | `users.consent` JSONB column and migration not yet shipped | Davit Karoiani | End of Sprint 4 (11 June 2026) |
| 3 | `users.privacy_notice_version` column not yet shipped — cannot prove which version of the notice a user agreed to | Davit Karoiani | End of Sprint 4 (11 June 2026) |
| 4 | Self-service account deletion in product (currently email-only via §5 of privacy notice) | Mariam Tskhomelidze | Post-Demo Day, Summer 2026 |
| 5 | Self-service data export in product (currently email-only) | Mariam Tskhomelidze | Post-Demo Day, Summer 2026 |
| 6 | Cross-border data transfer disclosure for PostHog Cloud tenant region — needs to be confirmed (EU vs US) and either documented in §3 of privacy notice or PostHog tenant moved to EU | Mariam Pirtskhalava | End of Sprint 4 |
| 7 | No automated test that a user who declined analytics never has events fired — needs a Jest/Detox test | Levan Kovziridze | End of Sprint 4 |

All gaps are tracked in the team repo as GitHub issues with the `lab-10-compliance` label and reviewed at every standup until closed.
