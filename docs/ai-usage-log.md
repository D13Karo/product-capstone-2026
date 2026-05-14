# AI Usage Log

**Team:** TheMergeConflicters
**Product:** CampusSport

This file is updated every time AI-generated output is used in the project. It is audited at Checkpoint 3.

---

## Entry Format

```
Date: YYYY-MM-DD
Story: [Story ID] — [Story summary]
Tool: [Google Stitch / Claude Code / Google AI Studio / GitHub Copilot]
Task: [What the AI was asked to generate or assist with]
Prompt summary: [Brief description of the prompt used]
Files changed: [List of files the AI output touched]
Result: Accepted / Modified / Discarded
Review notes: [What was checked. What was changed from AI output. Any errors or hallucinations caught.]
Reviewer: [Name]
```

---

## Log Entries

---
Date: 2026-04-13
Story: Lab 5 — Event schema and North Star Metric
Tool: Claude Code
Task: Assist with structuring the event schema and NSM document — team had defined the events and properties, AI helped format and fill in the template sections
Prompt summary: Given our 7 defined events (match_joined, user_signup_completed, etc.) and their properties from team discussion, format these into the event-schema template with example payloads
Files changed: 03-build/analytics/event-schema.md, 03-build/analytics/north-star-metric.md
Result: Modified
Review notes: AI output used as a formatting scaffold. Team added the second required property (app_open_source) to user_session_started which AI had missed. All event names verified against naming convention by Davit. Privacy checklist confirmed manually — no PII in any property. NSM rationale written by team based on interview synthesis.
Reviewer: Davit Karoiani

---
Date: 2026-04-16
Story: Lab 5 — High-fidelity prototype documentation
Tool: Claude Code
Task: Help structure the stitch-prototype-link.md template — team had the Stitch link, design decisions, and interview quotes; AI helped fill in the template format
Prompt summary: Format our Stitch prototype notes and design decisions into the template structure
Files changed: 02-design/prototypes/high-fidelity/stitch-prototype-link.md
Result: Modified
Review notes: Design decisions and interview evidence written by team. AI output used for template formatting only. Stitch link and incognito test confirmed by Davit.
Reviewer: Davit Karoiani

---
Date: 2026-04-16
Story: Lab 6 — Product roadmap, Sprint 1 plan, process map
Tool: Claude Code
Task: Help draft and structure the roadmap and sprint plan documents — team defined all stories, points, assignees, and ACs; AI assisted with document structure and formatting
Prompt summary: Given our agreed sprint structure (4 sprints, 40 points, 4 stories in Sprint 1), format these into the roadmap and sprint plan templates with correct Given-When-Then AC format
Files changed: 03-build/roadmap/product-roadmap.md, 03-build/roadmap/sprint-1-plan.md, 03-build/workflow/process-map.md
Result: Modified
Review notes: All user stories, ACs, assignees, and story points defined by team in planning session. AI helped structure into template format. Capacity calculation corrected by Davit (56% of max, not 67%). Interview evidence citations verified against actual interview logs. Process map branching conventions and DoD agreed by team before writing.
Reviewer: Davit Karoiani

---
Date: 2026-04-24
Story: Lab 7 — Architecture package and experiment plan
Tool: Claude Code
Task: Help document the architecture decisions the team had already discussed — component choices, stack rationale, and risk identification; AI assisted with structuring into templates
Prompt summary: Given our agreed stack (Next.js, Supabase, PostHog, Vercel) and identified risks, format these into the system-design, tech-stack, and risk-register templates
Files changed: 03-build/architecture/system-design.md, 03-build/architecture/tech-stack.md, 03-build/architecture/architecture-diagram-source.md, 03-build/architecture/risk-register.md, 03-build/experiments/experiment-plan.md
Result: Modified
Review notes: Stack choices decided by team. AI formatted decisions into template sections. Architecture diagram Mermaid source reviewed and PNG exported by Davit. Experiment thresholds (25% success, 10% failure) set by team. Race condition spike identified by Levan during review. Experiment dates updated to reflect actual launch (May 13).
Reviewer: Davit Karoiani

---
Date: 2026-05-13
Story: Lab 8 — Growth modeling deliverables
Tool: Claude Code
Task: Help structure the growth strategy and unit economics documents — team identified channels and assumptions; AI helped with template formatting and unit economics calculations
Prompt summary: Given our 3 channels (organiser outreach, group chats, QR posters) and LTV inputs from team discussion, format into growth-strategy and unit-economics templates and check the math
Files changed: 04-gtm/growth-strategy.md, 04-gtm/financials/unit-economics.md, 04-gtm/loops-and-moats.md
Result: Modified
Review notes: Channel selection made by team based on interview evidence. AI helped structure into template format and verify unit economics calculations. K-factor estimate (0.135) and network effect threshold (5 organisers + 40 players) discussed and agreed by team before writing. LTV proxy derivation reviewed by Davit — time-saved calculation confirmed against interview data. Defensibility section written honestly by team.
Reviewer: Davit Karoiani

---
Date: 2026-05-07
Story: S1-01, S1-02, S1-03, S1-04 — Sprint 1 full frontend build
Tool: Claude Code
Task: Build the complete React Native + Expo frontend — auth screen, feed screen, match detail, join confirmation, theme system, mock data layer, sport icons
Prompt summary: Session-based iterative build; implemented university email domain validation, match filtering by university domain, ThemeContext for dark/light mode, MaterialCommunityIcons sport icons replacing all emoji placeholders
Files changed: app/auth.tsx, app/feed.tsx, app/match/[id]/index.tsx, app/match/[id]/confirm.tsx, app/index.tsx, app/_layout.tsx, lib/mock-data.ts, constants/universities.ts, constants/colors.ts, context/ThemeContext.tsx, context/AuthContext.tsx, components/SportIcon.tsx
Result: Modified
Review notes: All generated code read line by line. Domain validation logic and ThemeContext reviewed by Davit. All Sprint 1 ACs verified manually against acceptance criteria in sprint-1-plan.md. Chess added as 6th sport after team discussion. Tournament section added to feed.
Reviewer: Davit Karoiani

---
Date: 2026-05-14
Story: Sprint 1 close-out — architecture docs and standup log
Tool: Claude Code
Task: Update architecture documents to reflect actual Sprint 1 stack (React Native + Expo, not the earlier Next.js draft); create standup log and usability findings
Prompt summary: Rewrite system-design.md and tech-stack.md to match what was actually built; create docs/standup-log.md with Sprint 1 entries; create 02-design/user-testing/usability-findings.md from 5-user test
Files changed: 03-build/architecture/system-design.md, 03-build/architecture/tech-stack.md, docs/standup-log.md, 02-design/user-testing/usability-findings.md
Result: Modified
Review notes: Architecture documents originally drafted with Next.js + Supabase stack; rewritten to reflect React Native + Expo. Content reviewed by Davit for accuracy against actual repo. Usability findings written from real participant sessions conducted by Mariam Pirtskhalava.
Reviewer: Davit Karoiani

---
