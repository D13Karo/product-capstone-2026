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
- Usage analytics is **first-party**: we review usage in the Django admin over our own database and do not send data to any third-party analytics tool (we trialled PostHog but removed it). Internal review under legitimate interest does not require a consent UI.
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
| Internal usage review via Django admin (our own database) | Legitimate interest | No — no third-party analytics; data never leaves our backend |
| Marketing emails ("here's a tournament you might like") | Consent (required by GDPR + ePrivacy) | Not yet built — and not yet needed because we send no marketing email |
| Sharing data with third-party processors (Railway, Vercel, SendGrid, Expo Push) | Same basis as the underlying processing — disclosed in privacy notice §3 | No separate consent required when basis is contract or legitimate interest |

**Summary:** at MVP scale, CampusSport processes data under contract and legitimate interest. Usage analytics is first-party (reviewed in our own Django admin), so there is no third-party-analytics consent requirement. The only consent moments are the OS-level push-notification prompt and Terms acceptance at signup.

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

### 2.3 Usage analytics — no consent moment required

We review product usage (signups, matches, joins, sessions) directly in the **Django admin over our own database**. No usage data is sent to a third-party analytics processor — we trialled PostHog but removed it. Because this is first-party processing under legitimate interest, and users are identified only by a system-generated ID (never email or name), there is no separate analytics-consent moment in the onboarding flow. Users retain the right to object to legitimate-interest processing under the privacy notice (§5, Right to object), after which we exclude their records from internal usage review.

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

### Category 3: Usage analytics (first-party — no consent required)

**Purpose:** Understand which features users engage with so we can improve the product.
**Mechanism:** Reviewed in the Django admin over our own database; no third-party analytics tool is used.
**Consent required?** No — first-party processing under legitimate interest, with users identified only by a system-generated ID. Users may object under the privacy notice (§5, Right to object), and we would exclude their records from internal review.

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

### Usage analytics (first-party)
**Withdrawal:** Because usage is reviewed in our own admin (no third-party analytics), a user exercises control via the **Right to object** (privacy notice §5) by emailing karoiani.davit@gmail.com. We then exclude their records from internal usage review and can delete their usage records under the Right to erasure. All usage data stays in our own database throughout.

---

## 5. Consent Storage Record

You must be able to prove consent was given, when, to what version of the privacy notice, and what action the user took.

### Current implementation

| Consent | Where stored | Schema | State |
|---------|--------------|--------|-------|
| ToS acceptance at signup | `users` table (PostgreSQL) — `created_at` timestamp implies acceptance at that moment | `users.created_at`, `users.privacy_notice_version` (gap — column does not yet exist) | Partial — the timestamp proves the user reached the Create Account button on a date when v1.0 of the privacy notice was live, but the explicit `privacy_notice_version` column is a gap |
| Push notification permission | Stored client-side by the OS; server records only the resulting Expo push token | `users.expo_push_token`, `users.expo_push_token_updated_at` | Implemented |

### Planned consent record schema (Sprint 4)

To be added to the `users` table in PostgreSQL:

```
consent: JSONB
{
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
| 1 | `users.consent` / `users.privacy_notice_version` column not yet shipped — cannot yet prove which version of the notice a user agreed to at signup | Davit Karoiani | End of Sprint 4 (11 June 2026) |
| 2 | Self-service account deletion in product (currently email-only via §5 of privacy notice) | Mariam Tskhomelidze | Post-Demo Day, Summer 2026 |
| 3 | Self-service data export in product (currently email-only) | Mariam Tskhomelidze | Post-Demo Day, Summer 2026 |

**Resolved / no longer applicable:** the previous gaps about a third-party-analytics opt-in UI, a PostHog cross-border transfer disclosure, and an analytics opt-out test no longer apply — we removed PostHog and use no third-party analytics processor. Usage is reviewed first-party in the Django admin (see `04-gtm/traction/usage-log.md`).

All remaining gaps are tracked in the team repo as GitHub issues with the `lab-10-compliance` label and reviewed at every standup until closed.
