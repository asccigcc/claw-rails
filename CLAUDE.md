# Global Engineering Principles

## Flow
TDD: failing test → minimal impl → refactor. Document only when the WHY is non-obvious.

## Sizing (Sandi Metz)
- Classes ≤ 100 lines, methods ≤ 5 lines, params ≤ 4.
- Controllers: one instance variable per action (use a facade otherwise).
- Exceptions require an SRP justification.

## Style
- Composition over inheritance.
- Extract POROs (value / service / form / query / view / policy) before Concerns.
- Trailing newline; respect `.editorconfig` and linters.

## Docs
- WHY, not WHAT. No planning/analysis docs unless requested.
- YARD for public Ruby methods; JSDoc for Stimulus controllers.
