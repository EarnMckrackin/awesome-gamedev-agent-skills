# Session 08 — Author: Web + Other Engines

> Public build-log entry. Value-focused only. No growth/marketing/strategy language.

- **Session:** S08
- **Wave:** 3 (parallel with S06, S07, S09–S11)
- **Date (UTC):** 2026-06-24
- **Depends on:** S05 (frozen standard, catalog, golden skill)
- **Status:** complete
- **Branch / commits:** `session/S08-web-other` — `feat(web-engines): …` (6 skills),
  `feat(other-engines): …` (4 skills), and this progress entry.

## Objective
Author the browser/JS engine skills and the "other engine" skills from the frozen
catalog, each original, version-pinned, and verified against primary documentation,
following `docs/SKILL-FORMAT.md` and the shape of the golden `love2d-core` skill.

## What was produced
Ten skills (each a `SKILL.md` plus one `references/` file for progressive disclosure):

**web-engines (6)**
- `phaser-core` — Phaser 3.90 game config, Scene lifecycle, loader, cameras,
  cross-scene comms (+ `references/scene-flow.md`).
- `phaser-arcade-physics` — Arcade bodies, velocity/gravity, colliders/overlap,
  groups, world bounds (+ `references/bodies-and-collision.md`).
- `pixijs-rendering` — PixiJS **v8** async `Application.init`, `Assets`, scene graph,
  ticker, events, render groups (+ `references/assets-and-display.md`).
- `threejs-scene-setup` — three.js scene/camera/renderer, `setAnimationLoop`, resize,
  `OrbitControls` (+ `references/scene-graph.md`).
- `threejs-gltf-loading` — `GLTFLoader`, `AnimationMixer`, DRACO/Meshopt/KTX2
  (+ `references/loaders-and-animation.md`).
- `threejs-materials-lighting` — PBR materials, lights, shadow maps, IBL
  (+ `references/materials-lights-table.md`).

**other-engines (4)** — `love2d-core` skipped (golden, done in S05)
- `bevy-ecs` — Bevy App/plugins, components/resources, systems, queries, scheduling,
  `Time` (+ `references/queries-and-scheduling.md`).
- `pygame-core` — pygame-ce loop, dt-motion, `Surface`/`Rect`, input, `Sprite`/`Group`
  collision (+ `references/sprites-and-collision.md`).
- `roblox-luau` — services, instances, events, server/client split, Remotes
  (server-authoritative) (+ `references/client-server.md`).
- `roblox-datastores` — `DataStoreService` load/save, `pcall`, `BindToClose`, retries,
  ordered stores (+ `references/sessions-and-limits.md`).

## Key decisions
- **The frozen catalog is the source of truth for names/versions**, not the
  illustrative "typical set" in the session brief. The brief listed generic names
  (`phaser`, `pixijs`, `threejs-game`, `bevy`, `pygame`, `love2d`, `roblox`) and an
  old Bevy version; the committed `docs/skill-catalog.md` defines the exact 10 names
  authored here. Followed the catalog.
- **Bevy version (gap noted per the frozen-standard policy).** The catalog pins
  "Bevy 0.16+ (pin hard)"; `docs.rs/bevy/latest` is now **0.19**. Resolution: set
  `compatibility: Bevy 0.16+` (the catalog floor) and restrict all example code to
  the ECS core that is identical across 0.16→0.19 (`App`/`add_plugins`/`add_systems`,
  `#[derive(Component/Resource)]`, `Query`/`Res`/`Commands`, `Time::delta_secs()`,
  required-component spawning of `Camera2d`). Deliberately avoided the buffered-event
  /message API, which was reworked after 0.16, and documented that churn in-skill and
  in the reference. Flagged here for S13.
- **three.js pinned to r150+ and verified against r184** (current `three@0.184.0`).
  Skills use the modern idioms: ES modules + import maps (required since r147), the
  `three/addons/` path, `renderer.setAnimationLoop`, physically-based light
  intensities (default since r155), and sRGB color-space tagging for color maps.
- **PixiJS pinned to v8 specifically.** The skill leads with the v8 break (empty
  constructor + `await app.init()`, `app.canvas`, unified `Assets`, `eventMode`) and
  calls out the v7→v8 migration so it doesn't silently fire on old code.
- **Roblox skills are server-authoritative by construction.** `roblox-luau` and
  `roblox-datastores` repeatedly stress server-side validation of all client input,
  `pcall`-wrapped network calls, and not overwriting good saves on a failed load.
