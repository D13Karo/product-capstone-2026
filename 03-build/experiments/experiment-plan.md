# Experiment Plan

**Team:** TheMergeConflicters
**Product:** KIU Sports Tracker
**Date launched:** 13 May 2026
**Owner:** Mariam Tskhomelidze

---

## 1. Hypothesis

We believe KIU students who participate in informal sports matches will sign up for early access to KIU Sports Tracker at a rate of 25% or more because they experience real pain when match time or location changes are buried in group chats and they miss games as a result.

---

## 2. Assumption Being Tested

Students who lose games to missed group chat updates care enough about the problem to take a concrete action — exchanging their email for early access — before the full product exists. If they will not act on the problem when the solution is one tap away, they will not switch from their current behaviour when the product launches.

---

## 3. Top 3 Riskiest Assumptions

| Rank | Assumption | Why risky | Why this experiment addresses it |
|------|------------|----------|----------------------------------|
| 1 | Students who experience group chat coordination pain will act on it when shown a targeted solution | All 10 interviews confirmed the pain, but interview willingness to complain is not the same as willingness to switch tools | A real signup action requires them to make a decision, not just agree verbally |
| 2 | The value proposition is clear in one screen without a product demo | Students may not understand what the app does from a short description and need to see the UI | A landing page with one headline and one visual tests whether the message lands without a walkthrough |
| 3 | Students can be reached through digital group channels without in-person persuasion | If only personal connections sign up, the channel is too narrow to support launch | Distributing through sports group chats and university networks tests passive reach beyond the team's immediate social graph |

---

## 4. Experiment Method

**Method chosen:** Smoke test

A one-screen landing page is published on Carrd (free tier). The page describes the core value proposition in one sentence, shows a single mockup image of the match list screen from the Stitch prototype, and presents one call to action: a Google Form signup for early access. No product is available yet — the signup tests whether students want it enough to act.

- **Channel:** KIU sports-related Messenger and WhatsApp groups; direct message to group admins of football, basketball, volleyball, and tennis groups; posted on KIU student notice boards if digital reach is insufficient by Day 3
- **Asset used:** Carrd landing page with one headline ("Never miss a KIU match update again"), one Stitch prototype screenshot, a 2-sentence description, and a Google Form for email + sport preference
- **Call to action:** "Join the early access list — be first to know when your match time changes"
- **Real target users are reached by:** sharing in groups where students already coordinate informal sports — not academic groups, not the team's own friend circles
- **What happens after a user responds:** email is captured in a Google Sheet; a one-question follow-up asks "What is the most frustrating part of organising sports at KIU?" to deepen interview coverage

---

## 5. Success, Gray Zone, Failure

These thresholds are set before launch and will not be changed after results appear.

- **Success threshold:** 25% or more of unique visitors submit the signup form
- **Gray zone:** 10% to 24% — demand signal is present but messaging or channel may be weak; run a second experiment with revised copy before committing full Sprint 2 scope
- **Failure threshold:** below 10% — the pain is real in interviews but students are not motivated to act on it digitally; revisit whether a digital-first solution is the right format or whether push notifications alone (Sprint 2) are the actual pull

---

## 6. Time Window and Sample Size

- **Experiment starts:** 13 May 2026
- **Experiment ends:** 21 May 2026 (Checkpoint 3 deadline)
- **Minimum sample target:** 50 unique visitors from non-team members
- **What counts as one valid data point:** one unique visit from a KIU student who arrived through a shared sports group link, not a direct link from a team member

---

## 7. Data Capture Plan

| Signal | How captured | Where recorded | Owner |
|--------|--------------|---------------|------|
| Unique page visits | Carrd built-in analytics | Experiment tracking sheet (Google Sheets) | Mariam T. |
| Form submissions (signups) | Google Form response sheet | Experiment tracking sheet — signups tab | Mariam T. |
| Sport preference | Google Form field (Football / Basketball / Volleyball / Tennis / Other) | Experiment tracking sheet — signups tab | Mariam T. |
| Follow-up qualitative answers | Google Form open text field | Interview extension notes in `01-discover/interviews/` | Davit |
| Referral source | Asked in Google Form ("How did you find this?") | Experiment tracking sheet — signups tab | Mariam T. |

Results are reviewed at Checkpoint 3 (May 21) before Sprint 3 planning. Davit (PO) decides scope implications based on the outcome.

---

## 8. Live Asset Checklist

- [x] Landing page published on Carrd and tested in incognito window
- [x] Google Form is live and responses flow to Google Sheet
- [x] Channel for real users selected (sports group chats — not academic or general groups)
- [x] Success and failure thresholds are frozen (set in section 5 above)
- [x] Team knows who monitors the experiment (Mariam T. checks daily, reports in standup AI note field)
- [x] Decision review date is set (May 21, Checkpoint 3)

---

## 9. Decision Rule

- **If result meets success threshold (≥25%):** proceed with Sprint 2 scope as planned; push notifications (S2-02) and organiser flow (S2-01) are the right next investments; the switching motivation is confirmed
- **If result falls below failure threshold (<10%):** pause and run a focused re-interview session before Sprint 2 planning; test whether the failure is a messaging problem (wrong value proposition copy) or a demand problem (students do not want a dedicated app and prefer to stay in chat); consider scoping Sprint 2 down to the push notification spike only

---

## 10. What Would Make This Experiment Invalid

- More than 30% of signups come from team members, family, or friends who were told directly about the project rather than discovering it through the sports group channel
- The landing page copy changes between launch and the May 21 review without recording the change date and original copy
- Fewer than 50 unique visitors are reached by May 21 — sample is too small to draw a meaningful conclusion; extend experiment window or expand distribution channel
- The Carrd analytics show most traffic came from direct URL access rather than the shared group link, indicating the spread was not through the target channel

---

*Experiment Plan | TheMergeConflicters | CS-PD-2026 | Spring 2026*
