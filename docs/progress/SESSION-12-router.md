# Session 12 — Master router (+ integration)

> Public build-log entry. Value-focused only. No growth/marketing/strategy language.

- **Session:** S12
- **Wave:** 4
- **Date (UTC):** 2026-06-24
- **Depends on:** S05 (router spec + catalog), S06–S11 (the authored skills)
- **Status:** complete
- **Branch / commits:** `main` — integration merges of `session/S06..S11` + the router commit

## Objective
Integrate the six wave-3 authoring branches into `main`, validate the full 62-skill tree, and
build the master router (`router/SKILL.md`) that detects a project's engine and the user's task,
then dispatches to the right specialized skill(s) per `router/ROUTER-SPEC.md`.

## What was produced
- **Integration.** Merged `session/S06-godot`, `S07-unity-unreal`, `S08-web-other`,
  `S09-disciplines`, `S10-genres`, `S11-workflows` into `main` with `--no-ff`. The branches
  carried disjoint folders, so the merges were conflict-free; the one overlap (the six
  `web-engines` skills appeared in both the S07 chain and the S08 branch) was byte-identical and
  resolved with no conflict. Removed the three leftover worktrees (`.worktrees/S08..S10`).
- **`router/SKILL.md`** (202 lines) — `name: router`; a description that triggers on broad
  game-dev intent plus all ten engine names and "which skill" cues. Body: the six-step routing
  algorithm; engine-detection precedence table; task classification; a `task → category → skill`
  routing table covering every authored skill (engine indexes, discipline↔engine-API pairings,
  genre composition, workflows); the progressive-disclosure read protocol; composition ordering
  and ownership rules; the unknown-engine fallback (ask once, default to Godot); and a worked-
  example table.
- **`router/references/engine-detection.md`** — full fingerprint table, secondary signals, and
  the Godot-C#/Unity/Bevy + multi-web + monorepo disambiguation rules.
- **`router/references/routing-table.md`** — the exhaustive per-skill trigger/​binding lookup for
  all 62 skills, plus an explicit "binding gaps" section (where an engine lacks a slot a genre
  composes, e.g. there is no `unity-2d-movement`).

## Key decisions
- **Merge the branch tips as-is rather than rewrite history.** The wave-3 commits were chained
  unevenly across branches (e.g. the local `S07` tip already contained the S06 godot, S08
  web-engines, and S11 workflows commits; `S06`/`S11` were therefore reachable already). Merging
  `S07→S08→S09→S10` with `--no-ff` brought in all 62 skills; `S06` and `S11` reported "already up
  to date". The end state (all six branch tips are ancestors of `main`) satisfies the integration
  goal without force-pushing or rebasing other sessions' work.
- **Router dispatches, never re-teaches.** Per the spec, the body holds the algorithm and the top
  routing rules; the exhaustive table and detection detail live in `references/` to keep the
  `SKILL.md` lean (202 lines, well under the 500-line budget).
- **Reference only skills that exist, and name gaps honestly.** Every skill the router points to
  was cross-checked against the 62 authored folders. Where a genre composes a slot an engine
  lacks (e.g. 2D-movement outside Godot), the router says so and binds to the closest real skills
  rather than inventing a name.
- **Router description runs to 613 chars (> the 200-char soft target).** The spec requires broad
  trigger vocabulary plus all engine names; the primary use case leads so it still triggers if a
  surface truncates the tail. Under the 1024 hard cap.

## Verification
- `python scripts/validate-skills.py` — **Validated 63 skill file(s). All checks passed.** (the 62
  skills + `router/SKILL.md`: `name` = folder, description ≤ 1024, < 500 lines, resolved
  `references/` links).
- Catalog coverage: all 62 catalog skills exist on `main` and match the catalog's names/categories
  exactly (godot 15, unity 8, unreal 6, web-engines 6, other-engines 5, disciplines 9, genres 9,
  workflows 4). `git branch --merged main` lists all six `session/*` branches.
- **Router test matrix (below): 18 representative prompts, every one resolves to existing skills.**

## Router test matrix

18 engine×task prompts run through the router's algorithm. "Engine (signal)" is the §1 fingerprint;
"Skills resolved" lists the load order (engine fundamentals → discipline → genre → workflow). Every
named skill exists on `main`. ✓ = the §3 routing table + §6 fallback resolve the prompt as shown.

