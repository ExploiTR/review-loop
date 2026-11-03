---
name: review-loop
description: >-
  Review-fix loop with no round cap. Each round a fresh read-only reviewer
  agent inspects the current change, the parent fixes every finding, and a new
  reviewer re-reviews until one says SATISFIED. Use when the user invokes
  /review-loop or asks to keep reviewing and fixing until a reviewer is
  satisfied.
disable-model-invocation: true
---

# Review loop

Repeat until a reviewer returns `SATISFIED`. No round cap.

## Scope

The work done in this conversation, including its uncommitted changes. If there is none, say so and stop. Ignore unrelated dirty files.

## Loop

1. Launch a new read-only review subagent.
2. Read the verdict (first line: `SATISFIED` or `NOT SATISFIED`).
3. If `NOT SATISFIED`: fix every finding, then go to step 1 with a new subagent.
4. If `SATISFIED`: stop and tell the user, linking that reviewer.

Never reuse a reviewer, edit files from a reviewer, or report done after a `NOT SATISFIED` verdict.

## Reviewer prompt

Give the repo path and a neutral description of the change. Tell it to read the changed files and any behavior doc they affect, and not to edit anything.

Do not mention earlier rounds, prior findings, or fixes. The reviewer judges the code as it stands.

Required output:
- First line: exactly `SATISFIED` or `NOT SATISFIED`.
- Then a numbered list. Each item: severity (`blocker`/`major`/`minor`), file, problem, why it breaks, exact fix.
- `SATISFIED` means no functional bugs remain. Style nits are not findings.

## Fixes

- Fix every finding before the next review.
- A behavior doc made wrong by the change is a finding. Update it together with the code.
