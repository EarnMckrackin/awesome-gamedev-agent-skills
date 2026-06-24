# Session 05 — Standards Lock & Router Spec (GATE)

> Public build-log entry. Value-focused only. No growth/marketing/strategy language.

- **Session:** S05
- **Wave:** 2 (the gate; runs alone after Wave 1)
- **Date (UTC):** 2026-06-24
- **Depends on:** S01, S02, S03, S04
- **Status:** complete
- **Branch / commits:** `main` → standards-lock commit (this session)

## Objective
Turn the Wave-1 research into **frozen, committed standards** plus a published catalog and
router spec, so the six authoring sessions (S06–S11) can run in parallel from committed files
alone, and prove the standard works by authoring one golden reference skill.

## What was produced
- **`docs/SKILL-FORMAT.md`** — locked authoring standard. Folds in the S01 lessons (L1–L10)
  and the S02 format refinements: exact frontmatter limits, the forbidden agent-specific
  frontmatter keys, the claude.ai 200-char description note, a rubric tagged to the validator,
  and a pointer to the golden skill. Marked **frozen** for Wave 3.
- **`docs/COMPATIBILITY.md`** — the 9-surface support matrix (Claude Code, Claude Agent SDK,
  Claude.ai, Kiro, Gemini CLI, Codex CLI, Cursor, Windsurf, Cline), the portable-core rule,
  and the adapter-generation mapping for the three rules-based editors.
- **`docs/INSTALLATION.md`** — per-agent install instructions (native folder copy for the CLI
  agents and Kiro, zip upload for Claude.ai, generated adapters for Cursor/Windsurf/Cline).
- **`docs/skill-catalog.md`** — the **frozen v1 catalog** (the file authoring sessions read):
  one table per category for all **62** skills, each with target version, difficulty, verified
  primary-doc sources, router signals (file/extension detection + phrase triggers), and status.
- **`router/ROUTER-SPEC.md`** — the master-router specification: engine detection by project
  files (with precedence + disambiguation), task classification, the task→skill routing-table
  format, the progressive-disclosure read protocol, multi-skill composition, and unknown-engine
  fallback. S12 implements this as `router/SKILL.md`.
- **`skills/other-engines/love2d-core/`** — the **golden reference skill** (`SKILL.md` +
  `references/state-stack.md`), authored end-to-end against LÖVE 11.5 and marked `golden-done`
  in the catalog so its owning authoring session (S08) skips it.
- This progress entry; `docs/progress/INDEX.md` updated.

## Key decisions
- **The standard is frozen, not advisory.** `docs/SKILL-FORMAT.md` and `docs/skill-catalog.md`
  carry a FROZEN banner; authoring sessions build to them as written and record any gap rather
  than diverging. QA (S13) resolves genuine conflicts.
- **The catalog is the single source of truth for names.** Git does not track empty
  directories, so no placeholder skill folders were created; locking the exact 62 names in the
  committed catalog is what the worktree sessions actually read. Each authoring session creates
  its own real folders.
- **One engine set, additive concepts.** The router selects exactly one engine skill set from a
  project fingerprint, then layers the discipline/genre/workflow skills the request triggers;
  genres compose engine + discipline skills and never re-teach primitives.
- **Golden skill = `love2d-core`.** LÖVE is small, stable, and fully documented, making it an
  ideal exemplar of the full skill shape (routing-rule description, six body sections, ≥2
  version-pinned patterns, pitfalls, and a `references/` file for progressive disclosure).

## Verification
- **Name validity + uniqueness:** parsed the committed catalog and confirmed **62** skill names,
  all unique, all matching `^[a-z0-9]+(-[a-z0-9]+)*$` and ≤64 chars (longest:
  `threejs-materials-lighting`, 26). Count matches the per-category totals (15+8+6+6+5+9+9+4).
- **Golden skill API correctness:** the load-bearing LÖVE 11.5 facts were verified against the
  primary wiki via Firecrawl — the `love.load`/`love.update(dt)`/`love.draw`/`love.keypressed`/
  `love.quit` callbacks; `conf.lua`'s `love.conf(t)` with `t.version = "11.5"`, `t.window.*`,
  `t.window.vsync` as a number (since 11.0) and `t.modules.*`; and that **color components are
  0–1 floats since 11.0** (the most common version bug).
- **Validator:** `python scripts/validate-skills.py` → "Validated 1 skill file(s). All checks
  passed." (exit 0). The golden skill's `description` is 191 chars (≤200 soft target, ≤1024 hard
  limit); `SKILL.md` is 147 lines (< 500); the `references/state-stack.md` link resolves.
- **Wave-1 integration:** confirmed the SESSION-01/02/03 progress docs were already tracked and
  committed; `plan/` and the local strategy files remain gitignored.

## Sources consulted (primary docs)
- `https://love2d.org/wiki/love` — LÖVE callback list and module overview (for the golden skill).
- `https://love2d.org/wiki/Config_Files` — `conf.lua` / `love.conf` fields and defaults (11.x).
- `https://love2d.org/wiki/love.graphics.setColor` — the 0–1 color range since 11.0.
- The Wave-1 research (S01 lessons, S02 compatibility matrix, S03 catalog + verified source map)
  was the basis for the standards, compatibility/install docs, and catalog.

## Handoff to next sessions
- **S06–S11** read their section of `docs/skill-catalog.md` + `docs/SKILL-FORMAT.md` + the golden
  skill, then author on their own branches. Re-verify each cited page and pin code to the listed
  version before writing. Skip `love2d-core` (already done).
- **S12** implements `router/SKILL.md` from `router/ROUTER-SPEC.md` and cross-checks every skill
  name against the catalog.
- **S13** can seed its routing test set from the worked-examples matrix in the router spec.

## If partial — remaining work
- None for the gate. Non-blocking follow-ups for later sessions: re-verify the `†` source URLs
  (slug-pattern, not individually fetched) at authoring time; confirm Windsurf's live per-file
  character cap when the adapter generator is built.
