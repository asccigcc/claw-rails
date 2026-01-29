---
name: rails-controllers
description: Apply when writing or editing Rails controllers and their collaborating POROs (service, form, query, view, policy, decorator, value objects). Use when extracting logic out of fat controllers or models.
---

# Rails Controllers & Supporting POROs

Controllers stay thin. Push logic into single-purpose Plain Old Ruby Objects, grouped by intent.

## Object Types

| Object | When to use | Key behavior |
| --- | --- | --- |
| **Value Object** | Attribute has business logic beyond get/set (e.g. `PhoneNumber`, `Money`) | Implement `#hash`, `#eql?`; add comparison methods like `#better_than?`. |
| **Service / Action Object** | Multi-model action, external API, alternative strategies | Single-purpose class, dependency injection for variations. |
| **Form Object** | Multi-model form submission | Replaces `accepts_nested_attributes_for`. Consolidates validation. |
| **Query Object** | Complex SQL beyond a scope | Accepts a `Relation`, composable; test against real DB records. |
| **View Object** | Logic that exists only for presentation | 1:1 with a template; avoid the "Presenter" name. |
| **Policy Object** | Authorization, eligibility, complex read rules | Returns boolean. Memory-only; keep separate from Query Objects. |
| **Decorator** | Conditional extra responsibilities on a core object | `SimpleDelegator`; preserve the original interface. |

## Controller Rules
- REST verbs only; no bespoke action names unless necessary.
- Strong parameters always — use your Rails version's API (see `rails-7-params` or `rails-8-params`).
- One instance variable per action; use a facade otherwise.
- No business logic inside the controller body.

## Testing
- Query Objects against real DB records.
- View Objects through template rendering.
- Form Objects through controller specs.
- Model specs stay focused on persistence.

## Anti-patterns
- Callback-hell chains on models.
- Using inheritance where composition fits.
- Making controllers know about more than one object.
