# Master Router — Specification

This document specifies the **master router**: the entry-point skill that detects a project's
engine and the user's task, then dispatches to the right specialized skill(s) so the collection
"installs once and routes itself." The router implements this as `router/SKILL.md`; this
spec is its contract.

> **Status: FROZEN for v1.** The detection signals here are derived from the `files:` / `says:`
> columns in [`../docs/skill-catalog.md`](../docs/skill-catalog.md) — the catalog is the source
> of truth for per-skill triggers; this spec is the *algorithm* that uses them.

## What the router is (and is not)

- **Is:** a small, always-relevant skill whose description triggers on game-development requests.
  It fingerprints the workspace, classifies the task, names the minimal set of specialized
  skills to load, and tells the agent to read them. It is a *dispatcher and composer*.
- **Is not:** a place that re-teaches engine APIs. The router points; the specialized skills
  teach. It must stay lean (< 500 lines as a `SKILL.md`; push long tables into `references/`).

## The routing algorithm (overview)

```
1. DETECT ENGINE      project fingerprint → at most one engine skill set (or "unknown")
2. CLASSIFY TASK      user phrasing → discipline(s) + at most one genre + workflow(s)
3. RESOLVE SKILLS     engine skill(s) + discipline(s) + genre + workflow(s)  (the minimal set)
4. READ (disclosure)  open only the chosen SKILL.md bodies; references/ only when needed
5. COMPOSE            order: engine fundamentals → discipline concept → genre glue → workflow
6. FALLBACK           if engine unknown or no skill fits, see §7
```

---

## 1. Engine detection (project fingerprint)

Scan the workspace for the highest-confidence signal. Detection is **exclusive**: choose **one**
engine skill set. Use this precedence (stop at the first match):

| Priority | Engine | Primary signal (project files) | Secondary signals | Skill set root |
|:--------:|--------|--------------------------------|--------------------|----------------|
| 1 | **Godot** | `project.godot` | `*.tscn`, `*.gd`, `*.gdshader`, `export_presets.cfg` | `skills/godot/` |
| 2 | **Unreal** | `*.uproject` | `Source/**/*.Build.cs`, `Config/Default*.ini`, `*.uasset` | `skills/unreal/` |
| 3 | **Unity** | `Assets/` **and** `ProjectSettings/ProjectVersion.txt` | `*.unity`, `*.asmdef`, `Packages/manifest.json` | `skills/unity/` |
| 4 | **Bevy** | `Cargo.toml` with a `bevy` dependency | `*.rs` with `App::new()` / `bevy::prelude` | `skills/other-engines/bevy-ecs/` |
| 5 | **Phaser** | `package.json` dep `phaser` | `import Phaser` / `new Phaser.Game` | `skills/web-engines/phaser-*` |
| 6 | **PixiJS** | `package.json` dep `pixi.js` | `import * as PIXI` | `skills/web-engines/pixijs-rendering/` |
| 7 | **Three.js** | `package.json` dep `three` | `import * as THREE`, `*.gltf`/`*.glb` | `skills/web-engines/threejs-*` |
| 8 | **LÖVE** | `conf.lua` or `main.lua` calling `love.*` | `*.love` | `skills/other-engines/love2d-core/` |
| 9 | **pygame** | `*.py` with `import pygame` | `pygame.init()` | `skills/other-engines/pygame-core/` |
| 10 | **Roblox** | `*.rbxl` / `*.rbxlx`, or `*.project.json` (Rojo) | `*.luau`, `DataStoreService` | `skills/other-engines/roblox-*` |

### Disambiguation rules (must-handle ambiguities)

