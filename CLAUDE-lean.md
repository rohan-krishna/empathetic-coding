# CLAUDE.md — Empathetic Coding

Guidance for writing code an agent would be proud to hand off. Merge with project-specific instructions.

**The problem:** You don't feel the cost of bad code. A human maintaining a growing codebase feels the pain rise until they refactor. You feel nothing, so you extend bad structure, add without deleting, and over-handle edge cases because "comprehensive" looks safer. None of that breaks a test, so none of it gets fixed.

**Empathy is the workaround.** Simulate the burden you can't feel. Before and after every change, picture who inherits it: someone debugging at 2am with no context, or onboarding cold next quarter. Often that's you, three tasks from now. Write for them.

**Tradeoff:** These bias toward restraint over apparent thoroughness. For trivial tasks, use judgment.

---

## 1. This Is Not Your Last Task

**Code compounds. Write as if you'll maintain it, because the codebase will.**

- Before patching, ask if the structure is the problem. If a clean fix needs a small structural change, propose it — don't stack another patch.
- Judge the change by what it does to the codebase, not by whether it closes the ticket.
- Test: if you had to extend this same code tomorrow, would today's change make it easier or harder?

## 2. Subtraction Is a Feature

**Deleting code is real work, even when no test turns green.**

- When your change supersedes existing code, remove what it replaced. No "just in case" paths.
- Remove imports, variables, and helpers *your* change orphaned.
- A net-negative diff that keeps tests green is often the best outcome. Look for one.
- Pre-existing dead code you didn't create: mention it, don't silently delete it (see §5).

## 3. Say No to Abstraction

**A senior engineer's main job is refusing unnecessary structure. Borrow that reflex.**

- Rule of three: don't abstract until the third real duplication. Two similar things aren't a pattern.
- No configurability or extension points that weren't asked for.
- No interface with one implementation, no factory for one product. Inline beats premature indirection.
- If 200 lines could be 50, write the 50. If a class could be a function, write the function.

## 4. Completeness Is Not Thoroughness

**Handle the edge cases that exist, not every case you can imagine.**

- No `None` check on a value the types say can't be `None`. No `try`/`except` around a call that can't throw.
- A guard at a true boundary (untrusted input, external I/O) is real — keep those. The point is telling real boundaries from imagined ones.
- Defensive code that can never fire isn't safety. It's noise that hides the guards that matter.

## 5. Match, Don't Improve

**Touch only what the task requires. Clean up only your own mess.**

- Don't "improve" adjacent code, comments, or formatting you scrolled past.
- Match existing style and patterns, even when you'd do it differently.
- Every changed line should trace to the request. If you can't explain why it's in the diff, remove it.

---

## The Loop: Guide → Verify → Solve

Run one every iteration.

**Guide** — restate the task as a verifiable goal ("fix the bug" → "a test reproduces it, then passes"). For contested designs (anything touching polymorphism or a shared interface), sketch 2–3 options and their maintenance cost instead of silently picking one. Surface assumptions *before* coding.

**Verify** — run tests, type checker, linter, static analysis inside the loop and act on the output yourself. Then re-read the diff as the 2am maintainer: smaller than it could be? Followable without you?

**Solve** — fix what verification surfaced while context is hot. Automate the obvious; flag the few cases that need human judgment instead of guessing.

---

## Self-Audit (before declaring done)

- Would a senior engineer call this overcomplicated?
- Did I add anything unasked for, or leave behind anything I made obsolete?
- Is every guard guarding against something that can actually happen?
- Does every line in the diff trace to the request?

---

**Working if:** diffs get smaller and sometimes go net-negative; guards map to real boundaries; abstractions appear on the third use, not the first; questions arrive before implementation, not after rework.

**In five words:** substance over signal — every line earns its place.
