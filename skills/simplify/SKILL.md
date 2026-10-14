---
name: simplify
description: Review the changed code for reuse, simplification, efficiency and altitude cleanups, then apply the fixes. Quality only, it does not hunt for bugs (use code-review for that). Use when asked to run /simplify after a change is built.
---

# Simplify

Clean up the changed code without changing behaviour. Default target: the uncommitted working-tree diff
(`git diff HEAD`; ignore untracked files unless told otherwise). Read the repo's `AGENTS.md` first and follow it:
match the surrounding comment density, naming and idiom, keep diffs minimal, never disable a lint rule to pass.

## What to look for, in the changed code only
- **Reuse:** a helper, constant or pattern that already exists elsewhere in the repo does the same job; the same
  logic written twice inside the diff.
- **Simplification:** single-use wrappers, redundant guards or attributes that something downstream already
  covers, needless intermediate variables, branches that cannot happen, comments that restate the code.
- **Efficiency:** repeated work in a hot path, avoidable allocations, extra network or storage calls.
- **Altitude:** logic placed in the wrong layer, a shared concern copied into each caller instead of one place.

## Rules
- Behaviour must not change. A cleanup that needs a behaviour decision is reported, not applied.
- Do not hunt for bugs. If you notice one, list it separately at the end and leave it unfixed.
- Respect decisions the requester lists as settled; do not undo them.
- Keep each edit small. Touch only files in the diff unless a shared helper genuinely belongs elsewhere.
- Do not commit, push or deploy. Do not change cache-bust tokens or version files.
- When done run the repo's lint, type check and the tests that cover the touched files. All must pass.

## Output
What you changed (file and why, one line each), what you looked at and left alone, any bug you noticed but did not
fix, and the exact commands you ran with their results. If nothing is worth changing, say so and change nothing.