- **Godot C# vs Unity vs Bevy** — all may contain a `.csproj`/`Cargo.toml`. The presence of
  `project.godot` means **Godot** (route `godot-csharp` for C# work) even when `.csproj` exists;
  Unity requires the `Assets/` + `ProjectSettings/` pair; Bevy requires a `bevy` dep, not just
  any Rust crate.
- **Multiple web deps** — a `package.json` may list more than one of `phaser`/`pixi.js`/`three`.
  Prefer the library actually imported in the file under discussion; if unclear, ask which
  renderer the task targets.
- **Monorepo / multiple engines** — if more than one engine fingerprint is present, route by the
  file or subdirectory the request is about; if still ambiguous, ask one targeted question
  (§7).
- **No engine files at all** — go to the unknown-engine fallback (§7); many discipline/genre/
  workflow tasks need no engine.

---

## 2. Task classification (phrasing → category)

After the engine, read the request for task signals. Match against the `says:` vocabulary in the
catalog. Three additive categories:

- **disciplines** — cross-engine *concepts*: `game-ai`, `procedural-gen`, `dialogue-systems`,
  `save-systems`, `audio-design`, `shader-programming`, `physics-tuning`, `level-design`,
  `input-systems`. Triggered by concept words ("pathfinding", "save slots", "fragment shader").
- **genres** — *whole-game templates*: `platformer`, `roguelike`, `rpg`, `fps-shooter`,
  `tower-defense`, `card-game`, `visual-novel`, `survival-crafting`, `puzzle`. Triggered by
  genre words ("make a roguelike", "deckbuilder").
- **workflows** — *process/shipping*: `game-jam`, `prototype-fast`, `steam-publish`,
  `itch-publish`. Triggered by process words ("publish on Steam", "vertical slice").

Some tasks also carry **file signals** (`*.yarn`/`*.ink` → `dialogue-systems`/`visual-novel`;
`steam_appid.txt` → `steam-publish`). Use them to raise confidence.

---

## 3. Resolution: the task→category→skill routing table format

The router fills a table of this shape (one row per routing rule). It is a **lookup**, not prose:

| Task signal (`says:`/`files:`) | Needs engine? | Engine-skill slot | Discipline/genre/workflow skills | Notes |
|--------------------------------|:-------------:|-------------------|----------------------------------|-------|
| "platformer", "double jump" | yes | `{engine}-2d-movement` | `platformer`, `level-design` | genre composes movement + tilemap |
| "save / load", "save slots" | optional | (engine save API if present) | `save-systems` | concept-first; engine-agnostic if no engine |
| "enemy AI", "behavior tree" | yes | `{engine}` AI skill¹ | `game-ai` | ¹ `unity-navmesh` / `unreal-behavior-trees` / Godot nav |
| "publish on Steam" | no | — | `steam-publish` | pure workflow |

`{engine}` is filled from §1 (e.g. `godot`, `unity`). Where a genre lists `composes:` in the
catalog, expand it into the engine + discipline skills for the **detected** engine.

---

## 4. Progressive-disclosure read protocol

The router must respect the three-tier disclosure budget (see `SKILL-FORMAT.md` §1):

1. **Preloaded:** only every skill's `name` + `description` are in context. The router decides
   purely from these plus the fingerprint — it does **not** pre-read bodies.
2. **On selection:** read the body of **each chosen** `skills/<category>/<name>/SKILL.md` — and
   only those. Do not bulk-load a category.
3. **On demand:** read a skill's `references/*.md` only when the specific subtask requires that
   depth (the skill body says when).
4. **Re-route on pivot:** if the task changes mid-task (e.g. from movement to saving), select
   and read the newly relevant skill rather than keeping everything loaded.

The router's output to the user/agent should name the skills it is loading and why, e.g.:
*"Detected Godot (`project.godot`). Loading `godot-2d-movement` for the controller and
`platformer` for jump feel; will open `references/coyote-time.md` if you want input buffering."*

---

## 5. Multi-skill composition

- **One engine set, additive concepts.** Exactly one engine skill set (§1); add the disciplines
  the task needs and, usually, **at most one** genre. Workflows attach independently.
- **Ordering:** engine fundamentals → discipline concept → genre orchestration → workflow.
- **Ownership on overlap:** the **engine** skill owns API/syntax; the **discipline** skill owns
  the portable concept/algorithm and defers to the engine skill for code; the **genre** skill
  owns structure/glue and never re-teaches a primitive (it links to the others). This mirrors the
  catalog's composition rules.
- **Hand-offs:** when a genre's `composes:` names a slot like `*-2d-movement`, bind it to the
  detected engine (`godot-2d-movement`, etc.). If that engine lacks the exact skill, see §7.

---

## 6. Worked examples (end-to-end)

| Request | Detected engine | Skills loaded (in order) |
|---------|-----------------|--------------------------|
| "add a double jump to my Godot player" | Godot (`project.godot`) | `godot-2d-movement` → `platformer` (jump feel); `references/` for coyote-time on demand |
| "make an inventory for my Unity RPG" | Unity (`Assets/`+`ProjectSettings/`) | `unity-scriptableobjects` → `rpg` → `save-systems` |
| "procedural dungeon generator in Bevy" | Bevy (`Cargo.toml`+bevy) | `bevy-ecs` → `procedural-gen` → `roguelike` |
| "branching dialogue from a .yarn file" | (none required) | `dialogue-systems` (file signal `*.yarn`); add engine UI skill if an engine is detected |
| "how do I design save slots with migration?" | (none) | `save-systems` only — concept question, no engine |
| "publish my game on itch with butler" | (any/none) | `itch-publish` only |

This matrix doubles as a seed for the routing test set.

---

## 7. Unknown-engine & no-skill fallback

**Engine unknown (no fingerprint, no engine named):**

1. Check the request for an engine name in plain text ("in Unity", "using Phaser"). If found,
   adopt it.
2. If the task is a pure concept/genre/workflow question, route to the **engine-agnostic**
   discipline/genre/workflow skill directly — no engine needed (e.g. "what's a good save format?"
   → `save-systems`).
3. Only if engine choice actually blocks the answer, ask **one** targeted question
   ("Which engine — Godot, Unity, Unreal, or a web/other engine?") and proceed on the answer.
4. Never invent an engine or load an engine skill on a guess.

**Engine known but no specific skill covers the subtask:**

- Load the closest engine skill plus the relevant discipline, and **state the gap** plainly
  (e.g. "there's no dedicated Godot particles skill yet; using `godot-shaders` + general
  `audio-design`-style guidance"). Do not fabricate a skill name.

**Conflicting/!multiple genres:** pick the dominant genre from the phrasing; mention the
secondary and offer to load it if the user confirms.

---

## 8. Implementation notes

- Implement as `router/SKILL.md` using the open standard. The **description** must carry broad
  game-dev trigger vocabulary plus the engine names and "router/route/which skill" cues so it
  activates on game-dev requests across agents.
- Keep the body lean; if the full routing table grows past the budget, move it to
  `router/references/routing-table.md` and keep the algorithm + top rules in the body.
- The router contains **no** forbidden frontmatter keys (see `SKILL-FORMAT.md` §2.2); its
  always-relevant behavior is expressed through the description, and the always-on mapping is
  applied per agent at adapter-generation time (`COMPATIBILITY.md`), not via `alwaysApply`/
  `trigger` in the committed file.
- The router's `name` is `router` (it lives in `router/`, so the validator expects
  `name: router`).
- Cross-check every skill the router references against `docs/skill-catalog.md` so it never
  points at a name that does not exist.
