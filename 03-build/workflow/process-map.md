# Team Development Process Map

**Team:** TheMergeConflicters  
**Product:** KIU Sports Tracker  
**Last Updated:** 16 April 2026  
**Version:** 1.0

---

## Overview

This document describes how work flows through our team from idea to deployed increment. It is the operational agreement all team members commit to for the duration of the sprint arc. If a process step is unclear, this document is the source of truth — not a Messenger message, not a verbal agreement in lab.

---

## Scrum Roles

| Role | Name | Responsibilities |
|------|------|-----------------|
| Product Owner | Davit Karoiani | Owns and orders the backlog. Accepts or rejects sprint increments against AC. Reviews AI-generated features against interview evidence. Final decision on scope changes. |
| Scrum Master (Sprint 1) | Mariam Tskhomelidze | Facilitates standups, sprint planning, review, and retrospective. Runs AI output review check at each standup. Maintains `docs/ai-usage-log.md` currency. Escalates unresolved blockers. |
| Scrum Master (Sprint 2) | Levan Kovziridze | Rotates after Sprint 1 retrospective. Same responsibilities. |

---

## Story Lifecycle

A story moves through these states in order. A story cannot skip states.

```
Backlog → Refined → Sprint Backlog → In Progress → In Review → Done
```

| State | Meaning | Who Sets It |
|-------|---------|------------|
| Backlog | Exists but not yet ready for a sprint | PO |
| Refined | Has user story, AC, story points, and interview evidence. Ready to commit to a sprint. | PO after team refinement |
| Sprint Backlog | Committed to the current sprint. Developer and AI tool assigned. | SM after Sprint Planning |
| In Progress | Developer has started work. Branch created. | Developer who pulled the story |
| In Review | PR raised. Awaiting human review. | Developer |
| Done | All DoD criteria met. PO confirmed AC. Merged to main. | PO |

**Only one story per developer may be In Progress at a time.** If a second story is pulled before the first is merged, the SM flags it at standup.

---

## Definition of Done

A story is Done when every item below is confirmed. Not "mostly done." Every item.

- [ ] Code reviewed by at least one team member who is not the original author
- [ ] Pull request merged to `main` via GitHub PR — no direct pushes to main
- [ ] Acceptance criteria confirmed as met by the Product Owner — not by the developer who built it
- [ ] If AI-generated: all AI-generated code blocks are annotated with inline comments explaining the logic in the reviewer's own words
- [ ] If AI-generated: entry added to `docs/ai-usage-log.md` before the PR is raised
- [ ] Feature works in the deployed Vercel environment — not just locally on the developer's machine
- [ ] No new known bugs introduced to the main branch

A story that is "done except for deployment" is In Review, not Done. A story that "works locally" is In Review, not Done.

---

## AI Review Process

All team members use AI tools. The following steps apply to every piece of AI-generated output before it is committed to any branch.

### Review Steps

1. **Generate:** Developer uses the designated AI tool for the story (assigned at Sprint Planning).
2. **Read every line:** Developer reads the entire AI output before accepting any of it. No tab-to-accept without reading the line.
3. **Check against AC:** For every acceptance criterion on the story, run through the generated code and confirm the AC produces a pass result. If any AC fails, edit the output until it passes — do not raise a PR with a known AC failure.
4. **Security and privacy check:** For any endpoint, form, or data-handling code — confirm no SQL injection vector, no PII in logs or event properties, no hardcoded credentials.
5. **Annotate:** Add inline comments to AI-generated blocks explaining the logic in the developer's own words. If you cannot explain it, you cannot merge it.
6. **Log:** Add an entry to `docs/ai-usage-log.md` before raising the PR. The PR will be returned without review if the log entry is missing.
7. **PR review:** The human reviewer checks annotations and the log entry as part of their review. A PR from AI-generated code with no annotation is returned to the developer without merge.

### Default AI Tool per Story Type

| Story Type | Default Tool | Why |
|-----------|-------------|-----|
| UI screens and components | Google Stitch | Fastest path from AC to working UI; already used in Lab 5 prototype |
| Complex multi-file backend logic | Claude Code | Best at understanding codebase context across multiple files |
| Auth flow and AI features | Google AI Studio | Prompt prototyping before API integration; handles session logic well |
| Boilerplate, repetitive patterns | GitHub Copilot | Ambient completion for repetitive code — always on in IDE |

