# Pre-Mortem Analysis (Gary Klein)

## When this lens applies

Use *before* the operator commits to a project, launch, hire, or deal -- while
changes are still cheap. Pre-Mortem is the lens for "this is about to start,
what kills it?" It is structurally different from Inversion: Inversion catalogs
failure modes abstractly; Pre-Mortem narrates the failure as if it already
happened, which surfaces failure modes the abstract version misses.

## Prompt template

```
I'm about to start a project and I want you to run a Pre-Mortem
Analysis on it, based on psychologist Gary Klein's method. Here's
how it works:

Imagine it's 6 months from now and this project has failed
completely. It's dead. Now work backwards and tell me every
plausible reason why it failed. Be specific. Don't give me generic
risks like 'poor execution.' Give me scenario-level detail: what
went wrong, when, and why I didn't see it coming. Then for each
failure scenario, give me one preventive action I can take right
now before I start.

My project: <PROJECT>
```

## What a strong answer looks like

- Adopts the past tense throughout -- "the launch missed the September window
  because...", "the second hire quit in week 6 after..." Past tense is what makes
  the lens different from generic risk analysis.
- Each failure scenario has: what happened, *when* it happened in the timeline,
  and *why the operator didn't see it coming*. The blindspot element is critical
  -- it's where the actual value of the technique lives.
- Covers at least these failure surfaces: demand / customer reaction, execution
  / timeline, people / team, money / runway, competitive response, external
  (regulatory, supplier, market).
- One *concrete preventive action* per scenario -- something that can be done in
  week 1 of the project, before the failure is locked in.
- Identifies the *earliest tripwire* -- what would be the first observable signal
  that one of these failure paths is starting?

## Failure modes to avoid

- Future tense ("the project might fail because...") -- that's risk analysis, not
  Pre-Mortem. Force the past tense.
- Generic failures ("the team wasn't aligned"). The scenario must be specific
  enough that a reader can picture the room.
- Symmetric, copy-paste preventive actions ("communicate more", "track KPIs").
  Each preventive action should be tailored to its specific scenario.
- Skipping the "why didn't I see it coming" component. That's where the
  blindspots live, and surfacing them is the entire reason this lens is more
  powerful than a normal risk register.
- Failing to flag the *kill-shot* scenarios -- the failure modes that don't just
  slow the project down but end it. These deserve disproportionate preventive
  investment.
