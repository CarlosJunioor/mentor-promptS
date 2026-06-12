---
name: talk-with-mentor
description: Runs a structured interview session with the OutSystems Mentor AI about itself — its features, modes, visibility scope, limitations, and preferred prompt formats — and routes every claim into the learning pipeline as a hypothesis. Use when the user wants to ask Mentor how it works, discover what Mentor can do, learn its modes or features, or improve mentor prompts in general — or whenever the user types /talk-with-mentor.
---

# Talk-with-Mentor — interview Mentor about itself

This is an **interview session**. Claude writes interview questions, the user pastes them
into Mentor inside ODC Studio, and pastes Mentor's answers back. The goal: extract what
Mentor *says* about its own features, modes, and limits, and turn it into testable
hypotheses that sharpen every future prompt.

## The golden epistemic rule (critical)

**Mentor's self-reports are claims, not facts.** An AI describing itself confabulates —
it may cite features it doesn't have or deny limits it does have. Therefore:

- **Nothing from an interview goes into `mentor-behavior.md` directly. Ever.**
- Every claim is routed as a **hypothesis** into `knowledge/learnings-points.md`
  (⚪ if testable, 🟣 if loose) and proven later via `/limit-testing`.
- The three skills form a loop: **`/talk-with-mentor` (ask) → `/limit-testing` (prove)
  → `/mentor` (use)**.

Claims about *documented product facts* (modes, menu entries, feature names) are more
trustworthy than *introspective* claims ("I always update references"). Tag each claim
accordingly — see step 5.

## Knowledge base

- `knowledge/mentor-interviews.md` — the **interview log**: verbatim Q&A per dated
  session. This skill's primary output. Raw answers are never lost.
- `knowledge/learnings-points.md` — where distilled claims land as hypotheses (A or B).
- `knowledge/mentor-behavior.md` — read-only here: used to spot contradictions between
  what Mentor claims and what testing already proved.
- [QUESTIONS.md](QUESTIONS.md) — the question bank, organized by topic, with coverage
  state per topic.

## Workflow

1. **Read** `mentor-behavior.md` (§2–§3) and `mentor-interviews.md`. Show the user the
   topic map from [QUESTIONS.md](QUESTIONS.md) with what's already covered.
2. **Pick one topic** for this session (identity & modes, visibility scope, editing
   capabilities, self-known limitations, prompt-format preferences, safety behavior).
   The user can also bring a free-form question — slot it under the nearest topic.
3. **Compose the interview prompt** — 3 to 5 questions max per prompt, from the bank
   plus anything the previous answer opened up. Interview prompts are read-only and
   safe: they ask Mentor to *describe*, never to *change* anything. Always include:
   "If you do not know or cannot verify something about yourself, say so explicitly —
   do not guess."
4. **User runs it in Mentor and pastes the full answer back.** Record it **verbatim**
   in `mentor-interviews.md` under a dated session heading (question → answer pairs).
5. **Distill each claim** and classify it:
   - **Matches** something already in `mentor-behavior.md` → note the corroboration in
     the log; nothing else to do.
   - **Contradicts** `mentor-behavior.md` → flag it loudly to the user; add a ⚪ entry
     in learnings-points section A to re-test (observed behavior outranks self-report
     until re-proven).
   - **New & testable** ("I can rename and update references") → ⚪ hypothesis in
     section A with a suggested test.
   - **New & vague** → 🟣 observation in section B.
   Tag each as `[product-fact]` or `[introspection]` — introspection claims get tested
   first, trusted last.
6. **Follow-up rounds** — drill into the most useful answers (2–3 rounds max per
   session). Ask Mentor for concrete examples: "show the exact prompt format you handle
   best", "list the node types you can create".
7. **Close the session**: update the topic's coverage state in QUESTIONS.md, add a
   one-paragraph summary to learnings-points section D, and tell the user which new
   ⚪ hypotheses are now queued for `/limit-testing`.

## Rules

- Interview prompts must be **read-only** — never mix in a change request.
- Never mention the `.md` knowledge files inside a prompt for Mentor (two-layer rule).
- 3–5 questions per prompt; one topic per session.
- Verbatim answers go in the log **before** any distillation — raw data first.
- A claim repeated by Mentor twice is still a claim. Only `/limit-testing` validates.
- End every message containing an interview prompt by reminding the user to paste
  Mentor's full response back.
