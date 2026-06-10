# Privacy Notice

**Product:** CampusSport
**Team:** TheMergeConflicters
**Version:** 1.0
**Date:** 12 December 2025
**Effective from:** 7 May 2026 (deployment of Sprint 1 to public Vercel URL — first non-team signup possible from this date)

---

## 1. Who We Are

CampusSport is a push-based coordination tool for Kutaisi International University students who play informal sports matches on campus — it lets organisers post a match once and automatically notifies registered participants about times, locations, and schedule changes. It is developed by TheMergeConflicters, a student team at Kutaisi International University as part of CS-PD-2026 (Product Development for Software Engineers, Spring 2026).

**Data controller contact:**
Name: Davit Karoiani (Product Owner, responsible for data requests)
Email: karoiani.davit@gmail.com
GitHub: @D13Karo

---

## 2. What Personal Data We Collect and Why

Every row below describes data CampusSport actually collects, verified against the live Django backend (`SportActivityApp/backend/`) and the React Native Expo frontend (`SportActivityApp/frontend/`) on 12 December 2025.

### 2.1 Account and Identity Data

| Data category | Specific fields | Why we collect it | Lawful basis | Who can access it |
|---------------|----------------|-------------------|--------------|-------------------|
| Account credentials | Email address (KIU domain enforced), password (bcrypt hash via Django default hasher) | To create and authenticate your account so only verified KIU students can see KIU matches | Contract: necessary to provide the service | Backend service (Django ORM), Railway/Render database admin, SendGrid (email address only, for verification + password reset) |
| Display profile | Display name, university_domain (e.g. `kiu.edu.ge`) | To show your name to other players in the match roster and to filter the match list to your university only | Contract: necessary to provide the core "only see your campus's matches" promise | Backend service, other authenticated users at the same university (display name only, visible in match rosters) |
| Auth state | JWT access token, JWT refresh token, `tokens_invalidated_at` timestamp | To maintain your authenticated session and to allow server-side session revocation if needed | Contract | Backend service only; tokens stored in Expo `AsyncStorage` on the client device |
| Push notification token | Expo push token (per-device identifier issued by Expo / FCM / APNs) | To deliver push notifications when a match you joined changes | Contract: this is the core feature the user signed up for; without the token we cannot deliver notifications | Backend service, Expo Push Service (a Google Cloud-backed service), and downstream FCM (Android) / APNs (iOS) |

**Lawful basis options used:** Contract (processing necessary to provide the service the user signed up for), Legitimate interest (improving the product through analytics), Consent (currently not used — see Section 11 gap note).

### 2.2 Usage and Behavioural Data

All event names come from `03-build/analytics/event-schema.md` v1.0. No event in our schema captures email, name, or any other PII (verified by Davit on 09 April 2026 and re-verified on 12 December 2025).

| Data category | Specific fields | Why we collect it | Lawful basis | Third-party processor |
|---------------|----------------|-------------------|--------------|----------------------|
| Acquisition events | `user_signup_completed` (signup_method, onboarding_time_seconds) | To measure signup funnel performance | Legitimate interest | PostHog Cloud |
| Activation events | `match_joined` (match_id, sport_type, spots_remaining_after_join, time_to_match_hours) | This is our North Star Metric — it tells us whether the product actually works | Legitimate interest | PostHog Cloud |
| Retention events | `match_created`, `user_session_started` (device_type, app_open_source), `match_result_logged`, `match_left` | To measure organiser supply, session frequency, and RSVP cancellation rates | Legitimate interest | PostHog Cloud |
| Referral events | `match_invite_sent` (match_id, share_method) | To measure whether share links drive new signups | Legitimate interest | PostHog Cloud |
| Universal event properties | `user_id` (system-generated UUID — never email), `timestamp` (ISO 8601 UTC), `session_id` (UUID), `platform` (`web` / `ios` / `android`) | Attached to every event for attribution and debugging | Legitimate interest | PostHog Cloud |
| Server logs | HTTP method, request path, response status code, timestamp, client IP address (last 30 days) | To diagnose errors and abuse | Legitimate interest | Railway/Render hosting platform |

### 2.3 Location Data

| Data category | Specific fields | Why we collect it | Lawful basis | Third-party processor |
|---------------|----------------|-------------------|--------------|----------------------|
| Match venue (not user location) | Free-text venue name set by the organiser (e.g. `KIU Arena`, `KIU Pitch 2`) | To tell players where to go for the match | Contract | Backend service only |

CampusSport **does not** collect GPS location, device location, or IP-derived precise location from any user. The only location data in the system is the venue name typed by an organiser, which is content, not personal data about a player.

### 2.4 Transaction and Activity Data

