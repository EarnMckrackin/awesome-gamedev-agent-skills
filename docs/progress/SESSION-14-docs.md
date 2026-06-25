# Session 14 — README & Docs Polish

> Public build-log entry. Value-focused only. No growth/marketing/strategy language.

- **Session:** S14
- **Wave:** 6
- **Date (UTC):** 2026-06-25
- **Depends on:** S13 (validator-green, rubric-clean, originality-clean tree; install/compat smoke test)
- **Status:** complete
- **Branch / commits:** `main` → docs-polish commit (this session)

## Objective
Make the public-facing docs excellent and exactly accurate to what shipped: rewrite the
README around the router, regenerate the catalog tables from reality, finalize
INSTALLATION / COMPATIBILITY / CONTRIBUTING, and set repository metadata (description +
topics). Every count and claim must match the repo; no growth language; all links resolve.

## What was produced

### README.md (full rewrite)
- **Value prop + badges.** One-line value proposition; four badges that reflect reality —
  license (Apache-2.0), skills (`62 + router`), format (Agent Skills), and a dynamic
  GitHub **last-commit** shield. **No CI badge** was added: there is no `.github/workflows/`
  in the repo, so a build badge would be a false claim.
- **Quick start** centered on the router: install once (Claude Code bulk-copy snippet that
  also installs `router/`), describe what you want in plain language, let it route.
- **How the router works** in three steps: detect engine → understand task → load skill(s),
  with the exclusive-engine / additive-concepts rule and a link to `ROUTER-SPEC.md`.
- **Catalog tables** for every category, each skill linked to its `SKILL.md`. Counts are
  generated from the tree and match the frozen catalog exactly:
  Godot 15 · Unity 8 · Unreal 6 · web-engines 6 · other-engines 5 · disciplines 9 ·
  genres 9 · workflows 4 = **62** (+ router).
- **Agent compatibility** summary table (9 agents) distilled from `COMPATIBILITY.md`.
- **Demo** section: a placeholder with explicit instructions to record a short router GIF
  (no fake asset is referenced; the `![…]` line is commented out until a clip exists).
- Repository layout, contributing pointer + validator command, Apache-2.0 license.

### CONTRIBUTING.md
- Added **"Updating the router when you add a skill"** — the previously missing piece: a new
  skill is not done until the router can reach it. Steps cover the catalog row, the routing
  table (`router/SKILL.md` §3a–d) and `references/routing-table.md`, engine detection (§1 +
  `references/engine-detection.md`) for new engines/signals, `composes:` for genres, and
  re-validation. The rubric checklist, `SKILL-FORMAT.md` link, conventional-commit
  convention, and validator usage were already present and are retained.

### docs/INSTALLATION.md
- Added an explicit note that **the router is itself a skill** and must be installed too,
  plus a `cp -R router .claude/skills/router` line in the bulk-install snippet (the dispatcher
  was previously omitted from the install steps). Per-agent steps (Claude Code, Kiro,
  Gemini/Codex shared `.agents/skills/`, Claude Agent SDK, Claude.ai, Cursor/Windsurf/Cline
  adapters) were already exact and are unchanged.

### docs/COMPATIBILITY.md
- Reviewed; already finalized in S13 with the "Verified install paths (S13 smoke test)"
  matrix (Kiro + Claude Code, six skills each). No change required.

### Repository metadata (via `gh`)
- **Description** set (value-focused): *"Game-development Agent Skills for AI coding agents:
  install once and a master router loads the right skill for your engine and task. 62
  original, version-pinned SKILL.md files across Godot, Unity, Unreal, web and more."*
- **Topics** confirmed to match the intended set (all 11 present): `gamedev`, `ai-agents`,
  `claude-code`, `kiro`, `godot`, `unity`, `unreal`, `skills`, `agent-skills`,
  `game-development`, `awesome-list`.

## Verification
- `python scripts/validate-skills.py` → **"Validated 63 skill file(s). All checks passed."**
  (docs changes don't touch skills, but re-run to confirm the tree stays green.)
- **Counts** cross-checked by enumerating `skills/*/*/SKILL.md` (62 total, per-category
  breakdown above) against `docs/skill-catalog.md` — exact match.
- **Links:** every skill link in the README points at an existing `SKILL.md`; doc-to-doc
  links (catalog, format, installation, compatibility, router spec, license, NOTICE) resolve.
- **No growth/marketing language:** README/CONTRIBUTING/INSTALLATION reviewed; no "star",
  launch, or competitor-comparison phrasing. Badges state facts only.
- `gh repo view` confirms the description and all 11 topics are live.

## Handoff to next sessions
- **S15 (final consolidation & release)** inherits accurate, link-clean public docs and live
  repo metadata. Open follow-ups: record the router demo GIF and drop it at
  `docs/assets/router-demo.gif` (README has the placeholder + instructions); optionally add a
  CI workflow that runs `validate-skills.py` (then a CI badge becomes truthful to add).

## If partial — remaining work
- None for S14. The demo GIF is intentionally left as an instructed placeholder (asset not
  recorded in this session); no broken image reference is committed.
