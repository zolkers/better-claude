# CSS

## Conventions

Apply consistent, maintainable styling. Follow WCAG accessibility guidelines. Prioritize performance, readability, and scalability. Use modern CSS features (custom properties, logical properties, container queries).

### Methodology: BEM

Use **BEM (Block__Element--Modifier)** for all custom CSS classes:

- **Block**: Standalone component (`.card`, `.nav`, `.form`).
- **Element**: Part of a block, prefixed with `__` (`.card__title`, `.card__body`).
- **Modifier**: Variation of a block or element, prefixed with `--` (`.card--featured`, `.card__title--large`).
- Never nest BEM selectors deeper than `.block__element--modifier`. No `.block__element__sub-element`.
- If an element needs its own sub-elements, promote it to a new block.

```css
/* Good */
.card { }
.card__header { }
.card__header--sticky { }

/* Bad -- too deep */
.card__header__title__icon { }
```

### Syntax & Code Style

- **Indentation**: 2 spaces.
- **One declaration per line**. Never write multiple declarations on a single line.
- **Opening brace** on the same line as the selector, preceded by a space.
- **Closing brace** on its own line, aligned with the selector.
- **Colon**: Followed by a single space (`color: red;` not `color:red;`).
- **Semicolons**: Always include the trailing semicolon on the last declaration.
- **Quotes**: Use double quotes (`font-family: "Open Sans"`, `url("image.png")`).
- **Zero values**: Omit the unit (`margin: 0;` not `margin: 0px;`).
- **Shorthand**: Use shorthand properties when setting all sides (`margin: 1rem` not `margin: 1rem 1rem 1rem 1rem`). Avoid shorthand when overriding only one side.
- **Colors**: Use lowercase hex (`#fff`, `#1a2b3c`). Use `hsl()` or CSS custom properties for theme colors.
- **Nesting**: Max 3 levels deep. Prefer flat selectors.

### Declaration Order

Order declarations by category:

1. **Layout** (`display`, `position`, `top/right/bottom/left`, `z-index`, `float`, `flex`, `grid`)
2. **Box Model** (`width`, `height`, `margin`, `padding`, `border`, `overflow`)
3. **Typography** (`font-*`, `line-height`, `text-*`, `letter-spacing`, `color`)
4. **Visual** (`background-*`, `box-shadow`, `opacity`, `border-radius`, `outline`)
5. **Animation** (`transition`, `transform`, `animation`)
6. **Misc** (`cursor`, `pointer-events`, `user-select`)

### Custom Properties (CSS Variables)

- Define global design tokens on `:root`.
- Prefix custom properties by domain: `--color-*`, `--font-*`, `--spacing-*`, `--radius-*`, `--shadow-*`.
- Use semantic names, not literal values (`--color-primary` not `--color-blue`).
- Support dark mode via `prefers-color-scheme` or a `.dark` class toggle.

```css
:root {
  --color-primary: #0d6efd;
  --color-on-primary: #fff;
  --color-surface: #fff;
  --color-on-surface: #212529;
  --color-error: #dc3545;

  --font-family-base: "Inter", system-ui, sans-serif;
  --font-family-mono: "Fira Code", monospace;
  --font-size-base: 1rem;

  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 3rem;

  --radius-sm: 0.25rem;
  --radius-md: 0.5rem;
  --radius-lg: 1rem;
  --radius-full: 9999px;
}
```

### Typography Standards

- **Base font size**: `16px` minimum (`1rem`). Never go below 14px for any readable text.
- **Line height**: `1.5` for body text, `1.2`-`1.3` for headings.
- **Line length**: Max `70-80 characters` per line (`max-width: 70ch` on text containers).
- **Type scale**: Use a consistent ratio (1.25 "Major Third" recommended). Example: 0.75rem, 1rem, 1.25rem, 1.563rem, 1.953rem, 2.441rem.
- **Font families**: Max 2 families per project (one sans-serif, one mono). Max 3 weights per family.
- **Fluid typography**: Use `clamp()` for responsive sizing instead of breakpoints.

```css
h1 { font-size: clamp(1.75rem, 4vw, 2.5rem); }
h2 { font-size: clamp(1.5rem, 3vw, 2rem); }
body { font-size: clamp(1rem, 1.2vw, 1.125rem); }
```

### Color & Contrast (Accessibility)

- **WCAG AA minimum**: Contrast ratio of `4.5:1` for normal text, `3:1` for large text (>=18px bold or >=24px).
- **Never rely on color alone** to convey information. Always pair with icons, text labels, borders, or underlines.
- **Status colors**:
  - Success: green + checkmark icon
  - Error: red + cross icon + text message
  - Warning: amber + warning icon
  - Info: blue + info icon
- **Dark mode**: Maintain contrast ratios. Reduce pure white (`#f5f5f5` instead of `#fff`) to avoid eye strain. Use overlays with 40%+ opacity under text on images.
- Test with tools: Chrome DevTools contrast checker, axe DevTools, Lighthouse.

