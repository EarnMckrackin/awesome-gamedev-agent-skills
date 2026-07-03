---
name: playtest-loop
description: >
  Runs the playtest-and-amend loop: turn feedback like "the jump feels floaty" or "landing
  kills my speed" into the smallest targeted change, behind named tuning values, with a debug
  overlay and a test checklist. One mechanic at a time. Use when a mechanic feels wrong, when
  tuning movement/feel numbers, or when iterating a kept prototype toward a vertical slice.
license: Apache-2.0
compatibility: Engine-agnostic process skill (applies to any engine or framework).
metadata:
  engine: none
  category: workflows
  difficulty: beginner
---

# Playtest Loop

Iterate one mechanic to "feels good" through tight play → feedback → smallest-amendment
cycles. The unit of work is an **amendment** — a 1–2 value change behind named tuning
variables — never a rewrite. The player's feedback is design input; your job is to translate
it into the smallest change that can be replayed and judged in minutes.

## When to use

- Use after a prototype earns a **keep** decision and now needs to *feel right* — tuning
  movement, jumps, landings, chases, camera pressure, or any core verb.
- Use whenever the user reports feel friction: "too floaty", "too fast", "sluggish",
  "unfair", "I keep bailing", "landing kills momentum", "make it feel better".
- Use when developing a game with an agent in short build-play-amend rounds.

**When *not* to use:** building the *first* rough toy and deciding keep/kill (use
`prototype-fast`); choosing juice techniques like screen shake or hit-stop (use `game-feel` —
then apply them through this loop); engine physics instability such as jitter or tunneling
(use `physics-tuning`); a timed competition (use `game-jam`).

## Core workflow

1. **Isolate one mechanic.** Work on exactly one core mechanic per pass — never introduce
   movement + combat + stealth together. If the request bundles several, sequence them and
   say so.
2. **Ship a playable round, not a system.** The smallest version the user can run right now
   beats a complete architecture they can run next week.
3. **Attach a test checklist** (Pattern 4) to every round: what to open, what to press, what
   to watch, and what "wrong" looks like. Untested rounds don't count as progress.
4. **Collect friction in the player's words**, then restate it in gameplay terms and name
   the system involved ("landing kills momentum" → landing code, speed retention).
5. **Make the smallest useful amendment** (Pattern 1): change 1–2 values or one short
   function. A rewrite is never the first response to feel feedback.
6. **Put every feel number behind a named tuning value** (Pattern 2) exposed in the editor
   or a config block, so the next round is a value tweak, not a code change.
7. **Keep the debug overlay alive** (Pattern 3). Every tuned value should be observable
   while playing. Removing debug is a release task, not a cleanup task.
8. **Tune slow-and-readable first** (Pattern 5). Raise speed, density, and pressure only
   after the mechanic is understood and fun at forgiving settings.
9. **Gate integration on a feel pass.** Only merge the mechanic into the real level or
   vertical slice once a fresh replay produces no new friction. Then return to step 1 for
   the next mechanic.

## Patterns

### 1. Friction → smallest amendment (translate, don't rewrite)

```text
"landing kills my speed"     -> raise landing_speed_retention; show speed before/after landing
"jump feels floaty"          -> raise fall_gravity_multiplier; cut jump_force ~10-20%
"I miss jumps off ledges"    -> add/raise coyote_time; add jump_buffer window
"turning feels sluggish"     -> raise ground_accel / turn_rate; debug input dir vs velocity dir
"grinds are hard to start"   -> widen rail snap_distance; show nearest rail + snap range
"the chase feels unfair"     -> slow it down and telegraph obstacles earlier; do NOT add more

RULE: one amendment = 1-2 named values, or one small function. If the fix seems to need a
rewrite, the feedback was too vague — ask one clarifying question instead of rebuilding.
```

### 2. Named tuning values, exposed where the engine shows them

```text
# Pseudocode — expose via the engine's inspector mechanism:
#   Godot: @export var   ·  Unity: [SerializeField]  ·  Unreal: UPROPERTY(EditAnywhere)
#   web/Lua/Python: one TUNING table/object at the top of the file
TUNING:
    push_accel              = 40.0     # units/s^2 while pushing
    max_speed               = 260.0
    jump_force              = 380.0
    fall_gravity_multiplier = 1.6      # gravity scale while falling (kills floatiness)
    coyote_time             = 0.10     # s of grace after leaving a ledge
    jump_buffer             = 0.12     # s a jump press is remembered before landing
    landing_speed_retention = 0.85     # fraction of horizontal speed kept on landing

# Anti-pattern: the same numbers inlined at their point of use ("velocity.y = -380"),
# where the next round of tuning becomes a code search instead of a slider drag.
```

### 3. The debug overlay contract

```text
While a mechanic is being tuned, the screen always shows:
  - current state (OnFoot / Skating / Airborne / Grinding / Bailing ...)
  - the judged quantity (speed, height, timer) — live, numeric
  - active grace windows (coyote remaining, buffered input)
  - the LAST FAILURE REASON in plain words ("bailed: landed on slope > 35°")
Failure reasons are the highest-value line: they turn "it feels random" into a tunable cause.
```

### 4. Test checklist — attach after every amendment

```md
## Test checklist
- Open: <scene/level to run>
- Do: <exact inputs to try, including the failure case>
- Expect: <the observable change from this amendment>
- Watch: <debug values that prove it>
- Wrong if: <the signal that means re-amend>
- Report: <what to tell me after playing — one sentence of friction is enough>
```

### 5. Slow first, then speed up

```text
First rounds ship SLOWER, MORE READABLE, MORE FORGIVING than the final target:
  lower speeds · wider timing windows · fewer obstacles · obvious telegraphs
Only after the player understands and enjoys the mechanic, raise (one per round):
  speed -> density -> pressure -> timing difficulty
Prefer predictable-fun defaults over realism: forgiving snap, input buffering, coyote
time, and landing forgiveness beat accurate simulation in almost every feel dispute.
```

## Pitfalls

- **Rewriting on vague feedback.** "Make it feel better" is a prompt for one clarifying
  question or one hypothesis amendment — not a new controller.
- **Mechanic soup.** Adding several new core mechanics in one pass makes friction
  unattributable. One mechanic per pass, tuned before the next lands.
- **Tuning for an expert on day one.** The first player is always unskilled at your game;
  first-round difficulty should embarrass you slightly.
- **Magic numbers at point of use.** Every inlined constant is a future grep; every named
  tuning value is a future slider.
- **Deleting debug as "cleanup".** The overlay is part of the process; strip it at release.
- **Integrating an unproven toy.** A mechanic that hasn't passed a feel test contaminates
  the slice with friction you'll misattribute to level design.
- **Chasing realism.** If realistic physics and readable fun disagree, fun wins; simulate
  less, author more.

## References

- For a catalog of common feel tuning values (movement, jump, camera, chase/stealth) with
  meanings and starting ranges, read `references/tuning-values.md`.

## Related skills

- `prototype-fast` — builds the first throwaway toy and makes the keep/kill call; this loop
  begins where its "keep" ends.
- `game-feel` — the juice techniques (shake, hit-stop, easing) you apply *inside* this loop.
- `physics-tuning` — when the friction is engine physics (jitter, tunneling, timestep).
- `input-systems` — implementation depth for buffering and input handling.
