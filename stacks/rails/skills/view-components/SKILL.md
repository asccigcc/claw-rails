---
name: view-components
description: Apply when creating or editing ViewComponents under app/view/components/**/*. Covers the sidecar pattern, style variants, slots, Stimulus/Turbo integration, and testing.
---

# ViewComponent Patterns

ViewComponents over partials for reusability, testability, and performance.

## Categories
1. **Base** — atomic UI (button, card, input).
2. **Composite** — combinations of base components (search form, notification tray).
3. **Domain** — business-specific (`ProductCard`, `UserProfile`).

## Organization
- Location: `app/view/components/`.
- **Sidecar pattern:** each component is a folder containing the Ruby class, template, preview, CSS, JS.
- One component = one visual element.
- Use **slots** for flexible content.
- Templates handle presentation only.

## Data Handling
- **Never query the DB from a component.** Pass prepared data via `initialize`.
- Controllers / services / query objects do the fetching.
- Use a presenter for non-trivial transformations before passing data in.
- Remember components may render many times per page — avoid hidden N+1s.

## Style Variants
Use `ViewComponentContrib::StyleVariants`.

```ruby
class ButtonComponent < ViewComponent::Base
  include ViewComponentContrib::StyleVariants

  style do
    base { %w[font-medium bg-blue-500 text-white rounded-full] }
    variants do
      color { primary { %w[bg-blue-500 text-white] } secondary { %w[bg-purple-500 text-white] } }
      size  { sm { "text-sm" } md { "text-base" } lg { "px-4 py-3 text-lg" } }
      disabled { yes { "opacity-75" } }
    end
    defaults { { size: :md, color: :primary } }
  end

  def initialize(size: nil, color: nil, disabled: false)
    @size = size
    @color = color
    @disabled = disabled
  end
end
```

```erb
<button class="<%= style(size: @size, color: @color, disabled: @disabled) %>">Click me</button>
```

## Stimulus Integration
Provide an identifier helper in `ApplicationViewComponent`:

```ruby
class ApplicationViewComponent < ViewComponent::Base
  private

  def identifier
    @identifier ||= self.class.name.sub("::Component", "").underscore.split("/").join("--")
  end

  alias_method :controller_name, :identifier
end
```

```erb
<div data-controller="<%= controller_name %>">
  <!-- markup -->
</div>
```

Wrap in a Turbo Frame when the component updates partially:

```erb
<turbo-frame id="<%= controller_name %>">
  <div data-controller="<%= controller_name %>">
    <!-- markup -->
  </div>
</turbo-frame>
```

## Testing
- RSpec component specs on rendered HTML.
- Assert dynamic class changes from style variants.
- Use Dry (`Dry::Struct`, `Dry::Monads`) when component inputs need strong typing.

## Performance
- Components should be cacheable. Use Russian Doll caching for nested ones.
- Custom cache keys for dynamic content.

## Migration
When converting views to components:
1. Identify reusable patterns.
2. Extract components gradually.
3. Keep backward compatibility until migrated.
