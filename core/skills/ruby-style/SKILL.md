---
name: ruby-style
description: Apply when writing or editing Ruby code (.rb files). Covers Ruby 3.x syntax conventions, hash shorthand, string quoting, and Sandi Metz class/method sizing rules.
---

# Ruby Style

## Syntax
- Ruby 3.x syntax.
- `snake_case` for methods and variables, `CamelCase` for classes/modules, `SCREAMING_SNAKE_CASE` for constants.
- Prefer string interpolation over concatenation.
- Modern hash syntax: `{ key: value }`.
- Shorthand hash syntax when a local variable name exactly matches the key:

  ```ruby
  age = 49
  user = { name: "David", age: }
  ```

- Double quotes only when interpolation is needed; single quotes otherwise.

## Sandi Metz Sizing Rules
| Rule | Limit | Notes |
| --- | --- | --- |
| Class length | ≤ 100 lines | Extract secondary concerns to new classes. |
| Method length | ≤ 5 lines | `if`/`else` branches limited to 1 line each. |
| Method params | ≤ 4 | Hash options count as parameters. Rails view helpers exempt. |
| Controller instance vars | 1 per action | Use a facade if more are needed. Prefix unused with `_`. |

Exceptions require an SRP justification, agreed with the reviewer.

## Examples

<example>
  ```ruby
  def validate_user
    return if anonymous?
    check_email_format
    check_age_restriction
  end
  ```
</example>

<example type="invalid">
  ```ruby
  def validate_user(user, rules, options, extras, context) # > 4 params
    if user.anonymous?
      user.skip!
    else
      rules.each { |r| r.apply(user, options, extras, context) } # > 5 lines
    end
  end
  ```
</example>
