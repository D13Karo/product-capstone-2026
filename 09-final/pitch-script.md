# UniSport Pitch — relay order, each speaks once

**Demo runs on PC** (two browser windows: player + organizer). Improvised delivery — these are the talking points, not a word-for-word lock.

---

## 1. DAVIT — Problem + Solution

Right now, somewhere on KIU campus, four students are walking to a volleyball match. They don't know it yet, but it was cancelled an hour ago. The cancellation is sitting in a Messenger group, buried under forty unrelated messages, and nobody saw it.

This isn't a rare story. We interviewed ten KIU students who play informal sports, and all ten told us the same thing: they cannot trust the schedule. Seven of them missed at least one game last semester because an update got lost in a group chat. The average pain they described was four out of five. Two campus sports groups we know of have already completely fallen apart — not because people stopped caring, but because the coordination kept failing. Giorgi put it perfectly: "I muted the group because it was too noisy. Then I missed the time change. You can't win either way."

That's the problem we're solving. UniSport replaces all of that with one screen. The organizer posts a match once — sport, time, place, player limit — and every registered player at their university gets it instantly. When anything changes, the notification goes out automatically. No reposting across three apps. No checking five chats before you leave the house. One source of truth, one push, and you always know where to be.

Levan will show you how it works.

---

## 2. LEVAN — Live Demo (PC, two browser windows)

This is the real product, live right now at unisport-412.pages.dev. On the left I'm signed in as a regular KIU student, on the right I'm the organizer of a match.

As a student, this is my feed. Only matches from my university — no spam, no chat, just games I can join. I'll tap this football match. I can see the sport, the time, the venue, and that five of ten spots are filled. I tap join, and I'm in — now it's six of ten.

Now watch the organizer side. Say the weather changed and I move the game from six to seven PM. I change one field, I save. And without me doing anything else, without reposting anywhere — every player who joined just got a notification with the new time. That's the whole product. The change reached everyone the moment it happened.

And every action you just saw is tracked in PostHog, which is how we measure whether people are getting value. Mariam will tell you why now is the moment for this.

---

## 3. MARIAM P. — Why Now + Market + Competition

Two things made this possible only recently. First, free push notifications across phones became available to small teams in early 2024 — before that, building this cost real money. Second, Facebook Groups quietly buried organic posts over the last couple of years, so the channel everyone used as their official announcement board became unreliable exactly when people needed it. The gap opened, and we're filling it.

And it's a real market. Five hundred active players at KIU is twelve thousand dollars a year. Expand to Free Uni, ISU, and TSU and that becomes seventy thousand. Across all Georgian universities it's over eight hundred thousand, and the European student market is more than a billion. We built those numbers from the ground up.

People ask, isn't this just another chat app? No. Messenger, WhatsApp, Facebook were never built for time-sensitive coordination — they treat a cancellation the same as a meme. Discord is closer, but it makes the organizer run a whole server, which nobody wants. We're honest that we score low on existing adoption — we're new. But on what actually matters for this problem, we win. And our real protection is switching cost: once an organizer builds their roster and match history in UniSport, leaving means rebuilding it all. Cotne proved this — he built a Telegram bot that worked perfectly, and it still failed because the organizer wouldn't switch platforms. That same stickiness protects us once organizers are in.

---

## 4. MARIAM T. — Traction + Business + Growth + Ask

Here's where we are. We launched four weeks ago at a single university, with zero marketing spend, and we already have around eighteen real signups. Before we'd even built the product, our smoke test pulled real signups straight from KIU sports group chats — that's the demand signal that told us this was worth building. And underneath all of it, ten interviews at an average pain of four out of five. We're early, but every number here is real and every one points the same direction.

The business is simple. Free for players, always. Organizers pay two dollars a month for the features that save them an hour a week — recurring schedules, templates, history. Cost to acquire a user is sixty-four cents, lifetime value is twenty-one dollars, that's thirty-three to one — and even if we're wrong by half, still sixteen to one.

The growth model is the best part. One organizer brings their whole group — about twenty players. So we go straight to organizers, one Messenger message at a time, sixty cents each, and eighty percent of their players follow them in. Twenty-five organizers across four universities gets us to five hundred users for three hundred dollars and twelve weeks of work.

So here's our ask. We're raising twenty-five thousand at a two hundred thousand pre-money valuation — eleven percent. Twelve thousand to engineering, eight thousand to four-university expansion, five thousand to compliance. By March 2027 we'll have five hundred active users, twenty-five paying organizers, and a signed partnership with KIU's sports office.

Because right now, somewhere on this campus, four students are still walking to a game that was cancelled an hour ago. Help us make that the last time. Thank you — we're happy to take your questions.

---

## Q&A — anyone grabs it

- **Just another chat?** Push-only, no chat. Adding chat is what broke everything else.
- **Moat?** Switching costs. Cotne's bot proves organizers don't move once settled.
- **Why $2?** Mamuka literally said "I'd switch immediately."
- **After KIU?** Same playbook — Free Uni / ISU / TSU.
- **Why not Discord?** The server setup is exactly the work we remove.

---

⚠️ **Levan: open both browser windows and sign in before you start.** Test the push once.
✅ Traction number (~18 signups) is real, from the Django admin. Honest + small beats fake + big.