| # | Prompt | Engine (signal) | Skills resolved (in order) | Rule | ✓ |
|:-:|--------|-----------------|----------------------------|------|:-:|
| 1 | "Add a double jump to my player" | Godot (`project.godot`) | `godot-2d-movement` → `platformer` → `level-design` | §3a+§3c | ✓ |
| 2 | "Build an inventory for my RPG" | Unity (`Assets/`+`ProjectSettings/`) | `unity-scriptableobjects` → `rpg` → `save-systems` | §3c | ✓ |
| 3 | "Procedural dungeon, permadeath" | Bevy (`Cargo.toml`+`bevy`) | `bevy-ecs` → `procedural-gen` → `roguelike` (no Bevy tilemap → `level-design`, gap noted) | §3c+§6 | ✓ |
| 4 | "Wire up branching dialogue" + `*.yarn` | none required (file signal) | `dialogue-systems` (+ engine UI skill iff an engine is detected) | §2/§6 | ✓ |
| 5 | "How should I design save slots with migration?" | none (concept Q) | `save-systems` only | §6.2 | ✓ |
| 6 | "Publish my build on itch with butler" | any/none | `itch-publish` only | §3d | ✓ |
| 7 | "Give the enemy a patrol/chase behavior tree" | Unreal (`*.uproject`) | `unreal-behavior-trees` → `game-ai` | §3b | ✓ |
| 8 | "Make sprites collide and bounce off walls" | Phaser (`package.json` `phaser`) | `phaser-core` → `phaser-arcade-physics` | §3a | ✓ |
| 9 | "Load a `.glb` character with its animation" | three.js (`three` dep, `*.glb`) | `threejs-scene-setup` → `threejs-gltf-loading` | §3a | ✓ |
| 10 | "Write a dissolve shader for my sprite" | Godot (`*.gdshader`) | `godot-shaders` → `shader-programming` | §3b | ✓ |
| 11 | "Add rebindable controls with gamepad support" | Unity (`*.inputactions`) | `unity-input-system` → `input-systems` | §3b | ✓ |
| 12 | "Set up the game loop and a sprite group" | pygame (`import pygame`) | `pygame-core` | §3a | ✓ |
| 13 | "Save player data between sessions" | Roblox (`DataStoreService`/`*.rbxlx`) | `roblox-datastores` (+ `roblox-luau`) → `save-systems` | §3a+§3b | ✓ |
| 14 | "Spawn waves that walk a path to my towers" | Godot (`project.godot`) | `tower-defense` → `game-ai` + `godot-2d-movement` + `level-design` | §3c | ✓ |
| 15 | "I want to make a game but don't know what to use" | unknown → ask once, **default Godot** | (after default) `godot-nodes-scenes` | §6.1/§6.3 | ✓ |
| 16 | "Make a deckbuilder card game" | Unity (`Assets/`+`ProjectSettings/`) | `card-game` → `unity-scriptableobjects` (+ Unity UI via `unity-csharp-scripting`; no UI skill → gap) | §3c+§6 | ✓ |
| 17 | "Visual novel with choices" + `*.ink` | none required (file signal) | `visual-novel` → `dialogue-systems` + `save-systems` (+ engine UI if detected) | §2/§3c | ✓ |
| 18 | "Make music duck under SFX" | Godot (`project.godot`) | `godot-audio` → `audio-design` | §3b | ✓ |

**Coverage check:** the 18 cases exercise all five engine families (Godot, Unity, Unreal, web,
other) plus the no-engine path; all four categories (engine, disciplines, genres, workflows); two
file-signal detections (`*.yarn`, `*.ink`, `*.inputactions`, `*.glb`, `*.gdshader`); the
unknown-engine fallback with the Godot default (#15); and three honest binding-gap cases (#3, #16,
and the conditional engine-UI hand-offs in #4/#17). No prompt resolves to a non-existent skill.

## Sources consulted
- `router/ROUTER-SPEC.md` (the frozen S05 contract) and `docs/skill-catalog.md` (per-skill
  `says:`/`files:` signals + `composes:` fields) — the router is built directly from these; no web
  research was needed for this session.

## Handoff to next sessions
- S13 (QA/originality) can use this test matrix as the seed for a larger routing regression set,
  run the validator (now covering `router/SKILL.md`), and do the originality pass across all 62
  skills + the router.
- The catalog statuses and `docs/progress/INDEX.md` were updated to mark S06–S12 done as part of
  this session (single-writer step).

## If partial — remaining work
- None for S12. Out of scope by design: generating per-editor adapters (Cursor/Windsurf/Cline)
  from the canonical `SKILL.md` files — that is a docs/tooling concern for a later wave.
