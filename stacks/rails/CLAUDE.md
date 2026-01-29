# Rails Stack

Senior Rails engineer. Hotwire (Turbo + Stimulus), Tailwind v4 + Flowbite, ViewComponents, Vite Ruby, RSpec.

## Version Detection (required before version-sensitive edits)
1. Read `Gemfile.lock` → look for `rails (X.Y.Z)`. Use the major version.
2. If absent, read `Gemfile` → `gem "rails", "~> X.Y"`.
3. If neither exists, **do not guess** — alert the user:
   > "No `Gemfile`/`Gemfile.lock` found. Which Rails version should I target? You can also pin it by adding `This project uses Rails 8.` to `.claude/CLAUDE.md`."
4. A `.claude/CLAUDE.md` line like "This project uses Rails 8." overrides auto-detection.

Version-tagged skills (`rails-8-*`, `rails-7-*`) activate only for their version; neutral skills always apply.

## Conventions
- REST controllers; strong params (API varies by version — see `rails-7-params` / `rails-8-params`).
- Business logic in services/interactors, not controllers or models.
- POROs before Concerns.
- Background jobs for long-running work.
- Plural tables, timestamps, FK constraints, indexes on queried columns.

## Performance & Security
- Watch N+1; eager-load; prefer bulk operations.
- Cache where warranted (Russian Doll for nested components).
- Authorize at the controller boundary; sanitize inputs; OWASP.

## i18n
User-facing strings go in `en.yml`.

Deeper patterns live in the stack skills. Use the `rails-expert` agent for multi-file or architectural work.
