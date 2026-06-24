# Skill Catalog (v1 — frozen)

This is the **frozen v1 catalog**: the authoritative list of every skill in the collection,
with its target version, primary documentation, router signals, and build status. It is the
file the authoring sessions (S06–S11) build against and the router session (S12) routes from.

> **Status: FROZEN at S05.** The names, categories, and counts below are locked for v1. New
> skills arrive as post-v1 contributions, not by editing this list mid-wave. The authoring
> standard is [`SKILL-FORMAT.md`](SKILL-FORMAT.md); where skills install is
> [`COMPATIBILITY.md`](COMPATIBILITY.md); the routing design is
> [`../router/ROUTER-SPEC.md`](../router/ROUTER-SPEC.md).

## How authoring sessions use this file

1. Find your category's table. Each row is one skill you will author as
   `skills/<category>/<name>/SKILL.md`.
2. The **Primary sources** are your starting point. **Re-verify the exact page** (Firecrawl)
   and **pin code to the listed version** before writing — docs move.
3. Put the **Router signals** to work in the `description` (the *says:* phrases are the trigger
   vocabulary; the *files:* patterns tell the router when your skill is relevant).
4. Skills marked **`golden-done`** already exist — skip them.

### Column legend

- **Skill** — the final `name`: lowercase-hyphen, ≤64 chars, equals the folder name, unique repo-wide.
- **Scope** — the single responsibility. One skill, one job.
- **Version** — the engine/runtime the patterns target. Pin code to this.
- **Diff** — difficulty: `beginner` / `intermediate` / `advanced`.
- **Primary sources** — official docs (paths are relative to each category's **doc root**,
  given above its table). `†` = cited by a confirmed slug pattern, not individually fetched —
  re-verify before use. `‡` = nearest authoritative anchor for that skill's topic.
- **Router signals** — `files:` project-file/extension detection · `says:` user-phrase triggers
  (and, for genres, `composes:` the skills it orchestrates).
- **Status** — `done` (authored & passing the validator) · `golden-done` (golden reference skill).
  All 62 v1 skills are authored as of S12; the master router (`router/SKILL.md`) routes them.

## Counts (v1 = 62 skills)

