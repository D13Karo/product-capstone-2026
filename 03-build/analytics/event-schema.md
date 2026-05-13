# Event Schema

**Team:** TheMergeConflicters  
**Product:** KIU Sports Tracker  
**Date:** 09 April 2026  
**Version:** 1.0  
**Status:** Blueprint (instrumentation code written in Lab 6)

---

## Naming Convention

All events follow this rule without exception:

```
object_action (snake_case, past tense)
```

Examples of correct names: `user_signup_completed`, `match_joined`, `match_created`  
Examples of incorrect names: `UserSignupCompleted`, `create_match`, `join`, `click_button`

---

## North Star Metric

> Weekly informal sports matches joined per active user

**Activation event that drives NSM:** `match_joined`

---

## Universal Properties

Every event automatically includes these properties. Do not repeat them in individual event definitions.

| Property | Type | Description |
|----------|------|-------------|
| `user_id` | string (UUID) | System-generated user identifier. Never an email address. |
| `timestamp` | ISO 8601 datetime | When the event fired. Always include timezone (Z for UTC). |
| `session_id` | string (UUID) | The session in which the event occurred. |
| `platform` | enum: web, ios, android | The platform the event was fired on. |

---

## Event Definitions

### ACQUISITION

---

#### `user_signup_completed`

**AARRR Stage:** Acquisition  
**Description:** Fires when a student successfully creates their account.  
**Fires when:** User submits the registration form and the account is created successfully in the database.  
**NSM connection:** None — acquisition only. Sets up the `user_id` that all subsequent NSM events are attributed to.

| Property | Type | Required | Description | Example |
|----------|------|----------|-------------|---------|
| `signup_method` | enum | Yes | How the user registered | `"kiu_email"` |
| `onboarding_time_seconds` | integer | No | Time from landing to account creation | `47` |

**Example payload:**
```json
{
  "event_name": "user_signup_completed",
  "user_id": "user_abc123",
  "timestamp": "2026-04-09T14:23:45Z",
  "session_id": "sess_xyz789",
  "platform": "web",
  "signup_method": "kiu_email",
  "onboarding_time_seconds": 47
}
```

---

### ACTIVATION

---

#### `match_joined` ← THIS IS YOUR ACTIVATION EVENT

**AARRR Stage:** Activation  
**Description:** Fires when a user successfully taps "Join" and confirms their spot in a match. This is the aha moment — the user got real value for the first time.  
**Fires when:** User taps the "Join Match" button and the system confirms their RSVP (spot reserved in database).  
**NSM connection:** This event directly drives the NSM. Each firing of this event increments the weekly match participation count for that `user_id`.

| Property | Type | Required | Description | Example |
|----------|------|----------|-------------|---------|
| `match_id` | string (UUID) | Yes | The match the user joined | `"match_def456"` |
| `sport_type` | enum | Yes | Type of sport for the match | `"football"` |
| `spots_remaining_after_join` | integer | Yes | How full the match is after this join | `3` |
| `time_to_match_hours` | integer | Yes | Hours until match starts | `18` |

**Example payload:**
```json
{
  "event_name": "match_joined",
  "user_id": "user_abc123",
  "timestamp": "2026-04-09T14:25:10Z",
  "session_id": "sess_xyz789",
  "platform": "web",
  "match_id": "match_def456",
  "sport_type": "football",
  "spots_remaining_after_join": 3,
  "time_to_match_hours": 18
}
```

---

### RETENTION

---

#### `match_created`

**AARRR Stage:** Retention  
**Description:** Fires when a student successfully schedules a new informal match for others to join.  
**Fires when:** User submits the match creation form and the match record is saved to the database.  
**NSM connection:** Indirect. A user creating a match is a power-user behaviour that generates supply for others to join, sustaining the weekly `match_joined` count across the platform.

| Property | Type | Required | Description | Example |
|----------|------|----------|-------------|---------|
| `match_id` | string (UUID) | Yes | Newly created match identifier | `"match_ghi789"` |
| `sport_type` | enum | Yes | Type of sport | `"basketball"` |
| `max_players` | integer | Yes | Maximum players the organiser set | `10` |
| `location` | string | Yes | Venue name (no address — not PII) | `"KIU Arena"` |

---

#### `user_session_started`

**AARRR Stage:** Retention  
**Description:** Fires when a user opens the app to check for new games.  
**Fires when:** Authenticated user loads the app home screen.  
**NSM connection:** Indirect. Tracks whether users return habitually; sessions without a subsequent `match_joined` indicate a browse-without-joining pattern to investigate.

