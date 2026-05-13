# Architecture Diagram — Source

**Final required file:** `03-build/architecture/architecture-diagram.png`
**Team:** TheMergeConflicters
**Product:** KIU Sports Tracker
**Date:** 24 April 2026

> This file contains the Mermaid source for the architecture diagram. Export the diagram below to PNG using the [Mermaid Live Editor](https://mermaid.live), paste the code, and save the PNG as `architecture-diagram.png` in this folder.

---

## 1. Diagram Goal

```text
This diagram shows how a KIU student signs up, browses matches, and joins one across the Sprint 1 system.
```

---

## 2. Required Boxes Included

| Box | Included | Notes |
|-----|----------|-------|
| User | Yes | Mobile browser entry point |
| Browser / client app | Yes | Next.js frontend |
| Frontend application | Yes | Next.js App Router |
| Authentication provider | Yes | Supabase Auth |
| Backend / server logic | Yes | Next.js server actions |
| Database | Yes | Supabase Postgres |
| Analytics / event tracking | Yes | PostHog Cloud |
| External APIs or services | Yes | Supabase managed Postgres is the external service |
| AI touchpoints | Yes | Annotated as build-workflow only — no runtime AI |

---

## 3. Mermaid Diagram

```mermaid
flowchart TD
    U["👤 KIU Student\n(Mobile Browser)"]

    subgraph CLIENT["Client — Next.js Frontend (Vercel)"]
        direction TB
        F_AUTH["Signup / Login Screen"]
        F_HOME["Match List Screen"]
        F_DETAIL["Match Detail Screen"]
        F_CONFIRM["Confirmation Screen\n'You're in!'"]
    end

    subgraph SERVER["Server — Next.js Server Actions (Vercel Serverless)"]
        direction TB
        SA_AUTH["Auth validation\n+ session check"]
        SA_LIST["Fetch matches\nordered by start_time"]
        SA_JOIN["Join match action\n(validate + write RSVP)"]
    end

    subgraph DB["Data — Supabase Postgres"]
        direction TB
        T_MATCHES[("matches\n(id, sport_type, start_time,\nlocation, spots_remaining)")]
        T_RSVPS[("rsvps\n(user_id, match_id,\ncreated_at)")]
        T_USERS[("users\n(id, email)")]
    end

    AUTH["🔐 Supabase Auth\n(email + password\nsession JWT)"]

    ANALYTICS["📊 PostHog Cloud\n(event tracking)"]

    AI_NOTE["🤖 AI tools: build workflow only\nStitch → UI scaffolds\nClaude Code → server action logic\nAI Studio → auth prototype\nNo runtime AI in product"]

    %% User opens app
    U -->|"opens app URL"| F_AUTH

    %% Auth flow
    F_AUTH -->|"submits email + password"| AUTH
    AUTH -->|"creates account\nor validates session"| SA_AUTH
    SA_AUTH -->|"reads / writes"| T_USERS
    SA_AUTH -->|"returns session token"| F_AUTH
    F_AUTH -->|"redirects on success"| F_HOME

    %% Analytics on signup
    F_AUTH -->|"emits user_signup_completed"| ANALYTICS

    %% Home screen loads
    F_HOME -->|"requests match list"| SA_LIST
    SA_LIST -->|"reads upcoming matches"| T_MATCHES
    SA_LIST -->|"returns match cards"| F_HOME
    F_HOME -->|"emits user_session_started"| ANALYTICS

    %% Detail + join flow
    F_HOME -->|"taps match card"| F_DETAIL
    F_DETAIL -->|"taps Join Match"| SA_JOIN
    SA_JOIN -->|"checks spots_remaining > 0\n+ no existing RSVP"| T_MATCHES
    SA_JOIN -->|"writes RSVP row\ndecrements spots_remaining"| T_RSVPS
    SA_JOIN -->|"confirms RSVP"| F_CONFIRM
    SA_JOIN -->|"emits match_joined\n(match_id, sport_type,\nspots_remaining_after_join,\ntime_to_match_hours)"| ANALYTICS

    %% Confirmation back to home
    F_CONFIRM -->|"taps Back to matches"| F_HOME

    %% AI note (not a data flow)
    AI_NOTE -.->|"governs build tools only"| SERVER

    %% Styling
    classDef screen fill:#e8f5e9,stroke:#388e3c,color:#1b5e20
    classDef server fill:#e3f2fd,stroke:#1976d2,color:#0d47a1
    classDef data fill:#fff8e1,stroke:#f57f17,color:#e65100
    classDef external fill:#fce4ec,stroke:#c62828,color:#b71c1c
    classDef ai fill:#f3e5f5,stroke:#7b1fa2,color:#4a148c,stroke-dasharray: 5 5

    class F_AUTH,F_HOME,F_DETAIL,F_CONFIRM screen
    class SA_AUTH,SA_LIST,SA_JOIN server
    class T_MATCHES,T_RSVPS,T_USERS data
    class AUTH,ANALYTICS external
    class AI_NOTE ai
```

---

## 4. Arrow Logic Summary

| Arrow | Label | From | To |
|-------|-------|------|-----|
| User opens URL | opens app URL | User | Signup/Login Screen |
| Signup/login form submit | submits email + password | Signup/Login Screen | Supabase Auth |
| Auth callback | creates account or validates session | Supabase Auth | Auth server action |
| User record read/write | reads / writes | Auth server action | users table |
| Session returned | returns session token | Auth server action | Signup/Login Screen |
| Auth success redirect | redirects on success | Signup/Login Screen | Match List Screen |
| Signup event | emits user_signup_completed | Signup/Login Screen | PostHog |
| Match list request | requests match list | Match List Screen | Fetch matches server action |
| Match read | reads upcoming matches | Fetch matches server action | matches table |
| Match cards returned | returns match cards | Fetch matches server action | Match List Screen |
| Session event | emits user_session_started | Match List Screen | PostHog |
| Match card tap | taps match card | Match List Screen | Match Detail Screen |
| Join tap | taps Join Match | Match Detail Screen | Join match server action |
| RSVP validation read | checks spots_remaining > 0 + no existing RSVP | Join server action | matches table |
| RSVP write | writes RSVP row, decrements spots_remaining | Join server action | rsvps table |
| RSVP confirmed | confirms RSVP | Join server action | Confirmation Screen |
| Activation event | emits match_joined | Join server action | PostHog |
| Back to matches | taps Back to matches | Confirmation Screen | Match List Screen |

---

## 5. AI Annotation

**AI tools exist in the build workflow only. No AI feature runs at product runtime.**

- Stitch: generated UI screen scaffolds (client layer) — code reviewed and annotated before merge
- Claude Code: generated join match server action logic (server layer) — annotated and reviewed per process map
- AI Studio: prototyped auth flow before Supabase integration — output treated as reference, not committed directly
- GitHub Copilot: inline suggestions across layers — each suggestion reviewed before acceptance

---

## 6. Final Export Check

- [x] Boxes match the written system design
- [x] Arrows are labeled
- [x] Database is visually distinct (yellow nodes)
- [x] Auth is shown where it happens (between frontend and Supabase Auth)
- [x] Analytics is shown where events fire (from frontend and server action)
- [x] Diagram is readable at normal zoom
- [x] PNG exported and committed as `architecture-diagram.png`

---

*Architecture Diagram Source | TheMergeConflicters | CS-PD-2026 | Spring 2026*
