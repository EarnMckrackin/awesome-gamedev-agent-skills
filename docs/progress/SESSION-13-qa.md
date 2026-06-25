# Session 13 — QA, Validation & Originality Audit

> Public build-log entry. Value-focused only. No growth/marketing/strategy language.

- **Session:** S13
- **Wave:** 5
- **Date (UTC):** 2026-06-25
- **Depends on:** S05 (standards + catalog + router spec), S06–S12 (the 62 skills + router)
- **Status:** complete
- **Branch / commits:** `main` → QA-pass commit (this session)

## Objective
Audit the full collection — 62 skills + `router/SKILL.md` — against the frozen authoring
standard: validate mechanically, check the rubric, run an originality pass, spot-check technical
accuracy against primary docs, sanity-check trigger/description quality, re-run the routing
matrix with edge cases, smoke-test installs across agents, and sweep for any growth language.
Fix every issue found.

## What was checked & the result

| # | Audit | Result | Issues → fixed |
|:-:|-------|:------:|----------------|
| 1 | Spec / validator (`scripts/validate-skills.py`) | ✅ PASS | 0 (green at baseline and after fixes) |
| 2 | Rubric (sample every category) | ✅ PASS | structural rubric clean; content nits fixed (below) |
| 3 | Originality (vs primary sources + public skills) | ✅ PASS | 0 copied passages; primary-source-only citations |
| 4 | Technical accuracy (spot-check vs primary docs) | ✅ PASS | 2 MAJOR + 10 MINOR fixed |
| 5 | Trigger / description quality | ✅ PASS | 1 dangling cross-ref fixed |
| 6 | Router matrix (18 + 8 edge cases) | ✅ PASS | 0 misroutes; all resolve to real skills |
| 7 | Cross-agent smoke test (Kiro + Claude Code) | ✅ PASS | router + 1/engine load in both |
| 8 | No growth/marketing language | ✅ PASS | 2 lines in `INDEX.md` neutralized |

## Method
- **Mechanical scan.** Ran the validator (63 files) plus a throwaway scanner for the warning-
  level rubric items the validator does not enforce: description length vs the 200-char
  claude.ai soft target, forbidden frontmatter keys, `compatibility` ≤500, code-block count
  (rubric wants ≥2), presence of when-to-use / pitfalls / references, and a marketing-language
  regex with context. (The scanner was deleted before commit; the canonical validator stays.)
- **Breadth audit.** Fanned out five read-only category audits (godot; unity+unreal;
  web-engines+other-engines; disciplines+workflows; genres). Each verified version-sensitive
  APIs against primary documentation via Firecrawl and reported findings with file:line and a
  proposed fix; all fixes were applied centrally from this session and re-validated.
- **Calibration.** Read `love2d-core` (golden), `godot-2d-movement`, `game-ai`, and `platformer`
  in full first to set the quality bar.

## Mechanical results (baseline, all green)
- **Validator:** "Validated 63 skill file(s). All checks passed." — `name` == folder, frontmatter
  limits, `description` ≤1024, `< 500` lines, resolved `references/` links.
- **Size:** every file < 500 lines (largest 208). **Frontmatter:** no forbidden keys; no
  `allowed-tools`; no `compatibility` > 500.
- **Structure:** every skill has ≥2 code blocks (the `platformer` genre has exactly 2, by
  design — genres orchestrate rather than re-teach) and a when-to-use / pitfalls / references
  set. The router carries 0 code blocks and no "pitfalls" heading **by design** — it is a
  dispatcher with a fallback section, not a code-teaching skill.
- **Descriptions > 200 chars** on most engine skills: a known, spec-tolerated trade-off
  (hard cap is 1024; each leads with its primary use case so it degrades gracefully if a
  surface truncates the tail). Left as-is per `SKILL-FORMAT.md` §2.3.

## Issues found & fixed (22 edits across 14 skills)

**Technical accuracy — MAJOR (2):**
- `godot-physics` — ray-query self-exclusion used `query.exclude = [self]`; `exclude` is
  `Array[RID]`, so corrected to `[self.get_rid()]`.
- `godot-csharp` — pinned ".NET 8" to Godot 4.3. Verified against the current stable Godot C#
  docs: **.NET 8 is required as of Godot 4.5** (Android export needs .NET 9); **4.3/4.4 target
  .NET 6.0**. Made the `compatibility` field, intro, workflow step, and pitfall version-aware.

**Technical accuracy — MINOR (10):**
- `godot-export` — `--quit-after` counts main-loop iterations (frames), not seconds.
- `godot-animation` — `create_tween()` returns a `Tween` (the `SceneTreeTween` name was a 4.0
  pre-release artifact).
- `audio-design` — dB sign error in a comment (`0.5` as a raw dB value is **+**0.5 dB).
- `save-systems` — rename-over-target is atomic on POSIX but not guaranteed atomic on Windows;
  reframed so the `.bak` copy is the real recovery guarantee.
- `physics-tuning` — noted `continuous_cd` is a bool on `RigidBody3D` but a `CCD_MODE_*` enum on
  `RigidBody2D`.
- `unity-csharp-scripting` — fixed a garbled `Start`/`Update` ordering pitfall sentence.
- `pixijs-rendering` — scoped the Vite top-level-await caveat to Vite ≤6.0.6 (per the v8 docs).
- `bevy-ecs` — softened an unverifiable "verified through 0.19" claim; corrected "bundles
  removed (0.15+)" to "deprecated in 0.15, removed in 0.16".