Tool assignments are made at Sprint Planning and documented in `sprint-1-plan.md`. Developers can change the tool if a better choice emerges. The change must be noted in the `ai-usage-log.md` entry with a reason.

---

## Branching and Pull Request Process

### Branch Naming

```
feature/[story-id]-[short-description]
fix/[story-id]-[short-description]
```

Examples:
```
feature/s1-01-user-auth
feature/s1-02-match-list-screen
fix/s1-04-join-confirmation-redirect
```

Branches are created from `main`. No branching from other feature branches.

### PR Process

1. Developer opens PR to `main` when the story passes all local AC checks and is deployed to Vercel preview.
2. **PR title format:** `[S1-XX] Short description` — e.g. `[S1-04] Join match and confirmation screen`
3. **PR description must include:**
   - Full user story (copy from sprint plan)
   - AC checklist — mark each AC as ✅ Passed or ❌ Failed with evidence (screenshot or console log)
   - AI tool used and one-line summary of what it generated (or "No AI used")
   - Link to the `ai-usage-log.md` entry (or "No AI used")
   - Screenshot or screen recording of the working feature in the Vercel preview URL
4. One team member reviews and approves — cannot be the original author.
5. PO (Davit) confirms AC is met — this can happen in PR comments or at the next standup.
6. Reviewer merges — not the original developer.

### No direct pushes to main

Any commit pushed directly to `main` without a PR is a process violation. The SM (Mariam T. in Sprint 1) flags it at the next standup and the commit is reverted if it has not been reviewed.

---

## Standup Format

**Cadence:** Daily  
**Location:** Messenger group chat (TheMergeConflicters)  
**Time:** 10:00 every day  
**Format:**

```
Yesterday: [What I completed — be specific, include story ID and whether it's In Review or Done]
Today: [What I am working on — include story ID]
Blocker: [Anything stopping me — or "none"]
AI note: [What AI generated yesterday. Accepted / modified / discarded. One sentence.]
```

**Who posts:** Every team member posts independently by 10:00. If a member has not posted by 11:00, the SM sends a direct message. Missing two standups in a row without notice is raised at the next Sprint ceremony.

---

## Blocker Resolution

| Blocker Type | First Action | If Unresolved After 24h |
|-------------|-------------|------------------------|
| Technical (code, environment, deployment) | Post in standup with specific description and what was already tried | SM escalates to full team in Messenger with story ID |
| Dependency on another story not yet merged | Flag in PR description or standup with the blocking story ID | SM reprioritises or re-assigns the blocking story |
| AI tool failure or hallucination | Note in `ai-usage-log.md`, try the next default tool in the table above | Bring to next standup for team decision; PO decides if story needs re-estimation |
| External dependency (API down, third-party quota) | Note in risk register, try alternative | PO adjusts sprint scope if unresolved by Day 5 of sprint |

---

## Sprint Ceremonies: Who Does What

| Ceremony | Facilitator | Required Attendees | Output |
|----------|------------|-------------------|--------|
| Sprint Planning | Scrum Master | All team members | Committed sprint backlog with AC, assignees, AI tools, and calibration anchors agreed |
| Daily Standup | Async — no facilitator | All team members post by 10:00 | Blockers surfaced; AI output reviewed; coordination confirmed |
| Sprint Review | Product Owner | All team members | PO accepts or rejects each story against AC. Backlog updated. Velocity recorded. |
| Retrospective | Scrum Master | All team members | 1 to 3 concrete process changes written into next sprint plan |

---

## ai-usage-log.md Entry Format

All AI-assisted work is logged in `docs/ai-usage-log.md`. This file is audited at Checkpoint 3.

```
---
Date: YYYY-MM-DD
Story: [Story ID] — [Story summary]
Tool: [Google Stitch / Claude Code / Google AI Studio / GitHub Copilot]
Task: [What the AI was asked to generate or assist with]
Prompt summary: [Brief description of the prompt used]
Files changed: [List of files the AI output touched]
Result: Accepted / Modified / Discarded
Review notes: [What was checked. What was changed from AI output. Any errors or hallucinations caught.]
Reviewer: [Name]
---
```

The log is a running file — append new entries, never overwrite old ones. The SM checks the log is current at each Sprint Review.

---

## Change Log

| Date | Version | Changes | Author |
|------|---------|---------|--------|
| 16 April 2026 | 1.0 | Initial process map | Mariam Tskhomelidze |

---

*Process Map | TheMergeConflicters | CS-PD-2026 | Spring 2026*
