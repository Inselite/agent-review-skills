---
name: code-review
description: Review the current diff (or a PR number, branch or path) for correctness bugs, plus reuse, simplification and efficiency problems, at a given effort level. Use when asked to run /code-review or to review changes before they are committed or deployed. Review only: it reports findings and does not edit unless asked to fix.
---

# Code review

Review the target diff and report real findings. Default target: the uncommitted working-tree diff (`git diff HEAD`, plus
`git diff --staged`; ignore untracked files unless told otherwise). A PR number, branch or path narrows or replaces it.
Read the repo's `AGENTS.md` first and apply its rules (style, tests, working docs) as part of the review.

## Method
1. Read the whole diff, then read the surrounding code of every changed function: callers, callees, the data they
   handle, the tests that cover them. A finding that ignores the surrounding code is usually wrong.
2. Hunt for what actually breaks, most severe first: wrong results, lost or duplicated work, races and re-entrancy,
   state that is not reset or is left stale, error and offline paths, permission or privacy leaks, escaping and
   injection, resource leaks, behaviour that contradicts the repo's own contract docs or tests, accessibility and
   layout regressions where the change touches UI. Then smaller issues: duplicated code that an existing helper
   covers, needless complexity, avoidable work.
3. Try to disprove each candidate. Trace the failing scenario through the code with concrete inputs and state; run a
   test or a small script when that is quick. Drop what you cannot make fail.
4. Rank by severity. Mark each finding CONFIRMED (traced or reproduced) or PLAUSIBLE (reads wrong, not traced end to
   end). Do not pad: no style nitpicks the repo does not enforce, no praise, no "consider" lists.

## Effort levels (low, medium, high, xhigh, max)
Higher levels read more of the surrounding code, try more scenarios and report more uncertain findings. At `high` and
above also probe the concurrency, stale-state and error-path angles on every changed entry point.

## Output
A ranked list. Each finding: `file:line`, the one-sentence defect, the concrete failing scenario (inputs, state,
result), a suggested fix, and CONFIRMED or PLAUSIBLE. Say plainly when you found nothing, and what you did not check.

## Rules
- Do not edit files, commit, push or deploy. With `--fix` in the request, apply the findings to the working tree
  afterwards, one small change each, then re-run the repo's lint and affected tests.
- Do not touch production systems. Never print secrets.
