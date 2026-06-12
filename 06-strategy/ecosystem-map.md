# Ecosystem Map

**Team:** TheMergeConflicters
**Product:** CampusSport
**Date:** 10 June 2026

---

## Overview

This document maps every relevant party in CampusSport's surrounding environment. The product is currently scoped to one university (KIU) with planned expansion to Free University, Ilia State University (ISU), and Tbilisi State University (TSU) in Sprints 5+ after Demo Day. The ecosystem reflects that geographic and institutional context.

---

## 1. Complements

Products and services that make CampusSport more valuable when used together.

| Complement | Description | Why it makes us more valuable |
|------------|-------------|-------------------------------|
| Google Calendar | The calendar most KIU students use to track classes and personal commitments | A player who can add a CampusSport match to their Google Calendar in one tap is more likely to commit, less likely to forget, and more likely to feel the time conflict if the match moves. A "Add to Calendar" button on the match detail screen converts an in-app RSVP into a system-level reminder. |
| Apple Health / Google Fit | Fitness tracking apps that log activity and sport minutes | A student tracking weekly active minutes gets independent confirmation of the value CampusSport delivers. We do not need integration to benefit — every match played increases the user's logged activity, which strengthens the habit loop around our product. |
| KIU university email + Single Sign-On | The KIU email infrastructure students already use daily | Our signup gate uses the KIU email domain to verify campus membership. The fact that students already have and check this email makes our signup almost frictionless and our identity verification credible without us building it from scratch. |
| Strava | Activity tracking community used by some KIU football and basketball players | Players who track activity socially have already opted into letting their sport be visible. CampusSport's match logs give them content to post; their activity creates positive social signal for organized informal sport that drives demand for tools like ours. |

---

## 2. Partners

Organizations that give us access, distribution, data, or credibility. Status is honestly reported — most are *Identified* at this stage because we are pre-launch.

| Organisation | What they provide | Relationship status | Next action |
|--------------|-------------------|---------------------|-------------|
| KIU Sports & Wellness Office | Endorsement credibility, access to KIU's official sports calendar, possible promotion in student newsletter | Identified | Davit (PO) drafts an introduction email by 17 June requesting a 30-minute meeting to demo CampusSport and propose a pilot during the autumn 2026 sports season |
| KIU Student Council | Distribution to student population via official channels, social media reach across all student cohorts | Identified | Mariam Pirtskhalava drafts a partnership proposal by 24 June offering anonymized aggregate match data in exchange for one post per month in the council's social channels |
| Free University, ISU, TSU sports coordinators (one named contact per university to be identified by Sprint 5) | Replicate the KIU playbook on three more campuses, expanding total addressable market 4× | Identified | After Demo Day (post-11 June), Davit identifies one named sports coordinator per university by 15 July via LinkedIn and KIU alumni network |
| Expo (the company behind expo-notifications) | Push notification infrastructure already in production use | Confirmed (commercial relationship — free tier) | Continue free-tier usage through MVP; evaluate paid tier when monthly active devices exceed 1,000 (~Sprint 8 estimated) |
| SendGrid (Twilio) | Transactional email for signup verification and password reset | Confirmed (commercial relationship — free tier) | Move to paid tier before public marketing launch beyond KIU |

**Relationship status definitions:**
- **Confirmed:** A formal agreement or working commercial relationship exists.
- **In discussion:** Active conversations have occurred. A contact name exists. None currently.
- **Identified:** We know this organization is strategically relevant but have not yet made contact.

---

## 3. Threats

Parties who could enter our market and compete. Including non-obvious platform, institutional, and technology shift threats per the rubric.

