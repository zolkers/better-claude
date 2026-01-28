# Bootstrap

## Detection

- `bootstrap` in `package.json` dependencies
- Bootstrap CDN links in HTML (`cdn.jsdelivr.net/npm/bootstrap`)
- `@import "bootstrap"` or `@import "~bootstrap"` in Sass/CSS files

## Conventions

Bootstrap is used as a **component library and grid system**. Use Bootstrap's built-in components and utilities. Customize via Sass variables, not by overriding classes.

### Usage Strategy (with Tailwind)

When both Tailwind and Bootstrap are present:

- **Tailwind**: Primary for custom layout, spacing, typography, and utility-first styling.
- **Bootstrap**: Use for pre-built interactive components (modals, dropdowns, toasts, accordions, carousels, tooltips, popovers) and the grid system if preferred over Tailwind's grid.
- **Avoid conflicts**: Do not apply Tailwind utilities and Bootstrap utilities for the same property on the same element. Pick one source of truth per concern.
- **Bootstrap JS components**: Use Bootstrap's JS for interactive components (or their framework-specific wrappers: `react-bootstrap`, `bootstrap-vue-next`, `ng-bootstrap`).

### Grid System

- Use Bootstrap's grid (`container`, `row`, `col-*`) OR Tailwind's grid/flex -- not both on the same layout.
- Always use the responsive grid classes: `col-12 col-md-6 col-lg-4`.
- Use `container` or `container-fluid`. Never set max-width manually when using Bootstrap's container.
- Use `g-*` (gutter) classes for spacing between columns instead of custom margins.

```html
<div class="container">
  <div class="row g-4">
    <div class="col-12 col-md-6 col-lg-4">
      <!-- Card content -->
    </div>
  </div>
</div>
```

### Components Usage

- **Use Bootstrap components as-is** for standard UI patterns. Do not rebuild modals, dropdowns, or tooltips from scratch.
- **Customize via Sass variables** before importing Bootstrap, not by overriding CSS after import.
- **Accessibility**: Bootstrap components include ARIA attributes by default. Do not remove them. Add `aria-label` where default context is insufficient.

```scss
// Customize Bootstrap BEFORE importing
$primary: #0d6efd;
$border-radius: 0.5rem;
$font-family-sans-serif: "Inter", system-ui, sans-serif;
$enable-reduced-motion: true;

@import "bootstrap/scss/bootstrap";
```

### Sass Variable Customization

- Override Bootstrap's Sass variables in a `_variables.scss` file imported before Bootstrap.
- Use Bootstrap's color functions (`shade-color()`, `tint-color()`) to generate palettes.
- Use `$spacer` and `$spacers` map for consistent spacing overrides.
- Enable only the Bootstrap modules you need to reduce bundle size:

```scss
// Import only what you need
@import "bootstrap/scss/functions";
@import "bootstrap/scss/variables";
@import "bootstrap/scss/mixins";
@import "bootstrap/scss/grid";
@import "bootstrap/scss/utilities";
@import "bootstrap/scss/buttons";
@import "bootstrap/scss/modal";
@import "bootstrap/scss/forms";
```

### Utility Classes

- Bootstrap 5 includes utility classes similar to Tailwind (`d-flex`, `p-3`, `text-center`, `bg-primary`).
- **When Tailwind is also present**: Prefer Tailwind utilities. Use Bootstrap utilities only within Bootstrap component markup for consistency.
- Use Bootstrap's `visually-hidden` class (equivalent to Tailwind's `sr-only`).

### Responsive Breakpoints

Bootstrap uses these breakpoints (align with Tailwind config if both are present):

| Breakpoint | Bootstrap class | Min-width |
|-----------|----------------|-----------|
| Extra small | (default) | `<576px` |
| Small | `sm` | `576px` |
| Medium | `md` | `768px` |
| Large | `lg` | `992px` |
| Extra large | `xl` | `1200px` |
| XXL | `xxl` | `1400px` |

**Note**: Bootstrap and Tailwind breakpoints differ slightly. When using both, pick one set as the source of truth and align the other in config.

### JavaScript Components

- Use the data attribute API (`data-bs-toggle`, `data-bs-target`) for simple interactions.
- Use the JavaScript API for programmatic control.
- Always initialize tooltips and popovers explicitly (they are opt-in).
- Clean up instances on component unmount (SPA context).

```js
// Initialize all tooltips
const tooltips = document.querySelectorAll('[data-bs-toggle="tooltip"]');
tooltips.forEach(el => new bootstrap.Tooltip(el));
```

### What NOT to Do

- Do not use Bootstrap's grid AND Tailwind's flex/grid on the same container.
- Do not override Bootstrap classes with `!important`. Customize via Sass variables.
- Do not include the full Bootstrap CSS if only using a few components. Use modular imports.
- Do not remove ARIA attributes from Bootstrap components.
- Do not mix Bootstrap 4 and Bootstrap 5 classes (e.g., `ml-3` is Bootstrap 4, `ms-3` is Bootstrap 5).

## Questions to Ask

- Exact Bootstrap version? (5.x for instance)
- Full or modular import (which components are being used)?
- Are you using Sass to customize Bootstrap or only the compiled CSS?
- Is there a framework wrapper (react-bootstrap, bootstrap-vue-next, ng-bootstrap)?
- Which interactive Bootstrap components are being used (modals, dropdowns, tooltips, toasts, etc.)?
