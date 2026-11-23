# Inversion (Munger)

## When this lens applies

Use when the operator is asking "how do I succeed?" on a goal where the failure
modes are numerous and well-known -- launches, hires, new markets, partnerships,
big investments. Inversion is the lens that forces a complete catalog of failure
paths before any success path is drawn.

## Prompt template

```
I want you to help me solve a problem using Inversion, the mental
model popularized by Charlie Munger. Instead of asking 'How do I
succeed at this?', I want you to start by asking 'How would I
guarantee failure at this?' List every way this could fail, every
bad decision I could make, and every assumption that would destroy
the outcome. Then flip each one into a concrete action I should take.

My goal: <GOAL>
```

## What a strong answer looks like

- Produces a numbered list of *specific* failure paths -- not "poor execution"
  but "we promise a 6-week launch, slip to 12, lose 40% of preorders during the
  silence."
- Covers the four failure surfaces: decisions, assumptions, execution, and
  people.
- For every failure mode, gives a *concrete preventive action* -- an action the
  operator could schedule on a calendar this week, not a principle.
- Flags which failure modes have the highest blast radius (kill the whole
  project) vs which are recoverable.

## Failure modes to avoid

- Generic risks ("market timing", "team morale") with no scenario attached --
  these are filler.
- Preventive actions that are themselves abstractions ("communicate better").
  The flip must be operational.
- Missing the boring failure modes (vendor invoice doesn't get paid, domain
  renewal lapses, a key person takes PTO during launch). These are where
  projects actually die.
- Listing 30 failure modes flat -- without prioritization, a long list is a
  non-answer.
