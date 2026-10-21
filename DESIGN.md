# Tenkemakker — design document

*Living document. Last updated 21 Oct 2026, 11:25, after version 2.*
Open decisions: see the **Tenkemakker decision board**: https://claude.ai/artifact/VQuDcDWzBJ1KyeXvmGVTLi (shared Claude artifact; the team updates it live).
App (published): https://claude.ai/artifact/5ojWwQoxmE9ALVXC98YUMy This document records what we've built and why; the board holds what we haven't decided.

## 1. The problem

**Person:** Ida, 13, 8th grade (fictional). AI is in her pocket while she does homework.

**Challenge:** By 2030 AI can do almost any school task in seconds. Why should Ida struggle through it herself — and how does she learn to *think* when the answer is always one prompt away? At the same time, AI could be the patient, always-available helper that many kids never had.

**Why it matters:** Thinking for yourself is the skill school exists to build. If AI quietly replaces it, kids lose it without noticing; if AI is banned, the kids with the least help at home lose the most.

**Our bet:** Keep AI as a *partner* that helps when it gets hard, but design it so that the effort stays with the child — and make that effort feel worth it.

## 2. Who it touches

| Who | Hope | Worry |
|---|---|---|
| Ida (13) | Help when stuck, without feeling stupid | Boring, preachy, or "school-ware" |
| Parent | Insight into how she's doing; something to talk about at dinner | Surveillance; not understanding the tech |
| Teacher | Pupils who can explain, not just answer | Can't tell own work from AI work |
| Classmates | Fun, shared goals | Ranking shames those at the bottom |

## 3. Design principles

1. **Never the answer.** Makker asks, hints, shows a similar example — never solves the task.
2. **Help in small steps, chosen by the child.** A visible hint ladder: question → tip → example. Ida decides when to climb.
3. **Teaching is learning.** After solving, roles swap: Makker has a typical misconception, Ida must explain. Explaining earns the most.
4. **Think out loud.** Ida can answer Makker by voice, so explaining doesn't depend on typing.
5. **Points for thinking, not speed or correctness alone.** Mistakes are free.
6. **The parent sees the shape, not the transcript.** Summaries and a dinner conversation starter, not a chat log. Ida knows what's shared.
7. **Joyful, age-right.** Looks like a game a 13-year-old would open — not a children's book, not a school portal.
8. **Fictional data only.**

## 4. What exists (v2)

Single file `index.html`. Three tasks (fractions, percent, equations), each with a 3-step hint ladder, a Makker misconception, keyword list and a dinner starter.

**Pupil view (Ida)**
- HUD: level (Nysgjerrig → Legende, every 100 TP), XP bar, tankepoeng (TP), streak (days in a row).
- Task card with answer check; wrong answers shake, no penalty.
- Hint ladder; each hint opens a conversation — Ida answers Makker by voice or text, Makker replies with a guiding question.
- Teach-back: Makker states a wrong idea; Ida explains until Makker "gets it".
- Makker: animated character (neutral / confused / happy / talking).
- Confetti, "+TP" pops, level-up and badge toasts.
- Six badges: Tenkte selv, Læreren, Står på, Snakket ut, Tenker høyt, Tre på rad.
- Class 8B board with a toggle: **Mot hverandre** (ranking, "X TP from passing Sofie") vs **Sammen** (class goal: 1500 TP → film Friday, no ranking).

**Parent view**
- Week card Mon–Thu (today live): tasks solved, hint mix per day, times explained, TP.
- "Det Ida har forklart": one-line summaries, not quotes.
- "Spør ved middagen": a conversation starter based on what she taught today.

### Point economy (TP)

| Action | TP |
|---|---|
| Solve with 0 / 1 / 2 / 3 hints | 30 / 20 / 12 / 6 |
| Answer a hint question (max 2 per task) | 3 |
| Teach Makker successfully | 25 |
| Wrong answer | 0 (no penalty) |

### Tech

- Published as a Claude artifact: Makker uses real Claude (`sample`, quick tier) for hint replies and judging Ida's explanation (returns `reply`, `understood`, `summary`).
- As a local file: scripted replies via keyword matching ("Øvingsmodus").
- Voice in: browser `SpeechRecognition` (Google Chrome/Edge, local file only; audio goes to Google's speech service). Voice out: `speechSynthesis`, Norwegian voice only.

## 5. Known gaps

- Speech recognition and real Claude don't work in the same place yet (local file vs artifact).
- Linux laptops usually lack a Norwegian TTS voice → Makker is silent there. Demo on a Mac if voice-out matters.
- Scripted mode can be gamed (say a keyword, get the "aha").
- Only 3 math tasks; no teacher view; no persistence between sessions.

## 6. Out of scope today

Real accounts, real pupil data, school integrations (Feide, LMS), content beyond the three demo tasks, a backend.

## 7. Demo outline (5 min)

1. **Problem** (45 s): Ida, homework, AI that can do it all. Hope vs worry.
2. **Who** (30 s): Ida, her parent, her class.
3. **Show it** (3 min): get stuck → answer a hint by voice → solve → teach Makker → points/class board (flip *Mot hverandre* ↔ *Sammen*) → parent card + dinner question.
4. **Next** (45 s): the open questions from the decision board — what we'd test with real kids and teachers.

## 8. Changelog

- **v1** (10:50): hint ladder, teach-back, parent week card, voice in/out, real/scripted Makker.
- **v1.1**: answer hints in the chat; speech language picker; no English voice for Norwegian.
- **v2** (11:20): joyful game design, TP, levels, streak, badges, class board with competition/cooperation toggle.
