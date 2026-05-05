# Event Schema
Team: TheMergeConflicters
Product: KIU Sports Tracker
Date: 09 April 2026
Version: 1.0

## Global Properties
*These properties are sent with EVERY event listed below.*
* `user_id` (String, UUID) - Anonymous system identifier
* `timestamp` (String, ISO 8601) - When the event occurred
* `session_id` (String, UUID) - Which session the event belongs to

## Event Dictionary

### 1. user_signup_completed
* **AARRR Stage:** Acquisition
* **Description:** Fires when a student successfully creates their account.
* **Properties:**
  * `signup_method` (String) - e.g., "kiu_email", "google"
  * `onboarding_time_seconds` (Integer)

### 2. match_joined (Our Activation / North Star Event)
* **AARRR Stage:** Activation
* **Description:** Fires when a user successfully taps "Join" and confirms their spot in a match.
* **Properties:**
  * `match_id` (String, UUID)
  * `sport_type` (String) - e.g., "football", "basketball", "tennis"
  * `spots_remaining_after_join` (Integer)
  * `time_to_match_hours` (Integer)

### 3. match_created
* **AARRR Stage:** Retention
* **Description:** Fires when a student successfully schedules a new informal match for others to join.
* **Properties:**
  * `match_id` (String, UUID)
  * `sport_type` (String)
  * `max_players` (Integer)
  * `location` (String) - e.g., "KIU Arena", "Dorm A Court"

### 4. user_session_started
* **AARRR Stage:** Retention
* **Description:** Fires when a user opens the app to check for new games.
* **Properties:**
  * `device_type` (String) - e.g., "mobile_web", "desktop"

### 5. match_invite_sent
* **AARRR Stage:** Referral
* **Description:** Fires when a user shares a match link with a friend to get them to join.
* **Properties:**
  * `match_id` (String, UUID)
  * `share_method` (String) - e.g., "copy_link", "messenger_share"

### 6. match_result_logged
* **AARRR Stage:** Retention
* **Description:** Fires when the match organizer inputs the final score after the game ends.
* **Properties:**
  * `match_id` (String, UUID)
  * `winning_score` (Integer)
  * `losing_score` (Integer)

### 7. match_left (Negative/Error Tracking)
* **AARRR Stage:** Retention
* **Description:** Fires when a user cancels their RSVP, freeing up a spot.
* **Properties:**
  * `match_id` (String, UUID)
  * `hours_before_start` (Integer) - Helps track last-minute flaking
