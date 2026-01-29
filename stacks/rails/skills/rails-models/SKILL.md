---
name: rails-models
description: Apply when writing or editing ActiveRecord models under app/models/**/*.rb. Covers model responsibility, scopes, validations, enums, and extracting POROs.
---

# ActiveRecord Model Standards

## Rules
- One model = one responsibility. Models under 100 lines.
- Validate at the model level.
- Use scopes for common queries.
- Use `enum` for state machines.
- Add DB indexes for frequently queried fields.
- Extract POROs for complex calculations or domain logic.
- Keep AR models focused on persistence — push domain logic out.

## Recommended Layout
1. Constants
2. Associations
3. Validations
4. Scopes
5. Class methods
6. Instance methods

## Example

```ruby
class Order < ApplicationRecord
  include Trackable

  enum status: { pending: 0, processing: 1, completed: 2 }

  belongs_to :user
  has_many :line_items

  scope :recent, -> { where(created_at: 1.week.ago..) }

  validates :total, numericality: { greater_than: 0 }

  def calculate_tax
    TaxCalculator.new(self).compute
  end
end
```

## Anti-patterns
- Extracting model mixins into `concerns/` as a default. Prefer composition / POROs.
- Database queries spread across views or components.
