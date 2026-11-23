---
name: problem-solving
description: Reason about a hard business problem (a stuck decision, a recurring failure, a costly commitment, an upcoming launch) by running 2-4 mental-model frameworks in parallel and synthesizing a decision brief. Use when the operator is stuck, second-guessing a call, or about to bet real time / money / focus on something.
triggers:
  - "stuck on"
  - "should I"
  - "help me think through"
  - "big decision"
  - "tough call"
  - "considering"
  - "thinking about"
  - "is it worth"
  - "torn between"
  - "weighing"
  - "should we"
  - "keeps happening"
  - "recurring"
  - "before I launch"
  - "before we commit"
  - "pre-mortem"
  - "second opinion"
  - "sanity check this decision"
  - "business problem"
  - "strategic decision"
---

# Problem solving

## Overview

Hard business problems do not get easier by reading more dashboards. They get
easier by being looked at through the right lens. This skill holds seven such
lenses -- each a battle-tested mental model -- and applies a subset of them in
parallel to the operator's problem, then fuses the outputs into a single brief.

Default behavior: pick **2-4 frameworks** that fit the problem type,
dispatch one subagent per framework in parallel, and present the full brief,
with its synthesis, in chat.
Never run all seven unless the operator asks -- that is theater, not analysis.

## When to use

Use when the operator surfaces a problem with at least one of these signatures:

- A decision they can't pull the trigger on (yes/no, A/B, in/out)
- A recurring failure that retries haven't fixed
- An upcoming commitment of real resources (money, headcount, a studio lease, a
  quarter)
- A "should I keep doing this?" question on an existing bet
- Asking for a second opinion or pressure-test on their own thinking

Do **not** use for: a factual lookup, a status check, a "what does X mean?"
question, or anything the operator already has a clear plan for. The skill's
value is in *applying lenses they haven't yet*, not in narrating frameworks at
them.

## Frameworks

Each lens lives in `frameworks/<lens>.md`. The file contains the verbatim
prompt, when the lens applies, what a strong answer looks like, and the common
failure modes for that lens.

| Lens                  | One-line use                                       | File                                  |
|-----------------------|----------------------------------------------------|---------------------------------------|
| First Principles      | Strip a problem to truths and rebuild              | `frameworks/first-principles.md`      |
| Inversion (Munger)    | Map every way to guarantee failure, then flip      | `frameworks/inversion.md`             |
| 5 Whys (Toyota)       | Drill from symptom to structural root cause        | `frameworks/five-whys.md`             |
| Second-Order Thinking | Trace 1st / 2nd / 3rd-order consequences           | `frameworks/second-order-thinking.md` |
| Regret Minimization   | Evaluate from age 80 looking back                  | `frameworks/regret-minimization.md`   |
| Opportunity Cost      | Make the "this versus that" tradeoff visible       | `frameworks/opportunity-cost.md`      |
| Pre-Mortem (Klein)    | Imagine the project already failed, work backwards | `frameworks/pre-mortem.md`            |

## Framework selection

Pick 2-4 lenses based on the problem signature. When in doubt, fewer focused
lenses beat more shallow ones.

| Problem signature                           | Default lens set                                      |
|---------------------------------------------|-------------------------------------------------------|
| Recurring failure / "this keeps happening"  | 5 Whys + First Principles                             |
| Major fork-in-the-road decision             | Second-Order + Regret Minimization + Opportunity Cost |
| About to launch / commit resources          | Pre-Mortem + Inversion + Opportunity Cost             |
| Stuck inside conventional wisdom            | First Principles + Inversion                          |
| Career / life-shape decision                | Regret Minimization + Opportunity Cost + Second-Order |
| "Why won't this work?" / risk-side analysis | Inversion + Pre-Mortem                                |
| Open-ended "help me think"                  | First Principles + Second-Order + Opportunity Cost    |

If the operator names a lens, honor it -- but still propose one complementary
lens to avoid single-frame blindness.

## Workflow

1. **Capture the problem.** Get a one-paragraph problem statement from the
   operator in their own words. If it's vague ("I'm stuck"), ask once for the
   concrete decision, deadline, and resources at stake. Do not ask a battery of
   clarifying questions -- one round, then proceed with what you have.

2. **Pick the lens set.** Use the table above. State your picks and one-line
   rationale to the operator before dispatching (e.g. *"Running Pre-Mortem +
   Inversion + Opportunity Cost -- this is a launch + a commitment."*). Accept a
   redirect, then proceed.

3. **Dispatch in parallel.** Send one subagent per chosen lens via the Agent
   tool, all in a single message so they run concurrently. Each subagent
   receives:

    - The operator's problem statement verbatim
    - The lens file (`frameworks/<lens>.md`) -- instruct the subagent to follow
      the prompt template inside
    - A response-length cap (target 400-600 words per lens; the brief is a
      decision tool, not a treatise)
    - A directive to be specific and to refuse vague answers -- push back on its
      own output before returning

4. **Write the brief.** Assemble outputs into one markdown brief in chat. Do
   not write it to a file. Brief structure:

   ```
   # <Problem title>

   ## Problem
   <verbatim problem statement>

   ## Lenses applied
   <bulleted list with one-line rationale per lens>

   ## <Lens 1 name>
   <subagent output>

   ## <Lens 2 name>
   ...

   ## Synthesis
   - Conflicts between lenses: ...
   - Convergence (where multiple lenses point the same way): ...
   - The decision this points to: ...
   - What would change the call: ...

   ## Next action
   <one concrete action with a date>
   ```

## Output rules

- **The brief is the deliverable.** Present the full brief in chat, in the
  structure above. Do not write it to a file.
- **No cheerleading and no hedging.** Each lens should commit to a position. "It
  depends" is a failure mode for these frameworks.
- **Name the conflict.** If two lenses point opposite directions, the synthesis
  must say so out loud. That tension is the actual insight; smoothing it over
  destroys the value of running multiple lenses.
- **Date the brief.** Use the current date, year-qualified, in the first line
  of the brief.

## Pitfalls

- **Running all seven by default.** Seven lenses produce a wall of text the
  operator won't read. 2-4 focused lenses produce a brief they will act on.
  Restraint is the skill.
- **Picking lenses that agree.** Two lenses that always reach the same answer
  (e.g. Inversion + Pre-Mortem on every problem) give false confidence. Mix
  risk-side lenses with value-side lenses.
- **Letting subagents return generic outputs.** "Make sure the team is aligned"
  is not a Pre-Mortem failure mode. Push subagents for scenario-level
  specificity in the dispatch prompt.
- **Treating the operator's framing as ground truth.** First Principles and 5
  Whys exist precisely to attack the framing. If every lens accepts the problem
  statement at face value, none of them are doing their job.
- **Stalling on missing data.** If the operator can't quantify something
  exactly, proceed with their best estimate and flag the sensitivity in the
  synthesis. Frameworks beat spreadsheets on the decisions this skill is for.
- **Skipping the synthesis.** Seven separate analyses with no synthesis is worse
  than one good lens -- it offloads the integration work back onto the operator.

## References

- `frameworks/first-principles.md`
- `frameworks/inversion.md`
- `frameworks/five-whys.md`
- `frameworks/second-order-thinking.md`
- `frameworks/regret-minimization.md`
- `frameworks/opportunity-cost.md`
- `frameworks/pre-mortem.md`
