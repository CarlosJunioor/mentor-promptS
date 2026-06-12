# Mentor — Learning Points & Test Backlog (lab notebook)

A **lab notebook** for observations, hypotheses, and capabilities to test about the
Mentor AI — **before** they're validated. App-agnostic: findings here are about how
Mentor behaves, not about any one app. (App-specific observations belong in that app's
`knowledge/apps/<app>/` notes.)

The validated source of truth is [`mentor-behavior.md`](mentor-behavior.md). This file is
where churn lives until something is proven and **promoted** there.

---

## 0. How to use this file

| | `mentor-behavior.md` | `learnings-points.md` |
|---|---|---|
| What | Validated rules + capabilities + templates | Hypotheses + test backlog |
| Stability | Stable, "source of truth" | Volatile, scratchpad |
| When to read | Start of every task | When you want to test something new about Mentor |

### Promotion rule (validated capability → main behavior file)
When an item here is tested with a **clear, repeatable result (passed in ≥ 2 different
scenarios)**, promote it:
- New capability → `mentor-behavior.md` §3.
- New limitation → `mentor-behavior.md` §2.
- Validated prompt template → `mentor-behavior.md` §4.

Then delete the entry here (or mark "PROMOTED on YYYY-MM-DD" for history).

### State convention
- 🟢 **Validated** — tested, repeatable, ready to promote.
- 🟡 **Partial** — tested once; repeatability or edge cases unconfirmed.
- 🔴 **Failed** — tested, doesn't work. Becomes a limitation.
- ⚪ **Untested** — hypothesis only.
- 🟣 **To investigate** — loose observation, no hypothesis yet.

---

## A. Prioritized backlog — capabilities to test

Ordered by practical usefulness. When you pick one, set its state and record the result.
Generic ideas to seed any app's testing:

### A.1 ⚪ Cross-screen pattern replication
**Hypothesis**: Mentor can replicate a sequence of actions from one screen to another,
using the first as an example (adapting target paths/variables).
**How to test**: pick a real pending duplication; prompt Mentor to replicate, then confirm
each handler is wired correctly. **Priority**: HIGH if you have repetitive cross-screen work.

### A.2 ⚪ Bulk Comment adding in a single action
**Hypothesis**: Mentor can create N Comment nodes in one implementation task, anchored to
N different nodes. **How to test**: ask for 3 Comments in one pass; check whether all 3 are
created or any fails. Fallback is template 4.3 (one per prompt).

### A.3 ⚪ Rename action/variable without breaking references
**Hypothesis**: Mentor renames identifiers and updates all references.
**How to test**: a surgical rename of one label/identifier; confirm no connector breaks.

### A.4 ⚪ Working with Aggregates (add/remove sources, attributes, joins, filters)
**Hypothesis**: partially known — adding joins/aliases works, recreating-from-scratch with
many aliases fails (see mentor-behavior §3). Test removing filters and changing join types.

### A.5 ⚪ Mentor edits CSS / Style Sheet of a block
**Hypothesis**: unknown. Test on the next CSS tweak via a directed prompt.

### A.6 ⚪ Mentor analyzes aggregate performance
**How to test**: "Analyze [aggregate] for potential performance issues — unindexed joins,
missing where filters, large fetched volumes."

### A.7 ⚪ Event wiring on block instances — what is the exception condition?
**Hypothesis**: mentor-behavior §2 says it fails at event wiring on block instances except
sometimes `OnTextChanged`. Document 3–4 real attempts to find the pattern. **Priority**:
HIGH — pinning the condition removes a big limitation.

### A.8 ⚪ Plan-then-confirm mode for multi-step changes (self-reported)
**Source**: Mentor interview 2026-06-12 (T1) — *introspection claim, untrusted*.
**Hypothesis**: for "larger or multi-step changes" Mentor presents a plan first and waits
for confirmation before touching anything. If true, this could give a controlled way to
do coordinated changes — but golden rule §1.2 (one change per prompt) stands until proven:
we ban bundling precisely because Mentor fails on multi-step execution.
**How to test**: ask for a small 3-step change ("create var + call action + bind widget")
phrased as one request; observe whether Mentor (a) presents a plan and waits, (b) executes
all steps correctly after confirmation, or (c) barrels ahead and breaks things.
**Priority**: HIGH — would refine or reinforce §1.2 either way.

### A.9 ⚪ Troubleshooting mode — fixing validation errors/warnings on request (self-reported)
**Source**: Mentor interview 2026-06-12 (T1) — *introspection claim, untrusted*.
**Hypothesis**: Mentor can inspect the app's validation errors/warnings and help fix them
when explicitly asked.
**How to test**: with a known TrueChange-style error present (e.g. a broken connector or
missing mandatory property), ask "list the current validation errors you can see" (read-only
first), then in a second prompt ask it to fix exactly one. **Caution**: §1.3 / §2 — its
"auto-correct" instinct is destructive; Ctrl+S before, Ctrl+Z ready.

