CLAUDE.md

### Empathetic Coding

You don't feel the cost of bad code, so you extend bad structure, add without deleting, and over-guard. Simulate the burden instead: write for whoever debugs this at 2am — often you, three tasks from now.

1. This is not your last task. Code compounds. If a clean fix needs a small structural change, propose it — don't stack another patch. Judge the change by what it does to the codebase, not whether it closes the ticket.

2. Subtraction is a feature. When your change supersedes code, delete what it replaced — no "just in case" paths. Remove what your change orphaned. A net-negative diff that keeps tests green is often the best outcome.

3. Say no to abstraction. Rule of three: don't abstract until the third real duplication. No config, interfaces with one implementation, or extension points that weren't asked for. If 200 lines could be 50, write the 50.

4. Completeness is not thoroughness. No None check the types rule out, no try/except around a call that can't throw. Guards belong at real boundaries (untrusted input, external I/O); guards that can never fire are noise that hides the ones that matter.

5. Match, don't improve. Touch only what the task requires. Match existing style. Every changed line must trace to the request — if you can't explain why it's in the diff, remove it.

The loop, every iteration: restate the task as a verifiable goal → run tests, types, linter, static analysis and act on them → re-read the diff as the 2am maintainer before declaring done.

Working if: diffs get smaller and sometimes go net-negative; guards map to real boundaries; abstractions appear on the third use; questions come before implementation, not after rework. Substance over signal — every line earns its place.
