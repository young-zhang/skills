---
name: pony-review
description: Two-stage review and fix of a change. Runs ponytail-review with ponytail at the ultra level, fixes the findings that hold up, then runs a duel-review (Claude and the Codex CLI) of the result and fixes again. Use when the user asks for pony-review by name, or as the final review step of the commit-often skill. Not for a quick single review (use ponytail-review or duel-review alone).
argument-hint: "[PR or MR link or number, branch, or commit range]"
compatibility: Claude Code only. Uses the Skill and AskUserQuestion tools. Needs the codex CLI, logged in. Needs the gh CLI only for a GitHub pull request, and the glab CLI only for a GitLab merge request.
---

# Pony Review

Review a change in two stages, and fix what each stage finds:

1. A ponytail review at the ultra level: is the change correct, safe, tested,
   and no larger than the need.
2. A duel review: Claude and the Codex CLI each review the change, debate the
   findings, and merge them into one list.

Run every step without a pause for progress reports or for permission. Stop
only for a decision that the user must make (see "When to ask").

## 1. Find the target

Use the target in the arguments, if there is one: a GitHub pull request or
GitLab merge request (link or number), a branch, or a commit range such as
`abc1234..HEAD`.

If there is no target:

- On a branch other than the default branch: review the commits since the
  branch left the default branch, `$(git merge-base <default> HEAD)..HEAD`,
  plus any uncommitted changes.
- On the default branch with uncommitted changes: review those changes.
- Otherwise: use AskUserQuestion to ask which commits to review. Offer the
  last commit, the commits not yet pushed (`@{upstream}..HEAD`), and a
  custom range.

Record the target and the base commit. Every later step uses the same target.

## 2. Ponytail review at the ultra level

1. Invoke the `ponytail` skill with the argument `ultra`. At this level the
   review also asks whether each part of the change is needed at all.
2. Invoke the `ponytail-review` skill on the target.
3. Decide on each finding (see "Act on findings"), then make the fixes.
4. Run the tests and checks that the repository has. Fix what fails.

## 3. Duel review

Invoke the `duel-review` skill on the target as it is now, after the fixes
from step 2.

`duel-review` fetches a GitHub pull request with `gh`, or a GitLab merge
request with `glab`. If the target is neither, replace its fetch step with
these commands, and keep every other step:

```sh
git log --format='%h %s%n%n%b' <base>..HEAD   # stands in for the PR title and description
git diff <base>..HEAD                         # committed changes
git diff HEAD                                 # uncommitted changes, if any
```

Then decide on each merged finding, make the fixes, and run the tests and
checks again.

## Act on findings

Judge each finding on its merits. A finding from a review is a claim, not an
order.

- Fix it if you can confirm the problem in the code and the fix is clear.
- Skip it if it is wrong, or if the cost of the fix is larger than the risk.
  Record a one-line reason.
- Never remove or change behavior that the user asked for without asking
  first. Ultra-level ponytail findings often propose this.

## When to ask

Use AskUserQuestion only when you cannot decide from the request, the code,
or a sensible default. For example:

- A finding proposes to remove or change something the user asked for.
- A contested finding from the duel review has a real trade-off that only
  the user can judge.
- The target is not clear (see step 1).

Collect such questions and ask them together at the end of a stage, up to
four in one AskUserQuestion call. Fix the other findings first. Do not stop
to ask once per finding.

## Commits

Do not commit unless the user or the calling skill asks for commits. If the
`commit-often` skill invoked this skill, follow its commit rules for each fix.

## Report

End with a short report:

- The target that you reviewed.
- Per stage: the findings that you fixed, and the findings that you skipped
  with the reason for each.
- Contested findings from the duel review that are still open.
- The tests and checks that you ran, and their result.
