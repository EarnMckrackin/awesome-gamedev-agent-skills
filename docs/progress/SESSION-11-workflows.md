# Session 11 — Author: Workflows

> Public build-log entry. Value-focused only. No growth/marketing/strategy language.

- **Session:** S11
- **Wave:** 3
- **Date (UTC):** 2026-06-24
- **Depends on:** S05 (standards + catalog + golden skill)
- **Status:** complete
- **Branch / commits:** `session/S11-workflows`

## Objective
Author the four workflow (process) skills from the frozen catalog — how to ship a game
under real constraints — as decision-first, checklist-driven playbooks, with the two
publishing skills verified command-by-command against official tooling docs.

## What was produced
- `skills/workflows/game-jam/SKILL.md` — scope-to-the-clock planning for timed jams:
  rules-first prep, one-sentence concept, a length-based scope budget, a 48-hour timeline,
  feature triage, and a pre-submission build check.
- `skills/workflows/prototype-fast/SKILL.md` — hour-scale mechanic validation: one question,
  throwaway-vs-keep decided up front, hard timebox, greybox everything else, observable
  keep/kill criteria, and spike containment.
- `skills/workflows/steam-publish/SKILL.md` (+ `references/steampipe-build-scripts.md`) —
  Steamworks + SteamPipe: prerequisites and least-privilege build account, app/depot/package
  config, a minimal `app_build.vdf`, the `steamcmd` upload command, setting a build live per
  branch, and the two-track release checklists. Reference holds multi-depot scripts,
  `FileExclusion`/`FileProperties`, branch setup, the CI/CD login-token workflow, patch-size
  hygiene, and a troubleshooting table.
- `skills/workflows/itch-publish/SKILL.md` (+ `references/butler-ci.md`) — itch.io + butler:
  project-page setup, install/login, portable-build prep, `butler push <dir> user/game:channel`,
  channel-name → platform-tag rules, versioning, and update-via-re-push. Reference holds the
  `BUTLER_API_KEY` CI flow, `broth` install, a GitHub Actions example, the full flag/command
  table, and the update-check API.

## Key decisions
- **Authored directly rather than fanning out to subagents.** All source material was
  gathered and verified first-hand via Firecrawl in this session; direct authoring kept the
  operational commands accurate and the prose original, and every file was reviewed against
  the `docs/SKILL-FORMAT.md` rubric before commit.
- **Publishing skills are operational checklists, not tutorials.** Exact commands, file
  formats (VDF KeyValues), channel rules, and the gotchas that block a release (default
  branch can't be auto-set-live; store page reviewed before build; portable folder not
  installer) are foregrounded; deep/rarely-needed detail pushed to `references/`.
- **All four are engine-agnostic** (`metadata.engine: none`), since they are process skills.
- **No secrets.** Both publishing skills explicitly keep credentials/tokens out of the repo
  and show the supported secret-based CI auth (`BUTLER_API_KEY`, preserved `config.vdf`).

## Verification
- `python scripts/validate-skills.py` — all `skills/workflows/**` files pass (name=folder,
  description ≤1024, <500 lines, resolved `references/` links). The only validator failures
  observed during the session were in `skills/unity/**` (a different parallel session's
  in-progress files); no workflow file was ever flagged.
- Sizes: SKILL.md files 125–184 lines; references 117 and 155 lines — all well under 500.
- Descriptions 248–292 chars (lead with the primary use case so they trigger even if a
  surface truncates the tail).
- Publishing commands/steps verified against the official Steamworks and itch.io/butler docs
  (see sources). Steam Direct fee noted as "at time of writing" since pricing can change.

## Sources consulted (primary docs)
- itch.io butler manual — `https://itch.io/docs/butler` (installing, login, pushing,
  single-files): `butler push`/`push-preview`/`status`, channel naming, flags,
  `BUTLER_API_KEY`, broth install, 30 GB limit, update API.
- Steamworks — `https://partner.steamgames.com/doc/sdk/uploading` (SteamPipe, ContentBuilder,
  `app_build.vdf`/`depot_build.vdf`, steamcmd, CI/CD), `/doc/store/releasing` (release
  checklists + manual release), `/doc/store/application/branches` (default vs beta branches).

## Handoff to next sessions
- S12 (router) can route to `game-jam`, `prototype-fast`, `steam-publish`, `itch-publish`
  using the catalog's `says:`/`files:` signals; the skills cross-link to each other and to
  engine/genre skills via "Related skills".
- S13 (QA/originality) can run the rubric/originality pass; all prose and example
  config/commands were written from scratch from the cited primary docs.

## If partial — remaining work
- None. Optional `asset-pipeline` skill was not in scope for v1 (not in the frozen catalog's
  workflows table) and was intentionally skipped.
