# spec-kitty

> Spec-driven development workflow for **Phenotype** — 14 slash commands that orchestrate the full `kitty-specs/` lifecycle from constitution through merge.

[![GitHub](https://img.shields.io/badge/repo-KooshaPari%2Fspec--kitty-blue)](https://github.com/KooshaPari/spec-kitty)
![version](https://img.shields.io/badge/version-0.11.0-blueviolet)
![commands](https://img.shields.io/badge/commands-14-success)
![license](https://img.shields.io/badge/license-MIT-green)

---

## What is spec-kitty?

`spec-kitty` is a **plugin of 14 Claude Code slash commands** that implement a strict, opinionated spec-driven development workflow. Each command corresponds to a phase of the `kitty-specs/<###-feature>/` lifecycle — from authoring the project constitution, through specification, planning, task breakdown, implementation, and finally acceptance + merge.

This repository is the **canonical source** of those 14 commands. Mirror copies exist in:

- `~/.claude/commands/` (user-global, on PATH for every project)
- `repos/*/.claude/commands/` (per-project mirrors where local overrides live)

Run `gh repo sync` or copy files back to refresh those locations when commands evolve.

---

## The 14 commands

| # | Command | Phase | Purpose |
|---|---------|-------|---------|
| 00 | `/spec-kitty.constitution` | Foundational | Create or update the **project constitution** (technical standards, quality gates, governance) via a 4-phase discovery interview. |
| 01 | `/spec-kitty.specify` | Spec | Generate `kitty-specs/<###-feature>/spec.md` from a natural-language feature description. Runs in the **planning repo** — no worktree. |
| 02 | `/spec-kitty.clarify` | Spec | Resolve `[NEEDS CLARIFICATION]` markers left in `spec.md` with the user. |
| 03 | `/spec-kitty.plan` | Plan | Produce the implementation plan (`plan.md`) with technical decisions and architecture. |
| 04 | `/spec-kitty.tasks` | Plan | Break the plan into trackable **work packages** (`tasks/`). |
| 05 | `/spec-kitty.implement` | Build | Create one worktree per WP and execute the work. |
| 06 | `/spec-kitty.review` | Build | Review the diff produced by `/implement` for correctness, reuse, efficiency. |
| 07 | `/spec-kitty.checklist` | Quality | Run a quality checklist over the spec/plan before acceptance. |
| 08 | `/spec-kitty.analyze` | Quality | Cross-artifact analysis: spec ↔ plan ↔ tasks ↔ diff. |
| 09 | `/spec-kitty.research` | Quality | Deep, multi-source research fan-out for unfamiliar territory. |
| 10 | `/spec-kitty.status` | Meta | Print current feature status and outstanding items. |
| 11 | `/spec-kitty.dashboard` | Meta | Render a cross-feature dashboard of the active kitty-specs fleet. |
| 12 | `/spec-kitty.accept` | Close | Validate feature readiness and emit merge instructions. |
| 13 | `/spec-kitty.merge` | Close | Execute the merge choreography (worktree cleanup, branch archival). |

---

## Lifecycle diagram

```
   ┌─────────────────────────────────────────────────────────────────────┐
   │  Phase 0 — Constitution (one-time, optional)                        │
   │     /spec-kitty.constitution                                         │
   └──────────────────────────────┬──────────────────────────────────────┘
                                  │
   ┌──────────────────────────────▼──────────────────────────────────────┐
   │  Phase 1-2 — Spec authoring (in planning repo, no worktree)         │
   │     /spec-kitty.specify  →  /spec-kitty.clarify                    │
   └──────────────────────────────┬──────────────────────────────────────┘
                                  │
   ┌──────────────────────────────▼──────────────────────────────────────┐
   │  Phase 3-4 — Plan & tasks (still planning repo)                     │
   │     /spec-kitty.plan  →  /spec-kitty.tasks                          │
   └──────────────────────────────┬──────────────────────────────────────┘
                                  │
   ┌──────────────────────────────▼──────────────────────────────────────┐
   │  Phase 5-6 — Build (worktree per WP)                                │
   │     /spec-kitty.implement  →  /spec-kitty.review                    │
   └──────────────────────────────┬──────────────────────────────────────┘
                                  │
   ┌──────────────────────────────▼──────────────────────────────────────┐
   │  Phase 7-9 — Quality gates                                          │
   │     /spec-kitty.checklist  →  /spec-kitty.analyze  → /spec-kitty.research │
   └──────────────────────────────┬──────────────────────────────────────┘
                                  │
   ┌──────────────────────────────▼──────────────────────────────────────┐
   │  Phase 10-11 — Visibility                                           │
   │     /spec-kitty.status  →  /spec-kitty.dashboard                    │
   └──────────────────────────────┬──────────────────────────────────────┘
                                  │
   ┌──────────────────────────────▼──────────────────────────────────────┐
   │  Phase 12-13 — Close                                                │
   │     /spec-kitty.accept  →  /spec-kitty.merge                        │
   └─────────────────────────────────────────────────────────────────────┘
```

---

## Layout

```
spec-kitty/
├── README.md
├── LICENSE
├── .claude-plugin/
│   └── plugin.json         # Plugin manifest (name, version, commands, lifecycle map)
└── commands/
    ├── spec-kitty.constitution.md
    ├── spec-kitty.specify.md
    ├── spec-kitty.clarify.md
    ├── spec-kitty.plan.md
    ├── spec-kitty.tasks.md
    ├── spec-kitty.implement.md
    ├── spec-kitty.review.md
    ├── spec-kitty.checklist.md
    ├── spec-kitty.analyze.md
    ├── spec-kitty.research.md
    ├── spec-kitty.status.md
    ├── spec-kitty.dashboard.md
    ├── spec-kitty.accept.md
    └── spec-kitty.merge.md
```

---

## Installation

### Plugin install (preferred)

```bash
# Add as a Claude Code plugin
gh repo clone KooshaPari/spec-kitty ~/.claude/plugins/spec-kitty
```

Then reference the commands from any project — no per-project install required.

### Manual mirror

```bash
# Copy the 14 commands into your global commands dir
cp commands/spec-kitty.*.md ~/.claude/commands/

# Or into a specific project
cp commands/spec-kitty.*.md <project>/.claude/commands/
```

---

## Compatibility

- **Claude Code** ≥ 0.11.0 (commands reference `/spec-kitty.*` slash-command namespace)
- **Git** ≥ 2.30 (worktree-based WP isolation)
- **Phenotype** ecosystem only — the commands assume `kitty-specs/<###-feature>/` layout and the `spec-kitty` CLI binary on `PATH`.

---

## Versioning

| Version | Notes |
|---------|-------|
| 0.11.0  | Current. All 14 commands present; planning-repo workflow (no worktree during specify/plan). |

Bump the `version` field in `.claude-plugin/plugin.json` when commands change shape.

---

## License

MIT © 2026 KooshaPari. See `LICENSE`.
