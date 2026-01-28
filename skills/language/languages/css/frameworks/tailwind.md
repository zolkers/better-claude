# Tailwind CSS

## Detection

- `tailwind.config.js` or `tailwind.config.ts` in project root
- `@tailwind` directives in CSS files
- `tailwindcss` in `package.json` dependencies

## Conventions

Tailwind is the **primary styling approach**. Write utility classes directly in HTML/JSX. Custom CSS is the exception, not the rule.

### Class Order

Follow the official Tailwind class order (enforced by `prettier-plugin-tailwindcss`):

1. **Layout**: `container`, `block`, `flex`, `grid`, `hidden`
2. **Positioning**: `static`, `relative`, `absolute`, `fixed`, `sticky`, `inset-*`, `z-*`
3. **Box Model**: `w-*`, `h-*`, `m-*`, `p-*`, `overflow-*`
4. **Flexbox/Grid children**: `basis-*`, `grow`, `shrink`, `order-*`, `col-*`, `row-*`
5. **Typography**: `font-*`, `text-*`, `leading-*`, `tracking-*`
6. **Visual**: `bg-*`, `border-*`, `rounded-*`, `shadow-*`, `opacity-*`, `ring-*`
7. **Transitions/Transforms**: `transition-*`, `duration-*`, `ease-*`, `transform`, `translate-*`, `rotate-*`, `scale-*`
8. **Interactivity**: `cursor-*`, `select-*`, `pointer-events-*`
9. **State variants**: `hover:`, `focus:`, `active:`, `disabled:`, `group-hover:`

```html
<!-- Good: ordered -->
<div class="relative flex w-full items-center gap-4 rounded-lg bg-white p-4 shadow-md transition-shadow hover:shadow-lg">

<!-- Bad: random order -->
<div class="shadow-md hover:shadow-lg bg-white p-4 flex gap-4 rounded-lg w-full relative items-center transition-shadow">
```

### Utility-First Principles

- **Prefer utilities over custom CSS**. If Tailwind has a utility, use it.
- **Extract components** (not utility classes) when patterns repeat 3+ times. Use framework components (React, Vue, Blade) or `@apply` in CSS as a last resort.
- **`@apply` is discouraged** -- it defeats the purpose of Tailwind. Use it only for base element styles that cannot be composed in markup (e.g., styling `prose` content, third-party components you cannot control).

```css
/* Acceptable @apply: base elements you cannot add classes to */
@layer base {
  h1 {
    @apply text-3xl font-bold leading-tight;
  }
}

/* Avoid: creating component classes with @apply */
/* Instead, create a reusable component in your framework */
```

### Design Tokens via Config

- Define **all** project colors, fonts, spacing, and breakpoints in `tailwind.config.js`. Never use arbitrary values (`text-[#1a2b3c]`) when a token exists.
- Arbitrary values are acceptable for **one-off cases** that don't belong in the design system (exact icon sizes, specific SVG positions).
- Extend the default theme rather than overriding it, unless a strict brand system requires it.

```js
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#eff6ff',
          500: '#3b82f6',
          700: '#1d4ed8',
          900: '#1e3a5a',
        },
        surface: '#ffffff',
        'on-surface': '#212529',
      },
      fontFamily: {
        sans: ['"Inter"', 'system-ui', 'sans-serif'],
        mono: ['"Fira Code"', 'monospace'],
      },
      spacing: {
        '18': '4.5rem',
        '88': '22rem',
      },
    },
  },
}
```

### Responsive Design

- **Mobile-first**: Unprefixed utilities = mobile. Use `sm:`, `md:`, `lg:`, `xl:`, `2xl:` for larger screens.
- **Group responsive changes** per element, not per breakpoint.

```html
<!-- Good: all responsive info in one place -->
<div class="grid grid-cols-1 gap-4 md:grid-cols-2 lg:grid-cols-3">

<!-- Bad: splitting across multiple elements for the same layout -->
```

### Dark Mode

- Use `dark:` variant. Configure `darkMode: 'class'` for manual toggle or `darkMode: 'media'` for system preference.
- Every color usage must have a dark mode counterpart where contrast changes.

```html
<div class="bg-white text-gray-900 dark:bg-gray-900 dark:text-gray-100">
```

### Accessibility with Tailwind

- Use `sr-only` for screen-reader-only text.
- Use `focus-visible:` (not `focus:`) for keyboard-only focus rings.
- Use `motion-safe:` and `motion-reduce:` for animations.
- Ensure all `bg-*` / `text-*` combinations meet WCAG AA contrast ratios.

```html
<button class="rounded bg-primary-700 px-4 py-2 text-white focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-primary-500 motion-safe:transition-colors">
  Save
</button>
<span class="sr-only">Close dialog</span>
```

### Performance

- Configure `content` paths correctly so PurgeCSS removes unused classes.
- Avoid `safelist` unless absolutely necessary.
- Use Tailwind's JIT mode (default since v3).

### Plugins

- Use `@tailwindcss/typography` for rich text / prose content.
- Use `@tailwindcss/forms` for better default form styling.
- Use `@tailwindcss/container-queries` for `@container` support.

### What NOT to Do

- Do not mix Tailwind utility classes and custom BEM classes on the same element for the same concerns (layout with Tailwind, layout also in BEM).
- Do not create `.btn-primary { @apply ... }` classes -- create a `<Button>` component instead.
- Do not use `!important` in Tailwind classes. If specificity is an issue, fix the source, not the symptom.
- Do not hardcode colors outside the config (`text-[#ff0000]` for a color used in multiple places).

## Questions to Ask

- Tailwind v3 ou v4 ?
- Dark mode par toggle (`class`) ou preference systeme (`media`) ?
- Quels plugins Tailwind sont utilises (`typography`, `forms`, `container-queries`) ?
- Y a-t-il un design system Figma avec des tokens a mapper dans le config ?
- Utilises-tu `prettier-plugin-tailwindcss` pour l'ordre des classes ?
