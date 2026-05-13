# AI Usage Log

**Team:** TheMergeConflicters
**Product:** KIU Sports Tracker

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
Task: Generate fully completed event-schema.md and north-star-metric.md matching teacher templates and grading rubric
Prompt summary: Provide complete implementations of event schema (7 events across AARRR funnel with properties, payloads, privacy confirmation) and NSM document for KIU Sports Tracker, based on interview evidence from 10 KIU student interviews
Files changed: 03-build/analytics/event-schema.md, 03-build/analytics/north-star-metric.md
Result: Modified
Review notes: user_session_started initially had only one event-specific property; added app_open_source as second required property. Sprint 1 capacity was recalculated from 67% to 56% to meet rubric ≤60% target. All event names confirmed against snake_case past-tense naming convention. Privacy confirmation checklist verified — no PII in any event property.
Reviewer: Davit Karoiani

---
Date: 2026-04-16
Story: Lab 5 — High-fidelity prototype documentation
Tool: Claude Code
Task: Generate stitch-prototype-link.md matching teacher template with design decisions tied to interview evidence
Prompt summary: Document the Stitch prototype for KIU Sports Tracker including 3 screens, Stitch brief, prompts used, and design decisions backed by interview quotes
Files changed: 02-design/prototypes/high-fidelity/stitch-prototype-link.md
Result: Accepted
Review notes: All three design decisions linked to specific interview evidence. Stitch link confirmed accessible in incognito window. Template structure verified against Lab-5 template.
Reviewer: Davit Karoiani

---
Date: 2026-04-16
Story: Lab 6 — Product roadmap, Sprint 1 plan, process map
Tool: Claude Code
Task: Generate product-roadmap.md, sprint-1-plan.md, and process-map.md matching teacher templates with full sprint arc, user stories, and Scrum process
Prompt summary: Build complete Lab 6 deliverables for KIU Sports Tracker with 4-sprint roadmap (40 story points), Sprint 1 plan with 4 user stories and Given-When-Then ACs, and process map with AI review process and branching conventions
Files changed: 03-build/roadmap/product-roadmap.md, 03-build/roadmap/sprint-1-plan.md, 03-build/workflow/process-map.md
Result: Modified
Review notes: Sprint 2 total reduced from 15 to 12 points. Sprints 2/3/4 theoretical max capacity added. Sprint 4 risks section added. Calibration anchor table updated to include AI review time in hour estimates. All story ACs verified in Given-When-Then format. Interview evidence quotes confirmed against interview logs.
Reviewer: Davit Karoiani

---
Date: 2026-04-24
Story: Lab 7 — Architecture package and experiment plan
Tool: Claude Code
Task: Generate system-design.md, tech-stack.md, architecture-diagram-source.md (Mermaid), risk-register.md, and experiment-plan.md matching teacher templates and grading rubric
Prompt summary: Build complete Lab 7 architecture package for KIU Sports Tracker Sprint 1: system design with 13 sections including component breakdown, request lifecycle, and data flow; tech stack with Next.js/Supabase/PostHog/Vercel choices and rejected alternatives; Mermaid architecture diagram; risk register with 4 risks and 2 spikes; experiment plan with smoke test hypothesis and numeric thresholds
Files changed: 03-build/architecture/system-design.md, 03-build/architecture/tech-stack.md, 03-build/architecture/architecture-diagram-source.md, 03-build/architecture/risk-register.md, 03-build/experiments/experiment-plan.md
Result: Modified
Review notes: Architecture diagram PNG exported from mermaid.live by Davit and committed as architecture-diagram.png. Experiment plan dates updated to reflect actual launch date (May 13 2026). Tech stack choices verified — Next.js, Supabase, PostHog, Vercel confirmed as team's actual intended stack. Race condition mitigation strategy (Postgres transaction with row-level check) reviewed and confirmed technically correct. All 13 system design sections present and complete.
Reviewer: Davit Karoiani

---
