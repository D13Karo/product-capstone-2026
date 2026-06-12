# 60-Second Launch Video — Planning Template

**Team:** TheMergeConflicters
**Product:** UniSport — https://unisport-412.pages.dev
**Target deadline to commit link:** 11 June 2026, 21:00 (2 hours before Checkpoint 4)

> **This is a planning template, not a finished script.** The 5-section structure, timing budget, and technical requirements come from the teacher's `video-script-template.md`. Everything inside the brackets is a **team decision** — sit down together, agree, and fill it in.

---

## The brief (non-negotiable, from the teacher's template)

- 60 seconds maximum. Judges stop watching at 62s.
- At least one team member on camera for ≥10 seconds.
- The live product shown on a real phone for ≥15 seconds.
- Linked in this file and from root `README.md` before 11 Jun 23:59.
- Vertical (9:16 / portrait) — TikTok / Reel format.

---

## Section 1: Hook (0:00 – 0:10) — make the viewer feel the problem

**Goal:** Show the pain without naming the product. If the viewer isn't pulled in within 5 seconds, they scroll past.

**Team to decide:**
- Who is on camera: [name(s)]
- Location: [where will you shoot — KIU pitch, dorm, common area, library? Be specific]
- The exact line spoken (1 sentence, 10 seconds max — read it aloud to time it): [draft your own line]
- What's visible in the shot: [describe the action and frame]

**Reference for inspiration (do not copy verbatim):** the opening of the pitch deck uses *"four students walking to a cancelled volleyball game"* as the hook — your video hook should hit the same beat (a real player about to miss a real match) but in your own words.

---

## Section 2: Problem (0:10 – 0:25) — show the old painful way

**Goal:** Show the current workaround failing visually. Can be silent with on-screen subtitles if the visuals are strong.

**Team to decide:**
- Who is on camera: [name(s)]
- The shot or shots: [describe — scrolling through a Messenger group? checking three apps? walking to the wrong venue?]
- Spoken line (if any — keep it short): [draft]

**What this section needs to communicate:** updates buried in chat, fragmentation across 2–3 platforms, the player either missing the message or finding it too late. Reference your interview evidence (10/10 interviews, 4.0/5 pain) if you want a number on screen.

---

## Section 3: Product (0:25 – 0:45) — live product on a real device

**Goal:** Show the real product completing the core flow. **This is the 20 seconds that absolutely must work.** No cuts within the demo if avoidable.

**Team to decide:**
- Who holds the player device: [name]
- Who holds the organizer device (for the change-the-time-trigger-push moment): [name]
- Which devices: [Android / iPhone, who's]
- The exact flow you'll show (must include the push notification appearing live within ≤5s of the organizer's edit):
  - Step 1: [describe]
  - Step 2: [describe]
  - Step 3: [describe — ideally the push notification arriving on the player device]

**Hard pre-check before shooting:** test the organizer-edit → player-receives-push end-to-end on your real production deployment **at least 3 times** before rolling camera. If push delay is >5 seconds, this section won't read on video. Fix the delay or move to the backup plan (Section: Backup).

---

## Section 4: Proof (0:45 – 0:55) — one real number

**Goal:** Show one number from your analytics that proves it works. Pull from the Django admin (see `04-gtm/traction/usage-log.md`). Don't round up. Don't project.

**Team to decide:**
- Who is on camera saying the number: [name]
- The number you'll cite — pull from the Django admin **the same day you shoot**:
  - Metric: [e.g. total joins (`match_joined`) / active users / signups since launch]
  - Value: [PULL FROM DJANGO ADMIN]
  - Source: Django admin, [date]
- The spoken line containing that number: [draft]

**If the number is small (e.g. 12 users), say 12.** The rubric explicitly rewards honesty here.

---

## Section 5: Call to Action (0:55 – 1:00) — URL + QR code

**Goal:** Tell the viewer where to go. Say the URL clearly and slowly. Show a QR code on screen.

**Team to decide:**
- Who is on camera: [name(s)]
- Where to shoot: [location]
- The exact line: [draft — say the URL at half-speed, e.g. "uni-sport-four-one-two dot pages dot dev"]
- QR code: generate one pointing to https://unisport-412.pages.dev (use any QR generator; print A4 size)

---

## Production planning (team decides together)

| Decision | Your answer |
|---|---|
| Shoot date and time | [agree as a team] |
| Total scenes to shoot | 5 (one per section above) |
| Director / call-the-shots role | [name] |
| Camera operator (the one holding the phone) | [name] |
| Editor (post-production, cuts the takes together) | [name] |
| Editing tool | [iPhone iMovie / CapCut / DaVinci Resolve — whoever's comfortable] |
| Upload destination | [Google Drive "Anyone with the link can view" / YouTube unlisted / direct repo commit if <25 MB] |

---

## Final video links (FILL AFTER SHOOTING)

**Google Drive link:** [paste after upload]
**YouTube unlisted link (backup):** [paste after upload]
**Local file path in repo (if committed directly):** [e.g. `09-final/demo-video.mp4`]

**Status:** [ ] Planned [ ] Shot [ ] Edited [ ] Uploaded [ ] Linked from README

---

## Backup plan if the shoot fails

If something blocks the shoot (rain, push not firing, team availability):

1. **Indoor fallback location:** dorm, library, common area — Sections 1 and 5 can be staged with a phone and printed QR code.
2. **Section 3 fallback:** if push doesn't fire within 5s on the live deployment, use a screen recording from a successful earlier test. Cut to it. Do not paper it over with voiceover.
3. **Last-resort fallback:** if no video at all by 21:00 tomorrow, commit this file with the script and status `[ ] Not shot — submission cycle did not allow`. You lose video points (~2 of 15), but you don't miss Checkpoint 4 entirely.

---

## Quality check before submitting

- [ ] Total spoken script under 60s when read at normal speed (stopwatch test)
- [ ] At least one team member on camera ≥10s total
- [ ] Live product on a real device ≥15s total
- [ ] Live URL spoken clearly AND shown as QR on screen
- [ ] Shoot date and team roles agreed
- [ ] Push end-to-end tested before rolling camera
- [ ] This file updated with the final link before 11 June 21:00
- [ ] Root README updated to link the video

---

*60-Second Launch Video Plan | UniSport | TheMergeConflicters | Lab 12*