| Data category | Specific fields | Why we collect it | Lawful basis | Third-party processor |
|---------------|----------------|-------------------|--------------|----------------------|
| Match RSVP records | user_id, match_id, RSVP timestamp, RSVP status (joined / left) | To show match rosters, calculate quorum, and send the right notifications to the right people | Contract | Backend service (PostgreSQL on Railway/Render) |
| Match records (organiser-created) | match_id, creator user_id, sport_type, max_players, location, start_time, status | To make matches visible to other players at the same university | Contract | Backend service |
| Match result records | match_id, winning_score, losing_score, logged_by_user_id, timestamp | To close out the match lifecycle | Contract | Backend service |

CampusSport has **no payment processing in the MVP**. If a premium organiser tier is introduced post-Demo Day, a separate transaction data section will be added here at that time.

---

## 3. Third-Party Processors

Every service below receives some category of personal data. List verified against `SportActivityApp/backend/requirements.txt` and `SportActivityApp/frontend/package.json` on 12 December 2025.

| Processor | Service type | Data they receive | Their privacy policy |
|-----------|-------------|-------------------|---------------------|
| Railway or Render (backend host — final choice locked at deployment) | Application hosting + managed PostgreSQL | All stored user data, server logs, request IPs | railway.app/legal/privacy or render.com/privacy |
| Vercel or Netlify (frontend host — final choice locked at deployment) | Static site hosting + edge CDN | IP addresses in HTTP request logs (≤30 days) | vercel.com/legal/privacy-policy or netlify.com/privacy |
| PostHog Cloud | Product analytics | All events listed in §2.2, attached to system-generated `user_id` only — no email or name | posthog.com/privacy |
| Expo Push Service (operated by Expo, a unit of 650 Industries, on Google Cloud) | Push notification relay | Expo push token + notification payload (match title, time, venue) | expo.dev/privacy |
| Apple Push Notification Service (APNs) | Native iOS push delivery (only when an iOS native build is shipped) | Device token, notification payload | apple.com/legal/privacy |
| Firebase Cloud Messaging (FCM) | Native Android push delivery (only when an Android native build is shipped) | Device token, notification payload | firebase.google.com/support/privacy |
| SendGrid (Twilio) | Transactional email (signup verification code, password reset code) | Email address, transactional message body | sendgrid.com/policies/privacy |
| Redis (managed instance on Railway or Render) | Channels backplane for real-time updates | Session/channel identifiers; no message content persisted beyond seconds | redis.com/legal/privacy-policy or hosting provider's policy |
| GitHub | Source code hosting (no production user data) | None in production — listed for completeness because our deployment pipeline runs from GitHub | github.com/site/privacy |

We do not currently use Google Analytics, Mixpanel, Amplitude, Sentry, or any social-login provider (Google Sign-In / Facebook Login). If any is added later, this table must be updated before the change is deployed.

---

## 4. How Long We Keep Your Data

| Data category | Retention period | What triggers deletion |
|---------------|-----------------|-----------------------|
| Account credentials and profile | Until account deletion request + 30-day grace period (allows recovery on accidental deletion) | User submits erasure request per §5; or 24 months of inactivity (no `user_session_started` event) |
| Push notification token | Removed within 24 hours of user logging out on that device, or on account deletion | Logout, account deletion, or token invalidation by Expo/FCM/APNs |
| Match RSVPs | Until the user deletes their account, or 12 months after the match end time — whichever comes first | Scheduled job on the backend |
| Match records and results | 12 months after match end time (kept short-term for season-end review) | Scheduled job |
| Event analytics in PostHog | 12 months rolling (PostHog Cloud default for our project) | Automatic; PostHog handles deletion |
| Server logs (Railway/Render) | 30 days rolling | Automatic; hosting platform default |
| Email verification / password reset codes | Single-use; invalidated within 15 minutes of issue or on use | Automatic, server-side TTL |
| Consent record (once implemented — see §11) | Life of account + 24 months | Scheduled job after account deletion + 24 months |

---

## 5. Your Rights

Under GDPR, you have the following rights. For each right, we describe how to exercise it and our response time.

| Right | What it means | How to exercise it | Our response time |
|-------|--------------|-------------------|------------------|
| Right to access | Request a copy of all personal data we hold about you | Email karoiani.davit@gmail.com with subject `Data Access Request` | Within 30 days |
| Right to erasure | Request deletion of your personal data and account | Email karoiani.davit@gmail.com with subject `Erasure Request` | Within 30 days |
| Right to rectification | Request correction of inaccurate data | Email karoiani.davit@gmail.com with subject `Rectification Request` | Within 30 days |
| Right to restriction | Request we stop processing your data in specific ways (e.g. exclude you from PostHog analytics) | Email karoiani.davit@gmail.com with subject `Processing Restriction` | Within 30 days |
| Right to portability | Request your data in a machine-readable format (JSON export of your profile, RSVPs, and created matches) | Email karoiani.davit@gmail.com with subject `Data Portability Request` | Within 30 days |
| Right to object | Object to processing based on legitimate interest (analytics, server logs beyond what is contract-necessary) | Email karoiani.davit@gmail.com with subject `Processing Objection` | Within 30 days |

