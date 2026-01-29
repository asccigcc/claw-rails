# claw-rails

A Claude Code skill pack for Rails 7+ / 8 projects. Distributed via [`claw`](https://github.com/asccigcc/claw).

## What's in this pack

- **Skills** — on-demand rules Claude loads when relevant:
  - `ruby-style` (Ruby 3.x + Sandi Metz sizing)
  - `rails-models`, `rails-controllers`
  - `rails-7-params`, `rails-8-params` — version-tagged, activate based on `Gemfile.lock`
  - `stimulus`, `turbo`, `view-components`
  - `rails-frontend` (Tailwind v4 + Flowbite + Vite Ruby)
- **Agent** — `rails-expert` for multi-file / architectural work.
- **CLAUDE.md imports** — global engineering principles + Rails stack persona, injected via `@import`.

Version-specific skills activate based on what Claude sees in the project (`Gemfile.lock`). If neither `Gemfile` nor `Gemfile.lock` is present, Claude asks which Rails version to target.

## Install

```bash
claw install asccigcc/claw-rails         # current directory
claw install asccigcc/claw-rails -g      # global (~/.claude)
claw install asccigcc/claw-rails@v1.0.0  # pinned
```

Preview without changes:
```bash
claw install asccigcc/claw-rails --dry-run
```

Uninstall:
```bash
claw uninstall rails
claw uninstall rails -g
```

For local development of this pack:
```bash
cd /path/to/claw-rails
claw install . -g
```

## Layout

```
.
├── claw.toml                 # pack manifest
├── CLAUDE.md                 # global engineering principles
├── core/skills/              # cross-stack skills (ruby-style)
└── stacks/rails/
    ├── CLAUDE.md             # Rails-stack persona + version detection
    ├── skills/               # rails-models, rails-controllers, ...
    ├── agents/               # rails-expert
    └── commands/             # (empty for now)
```

## Adding / editing a skill

1. Create or edit `stacks/rails/skills/<name>/SKILL.md` with frontmatter:
   ```yaml
   ---
   name: skill-name
   description: When Claude should load this skill.
   ---
   ```
2. Add the path to `claw.toml` under `[provides].skills`.
3. Re-run `claw install` to link the new skill (existing links are left alone; idempotent).

## Version-aware skills

For frameworks with breaking API changes between versions, ship both variants and let Claude pick based on project signals:

- Tag version-specific skills: `rails-8-params/`, `rails-7-params/`.
- In the `description`, name the version explicitly so Claude only loads the matching one.
- If no version signal exists, the stack's `CLAUDE.md` tells Claude to ask the user or check `.claude/CLAUDE.md` for a pin.

All variants install together; activation is description-driven, not install-driven.