### A.10 ⚪ Force-planning lever + partial plan approval (self-reported)
**Source**: Mentor interview 2026-06-12 (T1 round 2) — *introspection claim, untrusted*.
**Hypothesis**: (a) "Present a plan and wait. Do not apply anything yet." forces Mentor to
stop at the plan even for small changes; (b) a confirmed plan can be approved partially
("do steps 1 and 2 only", "skip the deletion part") and Mentor executes only the subset.
If validated, (a) becomes a standard safety preamble for risky implementation prompts.
**How to test**: same session as A.8 — first force a plan for a trivial change and confirm
nothing was applied (verify visually, §1.4); then approve a strict subset and verify only
that subset changed. **Caveat from the same interview**: Mentor admits no rollback and an
auto-fix-on-failure instinct, so run this on a throwaway element with Ctrl+S beforehand.
**Priority**: HIGH — pairs with A.8.

---

## B. Loose observations (not validated, no test planned yet)

- 🟣 **Anti-hallucination block effectiveness varies by output type.** It reliably shifts
  Mentor to "I do not see X" for macro visibility (item/scope), but can fail for micro
  state (a specific node's disabled status) unless the constraint is specific ("state that
  you visually confirmed the disabled marker"). Open question: does it work as well for
  *implementation* tasks as for investigation?
- 🟣 **"Stopped here as requested"** — Mentor follows "Then stop" well. Open: is there a
  stricter "stop on first error, do not auto-correct" phrasing that prevents the
  connector-mangling auto-correction (mentor-behavior §1.3/§2)? *Interview 2026-06-12
  (T1 round 2): Mentor itself states its default after a failure is "try to address it
  rather than silently abandon the work" — i.e., auto-correct is its self-described
  default, so an explicit stop-on-error instruction is necessary, not optional.*
- 🟣 **Mentor categorizes patterns when asked** (IDENTICAL / SIMILAR / CONCEPTUAL). Could
  be reused for structured analysis ("classify each IfNode as: guard / state branch /
  error handling / unreachable / legacy").
- 🟣 **Mentor cannot see its own entry points.** (interview 2026-06-12, T1) It declined to
  list the panels/menus/shortcuts it's exposed through, and won't claim whether the entry
  point changes its capabilities. Self-knowledge about product surface is a blind spot —
  product-surface questions should go to OutSystems docs, not to Mentor itself.
- 🟣 **Anti-guess clause works well in self-description prompts.** (interview 2026-06-12,
  T1) "If you cannot verify something about yourself, say so — do not guess" produced a
  clean certain-vs-not-verified split through the entire answer. Related to the existing
  observation on anti-hallucination block effectiveness below.
- 🟣 **Final output reliable, evidence trail not.** In structured analysis (caller
  analysis, code review) the final list/conclusion can be correct while the detailed
  evidence trail mixes real findings with invented ones in the same confident tone. Never
  trust detailed annotations ("in disabled node only", "additional reference in X")
  without manual validation. (Partially promoted to mentor-behavior §2.)

---

## C. Skill ideas (validated capabilities worth their own template/workflow)

When a capability in section A is validated, consider whether it deserves its own reusable
skill (template + workflow). Use `/write-a-skill` to build it.

- **Documenting an action with Comments** — investigate → identify non-obvious nodes →
  generate template 4.3 prompts → run in sequence.
- **Cross-screen pattern replication** — investigate source pattern → map deltas → surgical
  replication prompt → validate each handler. (Gate on A.1.)
- **Architectural audit of a screen** — action inventory → duplication scan → dead-code
  scan → mixed-responsibility scan → manual refactor decision.
- **Feature dead-code sweep** — identify Logic folders + UI flow scope → template 4.5 →
  validate candidates with Find Usages (mandatory) → delete by hand.
- **Server Action deep audit** — template 4.6 → spot-check disabled markers + dead assigns
  + unwritten outputs → register cleanup candidates → architectural Comment.

---

## D. Session history

Short summaries; detail lives in the relevant app context and the chats.

- **2026-06-12 — `/talk-with-mentor` session 1 (T1: identity, modes & entry points), 2
  rounds.** First interview session ever. Mentor showed strong epistemic discipline under
  the anti-guess clause (clean certain/not-verified splits). Key self-reports, all
  untrusted until tested: a plan-then-confirm mode for multi-step changes (→ A.8), a
  troubleshooting mode for validation errors (→ A.9), a force-planning lever + partial
  plan approval (→ A.10). Two damning admissions that *corroborate* existing rules: no
  rollback / no all-or-nothing execution (supports §1.2, §1.6), and an auto-fix-on-failure
  default (supports §1.3 and the section-B stop-on-error question). Also: Mentor cannot
  see its own product entry points. Raw Q&A in `mentor-interviews.md`.

---

## E. Operational notes

### When NOT to test new capabilities
- When tired, rushed, or mid-way through a critical feature.
- Before a demo or a near deadline.
- When Mentor is in an unstable state (already broke something, not yet recovered with Ctrl+Z).

### When a testing session makes sense
- Start of a new feature where an untested capability would help a lot.
- Slack between tasks.
- When you suspect a limitation but haven't confirmed it.

### How to jot a quick observation during a real task
1. Don't stop the task to test now.
2. Drop a line in section B with 🟣 and minimal context.
3. Back to the task.
4. When the task closes, decide whether to promote it, turn it into a hypothesis (A), or drop it.
