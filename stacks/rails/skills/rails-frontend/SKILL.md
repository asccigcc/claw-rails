---
name: rails-frontend
description: Apply when writing or editing frontend assets in a Rails + Vite + Tailwind app (.erb, .js, .css under app/frontend). Covers file structure, Tailwind conventions, Flowbite integration, and accessibility.
---

# Rails Frontend

## Stack
- TailwindCSS v4 for styling
- Flowbite 2.5.2 for UI components
- Vite Ruby for build tooling
- Inter Variable font

## File Structure
```
app/frontend
├── components/                   # Reusable frontend components (JS or ViewComponents)
│   └── dropdown/
│       ├── dropdown.js
│       └── dropdown.html.erb
├── controllers/
│   ├── application.js            # Stimulus setup for debug mode
│   ├── index.js                  # Auto-registers controllers; imports Flowbite
│   └── sidebar_controller.js
├── entrypoints/
│   └── application.js            # Main JS entry: imports controllers, Turbo, CSS
├── images/
│   └── svg/
├── stylesheets/
│   └── application.tailwind.css  # Tailwind imports + theme variables
└── utils/
    └── helpers.js
```

## CSS Organization
- **Entry point:** `app/frontend/entrypoints/application.js` imports `application.tailwind.css`, Stimulus controllers, and Turbo.
- **Tailwind config:** `app/frontend/stylesheets/application.tailwind.css` holds Tailwind directives, theme variables, and plugins (`@tailwindcss/forms`, `@tailwindcss/typography`).

## Design Principles
1. **Clean aesthetic** — B2B SaaS feel, minimal chrome.
2. **Responsive** — mobile, tablet, desktop must all look good.
3. **Accessibility** — ARIA, semantic HTML, WCAG color contrast.
4. **Immediate feedback** — transitions on state changes.
5. **Consistent layout** — reuse shared partials and controllers.

## Best Practices
- Prefer Tailwind utilities over custom CSS.
- Keep each Stimulus controller focused on a single behavior.
- Mobile-first responsive breakpoints.
- Reference `--color-*` and `--size-*` CSS variables for theming.
- Code-split via Vite; lazy-load large assets; compress images.
- Descriptive `aria-label`s; semantic HTML.
- Use Flowbite UI patterns, override with Tailwind for brand consistency.
- Beautify components when no strict design is defined.
- Prefer double-quoted strings in JS unless single quotes avoid escaping.
- All hardcoded user-facing text belongs in `en.yml`.

## References
- [Tailwind docs](https://tailwindcss.com/docs/installation)
- [Flowbite docs](https://flowbite.com/docs/getting-started/introduction)
- [Stimulus Handbook](https://stimulus.hotwired.dev/handbook/introduction)
