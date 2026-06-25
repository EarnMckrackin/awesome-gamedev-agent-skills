<!-- markdownlint-disable MD033 MD041 -->

# awesome-gamedev-agent-skills

**Game-development expertise for AI coding agents — install once, and a router loads the
right skill for whatever you're building.**

[![License](https://img.shields.io/badge/license-Apache--2.0-blue)](LICENSE)
[![Skills](https://img.shields.io/badge/skills-62%20%2B%20router-brightgreen)](docs/skill-catalog.md)
[![Format](https://img.shields.io/badge/format-Agent%20Skills-informational)](docs/SKILL-FORMAT.md)
[![Last commit](https://img.shields.io/github/last-commit/AbhishekBarali/awesome-gamedev-agent-skills)](https://github.com/AbhishekBarali/awesome-gamedev-agent-skills/commits/main)

[Agent Skills](docs/SKILL-FORMAT.md) are small, focused capability files an AI coding agent
loads on demand. This repository is an original, cross-engine collection of **62 game-dev
skills** plus a **master router** that detects your engine and task and pulls in only the
skills you need — so you don't have to know which one to ask for.

Every skill is written from primary documentation, version-pinned to a stated engine release,
and verified by a mechanical [validator](scripts/validate-skills.py). Nothing here is copied
from other skill collections.

## Quick start

1. **Install once.** Copy the collection (the router + every skill) into your agent's skills
   directory. Per-agent commands are in [`docs/INSTALLATION.md`](docs/INSTALLATION.md); the
   short version for Claude Code:

   ```bash
   mkdir -p .claude/skills
   find skills -name SKILL.md -type f -exec dirname {} \; | while read -r d; do
     cp -R "$d" .claude/skills/
   done
   cp -R router .claude/skills/router
   ```

2. **Describe what you want.** Ask your agent a game-dev question in plain language:

   > "add a double jump to my Godot player"

3. **Let it route.** The router detects the engine (here, Godot from `project.godot`), picks
   the task skills (`godot-2d-movement` + `platformer`), and reads only those before writing
   code. You never name a skill yourself.

## How the router works

The [`router`](router/SKILL.md) is the entry point for any game-dev request. It runs three
steps, then composes the result:

1. **Detect the engine.** It fingerprints the project — `project.godot` → Godot, `*.uproject`
   → Unreal, `Assets/` + `ProjectSettings/` → Unity, a `bevy`/`phaser`/`pixi.js`/`three`
   dependency → that framework, and so on — and picks **exactly one** engine skill set. No
   project files? It reads the engine from your wording, or asks once.
2. **Understand the task.** It classifies your phrasing into cross-engine **disciplines**
   (AI, save systems, shaders, …), at most one **genre** (platformer, roguelike, RPG, …), and
   any **workflow** (game jam, ship to Steam/itch).
3. **Load the skill(s).** It reads only the chosen `SKILL.md` bodies — engine fundamentals
   first, then the discipline concept, then genre glue — and opens a skill's `references/`
   only when the subtask needs that depth. It re-routes when the task pivots to a new concern.

Engine selection is exclusive; disciplines, genres, and workflows are additive. Full design:
[`router/ROUTER-SPEC.md`](router/ROUTER-SPEC.md).

## Catalog

62 skills across 8 categories. The authoritative list, with target versions, primary sources,
and router signals, is [`docs/skill-catalog.md`](docs/skill-catalog.md).

### Engines

#### Godot — 15 ([`skills/godot/`](skills/godot/)) · Godot 4.x

| Skill | Scope |
|-------|-------|
| [`godot-gdscript`](skills/godot/godot-gdscript/SKILL.md) | GDScript language: typing, lifecycle, `@export`, signals, idioms |
| [`godot-nodes-scenes`](skills/godot/godot-nodes-scenes/SKILL.md) | Scene tree, node composition, instancing, autoloads, `PackedScene` |
| [`godot-signals-groups`](skills/godot/godot-signals-groups/SKILL.md) | Event-driven design with signals + groups |
| [`godot-2d-movement`](skills/godot/godot-2d-movement/SKILL.md) | `CharacterBody2D` kinematic movement, `move_and_slide`, slopes |
| [`godot-tilemap`](skills/godot/godot-tilemap/SKILL.md) | `TileMapLayer`/`TileSet`: autotiling, terrain, collision/nav layers |
| [`godot-physics`](skills/godot/godot-physics/SKILL.md) | Rigid/Area/Static bodies (2D+3D), collision layers, raycasts |
| [`godot-ui-control`](skills/godot/godot-ui-control/SKILL.md) | `Control` nodes: anchors, containers, themes, focus nav |
| [`godot-animation`](skills/godot/godot-animation/SKILL.md) | `AnimationPlayer`, `AnimationTree`, `Tween` |
| [`godot-shaders`](skills/godot/godot-shaders/SKILL.md) | Godot shading language: 2D `canvas_item` + 3D `spatial` shaders |
| [`godot-3d-essentials`](skills/godot/godot-3d-essentials/SKILL.md) | 3D nodes, cameras, lighting, environment/post, `GridMap` |
| [`godot-resources`](skills/godot/godot-resources/SKILL.md) | Custom `Resource` classes, `.tres`, data-driven design |
| [`godot-audio`](skills/godot/godot-audio/SKILL.md) | `AudioStreamPlayer`, buses, effects, sync-to-beat |
| [`godot-multiplayer`](skills/godot/godot-multiplayer/SKILL.md) | High-level multiplayer: `MultiplayerAPI`, RPCs, spawner/sync |
| [`godot-export`](skills/godot/godot-export/SKILL.md) | Export presets/templates, platform builds, headless CLI export |
| [`godot-csharp`](skills/godot/godot-csharp/SKILL.md) | C#/.NET in Godot: bindings, signals as events, GDScript interop |

#### Unity — 8 ([`skills/unity/`](skills/unity/)) · Unity 6 (6000.0 LTS)

| Skill | Scope |
|-------|-------|
| [`unity-csharp-scripting`](skills/unity/unity-csharp-scripting/SKILL.md) | `MonoBehaviour` lifecycle, component model, coroutines, serialization |
| [`unity-input-system`](skills/unity/unity-input-system/SKILL.md) | New Input System: actions, bindings, action maps, devices |
| [`unity-physics`](skills/unity/unity-physics/SKILL.md) | Rigidbody, colliders, layers, joints |
| [`unity-animation`](skills/unity/unity-animation/SKILL.md) | Animator Controllers, state machines, blend trees, humanoid IK |
| [`unity-scriptableobjects`](skills/unity/unity-scriptableobjects/SKILL.md) | Data architecture with `ScriptableObject` |
| [`unity-tilemap-2d`](skills/unity/unity-tilemap-2d/SKILL.md) | 2D Tilemap/Tile Palette, rule tiles, tilemap colliders |
| [`unity-navmesh`](skills/unity/unity-navmesh/SKILL.md) | AI navigation: NavMesh bake, `NavMeshAgent` |
| [`unity-build-pipeline`](skills/unity/unity-build-pipeline/SKILL.md) | Build/player/quality settings, code stripping, Addressables |

#### Unreal — 6 ([`skills/unreal/`](skills/unreal/)) · Unreal Engine 5.4+

| Skill | Scope |
|-------|-------|
| [`unreal-blueprints`](skills/unreal/unreal-blueprints/SKILL.md) | Blueprint visual scripting: classes, graphs, comms, common nodes |
| [`unreal-cpp-gameplay`](skills/unreal/unreal-cpp-gameplay/SKILL.md) | C++ gameplay + Gameplay Framework (GameMode, Pawn, Controller) |
| [`unreal-enhanced-input`](skills/unreal/unreal-enhanced-input/SKILL.md) | Enhanced Input: Input Actions, Mapping Contexts, Modifiers, Triggers |
| [`unreal-behavior-trees`](skills/unreal/unreal-behavior-trees/SKILL.md) | AI Behavior Trees + Blackboard, tasks/decorators/services |
| [`unreal-niagara`](skills/unreal/unreal-niagara/SKILL.md) | Niagara VFX: systems, emitters, modules, parameters |
| [`unreal-packaging`](skills/unreal/unreal-packaging/SKILL.md) | Packaging/cooking projects, build configs, shipping builds |

#### Web engines — 6 ([`skills/web-engines/`](skills/web-engines/)) · Phaser 3 · PixiJS v8 · three.js r165+

| Skill | Scope |
|-------|-------|
| [`phaser-core`](skills/web-engines/phaser-core/SKILL.md) | Phaser 3 game config, Scene lifecycle, loader, cameras |
| [`phaser-arcade-physics`](skills/web-engines/phaser-arcade-physics/SKILL.md) | Arcade Physics: bodies, velocity, colliders/overlap, groups |
| [`pixijs-rendering`](skills/web-engines/pixijs-rendering/SKILL.md) | PixiJS v8 scene graph: `Application`, `Container`, `Sprite`, events |
| [`threejs-scene-setup`](skills/web-engines/threejs-scene-setup/SKILL.md) | Three.js scene/camera/renderer/animation-loop, resizing, controls |
| [`threejs-gltf-loading`](skills/web-engines/threejs-gltf-loading/SKILL.md) | Loading glTF/GLB + skinned animation (`GLTFLoader`/`AnimationMixer`) |
| [`threejs-materials-lighting`](skills/web-engines/threejs-materials-lighting/SKILL.md) | Materials (PBR), lights, shadows, environment maps |

#### Other engines — 5 ([`skills/other-engines/`](skills/other-engines/)) · Bevy · pygame · LÖVE · Roblox

| Skill | Scope |
|-------|-------|
| [`bevy-ecs`](skills/other-engines/bevy-ecs/SKILL.md) | Bevy app + ECS: components, systems, queries, resources, plugins (Bevy 0.16+) |
| [`pygame-core`](skills/other-engines/pygame-core/SKILL.md) | pygame loop, `Surface`/`Rect`, sprites/groups, events (pygame 2.6) |
| [`love2d-core`](skills/other-engines/love2d-core/SKILL.md) | LÖVE `load/update/draw` loop, dt-driven motion, input, states (LÖVE 11.5) |
| [`roblox-luau`](skills/other-engines/roblox-luau/SKILL.md) | Roblox Luau scripting: services, instances, client/server model |
| [`roblox-datastores`](skills/other-engines/roblox-datastores/SKILL.md) | Persistent data with `DataStoreService`: sessions, limits, ordered stores |

### Disciplines — 9 ([`skills/disciplines/`](skills/disciplines/))

Cross-engine concepts that load alongside the detected engine skill.

| Skill | Scope |
|-------|-------|
| [`game-ai`](skills/disciplines/game-ai/SKILL.md) | NPC decision-making: FSMs, behavior trees, steering, pathfinding |
| [`procedural-gen`](skills/disciplines/procedural-gen/SKILL.md) | Noise, RNG, seeds, grid/dungeon/terrain generation |
| [`dialogue-systems`](skills/disciplines/dialogue-systems/SKILL.md) | Branching dialogue/narrative: nodes, conditions, variables (Yarn/Ink) |
| [`save-systems`](skills/disciplines/save-systems/SKILL.md) | Serialize/restore game state: formats, slots, versioning, autosave |
| [`audio-design`](skills/disciplines/audio-design/SKILL.md) | Buses/mixing, adaptive music, SFX, ducking |
| [`shader-programming`](skills/disciplines/shader-programming/SKILL.md) | Cross-engine shader concepts: vertex/fragment, UVs, common effects |
| [`physics-tuning`](skills/disciplines/physics-tuning/SKILL.md) | Tuning feel: fixed timestep, mass/drag, CCD, stability, layers |
| [`level-design`](skills/disciplines/level-design/SKILL.md) | Whitebox/blockout structure, tile/grid layout, pacing |
| [`input-systems`](skills/disciplines/input-systems/SKILL.md) | Input architecture: action mapping, rebinding, multi-device, buffering |

### Genres — 9 ([`skills/genres/`](skills/genres/))

Compositional templates that orchestrate engine + discipline skills; they never re-teach
primitives.

| Skill | Scope |
|-------|-------|
| [`platformer`](skills/genres/platformer/SKILL.md) | 2D platformer: run/jump/coyote-time, tiled levels, hazards, goals |
| [`roguelike`](skills/genres/roguelike/SKILL.md) | Grid/turn movement, procedural dungeons, permadeath, FOV, loot |
| [`rpg`](skills/genres/rpg/SKILL.md) | Stats/leveling, inventory, quests, dialogue, save/load, combat |
| [`fps-shooter`](skills/genres/fps-shooter/SKILL.md) | First-person controller, camera, shooting/hitscan, enemy AI |
| [`tower-defense`](skills/genres/tower-defense/SKILL.md) | Wave spawning, lane pathing, towers/targeting, economy |
| [`card-game`](skills/genres/card-game/SKILL.md) | Card data, deck/hand/discard zones, play rules, turn structure |
| [`visual-novel`](skills/genres/visual-novel/SKILL.md) | Branching script, character/bg display, text box + choices, backlog |
| [`survival-crafting`](skills/genres/survival-crafting/SKILL.md) | Resource gathering, inventory, crafting, needs, base building |
| [`puzzle`](skills/genres/puzzle/SKILL.md) | Grid/board state, match/rule resolution, undo, level progression |

### Workflows — 4 ([`skills/workflows/`](skills/workflows/))

| Skill | Scope |
|-------|-------|
| [`game-jam`](skills/workflows/game-jam/SKILL.md) | Scope/plan/execute a jam build under time limits; submit |
| [`prototype-fast`](skills/workflows/prototype-fast/SKILL.md) | Greybox a playable vertical slice quickly; cut scope; validate the fun |
| [`steam-publish`](skills/workflows/steam-publish/SKILL.md) | Steamworks onboarding: app/depots, store page, builds, release checklist |
| [`itch-publish`](skills/workflows/itch-publish/SKILL.md) | Publish/update on itch.io: project page, channels, `butler` uploads |

## Agent compatibility

The portable core of every skill is the `SKILL.md` open standard. Native agents drop the
folder in as-is; three rules-based editors are reached through generated adapters. Full
details — paths, triggers, adapter mapping — are in
[`docs/COMPATIBILITY.md`](docs/COMPATIBILITY.md).

| Agent | Native skills? | Install path | Adapter needed? |
|-------|:--------------:|--------------|:---------------:|
| Claude Code | ✅ | `.claude/skills/<name>/` | ❌ |
| Claude Agent SDK | ✅ | `.claude/skills/<name>/` | ❌ |
| Claude.ai | ✅ (zip upload) | per-account upload | ❌ |
| Kiro | ✅ | `.kiro/skills/<name>/` | ❌ |
| Gemini CLI | ✅ | `.agents/skills/<name>/` | ❌ |
| Codex CLI | ✅ | `.agents/skills/<name>/` | ❌ |
| Cursor | ❌ | `.cursor/rules/<name>.mdc` | ✅ generate |
| Windsurf | ❌ | `.windsurf/rules/<name>.md` | ✅ generate |
| Cline | ❌ | `.clinerules/<name>.md` | ✅ generate |

The one directory Gemini CLI and Codex CLI both read is `.agents/skills/<name>/SKILL.md` —
use it as the universal CLI location.

## Demo

<!-- TODO: record a short terminal capture of the router resolving a prompt and replace the
     line below with: ![router demo](docs/assets/router-demo.gif)
     Suggested clip: in a Godot project, prompt "add a double jump to my player" and show the
     router selecting godot-2d-movement + platformer before editing code. Keep it under ~15s. -->

_A short router demo GIF will live here. To record one: open a project an agent can see, ask a
plain-language game-dev question, and capture the router announcing which skills it loads and
why._

## Repository layout

```
skills/        62 specialized skills, grouped by engine / discipline / genre / workflow
router/        the master router skill (+ ROUTER-SPEC.md and references/)
docs/          authoring standard, installation, compatibility, catalog
templates/     SKILL.md template
scripts/       validate-skills.py and tooling
```

## Contributing

Contributions are welcome. Read [`CONTRIBUTING.md`](CONTRIBUTING.md) and the authoring standard
in [`docs/SKILL-FORMAT.md`](docs/SKILL-FORMAT.md). Every skill must pass the validator and the
rubric before merge:

```bash
python scripts/validate-skills.py
```

## License

[Apache-2.0](LICENSE). See [`NOTICE`](NOTICE) for attribution and trademark notes.
