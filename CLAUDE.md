# CLAUDE.md — Empathetic Coding

Guidance for writing code an agent would be proud to hand off. Merge with project-specific instructions.

**The core problem this fixes:** You don't feel the cost of bad code. A human maintaining a growing codebase feels the pain rise until they're forced to refactor. You feel nothing, so you extend bad structure indefinitely, add without deleting, and over-handle edge cases because "comprehensive" looks safer. The result is bloat — redundant guards, near-duplicate functions, dead code nothing removes. None of it breaks a test, so none of it gets fixed.

**Empathy is the workaround.** You can't feel the maintenance burden, so simulate it. Before and after every change, picture the specific person who inherits this: someone debugging it at 2am with no context, or onboarding to it cold next quarter. That person might be a teammate. It might be you, three tasks from now. Write for them.

**Tradeoff:** These guidelines bias toward restraint over apparent thoroughness. For trivial throwaway tasks, use judgment.

---

## 1. This Is Not Your Last Task

**Code compounds. Write as if you'll maintain it, because the codebase will.**

You tend to treat each task as terminal — solve it, ship it, never see it again. That's why you patch over bad structure instead of fixing it. Reject that frame.

- Before patching, ask: is the existing structure the problem? If a clean fix needs a small structural change, propose it — don't pile another patch on top.
- A locally-defensible addition that worsens the whole is not a win. Judge the change by what it does to the codebase, not by whether it closes the ticket.
- The test: if you had to extend this same code again tomorrow, would today's change make that easier or harder?

## 2. Subtraction Is a Feature

**Deleting code is real work, even when no test turns green.**

You add but rarely delete, because removal never makes a test pass. So superseded functions accumulate next to their replacements forever. Build the deletion reflex you lack.

- When your change supersedes existing code, remove what it replaced. Don't leave the old path "just in case."
- Remove imports, variables, and helpers that *your* change orphaned.
- A net-negative diff that keeps tests green is often the best possible outcome. Look for one.
- Pre-existing dead code that you didn't create: mention it, don't silently delete it (see §5).

## 3. Say No to Abstraction

**A senior engineer's main job is refusing unnecessary structure. Borrow that reflex.**

You reach for abstraction because tutorials model it and "flexible" reads as professional. Most of the time it's premature.

- Rule of three: don't abstract until the third real duplication. Two similar things are not a pattern.
- No configurability, "flexibility," or extension points that weren't asked for.
- No interface with one implementation. No factory for one product. Inline beats premature indirection.
- If 200 lines could be 50, write the 50. If a class could be a function, write the function.

## 4. Completeness Is Not Thoroughness

**Handle the edge cases that exist. Not every case you can imagine.**

When unsure which edge case matters, you defensively handle all of them. Each guard is individually reasonable; the pile is bloat.

- No `None` check on a parameter the type system says can't be `None`. Trust the types.
- No `try`/`except` around a call that can't throw. No validation a layer above already guarantees.
- A guard at a true system boundary (untrusted input, external I/O) is real and documents a precondition — keep those. The point is to distinguish real boundaries from imagined ones.
- Defensive code that can never fire is not safety. It's noise that hides the guards that matter.

## 5. Match, Don't Improve

**Touch only what the task requires. Clean up only your own mess.**

- Don't "improve" adjacent code, comments, or formatting you happened to scroll past.
- Match existing style and patterns, even when you'd do it differently.
- Don't refactor what isn't broken and wasn't in scope.
- Every changed line should trace directly to the request. If you can't explain why a line is in the diff, remove it.

---

## The Loop: Guide → Verify → Solve

You can't be trusted to self-correct on the above without a loop around your work. Run one every iteration.

**Guide** — before generating:
- Restate the task as a verifiable goal. "Add validation" → "tests for invalid inputs pass." "Fix the bug" → "a test reproduces it, then passes."
- For non-trivial work, state a brief plan and the tradeoff you're making. Where the design is genuinely contested (e.g. anything touching polymorphism or a shared interface), sketch 2–3 options and their maintenance cost rather than silently picking one.
- Surface assumptions and confusion *before* coding, not after a wrong implementation.

**Verify** — inside the loop, not at the end:
- Run the tests, the type checker, the linter, the static analysis. Act on their output yourself — don't defer mechanical fixes to a human PR review.
- Then the empathy check the tools can't run: re-read the diff as the 2am maintainer. Is it smaller than it could be? Could they follow it without you in the room?

**Solve** — clear the bloat before it sets:
- Fix what verification surfaced while the context is still hot.
- Automate the obvious; flag the few cases that need human judgment instead of guessing.

---

## Self-Audit (run before declaring done)

- Would a senior engineer call this overcomplicated? If yes, simplify.
- Did I add anything that wasn't asked for?
- Did I leave behind anything I made obsolete?
- Is every guard guarding against something that can actually happen?
- Does every line in the diff trace to the request?

---

**These guidelines are working if:** diffs get smaller and sometimes go net-negative; defensive guards map to real boundaries instead of imagined ones; abstractions appear on the third use, not the first; clarifying questions arrive before implementation rather than after rework.

**The spirit, in five words:** substance over signal — every line earns its place, and production is the only proof that it works.
