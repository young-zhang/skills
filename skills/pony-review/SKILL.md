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

The fixes go into the working tree, so the target must end at the
checked-out `HEAD`. For a pull request, merge request, or branch, its head
branch must be the branch that is checked out. If the target ends somewhere
else, ask the user. Do not switch branches yourself.

Record the target and the base commit. For a pull request or merge request,
fetch its target branch from the repository that it targets, which can be a
remote other than `origin`: `git fetch <remote> <target branch>`. The base
commit is then `git merge-base FETCH_HEAD HEAD`. A local target branch can
be out of date and give a wrong base. Every later step uses the same target.

## 2. Ponytail review at the ultra level

1. Invoke the `ponytail` skill with the argument `ultra`. At this level the
   review also asks whether each part of the change is needed at all.
2. Invoke the `ponytail-review` skill on the target.
3. Decide on each finding (see "Act on findings"), then make the fixes.
4. Run the tests and checks that the repository has. Fix what fails.

## 3. Duel review

Invoke the `duel-review` skill on the target as it is now, after the fixes
from step 2.

`duel-review` fetches the diff of a pull request with `gh` or of a merge
request with `glab`. That remote diff does not include the fixes from step 2,
which are not pushed. Take the diff from the local checkout in its place, and
keep every other step:

```sh
git diff <base>                               # committed and uncommitted changes
git status --short --untracked-files=all      # new files, marked ??, that git diff leaves out
git log --format='%h %s%n%n%b' <base>..HEAD   # title and description, if the target is not a PR or MR
```

Give the review the contents of each new file that belongs to the change.

For a pull request or merge request, take only the title and description
from `gh` or `glab`.

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