| Threat | Type | Likelihood | Our counter-strategy |
|--------|------|------------|----------------------|
| **Meta (Facebook / Messenger) ships event-level push notifications scoped to RSVP groups** | Platform | Low | Meta's existing event feature is a calendar entry, not an RSVP-with-quorum loop. Their revenue strategy does not direct engineering attention at granular community coordination for sub-100-person groups. If Meta does ship this, our counter is to be the cross-platform layer that organizers actually want — Meta will not allow the organizer the same control we do. We monitor the Meta developer changelog quarterly. |
| **KIU IT or KIU Sports & Wellness builds an in-house sports coordination feature inside the KIU student portal** | Institutional | Medium | We approach KIU Sports & Wellness as a partner before they build (see Partners §2). Offering anonymized aggregate data + an embeddable match feed positions us as the supplier rather than the competitor. If they build anyway, we have the moat of cross-university coverage (KIU + Free Uni + ISU + TSU) that an institutional tool cannot replicate without explicit data-sharing agreements between rival universities. |
| **A well-funded sports-tech startup (e.g. Spond, Heja, Playsta) enters the Georgian university market** | Direct competitor | Low (12-month horizon) | The Georgian university market is small enough to be unattractive to a well-funded generalist. Our counter is to lock in the organizer-side moat (switching cost via accumulated participant history and recurring schedules) before any generalist competitor reaches Tbilisi. |
| **Discord ships a "campus mode" with university email gating and dedicated sports event templates** | Platform | Low | Discord's organizer setup cost (server creation, role configuration, channel structure) is the existing friction that keeps adoption near zero at KIU. A "campus mode" would have to eliminate this friction, which would contradict Discord's core product identity. If it happens, we compete on simplicity per match — one post, no server. |
| **A KIU student team (next year's capstone or otherwise) builds an open-source clone** | Direct competitor (institutional) | Medium | The capstone cohort that produced CampusSport graduates and a successor team could build a copy. Our counter is to actively recruit one organizer per university into the moat before academic-year handover, so a clone would have to displace an existing solution rather than enter an empty market. |
| **An LLM-powered scheduling assistant (ChatGPT plugin, Google Assistant skill) absorbs informal-coordination problems** | Technology shift | Low–Medium (24-month horizon) | An LLM scheduling agent solves *one player's* schedule but does not coordinate a group around a shared canonical schedule. The push-to-all-participants-on-change pattern requires a group identity and a backend, not an individual agent. We watch this space and consider an LLM-assisted match-creation flow as a 2027 feature, not a threat to the core. |

**Threat type definitions:**
- **Platform:** A larger platform (Meta, Discord, Google) adds our core functionality as a feature.
- **Institutional:** A university or sports body builds their own version.
- **Direct competitor:** A startup with similar product targeting similar users.
- **Technology shift:** A new technology makes our current approach redundant or trivial to replicate.

---

## 4. Complementors

Parties whose product or service increases demand for ours, even without a formal relationship.

| Complementor | How they increase demand for us | Priority for engagement |
|--------------|----------------------------------|--------------------------|
| KIU intramural sports tournaments (university-organized seasonal tournaments) | Tournaments create more matches per week per team during the tournament window, directly increasing the count of matches users need to coordinate. A busy tournament week is our highest-demand week. | Medium — explore a lightweight integration where tournament organizers can post tournament fixtures in CampusSport in bulk |
| Georgian Football Federation grassroots programs | The federation runs youth and amateur grassroots programs that overlap with the KIU student demographic. Their programs encourage informal play, which drives demand for tools that coordinate that play. | Low — passive brand alignment, no direct engagement needed in the short term |
| Campus fitness influencers (Instagram accounts run by KIU students about KIU sport) | Influencers create social proof that organized informal sport is desirable, which drives student curiosity about how to find and join games. | Medium — Mariam Tskhomelidze identifies 2–3 KIU sports influencers via Instagram and offers them an early-organizer badge / partnership by 1 July |
| Sportswear brands' campus campaigns (Nike Campus, Adidas Campus, etc.) | These campaigns position informal sport as aspirational, which strengthens the social motivation to attend and not miss games — exactly the gap we close. | Low — passive; revisit if a campus-level campaign with a CampusSport-shaped brief becomes available |

---

## Strategic Priorities

**The partner relationship we should prioritise in the next 90 days is KIU Sports & Wellness Office.** They control the most credible distribution channel inside KIU (the student newsletter and official sports calendar), and they own the institutional relationship that becomes the largest single threat (Threats §2 row 2) if it sours. The specific next step is for Davit Karoiani (Product Owner) to send an introduction email by 17 June requesting a 30-minute demo meeting and proposing a pilot during the autumn 2026 sports season. In exchange for newsletter promotion and inclusion in the official sports calendar, we offer the office anonymized aggregate match data — number of matches per week by sport, participation trends, peak times — that helps them plan facility allocation. The partnership flips the most likely institutional threat into the strongest distribution channel. If formalized before Demo Day, it becomes a credibility signal worth pointing to directly in the pitch.

**The threat most likely to materialise is KIU IT or KIU Sports & Wellness building an in-house sports coordination feature inside the KIU student portal.** We rate this medium because the pain point we documented is highly visible and easy for an institutional team to spot once they look for it. Our counter-strategy is to be inside the institutional conversation before the institutional build starts — by offering Sports & Wellness an embeddable match feed and an anonymized data feed, we make CampusSport infrastructure rather than a competitor. If they build anyway, our cross-university moat (Free Uni, ISU, TSU coverage planned for Sprints 5+) gives us a dimension a single-institution tool structurally cannot match.

**The complementor we could engage for a lightweight co-promotion is KIU campus sports influencers — specifically 2–3 student-run Instagram accounts that already post about KIU sport.** A lightweight engagement looks like: we offer them an "Early Organizer" badge in the app and they post one Story about CampusSport in exchange. No money changes hands. The first step is for Mariam Tskhomelidze to identify candidate accounts via Instagram search ("KIU football", "KIU basketball", "KIU sports") by 24 June and send a personal DM to each. Two yeses out of five DMs is a successful round. Their audience is a near-perfect overlap with our target segment, and a single Story can produce more signups than a week of cold outreach.