| Category | Count | Owning session |
|----------|:-----:|----------------|
| [godot](#godot-15) | 15 | S06 |
| [unity](#unity-8) | 8 | S07 |
| [unreal](#unreal-6) | 6 | S07 |
| [web-engines](#web-engines-6) | 6 | S08 |
| [other-engines](#other-engines-5) | 5 | S08 |
| [disciplines](#disciplines-9) | 9 | S09 |
| [genres](#genres-9) | 9 | S10 |
| [workflows](#workflows-4) | 4 | S11 |
| **Total** | **62** | |

---

## godot (15)

Target **Godot 4.x stable** (pin code to 4.3 / 4.4). Doc root: `https://docs.godotengine.org/en/stable/`

| Skill | Scope | Version | Diff | Primary sources (paths under doc root) | Router signals | Status |
|-------|-------|---------|------|------------------------------------------|----------------|--------|
| `godot-gdscript` | GDScript language: typing, lifecycle, `@export`, `await`/signals, idioms. | Godot 4.x | beginner | `/tutorials/scripting/gdscript/gdscript_basics.html` · `/getting_started/step_by_step/scripting_first_script.html` · `/tutorials/best_practices/index.html`† | files: `*.gd`, `project.godot` · says: "GDScript", "Godot script", "_ready/_process" | done |
| `godot-nodes-scenes` | Scene tree, node composition, instancing, autoloads, `PackedScene`. | Godot 4.x | beginner | `/getting_started/step_by_step/nodes_and_scenes.html` · `/getting_started/step_by_step/instancing.html` · `/classes/class_packedscene.html` · `/getting_started/introduction/key_concepts_overview.html` | files: `*.tscn`, `project.godot` · says: "scene", "node tree", "instance a scene" | done |
| `godot-signals-groups` | Event-driven design with signals + groups (decoupling, observer). | Godot 4.x | beginner | `/getting_started/step_by_step/signals.html` · `/classes/class_signal.html` · `/tutorials/scripting/groups.html` | files: `*.gd`, `*.tscn` · says: "signal", "emit", "connect", "groups" | done |
| `godot-2d-movement` | `CharacterBody2D` kinematic movement, `move_and_slide`, slopes, collisions. | Godot 4.x | intermediate | `/classes/class_node2d.html` · `/tutorials/physics/physics_introduction.html` · `/getting_started/first_2d_game/03.coding_the_player.html` | files: `*.tscn` w/ CharacterBody2D · says: "2D movement", "platformer controller", "move_and_slide" | done |
| `godot-tilemap` | `TileMapLayer`/`TileSet`: autotiling, terrain, collision & navigation layers, runtime edits. | Godot 4.3+ | intermediate | `/classes/class_tilemaplayer.html` · `/tutorials/2d/using_tilemaps.html` · `/tutorials/2d/using_tilesets.html` | files: `*.tscn` w/ TileMapLayer, `*.tres` tileset · says: "tilemap", "tileset", "autotile" | done |
| `godot-physics` | `RigidBody`/`Area`/`StaticBody` (2D+3D), collision layers/masks, raycasts. | Godot 4.x | intermediate | `/tutorials/physics/physics_introduction.html` · `/tutorials/physics/ray-casting.html` | files: `*.tscn` w/ physics bodies · says: "rigidbody", "collision layer", "raycast", "area2d" | done |
| `godot-ui-control` | `Control` nodes: anchors, containers, themes, focus/keyboard nav. | Godot 4.x | intermediate | `/classes/class_control.html` · `/tutorials/ui/size_and_anchors.html` · `/tutorials/ui/gui_navigation.html` | files: `*.tscn` w/ Control · says: "UI", "Control node", "anchors", "theme", "HUD" | done |
| `godot-animation` | `AnimationPlayer`, `AnimationTree` state machines/blending, `Tween`. | Godot 4.x | intermediate | `/tutorials/animation/introduction.html` · `/tutorials/animation/animation_tree.html` · `/classes/class_animationplayer.html` · `/tutorials/2d/2d_sprite_animation.html` | files: `*.tscn` w/ AnimationPlayer · says: "AnimationPlayer", "AnimationTree", "blend", "Tween" | done |
| `godot-shaders` | Godot shading language: `canvas_item` (2D) + `spatial` (3D) shaders, screen-reading. | Godot 4.x | advanced | `/tutorials/shaders/your_first_shader/your_first_3d_shader.html` · `/tutorials/shaders/screen-reading_shaders.html` | files: `*.gdshader`, `*.tres` shader · says: "Godot shader", "gdshader", "fragment/vertex" | done |
| `godot-3d-essentials` | 3D nodes, cameras, lighting, environment/post, `GridMap`. | Godot 4.x | intermediate | `/tutorials/3d/introduction_to_3d.html` · `/tutorials/3d/using_gridmaps.html` · `/tutorials/3d/environment_and_post_processing.html` | files: `*.tscn` w/ Node3D · says: "3D", "camera3d", "WorldEnvironment", "gridmap" | done |
| `godot-resources` | Custom `Resource` classes, `.tres`, data-driven design, `ResourceLoader`. | Godot 4.x | intermediate | `/tutorials/scripting/resources.html` · `/classes/class_resource.html` | files: `*.tres`, `*.res` · says: "custom Resource", "data resource", ".tres", "ResourceLoader" | done |
| `godot-audio` | `AudioStreamPlayer`, buses, effects, music/SFX, sync-to-beat. | Godot 4.x | intermediate | `/tutorials/audio/audio_buses.html` · `/tutorials/audio/audio_streams.html` · `/tutorials/audio/sync_with_audio.html` | files: `*.tscn` w/ AudioStreamPlayer · says: "Godot audio", "audio bus", "play sound/music" | done |
| `godot-multiplayer` | High-level multiplayer: `MultiplayerAPI`, RPCs, `MultiplayerSpawner`/`Synchronizer`. | Godot 4.x | advanced | `/tutorials/networking/high_level_multiplayer.html` · `/tutorials/networking/webrtc.html` | files: `*.gd` w/ `@rpc` · says: "multiplayer", "RPC", "networked", "MultiplayerSpawner" | done |
| `godot-export` | Export presets/templates, platform builds, headless/CLI export. | Godot 4.x | intermediate | `/tutorials/export/exporting_for_web.html` · `/tutorials/editor/command_line_tutorial.html` | files: `export_presets.cfg`, `project.godot` · says: "export Godot", "build for web/windows", "export template" | done |
| `godot-csharp` | Using C#/.NET in Godot: bindings, signals as events, GDScript interop. | Godot 4.x (.NET) | intermediate | `/tutorials/scripting/c_sharp/index.html` · `/tutorials/scripting/c_sharp/c_sharp_basics.html` · `/tutorials/scripting/c_sharp/c_sharp_signals.html` | files: `*.cs` + `*.csproj` in Godot proj · says: "Godot C#", "GodotSharp", "C# signals in Godot" | done |

---

## unity (8)

Target **Unity 6 (6000.0 LTS)** (also valid 2022 LTS). Doc root: `https://docs.unity3d.com/`
(Manual at `/Manual/`, API at `/ScriptReference/`).

| Skill | Scope | Version | Diff | Primary sources | Router signals | Status |
|-------|-------|---------|------|-----------------|----------------|--------|
| `unity-csharp-scripting` | `MonoBehaviour` lifecycle, GameObject/component model, coroutines, serialization. | Unity 6 LTS | beginner | `/Manual/index.html` · `/Manual/class-ScriptableObject.html`‡ · `/ScriptReference/MonoBehaviour.html`† | files: `*.cs` + `*.asmdef`, `ProjectSettings/` · says: "Unity script", "MonoBehaviour", "Start/Update" | done |
| `unity-input-system` | New Input System package: actions, bindings, action maps, devices. | Unity 6 (Input System 1.x) | intermediate | `/Manual/com.unity.inputsystem.html` · `/Manual/class-InputManager.html` | files: `*.inputactions`, `Packages/manifest.json` w/ inputsystem · says: "Unity Input System", "InputAction", "action map" | done |
| `unity-physics` | Rigidbody, colliders, layers, joints, `LayerBasedCollision`. | Unity 6 LTS | intermediate | `/Manual/PhysicsOverview.html` · `/Manual/PhysicsSection.html` · `/Manual/LayerBasedCollision.html` · `/Manual/class-HingeJoint.html` | files: `*.unity`, `*.prefab` w/ Rigidbody · says: "Unity physics", "Rigidbody", "collision layers", "joint" | done |
| `unity-animation` | Animator Controllers, state machines, blend trees, humanoid Avatar/IK. | Unity 6 LTS | intermediate | `/Manual/AnimationSection.html` · `/Manual/AnimationStateMachines.html` · `/Manual/InverseKinematics.html` · `/Manual/class-Avatar.html` | files: `*.controller`, `*.anim` · says: "Animator", "state machine", "blend tree", "Mecanim" | done |
| `unity-scriptableobjects` | Data architecture with `ScriptableObject` (config, event channels, registries). | Unity 6 LTS | intermediate | `/Manual/class-ScriptableObject.html` · `/ScriptReference/ScriptableObject.html`† | files: `*.asset`, `*.cs` w/ `: ScriptableObject` · says: "ScriptableObject", "SO architecture", "data asset" | done |
| `unity-tilemap-2d` | 2D Tilemap/Tile Palette, rule tiles, tilemap colliders. | Unity 6 LTS | intermediate | `/Manual/class-Tilemap.html` · `/Manual/PrefabInstanceOverrides.html`‡ | files: `*.unity` w/ Tilemap, `*.asset` tile · says: "Unity tilemap", "tile palette", "rule tile" | done |
| `unity-navmesh` | AI navigation: NavMesh bake, `NavMeshAgent`, AI Navigation package. | Unity 6 LTS | intermediate | `/Manual/nav-NavigationSystem.html` · `/Manual/class-CharacterController.html` | files: `*.unity` w/ NavMesh, `NavMesh.asset` · says: "NavMesh", "NavMeshAgent", "pathfinding Unity" | done |
| `unity-build-pipeline` | Build settings, players, quality settings, code stripping, Addressables pointer. | Unity 6 LTS | intermediate | `/Manual/WindowsStandaloneBinaries.html` · `/Manual/android-BuildProcess.html` · `/Manual/class-QualitySettings.html` · `/Manual/managed-code-stripping.html` | files: `ProjectSettings/EditorBuildSettings.asset` · says: "build Unity", "player settings", "IL2CPP", "Addressables" | done |

---

## unreal (6)

Target **Unreal Engine 5.4+** (docs verified at UE 5.8). Doc root:
`https://dev.epicgames.com/documentation/en-us/unreal-engine/`

| Skill | Scope | Version | Diff | Primary sources (slugs under doc root) | Router signals | Status |
|-------|-------|---------|------|-----------------------------------------|----------------|--------|
| `unreal-blueprints` | Blueprint visual scripting: classes, graphs, variables, comms, common nodes. | UE 5.4+ | beginner | `/overview-of-blueprints-visual-scripting-in-unreal-engine` · `/BlueprintAPI` | files: `*.uasset` (Blueprint), `*.uproject` · says: "Blueprint", "BP graph", "event graph" | done |
| `unreal-cpp-gameplay` | C++ gameplay + Gameplay Framework (GameMode, Pawn, Controller, Actor/components). | UE 5.4+ | advanced | `/unreal-engine-cpp-quick-start` · `/gameplay-framework-in-unreal-engine` | files: `Source/**/*.cpp`/`*.h` w/ `UCLASS`, `*.Build.cs` · says: "Unreal C++", "GameMode", "Pawn", "ACharacter" | done |
| `unreal-enhanced-input` | Enhanced Input: Input Actions, Mapping Contexts, Modifiers, Triggers. | UE 5.4+ | intermediate | `/enhanced-input-in-unreal-engine` | files: `*.uasset` (IA_/IMC_), `*.uproject` · says: "Enhanced Input", "Input Mapping Context", "IA_/IMC_" | done |
| `unreal-behavior-trees` | AI Behavior Trees + Blackboard, tasks/decorators/services, AIController. | UE 5.4+ | advanced | `/behavior-trees-in-unreal-engine` | files: `*.uasset` (BT_/BB_) · says: "Behavior Tree", "Blackboard", "AIController", "BTTask" | done |
| `unreal-niagara` | Niagara VFX: systems, emitters, modules, parameters. | UE 5.4+ | advanced | `/overview-of-niagara-effects-for-unreal-engine` | files: `*.uasset` (NS_/NE_) · says: "Niagara", "VFX", "particle system Unreal" | done |
| `unreal-packaging` | Packaging/cooking projects, build configs, shipping builds per platform. | UE 5.4+ | intermediate | `/packaging-your-project` | files: `*.uproject`, `Config/Default*.ini` · says: "package Unreal", "cook content", "shipping build" | done |

---

## web-engines (6)

Phaser, PixiJS, Three.js. Detected primarily from `package.json` dependencies + imports.

| Skill | Scope | Version | Diff | Primary sources | Router signals | Status |
|-------|-------|---------|------|-----------------|----------------|--------|
| `phaser-core` | Phaser 3 game config, Scene lifecycle, loader, cameras, cross-scene comms. | Phaser 3.90 | beginner | `https://docs.phaser.io/phaser/getting-started/making-your-first-phaser-game` · `https://docs.phaser.io/phaser/concepts/scenes` · `https://docs.phaser.io/api-documentation/class/scene` | files: `package.json` dep `phaser`, `import Phaser` · says: "Phaser", "Phaser scene", "preload/create/update" | done |
| `phaser-arcade-physics` | Arcade Physics: bodies, velocity, colliders/overlap, groups, world bounds. | Phaser 3.90 | intermediate | `https://docs.phaser.io/phaser/concepts/physics/arcade` · `https://docs.phaser.io/phaser/concepts/physics` · `https://docs.phaser.io/api-documentation/class/physics-arcade-sprite` | files: `package.json` dep `phaser` · says: "Arcade Physics", "collider", "Phaser physics", "overlap" | done |
| `pixijs-rendering` | PixiJS v8 scene graph: `Application`, `Container`, `Sprite`, events, render groups. | PixiJS v8 | intermediate | `https://pixijs.com/8.x/guides/getting-started/quick-start` · `https://pixijs.com/8.x/guides/concepts/scene-graph` · `https://pixijs.com/8.x/guides/components/scene-objects/sprite` · `https://pixijs.com/8.x/guides/components/events` | files: `package.json` dep `pixi.js`, `import * as PIXI` · says: "PixiJS", "Pixi sprite", "stage", "Container" | done |
| `threejs-scene-setup` | Three.js scene/camera/renderer/animation-loop, resizing, controls. | three.js r165+ | beginner | `https://threejs.org/manual/#en/fundamentals`† · `https://threejs.org/docs/#manual/en/introduction/Creating-a-scene`† | files: `package.json` dep `three`, `import * as THREE` · says: "three.js", "THREE.Scene", "WebGLRenderer", "render loop" | done |
| `threejs-gltf-loading` | Loading glTF/GLB models + skinned animations with `GLTFLoader`/`AnimationMixer`. | three.js r165+ | intermediate | `https://threejs.org/manual/#en/load-gltf`† · `https://threejs.org/docs/#examples/en/loaders/GLTFLoader`† | files: `*.gltf`/`*.glb` + `three` dep · says: "load gltf", "GLTFLoader", "AnimationMixer", "import 3D model" | done |
| `threejs-materials-lighting` | Materials (PBR/standard), lights, shadows, environment maps. | three.js r165+ | intermediate | `https://threejs.org/manual/#en/materials`† · `https://threejs.org/manual/#en/lights`† · `https://threejs.org/docs/`† | files: `three` dep · says: "three.js material", "MeshStandardMaterial", "lighting", "shadows" | done |

> Three.js docs/manual are single-page apps using `#en/...` hash routes; the `threejs.org/manual`
> and `threejs.org/docs` roots are verified — resolve the exact section before writing.

---

## other-engines (5)

Bevy, pygame, LÖVE, Roblox.

| Skill | Scope | Version | Diff | Primary sources | Router signals | Status |
|-------|-------|---------|------|-----------------|----------------|--------|
| `bevy-ecs` | Bevy app + ECS: components, systems, queries, resources, schedules, plugins. | Bevy 0.16+ (pin hard) | advanced | `https://bevy.org/learn/quick-start/getting-started` · `https://bevy.org/learn/quick-start/introduction` · `https://docs.rs/bevy/latest/bevy/`† | files: `Cargo.toml` dep `bevy`, `*.rs` w/ `App::new()` · says: "Bevy", "ECS system/query", "Commands", "Bevy plugin" | done |
| `pygame-core` | pygame game loop, `Surface`/`Rect`, sprites/groups, event + input handling. | pygame 2.6 / pygame-ce 2.5 | beginner | `https://www.pygame.org/docs/` · `https://www.pygame.org/docs/ref/sprite.html` · `https://www.pygame.org/docs/ref/surface.html` · `https://www.pygame.org/docs/tut/newbieguide.html` | files: `*.py` w/ `import pygame` · says: "pygame", "blit", "sprite group", "pygame loop" | done |
| `love2d-core` | LÖVE `love.load/update/draw` loop, dt-driven motion, `love.graphics`, input, states. | LÖVE 11.5 | beginner | `https://love2d.org/wiki/love` · `https://love2d.org/wiki/love.graphics`† · `https://love2d.org/wiki/Config_Files`† | files: `main.lua` w/ `love.*`, `conf.lua`, `*.love` · says: "LÖVE", "love2d", "love.draw", "love.update(dt)" | **golden-done** |
| `roblox-luau` | Roblox Luau scripting: services, instances, events, client/server model, Studio. | Roblox (current) / Luau | beginner | `https://create.roblox.com/docs` · `https://create.roblox.com/docs/reference/engine` | files: `*.luau`/`*.lua` + `*.rbxl(x)`, `*.project.json` (Rojo) · says: "Roblox", "Luau", "RemoteEvent", "Roblox service" | done |
| `roblox-datastores` | Persistent data with DataStoreService: get/set, sessions, limits, ordered stores. | Roblox (current) | intermediate | `https://create.roblox.com/docs/cloud-services/data-stores` · `https://create.roblox.com/docs/reference/engine`† | files: `*.luau`/`*.lua` w/ `DataStoreService` · says: "DataStore", "save player data Roblox", "GetDataStore" | done |

---

## disciplines (9)

Cross-engine concepts. These teach the *concept/algorithm* and route alongside the detected
engine skill; their sources are the authoritative engine pages that implement the concept, so
examples stay primary-sourced.

| Skill | Scope | Version basis | Diff | Primary sources | Router signals | Status |
|-------|-------|---------------|------|-----------------|----------------|--------|
| `game-ai` | NPC decision-making: FSMs, behavior trees, steering, pathfinding integration. | engine-agnostic | advanced | Unreal `/behavior-trees-in-unreal-engine` · Unity `/Manual/nav-NavigationSystem.html` · Godot `/tutorials/navigation/navigation_introduction_2d.html` | files: any engine + AI assets · says: "enemy AI", "behavior tree", "state machine", "steering", "pathfinding" | done |
| `procedural-gen` | Procedural content: noise, RNG, seeds, grid/dungeon/terrain generation. | engine-agnostic | advanced | Godot `/tutorials/math/random_number_generation.html` · Godot `/tutorials/2d/using_tilemaps.html` · Unity `/Manual/terrain-SetHeight.html` | files: any engine · says: "procedural generation", "perlin/simplex noise", "random seed", "dungeon generator" | done |
| `dialogue-systems` | Branching dialogue/narrative: nodes, conditions, variables, localization hooks. | Yarn Spinner / Ink | intermediate | `https://docs.yarnspinner.dev/` · `https://www.inklestudios.com/ink/` · Godot `/tutorials/scripting/resources.html` | files: `*.yarn`, `*.ink` · says: "dialogue system", "branching dialogue", "Yarn Spinner", "Ink", "conversation tree" | done |
| `save-systems` | Serialize/restore game state: formats, slots, versioning/migration, autosave. | engine-agnostic | intermediate | Godot `/tutorials/io/saving_games.html` · Godot `/tutorials/io/background_loading.html` · Roblox `/docs/cloud-services/data-stores` | files: `save*.json`/`*.sav`, save dirs · says: "save system", "save/load game", "persistence", "save slots" | done |
| `audio-design` | Game audio practice: buses/mixing, adaptive/dynamic music, SFX, ducking. | engine-agnostic | intermediate | Godot `/tutorials/audio/audio_buses.html` · Unity `/Manual/AudioMixer.html` · Godot `/tutorials/audio/sync_with_audio.html` | files: audio assets + mixer files · says: "audio design", "adaptive music", "audio mixer", "ducking", "SFX" | done |
| `shader-programming` | Shader concepts cross-engine: vertex/fragment, UVs, common 2D/3D effects. | engine-agnostic (GLSL/HLSL) | advanced | Godot `/tutorials/shaders/your_first_shader/your_first_3d_shader.html` · Godot `/tutorials/shaders/screen-reading_shaders.html` · Unity material docs (re-verify) | files: `*.gdshader`, `*.shader`, `*.hlsl`, `*.glsl` · says: "shader", "fragment shader", "dissolve/outline effect", "GLSL/HLSL" | done |
| `physics-tuning` | Tuning physics feel: fixed timestep, mass/drag, CCD, stability, layers. | engine-agnostic | intermediate | Godot `/tutorials/physics/physics_introduction.html` · Unity `/Manual/PhysicsOverview.html` · Unity `/Manual/LayerBasedCollision.html` | files: physics settings/config · says: "physics feel", "jitter/tunneling", "fixed timestep", "collision tuning" | done |
| `level-design` | Level/whitebox structure, tile/grid layout, pacing, blockout to playable. | engine-agnostic | intermediate | Godot `/tutorials/2d/using_tilemaps.html` · Godot `/tutorials/3d/using_gridmaps.html` · Unity `/Manual/class-Tilemap.html` | files: level/scene/map files · says: "level design", "whitebox/blockout", "tilemap layout", "level pacing" | done |
| `input-systems` | Input architecture: action mapping, rebinding, multi-device, buffering. | engine-agnostic | intermediate | Unity `/Manual/com.unity.inputsystem.html` · Unreal `/enhanced-input-in-unreal-engine` · Godot `/tutorials/inputs/input_examples.html` | files: `*.inputactions`, IMC assets, InputMap in `project.godot` · says: "input mapping", "rebind controls", "gamepad support", "input buffering" | done |

---

## genres (9)

**Compositional**: a genre skill orchestrates engine + discipline skills into a working
template (loop, glue, structure) and never re-teaches primitives. The `composes:` field lists
the skills it links out to.

| Skill | Scope | Version basis | Diff | Primary sources | Router signals · composes | Status |
|-------|-------|---------------|------|-----------------|---------------------------|--------|
| `platformer` | 2D platformer: run/jump/coyote-time, tiled levels, hazards, goals. | engine-agnostic | intermediate | Godot `/getting_started/first_2d_game/index.html` · Godot `/getting_started/first_2d_game/03.coding_the_player.html` · Godot `/tutorials/2d/using_tilemaps.html` | says: "platformer", "jump mechanic", "Mario-like" · composes: `*-2d-movement`, `godot-tilemap`/`unity-tilemap-2d`, `level-design` | done |
| `roguelike` | Grid/turn movement, procedural dungeons, permadeath, FOV, loot tables. | engine-agnostic | advanced | Godot `/tutorials/math/random_number_generation.html` · Godot `/tutorials/2d/using_tilemaps.html` | says: "roguelike", "procedural dungeon", "permadeath", "turn-based grid" · composes: `procedural-gen`, `*-tilemap` | done |
| `rpg` | Stats/leveling, inventory, quests, dialogue, save/load, turn or action combat. | engine-agnostic | advanced | Godot `/tutorials/scripting/resources.html` · `https://docs.yarnspinner.dev/` · Godot `/tutorials/io/saving_games.html` | says: "RPG", "stats/leveling", "inventory", "quest system" · composes: `dialogue-systems`, `save-systems`, `godot-resources`/`unity-scriptableobjects` | done |
| `fps-shooter` | First-person controller, camera, shooting/hitscan, basic enemy AI. | engine-agnostic | advanced | Unreal `/code-a-firstperson-adventure-game-in-unreal-engine` · Godot `/getting_started/first_3d_game/index.html` · Unity `/Manual/class-CharacterController.html` | says: "FPS", "first-person shooter", "hitscan", "aim/shoot" · composes: `*-3d`/`unreal-cpp-gameplay`, `input-systems`, `game-ai` | done |
| `tower-defense` | Wave spawning, pathing along lanes, towers/targeting, economy. | engine-agnostic | intermediate | Godot `/tutorials/navigation/navigation_introduction_2d.html` · Godot `/getting_started/first_2d_game/05.the_main_game_scene.html` | says: "tower defense", "wave spawner", "enemy path", "tower targeting" · composes: `game-ai`, `*-2d-movement` | done |
| `card-game` | Card data, deck/hand/discard zones, draw/play rules, turn structure, UI. | engine-agnostic | intermediate | Godot `/tutorials/scripting/resources.html` · Godot `/classes/class_control.html` | says: "card game", "deckbuilder", "hand/deck/discard", "TCG" · composes: `godot-resources`/`unity-scriptableobjects`, `godot-ui-control` | done |
| `visual-novel` | Branching script, character/bg display, text box + choices, save/log/backlog. | engine-agnostic | beginner | `https://www.inklestudios.com/ink/` · `https://docs.yarnspinner.dev/` · Godot `/classes/class_control.html` | files: `*.ink`, `*.yarn` · says: "visual novel", "VN", "branching story", "choice menu" · composes: `dialogue-systems`, `save-systems`, UI | done |
| `survival-crafting` | Resource gathering, inventory, crafting recipes, hunger/needs, base building. | engine-agnostic | advanced | Godot `/tutorials/scripting/resources.html` · Godot `/tutorials/io/saving_games.html` | says: "survival game", "crafting system", "inventory", "resource gathering" · composes: `save-systems`, `godot-resources`/`unity-scriptableobjects` | done |
| `puzzle` | Grid/board state, match/rule resolution, undo, level progression. | engine-agnostic | intermediate | Godot `/tutorials/2d/using_tilemaps.html` · Godot `/classes/class_control.html` | says: "puzzle game", "match-3", "grid logic", "tile-matching" · composes: `*-tilemap`, `level-design` | done |

---

## workflows (4)

Process skills; platform/tooling docs are the primary sources.

| Skill | Scope | Version basis | Diff | Primary sources | Router signals | Status |
|-------|-------|---------------|------|-----------------|----------------|--------|
| `game-jam` | Scope/plan/execute a jam build under time limits; submit to a jam. | itch.io jams | beginner | `https://itch.io/docs/creators/getting-started` · Godot `/getting_started/introduction/introduction_to_godot.html`‡ | says: "game jam", "48-hour game", "Ludum Dare/GMTK", "jam submission" | done |
| `prototype-fast` | Greybox a playable vertical slice quickly; cut scope; validate the fun. | engine-agnostic | beginner | Godot `/getting_started/first_2d_game/index.html` · `https://pixijs.com/8.x/guides/getting-started/quick-start`‡ | says: "prototype fast", "vertical slice", "MVP game", "greybox" | done |
| `steam-publish` | Steamworks onboarding: app/depots, store page, builds, release checklist. | Steamworks (current) | intermediate | `https://partner.steamgames.com/doc/home` · `https://partner.steamgames.com/doc/store`† · `https://partner.steamgames.com/doc/sdk`† | files: `steam_appid.txt`, Steamworks SDK · says: "publish on Steam", "Steamworks", "store page", "depot/build upload" | done |
| `itch-publish` | Publish/update on itch.io: project page, channels, `butler` CLI uploads. | itch.io / butler | beginner | `https://itch.io/docs/creators/getting-started` · `https://itch.io/docs/butler` | files: `.itch.toml`, butler usage · says: "publish on itch", "itch.io page", "butler push", "upload build itch" | done |

---

## Composition & single-responsibility rules

The router (S12) composes skills; each skill owns exactly one responsibility.

1. **Engine API vs cross-engine concept.** Engine skills teach *the engine's API* for a
   capability; `disciplines/*` teach the *portable concept/algorithm* and defer to the detected
   engine skill for syntax. Pairings:
   - `godot-shaders` / Unity-Unreal material docs ↔ `shader-programming`
   - `godot-audio` ↔ `audio-design` · `godot-physics`/`unity-physics` ↔ `physics-tuning`
   - `unity-input-system`/`unreal-enhanced-input`/Godot InputMap ↔ `input-systems`
   - `unity-navmesh`/`unreal-behavior-trees`/Godot navigation ↔ `game-ai`
   - `godot-tilemap`/`unity-tilemap-2d` ↔ `level-design`
   - `roblox-datastores`/Godot saving ↔ `save-systems`
2. **Genres never re-teach primitives.** A genre skill orchestrates engine + discipline skills
   into a working template and links out (see each genre's `composes:`).
3. **Engine selection is exclusive; concepts/genres are additive.** The router picks **one**
   engine skill set from project-file detection, then layers the discipline/genre/workflow
   skills the user's phrasing triggers.
4. **De-duplication decisions (locked):**
   - No separate `godot-save-load` — covered by `save-systems` (which cites Godot saving docs).
   - `unity-tilemap-2d` (engine tool) is kept separate from `level-design` (design practice).
   - `dialogue-systems` owns Yarn/Ink; `visual-novel` and `rpg` consume it.
   - `procedural-gen` owns noise/RNG; `roguelike` and `survival-crafting` consume it.

## Naming guarantees

All 62 names are unique repo-wide, lowercase-hyphen, ≤64 chars, match
`^[a-z0-9]+(-[a-z0-9]+)*$`, and equal their folder names. This list is the authority for the
exact folder name each authoring session must use. (Verified at S05 — see
`docs/progress/SESSION-05-standards-lock.md`.)