- **One responsibility per skill, with explicit hand-offs.** Phaser core vs arcade
  physics, three.js setup vs gltf vs materials, and Roblox scripting vs datastores are
  split; each `description` names the sibling skill that owns the adjacent concern, and
  every skill ends with a "Related skills" cross-reference block for the router.

## Verification
- **Validator:** `python scripts/validate-skills.py` (run in the S08 worktree) →
  "Validated 11 skill file(s). All checks passed." (exit 0). The 11 are the golden
  `love2d-core` plus the 10 authored here; all pass name==folder, `description` ≤1024,
  `SKILL.md` < 500 lines, and resolved `references/` links.
- **Descriptions:** all within the 1024-char hard limit. Several exceed the 200-char
  claude.ai soft target by design — each is front-loaded with what-it-does + primary
  use so a 200-char truncation still triggers correctly, with the tail carrying extra
  trigger vocabulary and the sibling-skill pointer (a deliberate L1 trade-off; noted
  for S13's human review).
- **API correctness (primary docs via Firecrawl):** Phaser scene lifecycle + Arcade
  body/collider API (docs.phaser.io); PixiJS v8 `new Application()` + `await app.init`,
  `Assets.load`, `app.canvas` (pixijs.com/8.x); Bevy ECS App/systems/queries and
  `Time::delta_secs()`/`elapsed_secs()` (bevy.org quick-start + docs.rs); three.js
  module/import-map + `WebGLRenderer`/`PerspectiveCamera`, `GLTFLoader`, and the three
  shadow switches (threejs.org/manual, verified r184); pygame-ce current (2.5.7) loop
  + sprites; Roblox `DataStoreService` get/set/update/ordered + `pcall`/`BindToClose`
  and the Remote/service model (create.roblox.com/docs).
- **Originality:** every `SKILL.md` and reference is written from scratch from the
  primary docs above; no text or code copied from other skill collections.

## Build/infra note (for the integrator, S12)
This session ran in **parallel mode** sharing one repository on disk. The main working
directory had a shared, contended `HEAD` (sessions S06/S07/S11 committing onto one
chain), and the first web-engines commit briefly landed on `session/S07-unity-unreal`
before being moved. Recovery: created an isolated worktree on `session/S08-web-other`
and `cherry-pick`ed the web-engines commit onto it; all S08 commits now live on
`session/S08-web-other` and contain **only** `skills/web-engines/**`,
`skills/other-engines/**`, and this progress doc. The earlier mislabeled commit may
still be reachable from `session/S07-unity-unreal` (its branch was checked out
elsewhere and not force-moved, to avoid disrupting that session); the content is byte
-identical to the S08 commit, so a merge of both branches is conflict-free. No shared
files (`docs/progress/INDEX.md`, the catalog, the standard) were edited.

## Sources consulted (primary docs)
- `https://docs.phaser.io/phaser/concepts/scenes` · `.../concepts/physics/arcade` —
  Phaser 3 scene lifecycle and Arcade Physics API.
- `https://pixijs.com/8.x/guides/getting-started/quick-start` — PixiJS v8 async init.
- `https://bevy.org/learn/quick-start/getting-started/{ecs,apps,resources}/` ·
  `https://docs.rs/bevy/latest/bevy/` (and `…/time/struct.Time.html`) — Bevy ECS API
  and current version (0.19).
- `https://threejs.org/manual/en/{fundamentals,load-gltf,shadows}.html` ·
  `https://www.npmjs.com/package/three` — three.js idioms and current revision (r184).
- `https://github.com/pygame-community/pygame-ce/releases` — pygame-ce current (2.5.7).
- `https://create.roblox.com/docs/cloud-services/data-stores` — DataStoreService API.

## Handoff to next sessions
- **S12** routes to these 10 skills. Engine detection: `package.json` deps
  (`phaser` / `pixi.js` / `three`), `Cargo.toml` dep `bevy`, `import pygame`, and
  Roblox `*.luau`/`*.rbxl(x)`/Rojo `*.project.json`. Trigger vocabulary and sibling
  hand-offs are embedded in each `description` and "Related skills" block.
- **S13** should sanity-check the Bevy version note above and the >200-char
  descriptions, and may re-verify the three.js/PixiJS pins (both move fast).

## If partial — remaining work
- None for S08's scope. Optional follow-ups: when Bevy/PixiJS next ship a major
  release, re-verify the pinned snippets; consider a worked example pairing
  `bevy-ecs` with `game-ai`/`procedural-gen` once those discipline skills land.
