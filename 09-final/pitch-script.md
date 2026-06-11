# UniSport Pitch Script — 7 minutes, 4 speakers

**For:** Demo Day, 11 June 2026
**Target runtime:** 6:30 (with ≤30s buffer to the 7:00 cap)

> **Speakers — recommended rotation:**
> - **D** = Davit (PO) — opens & closes, business slides
> - **MP** = Mariam Pirtskhalava — discovery & competition
> - **MT** = Mariam Tskhomelidze — analytics & growth
> - **L** = Levan — the live demo
>
> Swap if someone is uncomfortable with their slot. Davit-heavy by design (PO leads the pitch).
>
> *(Stage directions in italics. Beats are pauses. Don't skip them.)*

---

## Slide 1 — Problem [DAVIT, 0:00–0:40]

**[D walks to the front, holds one beat of silence, makes eye contact, then begins.]**

> Right now, somewhere on KIU campus, four students are walking to a volleyball match. They don't know it yet — but it was cancelled an hour ago. The update is sitting in a Messenger group, buried under forty unrelated messages.
>
> We interviewed ten KIU students who play informal sports. Every single one — ten out of ten — told us the same thing: they cannot trust the schedule. Seven of them missed at least one game last semester. Average pain: four out of five. Two campus sports groups have already completely dissolved — not from lack of interest, from logistics fatigue.
>
> One of our interviewees, Giorgi, said it best:
>
> *"I muted the group because it was too noisy. Then I missed the time change. You can't win either way."*

**[Beat — 2 seconds.]**

---

## Slide 2 — Solution [DAVIT, 0:40–1:05]

> UniSport replaces this with one screen. The organizer posts a match once — sport, time, place, player limit. Every registered KIU player gets a push notification. When anything changes — time, venue, cancellation — the notification fires automatically.
>
> No re-announcement cycle. No three-app check. One source. One push. One truth.

**[Pass to Mariam P.]**

---

## Slide 3 — Why Now [MARIAM P., 1:05–1:35]

> Two things changed in the last two years. Expo Push Service went free for cross-platform mobile push in early 2024 — before that, building this required paid services. The infrastructure didn't exist at this cost two years ago.
>
> In parallel, Facebook Groups algorithmically deprioritized group posts since 2023. The "official" announcement channel became measurably less reliable.
>
> The gap finally opened. We're filling it.

**[Pass back to Davit.]**

---

## Slide 4 — Market [DAVIT, 1:35–2:05]

> Bottom-up. Five hundred active KIU players at two dollars a month organizer tier gives us twelve thousand a year at KIU alone. Adding Free Uni, ISU, and TSU brings us to seventy thousand.
>
> The Georgian university market is eight hundred forty thousand a year. The European TAM — fifty million students playing informal sport weekly — is one point two billion.
>
> Numbers we built ourselves, not numbers from a report.

**[Pass to Levan for the live demo.]**

---

## Slide 5 — Product + LIVE DEMO [LEVAN, 2:05–3:35]

**[L rotates the demo device toward the audience.]**

> This is the live product, at uni-sport-four-one-two dot pages dot dev. I'm signed in as a KIU student.

**[Step 1 — match list visible]**
> Only KIU matches. No spam, no chat.

**[Step 2 — tap a match]**
> Sport, time, venue, who's in. Currently five of ten.

**[Step 3 — tap Join]**
> One tap. "You're in." Quorum just moved to six of ten.

**[Step 4 — switch to second device, organizer view]**
> Now I'm the organizer. Same match. Weather changed. I move the time from six to seven PM. One field. Save.

**[Step 5 — switch back to player device, wait for push notification]**
> And without me doing anything —

**[Push notification appears, hold for 2 seconds]**

> — every player just got the update. New time. Same match. No re-announcement.

**[Step 6 — close the demo]**
> Every action you just saw is logged in PostHog. That's how we measure our North Star Metric — matches joined per active user per week.

**[Pass to Mariam T.]**

> ⚠️ **Test this end-to-end on both devices BEFORE you go in.** If push doesn't fire within 5 seconds, the demo fails. Backup plan: pre-recorded screen capture, played in silence while Levan narrates the same script over it.

---

## Slide 6 — Traction [MARIAM T., 3:35–4:05]

> Real numbers from our PostHog dashboard, pulled this morning.

**[Read the actual numbers you pulled — see posthog-howto.md.]**

> [N] signups since launch on May seventh.
> [X] of them have fired the activation event — joining a match.
> That's a [%] activation rate in our first thirty-three days at one university.
>
> Our smoke test in May produced [Y] signups from real KIU sports group chats before the product shipped. That's the signal that confirmed demand.

**[Pass back to Davit.]**

> ⚠️ **If your real numbers are small, say the real numbers.** The rubric explicitly rewards honesty here.

---

## Slide 7 — Business Model [DAVIT, 4:05–4:35]

> Free for players, always. The premium tier is the organizer — two dollars a month for recurring schedules, match templates, participant history.
>
> Mamuka, in interview five, told us he'd switch immediately for auto-reminders.
>
> Our blended CAC is sixty-four cents. LTV is twenty-one dollars on the conservative value proxy. The ratio is thirty-three to one. We stress-tested it — halve the lifetime, still sixteen to one.

**[Pass to Mariam T.]**

---

## Slide 8 — Go-to-Market [MARIAM T., 4:35–5:10]

> One organizer brings twenty players. That's the leverage.
>
> We go directly to organizers — Messenger DM, sixty cents CAC, eighty percent of their player group follows them in. Twenty-five organizers across KIU, Free Uni, ISU, and TSU gets us to five hundred users for three hundred dollars in team time. Twelve weeks.
>
> Group chats and QR posters at the football pitch are our supporting channels — both at sub-dollar CAC.

**[Pass to Mariam P.]**

---

## Slide 9 — Competition [MARIAM P., 5:10–5:40]

> Every general-purpose chat app fills this gap by default — and that's exactly why the gap exists. Messenger, Facebook, WhatsApp were never designed for time-sensitive coordination. Discord is structurally closer, but the organizer setup cost kills adoption.
>
> We score one on existing user adoption. That's honest. We score five on the four dimensions that actually matter for this problem.
>
> Our moat is switching costs. Once an organizer has built a roster and a match history in UniSport, recreating it is real work. Cotne in interview ten proved this against us — his Telegram bot failed because the organizer wouldn't migrate from Messenger. Once we have the organizer, the same mechanic protects us.

**[Pass back to Davit for the close.]**

---

## Slide 10 — Ask + Close [DAVIT, 5:40–6:30]

> We're raising twenty-five thousand at two hundred thousand pre-money. Eleven percent dilution.
>
> Twelve thousand goes to engineering for six months — Levan and I keep shipping, we go native on iOS and Android. Eight thousand goes to four-university expansion — organizer outreach, QR posters, the playbook we just described. Five thousand goes to GDPR compliance and legal — formal registration with the Georgian PDP Service.
>
> By March 2027, we will have five hundred active users across four Tbilisi universities, twenty-five paying organizers at six hundred a month MRR, and a signed MOU with KIU Sports and Wellness.

**[Beat. Eye contact across the panel.]**

> Because right now, somewhere on KIU campus, four students are still walking to a cancelled volleyball game.

**[Beat.]**

> Help us make that the last time.

**[Beat.]**

> Thank you. We're happy to take your questions.

---

## Speaker time distribution

| Speaker | Slides | Total time |
|---|---|---|
| Davit | 1, 2, 4, 7, 10 | ~2:30 |
| Mariam P. | 3, 9 | ~1:00 |
| Mariam T. | 6, 8 | ~1:05 |
| Levan | 5 + demo | ~1:30 |
| **Total** | | **~6:05** + 25s buffer = **~6:30** |

## Q&A — top 5 anticipated questions with short answers

1. **"Why won't this just become another chat people ignore?"**
   → Push-only, no in-app chat, single canonical channel. Architectural decision, not a feature flag. Every workaround in our research that added a chat channel made fragmentation worse.

2. **"What's the moat?"**
   → Switching costs at the organizer layer. Once an organizer has built a roster + match history in UniSport, recreating it is real work. Cotne's failed Telegram bot proves the mechanic works against incumbents — we expect it to work for us once organizers adopt.

3. **"Why $2/month — what's the willingness-to-pay evidence?"**
   → Mamuka explicitly: *"If auto-reminders existed, I would switch immediately."* Validating the $2 number in Phase 2 with 5 paid pilot organizers post-Demo-Day.

4. **"What happens after KIU?"**
   → Same playbook at Free Uni, ISU, TSU — confirmed in our roadmap. The organizer-outreach channel is geography-independent.

5. **"Why not just use Discord?"**
   → Discord requires the organizer to set up and moderate a server — that's exactly the work we eliminate. Zero of ten interviewees used Discord for informal sports coordination.

## Final pre-pitch checklist

- [ ] All four speakers have read their lines aloud once
- [ ] Slide 6 numbers pulled from PostHog and inserted
- [ ] Live URL https://unisport-412.pages.dev works in the venue
- [ ] Push notification tested end-to-end (≤5s delay)
- [ ] Two phones charged ≥80%, on do-not-disturb OFF
- [ ] Backup pre-recorded demo screen capture ready (in case live demo fails)
- [ ] One full run-through done, timed under 7:00