- `threejs-scene-setup`, `threejs-gltf-loading`, `threejs-materials-lighting` — aligned the
  stated baseline to the frozen catalog's **r165+** (kept the "verified against r184" note and
  the accurate historical feature-floor notes for r147/r155).

**Trigger quality — MINOR (1):**
- `unity-tilemap-2d` — removed a dangling `` `unity` `` cross-reference (no such skill); pointed
  3D grid work at Unity's own tooling and named the gap.

**No-growth sweep (1 doc):**
- `docs/progress/INDEX.md` — neutralized the only growth phrasing in the committed tree: S16
  "Launch / go-to-market" → "Community handoff & maintenance", and "→ launch" → "→ community
  handoff". (The go-to-market plan itself stays in the gitignored `plan/`.) Every other
  `launch`/`grow` hit was technical/domain text or the anti-growth rule statements themselves.

**Originality:** no copied prose or code found in any sampled skill; citations are primary
documentation only. A web check confirmed no public game-dev `SKILL.md` collection mirrors this
one's taxonomy, names, or phrasing. `NOTICE` is accurate as written — no change needed.

## Router test matrix (re-run + edge cases)
None of the QA edits touched any `description` field, so routing behaviour is unchanged from
S12. The S12 18-case matrix was re-traced against `router/SKILL.md` + its `references/` (all 18
still resolve to existing skills) and extended with **8 edge cases**:

| # | Edge-case prompt / signal | Router behaviour | Rule |
|:-:|---------------------------|------------------|------|
| E1 | "Best way to structure input rebinding?" — no project files | `input-systems` only; no engine needed | §6.2 |
| E2 | Monorepo with both `project.godot` and a `three` dep | route by the file/subdir in question; ask once if ambiguous | §1 (monorepo) |
| E3 | `package.json` lists both `phaser` and `pixi.js` | prefer the lib imported in the file under discussion; else ask which renderer | §1 (multi-web) |
| E4 | `project.godot` **and** a `.csproj` present | Godot wins → `godot-csharp` (not Unity/Bevy) | §1 (disambiguation) |
| E5 | "Make a Pawn possess a Character in Unreal" — no `.uproject` | adopt Unreal from prose → `unreal-cpp-gameplay` | §6.1 |
| E6 | "Add 2D movement to my Unity platformer" | no `unity-2d-movement` skill → `unity-csharp-scripting` + `unity-physics` + `platformer`; state the gap | §6 (binding gap) |
| E7 | "A roguelike deckbuilder" — two genres | dominant `card-game`; mention `roguelike`, offer to load | §6 (conflicting genres) |
| E8 | "Now add a save system" after movement work | re-route on pivot → `save-systems` | §4.4 |

All 26 cases (18 + 8) resolve to skills that exist on `main`; the unknown-engine path asks once
and defaults to Godot; binding gaps are stated honestly rather than papered over with invented
skill names.

## Cross-agent smoke test
Installed the router + one skill per engine family (`godot-2d-movement`, `unity-csharp-scripting`,
`unreal-cpp-gameplay`, `phaser-core`, `love2d-core`) into each agent's documented location and
confirmed each loads (valid frontmatter; `name` == folder; `description` present):

- **Kiro** → `.kiro/skills/<name>/`: all six load.
- **Claude Code** (v0.20.2) → `.claude/skills/<name>/`: all six load; `claude --help` confirms
  skills resolve via `/skill-name`.
- **CLIs detected:** Claude Code, Gemini CLI, Cursor, Kiro. Codex CLI absent (shares Gemini's
  `.agents/skills/` path, so not separately exercised).

The temporary installs were removed after the check (the canonical copies live in `skills/`).
Results recorded in `docs/COMPATIBILITY.md` (new "Verified install paths (S13 smoke test)"
section). Live auto-trigger on a real prompt in a brand-new session remains the manual step
documented in `INSTALLATION.md`.

## Verification
- `python scripts/validate-skills.py` → **"Validated 63 skill file(s). All checks passed."**
  (exit 0) — both at baseline and after all 22 fixes.
- `git status` clean apart from the intended 16 modified files; the throwaway QA scanner was
  deleted before commit.

## Sources consulted (primary docs)
- Godot (stable): `tutorials/scripting/c_sharp/c_sharp_basics.html` (.NET version),
  `classes/class_physicsrayqueryparameters2d.html` (`exclude` type),
  `classes/class_tween.html`, `tutorials/editor/command_line_tutorial.html` (`--quit-after`),
  `classes/class_diraccess.html` (`rename_absolute`).
- Unity 6 ScriptReference/Manual; Unreal Engine docs (Enhanced Input, Niagara, packaging).
- PixiJS v8 quick-start; Bevy 0.15→0.16/0.16→0.17 migration guides + `docs.rs/bevy`; three.js
  manual/docs + migration notes; Phaser, pygame-ce, Roblox, Yarn/Ink, Steamworks, itch butler.

## Handoff to next sessions
- **S14 (README & docs polish)** inherits a validator-green, rubric-clean, originality-clean
  tree with a current install/compat smoke test recorded. The routing matrix here is the
  regression seed.
- The frozen catalog and skills now agree on the three.js baseline (r165+); no other
  catalog/skill version conflicts remain.

## If partial — remaining work
- None for S13. Optional, non-blocking polish noted but not applied (no misroute, no rubric
  failure): symmetry "when-not" pointers between `rpg`/`visual-novel` and `card-game`/`roguelike`
  on the shared word "branching"/"deckbuilder", and naming `godot-multiplayer` in the
  `fps-shooter` netcode pitfall.
