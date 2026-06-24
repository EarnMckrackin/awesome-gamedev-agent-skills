# Session 10 — Author: Genres

> Public build-log entry. Value-focused only. No growth/marketing/strategy language.

- **Session:** S10
- **Wave:** 3 (parallel with S06–S09, S11)
- **Date (UTC):** 2026-06-24
- **Depends on:** S05
- **Status:** complete
- **Branch / commits:** `session/S10-genres` (isolated worktree) → genre-authoring commit(s)

## Objective
Author the nine engine-agnostic **genre** skills from the frozen catalog as compositional
playbooks: each defines the core loop, must-have systems, design knobs, failure modes, and how
to compose with engine + discipline skills — without re-teaching primitives.

## What was produced
All under `skills/genres/`, each a `SKILL.md` plus one `references/` depth file:

- **`platformer`** — run/jump feel (coyote time, jump buffering, variable height, asymmetric
  gravity), tiled levels, hazards, camera; `references/feel-tuning.md` (jump math, corner
  correction, one-way/moving platforms, camera follow).
- **`roguelike`** — turn scheduler, procedural dungeons, FOV, permadeath, loot; anchored to the
  Berlin Interpretation; `references/generation-fov-loot.md` (rooms/BSP/cellular, connectivity,
  shadowcasting, weighted depth-scaled tables, run vs. profile state).
- **`rpg`** — stats/leveling, inventory/equipment, quests, dialogue, save, combat;
  `references/stats-combat-quests.md` (derived stats, damage formulas, XP curves, turn-vs-action,
  quest state model).
- **`fps-shooter`** — first-person controller, hitscan/projectile shooting, TTK, recoil/spread,
  enemy AI; `references/shooting-and-feel.md` (falloff, recoil patterns, TTK math, lag comp, AI FSM).
- **`tower-defense`** — pathing, wave spawner, tower targeting, economy, lives;
  `references/balancing.md` (path representations, targeting modes, DPS/economy math, scaling, pacing).
- **`card-game`** — card data, zones, draw/reshuffle, turn phases, effect resolution;
  `references/effect-resolution.md` (effects-as-data interpreter, queue/stack, triggers/keywords,
  Fisher–Yates, deckbuilder vs. constructed, curves).
- **`visual-novel`** — branching script, presentation, choices, save-anywhere, backlog/skip/auto;
  `references/script-and-flow.md` (script data model, route structures, flags, save data, localization).
- **`survival-crafting`** — gather→craft→build loop, needs, tech tree, base building;
  `references/needs-and-crafting.md` (needs thresholds, tech-tree graph, gather/respawn, building, escalation).
- **`puzzle`** — board model, rule resolution (match-3/sokoban/logic), cascades, undo, generation;
  `references/board-and-resolution.md` (match-3 gravity/refill, deadlock+reshuffle, sokoban, undo,
  solvable generation, input locking).

## Key decisions
- **Genres orchestrate, never re-teach.** Each skill defines the loop and systems and links out
  to engine + discipline skills via a "Composition" section using exact catalog names; primitives
  (physics, tilemaps, dialogue runtime, RNG) are deferred, not duplicated.
- **Engine-agnostic, pseudocode patterns.** Code blocks are clearly labelled engine-neutral
  pseudocode (y-axis down to match 2D engines) so they stay correct without claiming any specific
  engine API — consistent with how the catalog frames genres and disciplines.
- **One `references/` file per skill.** Lean playbooks (≤~150 lines) with depth (formulas, tables,
  algorithms) pushed into a single reference file each, per the progressive-disclosure budget.
- **Consistent body shape** across all nine: when-to-use (with sibling pointers) → core loop →
  must-have systems → design knobs (table) → ≥2 patterns → pitfalls/failure modes → composition →
  references. Mirrors the golden skill's structure.

## Verification
- **Validator:** `python scripts/validate-skills.py` → "Validated 10 skill file(s). All checks
  passed." (exit 0) — the 9 genre skills plus the golden skill.
- **Size budget:** every `SKILL.md` is 122–147 lines (< 500); every `references/` file is 89–122
  lines. Bodies are well under the ~5k-token tier-2 budget.
- **Descriptions:** all nine are 184–198 chars (≤200 soft target for claude.ai, ≤1024 hard limit),
  third person, leading with what-it-does then "Use for …" with trigger vocabulary.
- **Cross-skill names:** every hyphenated skill reference in the bodies was checked against the
  frozen 62-name catalog set — all resolve to real skills (engine, discipline, or workflow).
- **Reference links:** all `references/*.md` links resolve (validator-enforced).
- **Skills authored:** 9; all pass the rubric in `docs/SKILL-FORMAT.md`: yes.

## Sources consulted (design references)
- `https://www.roguebasin.com/index.php/Berlin_Interpretation` — canonical roguelike
  "roguelikeness" factors (RogueBasin / IRDC 2008), used to anchor the `roguelike` design.
- Established, widely-documented design patterns were used as *patterns only* and written from
  scratch: platformer game-feel (coyote time / jump buffering / variable jump height),
  tower-defense wave/economy balancing, match-3 cascade resolution, hitscan-vs-projectile and
  time-to-kill, deckbuilder draw/discard/reshuffle, and the survival gather→craft→build loop.
- Implementation primitives are deferred to the discipline/engine skills cited in each
  "Composition" section (e.g. `procedural-gen`, `save-systems`, `dialogue-systems`, `game-ai`).

## Handoff to next sessions
- **S12 (router):** these genres are compositional entry points. Each `SKILL.md` lists its
  composition targets by exact name; the router can layer a genre on top of the detected engine
  set plus the triggered discipline skills. Router signal phrases match the catalog `says:` cues.
- **S13 (QA):** descriptions, sizes, names, and links are pre-checked; an originality pass can
  confirm the prose is written from scratch (no copied text/code).

## If partial — remaining work
- None. All nine catalog genres are authored and pass the validator. `docs/progress/INDEX.md`
  is intentionally left untouched (shared file, edited by the consolidating session to avoid
  parallel-wave conflicts).
