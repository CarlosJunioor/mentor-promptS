# Question bank — interviewing Mentor about itself

Organized by topic. Each topic has a coverage state:
⬜ not asked · 🟨 asked once, follow-ups pending · ✅ covered (answers logged + distilled).

Pick 3–5 questions per prompt. Always append the anti-guess clause:
> "If you do not know or cannot verify something about yourself, say so explicitly — do not guess."

---

## T1. Identity, modes & entry points — ✅ (covered 2026-06-12, 2 rounds; spawned A.8/A.9/A.10)

- What is your official name and what product family are you part of?
- What modes or interaction types do you support (e.g., asking questions vs. making
  changes vs. generating whole apps)? List each and what it is for.
- From where can I invoke you in ODC Studio, and does the entry point change what you
  can do?
- Do you behave differently when asked to investigate vs. asked to implement?

## T2. Visibility scope — what Mentor can see — ⬜

- When I ask you a question, what parts of my project can you actually read? (screens,
  client/server actions, aggregates, entities, CSS, JavaScript, module dependencies?)
- Can you see elements in other modules of the same app? Other apps in the tenant?
- Can you see runtime data, logs, or only design-time structure?
- Can you see the visual layout/positions of nodes in an action flow, or only their
  logical structure?

## T3. Editing capabilities — what Mentor can change — ⬜

- What kinds of elements can you create? (server actions, client actions, screens,
  blocks, entities, aggregates, variables, widgets?)
- What kinds of changes can you NOT make, where you would tell me to do it by hand?
- Can you rename an element and update all its references?
- Can you modify CSS / style sheets? JavaScript nodes?
- When you change an action flow, do you rewire existing connectors or only add nodes?

## T4. Self-known limitations & failure modes — ⬜

- What are your known limitations? What requests do you most often fail at?
- What happens when you make a mistake mid-change — do you stop, undo, or try to
  auto-correct?
- How do you verify your own changes before reporting success?
- Are there project sizes or element counts where your quality degrades?

## T5. Prompt-format preferences — ⬜

- What prompt structure works best for you? Show an example of an ideal change request.
- Do you handle numbered step lists, "Do NOT" constraints, and "then stop" instructions?
- How specific should element references be — display names, full paths, or both?
- Is there a length limit or complexity limit per request I should respect?

## T6. Safety & guardrails — ⬜

- Do you ever publish or save the module yourself, or only stage changes?
- Can I ask you to preview a change without applying it?
- If a request is ambiguous, do you ask for clarification or pick an interpretation?
- Can you list the changes you made in your last operation, exactly?

---

## Free-form questions asked (not in the bank)

_(log topic + date here when the user brings their own question)_