If you believe we are processing your data unlawfully, you have the right to lodge a complaint with the **Personal Data Protection Service of Georgia** (personaldata.ge), the supervisory authority in our country of operation.

**Acknowledged gap:** an in-product self-service mechanism for these rights does not yet exist. All rights are exercised via email at MVP scale. A self-service privacy dashboard is on the post-Demo Day backlog — see §11.

---

## 6. Data Breach Procedure

In the event of a personal data breach, CampusSport commits to:

1. **Assess** the breach within 24 hours of becoming aware of it (scope, data categories affected, number of users affected, severity).
2. **Notify** the Personal Data Protection Service of Georgia within 72 hours if the breach is likely to result in a risk to the rights and freedoms of natural persons.
3. **Notify affected users** without undue delay (target: within 72 hours) via the email on their account if the breach is likely to result in a high risk.
4. **Document** the breach in an incident postmortem committed to the team repo, including root cause, mitigation, and timeline.

**Person responsible for breach response:** Davit Karoiani (Product Owner) — karoiani.davit@gmail.com
**Backup responsible:** Mariam Tskhomelidze (Scrum Master)

---

## 7. Cookies and Tracking Technologies

CampusSport's frontend is a React Native Expo app. We do not use HTTP cookies in the traditional web sense. The equivalent storage we use:

| Storage mechanism | Purpose | Duration | Can you opt out? |
|-------------------|---------|----------|-----------------|
| Expo `AsyncStorage` — JWT access token | Maintain your login session on the client device | Until logout or 7 days (whichever first); refresh token rotates it | No — required to use the product |
| Expo `AsyncStorage` — JWT refresh token | Issue new access tokens without re-prompting login | Until logout or 30 days | No — required to use the product |
| Expo `AsyncStorage` — theme preference (light/dark) | Remember your dark/light mode preference | Until app uninstall or manual reset | Yes — change in Settings |
| Expo push token (per-device) | Receive push notifications | Until logout on that device, or token revocation | Yes — disable notifications in OS-level app settings |
| Vercel/Netlify edge cache cookie (host-set) | CDN cache routing | Session | Standard CDN behaviour, not used for tracking |
| PostHog session ID (set in `AsyncStorage` by `posthog-react-native`) | Group events into a session for analytics | 30 minutes of inactivity | Yes once consent UI ships — see §11 |

We do not use Google Analytics cookies, advertising cookies, or any cross-site tracking technologies.

---

## 8. Changes to This Notice

We will update this notice when our data practices change (new processor added, new data category collected, retention period changed). We will:

- Update the version number and date at the top
- Notify users of material changes by email and in-app banner
- Keep prior versions in the team repo's git history at `08-legal/privacy-notice.md` so users can see what changed

Continued use of the product after a material change is **not** by itself acceptance of the updated notice. For material changes that affect lawful basis, we will re-collect consent where required.

---

## 9. Children

CampusSport is intended for KIU students aged 18 and over. Our signup gate requires a verified KIU email address, which the university issues only to enrolled students. We do not knowingly collect data from anyone under 16. If you believe we have collected such data, please contact karoiani.davit@gmail.com and we will delete the account within 7 days.

---

## 10. Contact

For any question related to this privacy notice or to exercise your rights:

**Name:** Davit Karoiani (Product Owner, Data Controller contact)
**Email:** karoiani.davit@gmail.com
**Response time:** Within 5 business days for general questions, within 30 days for formal rights requests
**Repository:** github.com/D13Karo/SportActivityAppBACKEND and SportActivityAppFRONTEND

---

## 11. Known Gaps from This Audit (will be closed before public marketing launch)

These were identified during the Lab 10 audit on 12 December 2025 and are tracked in `03-build/privacy-security/consent-form.md` §6:

| Gap | Owner | Target |
|-----|-------|--------|
| In-product consent UI for PostHog analytics does not yet exist — analytics currently run under legitimate interest with notice in this document; explicit consent is a stronger position before public launch | Davit | Sprint 4 (5–11 Jun 2026 — Demo Day prep window) |
| Self-service rights dashboard (export, delete) — currently email-only | Mariam T. | Post-Demo Day (Summer 2026) |
| Cross-border processor disclosure (PostHog stores in US/EU depending on tenant — needs to be confirmed and documented) | Mariam P. | Sprint 4 |
| Sub-processor list for SendGrid and Expo Push not yet enumerated to second tier | Levan | Post-Demo Day |

---

*This privacy notice was last updated on 12 December 2025. Version 1.0. Source files audited: `SportActivityApp/backend/requirements.txt`, `SportActivityApp/frontend/package.json`, `03-build/analytics/event-schema.md`.*
