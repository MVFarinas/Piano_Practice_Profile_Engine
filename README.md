# Piano Practice Profile Engine

## Overview

An AI-powered MIDI listening tool that builds persistent, longitudinal profiles of piano student weaknesses and delivers actionable insights to **teachers, parents, and students themselves.** This is **not a lesson platform** — it's a diagnostic and accountability layer designed to complement human instruction and existing practice workflows.

**Core Philosophy:** The app listens, learns recurring mistakes across sessions, and gives everyone involved — the teacher, the student, the parent paying for lessons — the insight they need to make informed decisions about progress, practice quality, and next steps. Technique, interpretation, and what to practice — that stays with the teacher.

---

## The Problem

Existing piano learning apps either:
1. **Replace the teacher** (Simply Piano, Flowkey, Skoove) — full lesson platforms, built for self-teaching
2. **Add gamification between lessons** (Tonara, Piano Marvel) — good on dashboards and assignments, but don't actually tell you *what's going wrong* or *whether the student is really practicing*

**The Gap:** No one provides real transparency into practice quality and recurring weakness patterns to the people who actually need it: teachers who need to know where to focus, students who want to improve faster, and parents who need to know if their investment in lessons is paying off.

This tool fills that gap.

---

## Stakeholders & Value Props

### Teachers
- **See the real problems**, not just "student practiced 40 minutes"
- Get actionable weekly reports: *"Student has missed F# in D major in 4 of 6 sessions. Recommend focusing on key-signature work next lesson."*
- Spend lesson time on what actually matters, not guessing what went wrong
- Transparent practice logs to review before each lesson

### Parents & Guardians
- **Verify that lessons are being used effectively** — see proof of practice, not just trust
- Know exactly where their child is struggling and improving
- Make informed decisions: *Is my investment in piano lessons paying off? Is my child engaged?*
- Avoid paying for lessons a disengaged student isn't using

### Students (Independent Learners)
- **Understand your own weaknesses** — granular, evidence-based feedback
- Don't waste practice time on what you're already good at; focus on what's actually hard
- See progress over time (am I getting better at those tricky rhythms?)
- Build better practice habits by knowing what to target

---

## Market Context & Competitive Analysis

See `MARKET_ANALYSIS.md` for the full breakdown. TL;DR:

- **Piano Marvel**: Teacher-integrated, strong sight-reading assessment, but error tracking is shallow (mostly score history, not pattern diagnosis)
- **Tonara**: Built around teacher assignments and chat, but doesn't model recurring mistakes intelligently
- **Yousician / Skoove**: Best-in-class real-time feedback, but reset each session — no persistent profile
- **ROLI AI Music Coach**: Closest conceptual competitor (conversational AI coach that "learns you"), but hardware-locked (£800+) and leans into vision-based posture feedback

**Our Wedge:** Software-only, works with any MIDI keyboard, zero hardware cost, builds the one thing no one else does well: the diagnostic profile that *actually* answers "what's wrong and how do I fix it?"

---

## Core Features (MVP)

### Phase 1: The Diagnostic Engine

1. **MIDI Input & Real-Time Listening**
   - Connect to any MIDI-enabled digital piano or keyboard
   - Listen for note events, timing, and velocity in real time
   - Stream data from performance to error classifier

2. **Ground Truth Setup (Demo Version)**
   - For MVP: hardcoded ground truth (one scale: C Major; one song: Für Elise or similar)
   - Teacher/parent/student selects the piece(s) they're working on from a predefined list
   - Future: teacher uploads PDF/MusicXML for custom repertoire

3. **Error Detection & Classification**
   - Detect wrong notes (pitch accuracy)
   - Detect rhythm mistakes (timing relative to expected beat)
   - Detect tempo drift (rushing or dragging relative to intended tempo)
   - Log each error with timestamp, context (piece, section, tempo), and confidence score