### Spacing System

- Use a **consistent spacing scale** based on `rem` and a base unit (e.g., 4px / 0.25rem increments).
- Standard scale: `0.25rem` (4px), `0.5rem` (8px), `0.75rem` (12px), `1rem` (16px), `1.5rem` (24px), `2rem` (32px), `3rem` (48px), `4rem` (64px).
- **Paragraph spacing** > line spacing. Heading margin-bottom > paragraph margin-bottom.
- **Component internal spacing**: Consistent padding within cards, modals, sections.
- Use `gap` (Flexbox/Grid) instead of margins between sibling elements when possible.

### Layout & Responsive Design

- **Mobile-first**: Write base styles for mobile, enhance with `min-width` media queries.
- **Fluid layouts**: Use `Flexbox` and `CSS Grid`. Avoid fixed pixel widths.
- **Container queries**: Use `@container` for component-level responsiveness.
- **Breakpoints** (standard):
  - `sm`: `640px`
  - `md`: `768px`
  - `lg`: `1024px`
  - `xl`: `1280px`
  - `2xl`: `1536px`
- **Touch targets**: Minimum `44x44px` for interactive elements (WCAG 2.5.5).
- **Text resizing**: Layout must not break when text is zoomed to 200%.
- Use `rem`/`em` for sizing. Reserve `px` only for borders, box-shadows, and fine details.

### Interaction & Animation

- **Transitions**: Prefer `150ms`-`300ms` for UI transitions. Use `ease-out` for entrances, `ease-in` for exits.
- **Reduce motion**: Always respect `prefers-reduced-motion`. Disable or simplify animations for users who need it.
- **Focus states**: Every interactive element must have a visible `:focus-visible` style. Never use `outline: none` without a replacement.
- **Hover states**: Provide visual feedback. Do not hide critical information behind hover-only interactions (fails on touch devices).

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}

:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}
```

### Component UX Patterns

- **Buttons**: Min height `40px`, min padding `0.5rem 1rem`. Primary action = filled, secondary = outlined, destructive = red variant. Disable with `aria-disabled` + visual dimming.
- **Forms**: Labels always visible (no placeholder-only inputs). Error messages below the field, red text + icon. Group related fields with `<fieldset>`. Inline validation on blur.
- **Modals/Dialogs**: Trap focus inside. Close on `Escape` key. Dim background with overlay. Max width `500-600px`.
- **Cards**: Consistent border-radius, shadow, padding. Click target covers entire card when card is a link.
- **Navigation**: Current page indicated visually (`aria-current="page"`). Hamburger menu on mobile with smooth open/close transition.
- **Loading states**: Show skeleton or spinner for actions >500ms. Never leave the user without feedback.
- **Toasts/Notifications**: Auto-dismiss after 5-8 seconds. Include close button. Use `role="alert"` for errors, `role="status"` for info.

### Performance

- **Critical CSS**: Inline above-the-fold styles. Lazy-load the rest.
- **Minimize reflows**: Avoid layout-triggering properties in animations (use `transform` and `opacity`).
- **`will-change`**: Use sparingly and only on elements about to animate.
- **Unused CSS**: Remove dead CSS. Use tools like PurgeCSS (auto with Tailwind).
- **Images in CSS**: Use `image-set()` for responsive backgrounds. Prefer CSS gradients over image files when possible.

### Security

- Never inject user-generated content into CSS via `style` attributes or `var()` without sanitization.
- Use Content Security Policy (CSP) headers to restrict inline styles in production.
- Avoid `@import` in CSS (blocks parallel downloads); use build tool bundling instead.

## Questions to Ask

- Souhaites-tu des commentaires dans le CSS (par section, minimaux, ou aucun) ?
- Quel niveau d'accessibilite vises-tu (WCAG A, AA, ou AAA) ?
- Le projet cible-t-il des navigateurs legacy (Safari 12, IE11) ou evergreen uniquement ?
- Y a-t-il une charte graphique / design system existant (couleurs, typo, spacing) ?
- Le projet utilise-t-il un pre-processeur (Sass, PostCSS) ou du CSS natif uniquement ?
- Y a-t-il des contraintes de performance specifiques (Core Web Vitals targets) ?
- Dark mode requis ?

## Project Structure

```
css/
├── base/
│   ├── reset.css            # Reset / Normalize
│   ├── variables.css        # CSS custom properties (tokens)
│   └── typography.css       # Base type styles
├── components/
│   ├── button.css
│   ├── card.css
│   ├── modal.css
│   └── form.css
├── layout/
│   ├── header.css
│   ├── footer.css
│   ├── sidebar.css
│   └── grid.css
├── utilities/
│   └── helpers.css          # Utility classes (.sr-only, .visually-hidden, etc.)
├── themes/
│   ├── light.css
│   └── dark.css
└── main.css                 # Imports all partials
```
