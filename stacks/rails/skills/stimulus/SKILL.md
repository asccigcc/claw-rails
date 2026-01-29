---
name: stimulus
description: Apply when writing or editing Stimulus controllers under app/frontend/controllers/**/*.js. Covers controller structure, data-attribute patterns, targets, actions, and auto-registration.
---

# Stimulus Controller Standards

## Principles
- One controller, one responsibility. Keep them small.
- Configure via `data-*` attributes, not global state.
- Define targets and actions explicitly.
- JSDoc on the class; document public methods.
- Do NOT manually register controllers in `index.js` — they are auto-registered:
  ```js
  const controllers = import.meta.glob("./**/*_controller.js", { eager: true });
  ```

## Pattern
```javascript
// app/frontend/controllers/dropdown_controller.js
import { Controller } from "@hotwired/stimulus"

/**
 * Dropdown controller handles toggle menu behavior.
 * @example
 * <div data-controller="dropdown">
 *   <button data-action="dropdown#toggle">Toggle</button>
 *   <div data-dropdown-target="menu">Content</div>
 * </div>
 */
export default class extends Controller {
  static targets = ["menu"]

  toggle() {
    this.menuTarget.classList.toggle("hidden")
  }
}
```

## Checklist
1. Data attributes for configuration.
2. Clear target definitions.
3. Explicit action mapping.
4. Error handling where user input or network is involved.
5. Test complex behaviors with system specs.

## Reference
- [Stimulus Handbook](https://stimulus.hotwired.dev/handbook/introduction)
