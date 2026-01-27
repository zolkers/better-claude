# HTML

## Conventions

Apply W3C standards. Prioritize semantic HTML over generic containers. Follow WCAG Accessibility (A11y) guidelines strictly.

### Structure & Semantics

- **Strict Semantics**: Never use `<div>` or `<span>` if a semantic tag exists (`<main>`, `<section>`, `<article>`, `<aside>`, `<header>`, `<footer>`, `<nav>`).
- **Heading Hierarchy**: Exactly one `<h1>` per page. Do not skip levels (e.g., do not jump from `<h2>` to `<h4>`).
- **Buttons vs. Links**:
    - Use `<a>` for navigation to a URL or anchor.
    - Use `<button>` for actions (form submission, opening modals, toggling state, JS triggers).
- **Lists**: Always use `<ul>`, `<ol>`, or `<dl>` for grouping repeating items. Do not use series of `<div>`s for lists.

### Syntax & Code Style

- **Indentation**: 2 spaces.
- **Lowercase**: All tags and attributes must be lowercase.
- **Quotes**: Always use double quotes `"` for attribute values.
- **Boolean Attributes**: Do not specify values (`<input disabled>` instead of `<input disabled="true">`).
- **Self-closing Tags**: Do not use the trailing slash in HTML5 (Use `<img>` instead of `<img />`).
- **Attribute Order**:
    1. `class`
    2. `id` / `name`
    3. `data-*`
    4. `src` / `for` / `type` / `href` / `value`
    5. `aria-*` / `role` / `alt`

### Accessibility (A11y)

- **Images**: Every `<img>` must have an `alt` attribute. If decorative, use `alt=""`.
- **Forms**: Every input must have an associated `<label>` using the `for`/`id` relationship. Use `placeholder` only for hints, not as a replacement for labels.
- **Contrast**: Ensure text meets minimum contrast ratios.
- **Interactivity**: All interactive elements must be focusable via keyboard and show a visible `:focus` state.
- **ARIA**: Use `aria-*` attributes only when native HTML semantics are insufficient ("No ARIA is better than bad ARIA").

### Performance & Assets

- **JS Loading**: Use `defer` (or `async` if independent) on `<script>` tags to prevent render-blocking. Place them in the `<head>`.
- **Images**: Use `loading="lazy"` for off-screen images. Use modern formats like WebP/AVIF where possible.
- **Dimensions**: Always specify `width` and `height` on images to prevent Layout Shifts (CLS).
- **Resource Hints**: Use `<link rel="preconnect">` or `<link rel="preload">` for critical assets (fonts, hero images).

### Meta & SEO

- **Language**: Always define `<html lang="en">` (or the target language).
- **Viewport**: Always include `<meta name="viewport" content="width=device-width, initial-scale=1.0">`.
- **Character Set**: Use `<meta charset="UTF-8">` as the first child of `<head>`.
- **Description**: Provide a unique `<meta name="description">` (max 160 characters) for every page.
- **Social**: Include Open Graph (`og:`) and Twitter card metas for better link sharing.

### Code Clarity & Maintenance

- **Comments**: Comment major sections (e.g., `<!-- Main Navigation -->`). Use comments to mark the end of long nested structures.
- **No Inline Styles**: Never use the `style=""` attribute.
- **No Inline Scripts**: Keep logic in external `.js` files. Avoid `onclick=""` attributes; use event listeners in JS.
- **Tables**: Use `<thead>`, `<tbody>`, and `<th>` with the `scope` attribute. Tables are for data only, never for layout.

## Project Structure

```
project/
├── index.html              # Root landing page
├── pages/                  # Sub-pages
│   ├── contact.html
│   └── about.html
├── assets/
│   ├── img/                # Compressed images & SVGs
│   ├── fonts/              # Local webfont files (woff2)
│   └── icons/              # UI icons
├── css/                    # Stylesheets
│   ├── main.css
│   └── variables.css
└── js/                     # Scripts
    ├── main.js
    └── utils/              # Helper scripts
```

## Boilerplate Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Proper Document Structure</title>
  <meta name="description" content="Brief description of the page content.">
  <link rel="stylesheet" href="css/main.css">
  <script src="js/main.js" defer></script>
</head>
<body>

  <header>
    <nav aria-label="Main menu">
      <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/about">About</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <article>
      <h1>Main Page Heading</h1>
      <p>Content goes here...</p>
      
      <figure>
        <img src="assets/img/photo.webp" alt="Description of the image" width="800" height="600" loading="lazy">
        <figcaption>Image caption</figcaption>
      </figure>

      <form action="/submit" method="post">
        <div class="form-group">
          <label for="user-email">Email Address</label>
          <input type="email" id="user-email" name="email" required>
        </div>
        <button type="submit">Send</button>
      </form>
    </article>
  </main>

  <footer>
    <p>&copy; 2026 Company Name</p>
  </footer>

</body>
</html>

```
## Questions to Ask (HTML)

- **Accessibility**: What level of accessibility is required (WCAG A, AA, or AAA)?
- **Architecture**: Is this a Multi-Page Application (MPA) or a Single-Page Application (SPA)?
- **CSS Strategy**: Which methodology are we using (Tailwind CSS, BEM, CSS Modules, or generic CSS)?
- **Browser Support**: Do we need to support legacy browsers (e.g., Safari 12) or "Evergreen" browsers only?
- **SEO**: Are there specific requirements for structured data (JSON-LD) or specific Open Graph metadata?
- **JS Integration**: Is the HTML being served statically, or will it be manipulated by a framework (React, Vue, Alpine.js)?
- **Performance**: Are there specific Core Web Vitals targets (e.g., strict LCP or CLS limits)?