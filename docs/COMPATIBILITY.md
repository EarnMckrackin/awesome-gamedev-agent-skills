# Compatibility

The portable core of every skill in this repository is the **Agent Skills open standard**
(`SKILL.md`). One canonical skill folder serves the agents that consume the standard natively;
three rules-based editors are supported through **generated adapters**. This page documents
where each agent looks for skills and how it triggers them. For copy-paste install commands,
see [`INSTALLATION.md`](INSTALLATION.md).

## Support matrix

| Agent | Native skills? | Skill format | Project install path | User / global path | Trigger | Adapter needed? |
|-------|:--------------:|--------------|----------------------|--------------------|---------|:---------------:|
| **Claude Code** | ✅ | `SKILL.md` | `.claude/skills/<name>/` | `~/.claude/skills/<name>/` | auto (Skill tool) · `/skills` menu | ❌ |
| **Claude Agent SDK** | ✅ (filesystem) | `SKILL.md` | `.claude/skills/<name>/` | `~/.claude/skills/<name>/` | auto via `skills` option (set `setting_sources`) | ❌ |
| **Claude.ai** | ✅ (zip upload) | `SKILL.md` at zip root | — (per-account upload) | — | auto in chat | ❌ (zip the folder; `description` ≤ 200 chars) |
| **Kiro** | ✅ | `SKILL.md` | `.kiro/skills/<name>/` | `~/.kiro/skills/<name>/` | auto · `/skill-name` | ❌ |
| **Gemini CLI** | ✅ | `SKILL.md` | `.agents/skills/<name>/` (or `.gemini/skills/`) | `~/.agents/skills/` (or `~/.gemini/skills/`) | `activate_skill` + consent | ❌ |
| **Codex CLI** | ✅ | `SKILL.md` | `.agents/skills/<name>/` | `~/.agents/skills/` | `$skill` · `/skills` · implicit | ❌ |
| **Cursor** | ❌ | `.mdc` rule | `.cursor/rules/<name>.mdc` | Settings → Rules | Agent Requested (description) · globs · `@name` | ✅ generate |
| **Windsurf** | ❌ | `.md` rule | `.windsurf/rules/<name>.md` | `global_rules.md` | `model_decision` · `glob` · `@name` | ✅ generate (respect char budget) |
| **Cline** | ❌ | `.md` / `.txt` rule | `.clinerules/<name>.md` | `Documents/Cline/Rules/` | `paths` glob · manual toggle | ✅ generate (fold description into body) |

**Native drop-in summary.** A single `SKILL.md` tree serves **Claude Code, Kiro, Gemini CLI,
and Codex CLI** (plus the Claude Agent SDK, and Claude.ai via zip upload). The one directory
that Gemini CLI **and** Codex CLI both read is **`.agents/skills/<name>/SKILL.md`** — use it as
the universal CLI location. Claude Code reads `.claude/skills/`; Kiro reads `.kiro/skills/`.
Only **Cursor, Windsurf, and Cline** require generated adapters.

## Verified install paths (S13 smoke test)

Checked 2026-06-25 against this repository. The router plus one skill per engine family were
installed into each native agent's documented location and confirmed to load — valid
frontmatter, `name` equals the folder, `description` present:

| Agent | Path tested | Skills installed | Result |
|-------|-------------|------------------|--------|
| **Kiro** | `.kiro/skills/<name>/` | `router`, `godot-2d-movement`, `unity-csharp-scripting`, `unreal-cpp-gameplay`, `phaser-core`, `love2d-core` | all 6 load (name==folder, description present) |
| **Claude Code** (v0.20.2) | `.claude/skills/<name>/` | same six | all 6 load; `claude --help` confirms skills resolve via `/skill-name` |

The six cover every engine family (Godot, Unity, Unreal, web, other) plus the router.
`scripts/validate-skills.py` passes on all 63 files (62 skills + router), so the same files
satisfy each agent's load contract once copied to its skills directory. **CLIs detected on the
test machine:** Claude Code, Gemini CLI, Cursor, Kiro (Codex CLI absent — not exercised; it
shares Gemini's `.agents/skills/` path). Live auto-trigger on a real prompt in a fresh session
is the manual step in [`INSTALLATION.md`](INSTALLATION.md#verify-after-installing).

## The portable-core rule

A committed `SKILL.md` contains **only** portable, agent-agnostic content:

- **Frontmatter:** `name`, `description` (required) plus optional `license`, `compatibility`,
  and `metadata` (`engine` / `category` / `difficulty`). Nothing else.
- **Body:** the open-standard Markdown playbook (when-to-use, workflow, patterns, pitfalls,
  references), with optional `references/`, `scripts/`, and `assets/`.

The following agent-specific keys are **forbidden** in a committed `SKILL.md` (the validator
rejects them); they belong only in generated adapters:

```
when_to_use   argument-hint   disable-model-invocation   user-invocable   model
paths         hooks           shell                      context          globs
alwaysApply   trigger
```

`allowed-tools` is part of the open standard but honored only by Claude Code; this repo omits
it by default. Keeping the core clean is what lets one file install everywhere — agent quirks
are applied at adapter-generation time, never baked into the source.

## Adapters (Cursor / Windsurf / Cline)

These editors use a *rules* system rather than skills. Adapters are **generated** from the
canonical `SKILL.md` and treated as build artifacts — never hand-edited, never a parallel copy
of the skill. The mapping:

| From `SKILL.md` | → Cursor `.mdc` | → Windsurf `.md` | → Cline `.md` |
|-----------------|-----------------|------------------|---------------|
| `name` | filename + `@id` | filename + `@id` | filename |
| `description` | `description:` (mode *Agent Requested*) | `description:` (`trigger: model_decision`) | **prepended into the body** (no description field) |
| file-scoped skill | `globs:` (Auto Attached) | `trigger: glob` + `globs:` | `paths:` |
| router / always-on skill | `alwaysApply: true` | `trigger: always_on` | no frontmatter |
| body | verbatim (rewrite refs as `@path`) | verbatim (enforce char budget; spill depth to references) | description + verbatim body |

The default mode mirrors how `SKILL.md` itself triggers: description-driven (Cursor *Agent
Requested*, Windsurf *model_decision*). The master router maps to an always-on rule.

**Fidelity caveats:**

- **Cline** has no description-driven trigger, so the adapter folds the description into the top
  of the body to preserve *what + when*.
- **Windsurf** enforces a per-file character budget; the generator trims or pushes depth into
  referenced files. Author bodies lean so they survive the conversion.

## Why this works

The open standard formalizes progressive disclosure (metadata → body → resources) and a small,
fixed frontmatter contract. Because the four CLI agents and the Claude surfaces all read that
same contract, a correct `SKILL.md` is portable by construction; the editors that don't read it
yet are reached by deterministic generation rather than by maintaining nine copies of every skill.