| Property | Type | Required | Description | Example |
|----------|------|----------|-------------|---------|
| `device_type` | enum | Yes | Device category: mobile_web, desktop | `"mobile_web"` |
| `app_open_source` | enum | Yes | How the user opened the app: direct, push_notification, share_link | `"push_notification"` |

---

#### `match_result_logged`

**AARRR Stage:** Retention  
**Description:** Fires when the match organiser inputs the final score after the game ends.  
**Fires when:** Organiser submits the result form for a completed match.  
**NSM connection:** Indirect. Indicates the full match lifecycle completed, which correlates with user satisfaction and likelihood of returning to join another match.

| Property | Type | Required | Description | Example |
|----------|------|----------|-------------|---------|
| `match_id` | string (UUID) | Yes | The match result was logged for | `"match_def456"` |
| `winning_score` | integer | Yes | Winning team's score | `3` |
| `losing_score` | integer | Yes | Losing team's score | `1` |

---

#### `match_left`

**AARRR Stage:** Retention  
**Description:** Fires when a user cancels their RSVP, freeing up a spot.  
**Fires when:** User taps "Leave Match" and confirms cancellation.  
**NSM connection:** Negative signal. A high `match_left` rate, especially at low `hours_before_start`, indicates reliability issues that will suppress the weekly `match_joined` NSM.

| Property | Type | Required | Description | Example |
|----------|------|----------|-------------|---------|
| `match_id` | string (UUID) | Yes | Match the user left | `"match_def456"` |
| `hours_before_start` | integer | Yes | How close to kick-off the cancellation happened | `2` |

---

### REFERRAL

---

#### `match_invite_sent`

**AARRR Stage:** Referral  
**Description:** Fires when a user shares a match link with a friend to get them to join.  
**Fires when:** User taps the share button and completes the share action (link copied or messenger share initiated).  
**NSM connection:** None — referral only. Drives new user acquisition which feeds future NSM counts.

| Property | Type | Required | Description | Example |
|----------|------|----------|-------------|---------|
| `match_id` | string (UUID) | Yes | Match being shared | `"match_def456"` |
| `share_method` | enum | Yes | How the invite was sent | `"copy_link"` |

---

### REVENUE (not applicable)

No paid tier in MVP. If a premium features tier is introduced post-Demo Day, a `subscription_started` event will be added at that point.

---

## Event Summary Table

| Event Name | AARRR Stage | Priority | NSM Driver |
|-----------|-------------|----------|-----------|
| `user_signup_completed` | Acquisition | Must | No |
| `match_joined` | Activation | Must | **Yes** |
| `match_created` | Retention | Must | Indirect |
| `user_session_started` | Retention | Must | Indirect |
| `match_result_logged` | Retention | Should | Indirect |
| `match_left` | Retention | Should | No (negative signal) |
| `match_invite_sent` | Referral | Should | No |

**Total events:** 7  
**Must-have events:** 4  
**Should-have events:** 3

---

## Privacy Confirmation

Confirm that no event in this schema captures PII:

- [x] No email addresses in any event property
- [x] No user names or display names in any event property
- [x] No phone numbers in any event property
- [x] No physical addresses in any event property
- [x] No payment card details in any event property
- [x] All user identification uses system-generated UUIDs only

**Schema reviewed by:** Davit Karoiani on 09 April 2026

---

## Instrumentation Notes for Lab 6

This is the blueprint only. In Lab 6, the actual code to fire these events will be written in the Google AI Studio app.

| Event Name | Where in Code | Frontend or Backend |
|-----------|--------------|-------------------|
| `user_signup_completed` | On signup form submission success callback | Frontend |
| `match_joined` | After RSVP record confirmed in database | Backend |
| `match_created` | After match record saved to database | Backend |
| `user_session_started` | On authenticated home screen page load | Frontend |
| `match_result_logged` | On result form submission success | Backend |
| `match_left` | After RSVP cancellation confirmed in database | Backend |
| `match_invite_sent` | On share button action completion | Frontend |

---

## Analytics Tool Selection

- [ ] Mixpanel (best for funnel analysis)
- [ ] Amplitude (best for retention curves)
- [x] PostHog (best for self-hosted, privacy-conscious products)
- [ ] Google Analytics 4 (best for web-only MVPs)

**Our choice:** PostHog  
**Reason:** Open-source, self-hostable, generous free tier, and privacy-first — appropriate for a student MVP handling university-community data.  
**Free tier limit:** 1M events per month on PostHog Cloud

---

## Change Log

| Date | Version | Changes | Author |
|------|---------|---------|--------|
| 09 April 2026 | 1.0 | Initial schema blueprint | Davit Karoiani |

---

*Event Schema | TheMergeConflicters | CS-PD-2026 | Spring 2026*
