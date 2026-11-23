# The 5 Whys (Toyota Production System)

## When this lens applies

Use when the operator describes a *recurring* problem -- the same fire, the same
complaint, the same miss, on a cadence. 5 Whys is the lens that distinguishes a
symptom from a structural cause. Do not use it for one-time decisions or
forward-looking bets; use it when something keeps breaking.

## Prompt template

```
I have a recurring problem and I need to find the actual root cause,
not just the surface symptom. Use the 5 Whys method from the Toyota
Production System. Start with the problem I describe. Ask 'Why does
this happen?' Then take my answer and ask 'Why?' again. Repeat this
at least 5 times, going deeper each round until we hit something
structural that I can actually fix. Don't accept vague answers from
me. Push me to be specific at every level.

My recurring problem: <PROBLEM>
```

## What a strong answer looks like

- Each "why" level is *more specific* than the level above it, not more
  abstract. Generality is a sign you're not actually drilling.
- Reaches at least 5 levels deep -- and notes if level 5 still feels like a
  symptom (then go to 6 or 7).
- The terminal cause is a *structural property* of the system -- a process gap, a
  missing handoff, an incentive mismatch, a tool limitation -- not a person's
  character flaw.
- Ends with a fix targeted at the terminal cause, plus a check: "if we fix this,
  do the upstream symptoms naturally disappear?"

## Failure modes to avoid

- "Because the team didn't try hard enough" is *always* a failure of the lens,
  not an answer. Push past it.
- Accepting the first plausible-sounding answer at each level -- the framework is
  designed to push past plausibility.
- Branching prematurely. Pick one chain and drill it; multi-causal analysis is a
  different framework.
- Stopping at "people" when the next "why" would lead to a system ("why don't
  they do X?" -> "because the tool makes X take 40 minutes" -> that's the real
  cause).
