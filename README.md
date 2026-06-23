# Empathetic Coding

Drop-in behavioral guidance that makes AI coding agents write maintainable code instead of bloat.

## The problem

Agents don't feel the cost of bad code. A human maintaining a growing codebase feels the pain rise until they refactor; an agent feels nothing, so it extends bad structure, adds without deleting, and over-guards every edge case because "comprehensive" looks safer. None of it breaks a test, so none of it gets fixed. The fix is empathy as a synthetic reflex — make the agent simulate the maintainer it can't feel.

## What's here

Three versions of the same guidance, same five principles, different lengths. Pick by where it runs.

| File | Size | Use it as |
|------|------|-----------|
| `CLAUDE.md` | ~1000 words | Reference / team onboarding — teaches the reasoning |
| `CLAUDE-lean.md` | ~700 words | The working middle |
| `CLAUDE-distilled.md` | ~290 words | Standing agent context, where every token competes |

## Usage

Rename your chosen version to `CLAUDE.md` and place it at the repo root, or load it into your agent's instructions. Keep it under ~200 lines: oversized context files measurably reduce task success and raise inference cost, so the distilled version is the safe default for always-on use.

## The five principles

1. **This is not your last task** — code compounds; fix structure, don't stack patches.
2. **Subtraction is a feature** — deleting code is real work; net-negative diffs are wins.
3. **Say no to abstraction** — rule of three; no structure that wasn't asked for.
4. **Completeness is not thoroughness** — guard real boundaries, not imagined ones.
5. **Match, don't improve** — every changed line traces to the request.

Wrapped in a loop every iteration: **guide → verify → solve.**

## Credits

Synthesizes the *Agent Centric Development Cycle* (AC/DC) from Tom Howlett / Sonar, the behavioral structure of Andrej Karpathy's `CLAUDE.md`, and findings from SlopCodeBench (rising complexity in 80% of agent trajectories, rising verbosity in 89.8%). [Reference Article &rarr;](https://www.infoworld.com/article/4182518/shipping-enterprise-quality-code-with-ai-agents.html)