4. **Longitudinal Profile Building**
   - Accumulate errors across sessions (same student, same piece or across repertoire)
   - Identify recurring patterns:
     - Specific pitches missed repeatedly (e.g., F# in D major)
     - Rhythmic weak spots (e.g., triplets, syncopation)
     - Tempo control issues in specific contexts
   - Aggregate stats by piece, key, rhythm type, hand, etc.

5. **Multi-Stakeholder Reporting**
   - **For Teachers:** Weekly summary + actionable recommendations for lesson focus
   - **For Parents:** Practice transparency report — frequency, quality, progress trend
   - **For Students:** Personal dashboard showing strengths, weaknesses, and improvement over time
   - All reports are non-judgmental and supportive (not shaming, not coercive)

6. **Simple Web / Desktop UI**
   - Student/Parent: basic "practice session" view — select piece, hit record, play, see immediate feedback ("wrong note," "rushed that measure")
   - Teacher: dashboard showing student profiles, reports, trends, and notes for upcoming lesson
   - Minimal design — clarity over polish

---

## Out of Scope (Intentionally)

These features are **not** part of this project:

- ❌ **Lesson content** (videos, structured curricula, method books)
- ❌ **Gamification** (badges, leaderboards, points, streaks)
- ❌ **Song library or piece recommendations**
- ❌ **Posture or technique analysis** (computer vision, hand tracking, motion sensors)
- ❌ **Microphone-based listening** (MIDI-only to start; acoustic piano support deferred)
- ❌ **Metronome, tempo adjustment tools, or playback controls**
- ❌ **Mobile-first design** — web/desktop first
- ❌ **Music theory instruction or ear training**
- ❌ **Enforcement or coercion** ("you must practice scales") — supportive transparency only

**Why?** These are already solved by others or require enforcement philosophies we explicitly reject. Our edge is the diagnostic profile. Everything else is distraction.

---

## Technical Architecture (Rough)
┌─────────────────┐
│ MIDI Keyboard │
└────────┬────────┘
│
v
┌─────────────────────────────┐
│ MIDI Input Handler │ ← parse note events, timing
└────────┬────────────────────┘
│
v
┌─────────────────────────────┐
│ Ground Truth Engine │ ← load hardcoded or uploaded
│ (Scale/Song Definitions) │ piece definitions
└────────┬────────────────────┘
│
v
┌─────────────────────────────┐
│ Error Classifier │ ← compare student MIDI to
│ (rule-based + light ML) │ ground truth; detect errors
└────────┬────────────────────┘
│
v
┌─────────────────────────────┐
│ Profile Engine │ ← accumulate errors, find
│ (time-series aggregation) │ recurring patterns
└────────┬────────────────────┘
│
v
┌──────────────────────────────────┐
│ Persistent Storage (DB) │ ← student, session, error logs
└────────┬─────────────────────────┘
│
v
┌──────────────────────────────────┐
│ Multi-Stakeholder Reports │ ← teacher, parent, student
│ (Weekly summaries, trends) │ dashboards & reports
└──────────────────────────────────┘

---

## Roadmap

### Milestone 1: Core Listening (Weeks 1–4)
- [ ] MIDI input setup and parsing
- [ ] Hardcoded ground truth (C Major scale, one simple song)
- [ ] Basic note/pitch detection vs. ground truth
- [ ] Rhythm timing logic (expected vs. actual)
- [ ] Simple session logging

### Milestone 2: Error Profiling (Weeks 5–8)
- [ ] Error classification (wrong note, early/late, off-tempo)
- [ ] Student profile data model
- [ ] Cross-session pattern aggregation
- [ ] Basic stats (error frequency by pitch, rhythm type, etc.)

### Milestone 3: Multi-Stakeholder Reporting (Weeks 9–12)
- [ ] Teacher dashboard with student profiles and recommendations
- [ ] Parent report (practice frequency, quality, trends)
- [ ] Student personal dashboard (weaknesses, improvements)
- [ ] Weekly summary generation

### Milestone 4: Polish & Validation (Weeks 13+)
- [ ] Test with real teacher + student + parent
- [ ] Refine error classification based on feedback
- [ ] Demo-ready UI and reporting
- [ ] Prepare pitch materials for companies

---

## Go-to-Market Strategy

**Primary Goal:** Build the product to sell (not to build a consumer business).

### Phase 1: Proof of Concept
- Build MVP
- Test with 1–3 volunteer teachers + their students (+ parents if willing)
- Get qualitative feedback: *Does the profile actually help them teach? Do parents see value in the transparency?*

### Phase 2: Outreach
- **LinkedIn cold outreach** to product leads at Piano Marvel, Tonara, Skoove
- **Music education conferences** (NAMM, MTNA) — booth presence or demo conversations
- **Music tech communities** — position as a solver of a real gap
- **Parent communities** (music parent forums, Facebook groups) — validate that parents actually want this

### Phase 3: Pitch
- Lead with the demo, not the idea
- Show a real teacher report, a parent transparency report, and a student improvement dashboard from real practice sessions
- Articulate the gap: "You have users and content; we have the accountability and insights layer you're missing"
- Propose: acquisition, licensing, or partnership

---

## Why This Scope Makes Sense

1. **It's defensible.** No competitor has nailed the persistent, multi-stakeholder error profile. You own it.
2. **It's buildable.** Error classification and time-series aggregation are hard but not bleeding-edge. 6–12 weeks to a demo is realistic.
3. **It's sellable.** Piano Marvel, Tonara, and others have users they want to make stickier and help succeed. A diagnostic transparency tool is a low-risk add to their platform.
4. **It's not a lifestyle business.** You're not competing on features or user growth — you're building IP that solves a real problem for three stakeholder groups.
5. **It solves a real pain point.** Parents want to know their money is being used well. Teachers want to know what to focus on. Students want to improve faster. This does all three.

---

## Getting Started

1. Clone or fork this repo
2. Set up your dev environment (Node.js + React for frontend, Python or Node for backend, your choice on DB)
3. Start with `ARCHITECTURE.md` for technical deep-dive
4. Follow the roadmap above
5. Build. Test. Demo. Sell.

---

## Questions?

If you're reading this because you're a teacher, a parent, a piano app founder, or someone curious about the concept — file an issue or reach out. Feedback is gold.

---

**Last Updated:** July 2026  
**Status:** Pre-Alpha / Active Development
