---
name: commit-often
description: Do a task as a series of small commits. Before each commit, ask the Codex CLI for a second opinion on the staged change with codex-consult. When the task is done, run pony-review (ponytail at the ultra level, then a Claude and Codex duel review) on all the commits, and fix what the reviews find. Runs only when invoked explicitly, as /commit-often followed by the task.
argument-hint: "[task]"
disable-model-invocation: true
compatibility: Claude Code only. Uses the Skill and AskUserQuestion tools. Needs Git and the codex CLI, logged in.
---

# Commit Often

Do the task in the arguments as a series of small commits. Get a second
opinion from the Codex CLI before each commit. When the task is done, review
all the commits and fix what the review finds.

Run every step without a pause for progress reports or for permission. Stop
only for a decision that the user must make (see "When to ask").

## 1. Start

1. Run `codex --version`. If the codex CLI is missing or not logged in, use
   AskUserQuestion: continue without the Codex steps, or stop.
2. Record the start commit: `git rev-parse HEAD`. The final review covers
   every commit after it.
3. Commit on the branch that is checked out. Do not create, switch, merge,
   or rebase branches unless the user asks.
4. Read the commit rules of the repository (for example `AGENTS.md`,
   `CLAUDE.md`, `CONTRIBUTING.md`), and follow them for every commit.
5. Split the task into work units. A work unit is the smallest change that
   leaves the code in a working state: it builds, and the existing tests
   pass. One work unit is one commit.

## 2. The commit loop

For each work unit:

1. Make the change.
2. Run the tests and checks that the repository has. Fix what fails.
3. Stage the change with `git add`.
4. Invoke the `codex-consult` skill. Ask Codex to run `git diff --cached`
   and give a second opinion on the commit: bugs, callers that the change
   breaks, missing tests, and whether the commit does one thing. Give Codex
   one line on the task, so it can judge the change against its purpose.
5. Judge each point that Codex makes (see "Act on findings"). Fix the
   points that hold up, then run the tests and stage again. Do not consult
   Codex again on the same commit unless the fix was large.
6. Commit. The message says what the commit changes and why.

Do not stop to show the Codex answer to the user. Note in the final report
what Codex found and what you did about it.

## 3. Final review

When every work unit is committed, invoke the `pony-review` skill with the
argument `<start>..HEAD`, where `<start>` is the start commit from step 1.

Commit the fixes from each review stage with the commit loop in step 2,
including the Codex consult.

## Act on findings

Judge each finding on its merits. A finding from a review is a claim, not an
order.

- Fix it if you can confirm the problem in the code and the fix is clear.
- Skip it if it is wrong, or if the cost of the fix is larger than the risk.
  Record a one-line reason.
- Never remove or change behavior that the user asked for without asking
  first.

## When to ask

Use AskUserQuestion only when you cannot decide from the request, the code,
or a sensible default. For example:

- The task is unclear in a way that changes what you build.
- A finding proposes to remove or change something the user asked for.
- A contested review finding has a real trade-off that only the user can
  judge.

For any other choice, take the sensible default, and list it in the final
report. Collect questions and ask them together, up to four in one
AskUserQuestion call, so that you stop as few times as possible.

## Report

End with a short report:

- The commits that you made, one line each, with the short SHA.
- What the Codex consults found, and what you changed because of them.
- The `pony-review` report: findings fixed, findings skipped with reasons,
  and contested findings that are still open.
- The defaults that you chose without asking.
- The tests and checks that you ran, and their result.
