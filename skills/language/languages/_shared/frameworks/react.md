# React

Inherits from `_shared/conventions.md` and `javascript/javascript.md` or `typescript/typescript.md` depending on the project.

## Detection

This guide applies when `react` is found in `package.json` dependencies.

## Conventions

### Components

- Use functional components only -- no class components.
- One component per file. File name = component name in PascalCase.
- Keep components small -- if it exceeds 100 lines, extract sub-components.
- Prefer composition over prop drilling -- use context or composition patterns.
- Co-locate related files: `Button.tsx`, `Button.test.tsx`, `Button.module.css`.

### Hooks

- Only call hooks at the top level -- never inside conditions, loops, or nested functions.
- Extract repeated logic into custom hooks prefixed with `use` (e.g., `useFetch`, `useDebounce`).
- Use `useMemo` / `useCallback` only when there's a measured performance issue -- don't optimize prematurely.
- Use `useRef` for values that don't trigger re-renders (timers, previous values, DOM refs).
- Always include proper dependencies in `useEffect` / `useMemo` / `useCallback` -- no eslint-disable for exhaustive-deps.

### State Management

- Start with `useState` for local state.
- Use `useReducer` when state logic is complex (multiple sub-values, next state depends on previous).
- Use context (`useContext`) for truly global state (theme, auth, locale) -- not for everything.
- Use a state library (Zustand, Jotai, Redux Toolkit) only when context becomes unwieldy.
- Never store derived data in state -- compute it during render.

### Effects

- Every `useEffect` should do ONE thing -- split multiple concerns into separate effects.
- Always return a cleanup function for subscriptions, timers, and event listeners.
- Use `useEffect` for side effects only (fetching, subscriptions, DOM manipulation) -- not for state derivation.
- Prefer event handlers over `useEffect` when responding to user actions.

### Props

- Destructure props in the function signature.
- Use `children` for component composition instead of render props when possible.
- Use `ComponentProps<"element">` to extend native HTML element props.
- Default values via destructuring, not `defaultProps`.

```tsx
interface ButtonProps extends ComponentProps<"button"> {
  variant?: "primary" | "secondary";
  isLoading?: boolean;
}

function Button({ variant = "primary", isLoading, children, ...rest }: ButtonProps) {
  return (
    <button disabled={isLoading} className={variant} {...rest}>
      {isLoading ? <Spinner /> : children}
    </button>
  );
}
```

### Rendering

- Use `key` prop with stable, unique identifiers -- never array index (unless list is static and never reordered).
- Avoid inline object/array creation in JSX -- extract to constants or `useMemo`.
- Use fragments `<>...</>` to avoid unnecessary wrapper divs.
- Conditional rendering: `&&` for simple cases, ternary for if/else, early return for complex cases.

### Performance

- Use `React.lazy()` + `Suspense` for code splitting.
- Use `React.memo()` only on components that re-render with the same props frequently.
- Profile before optimizing -- use React DevTools Profiler.
- Virtualize long lists with `react-window` or `@tanstack/virtual`.

### Error Handling

- Use Error Boundaries for catching render errors -- at page and feature level.
- Show fallback UI, never a white screen.
- Log errors to a monitoring service (Sentry, etc.).

### Testing

- Test behavior, not implementation -- "what the user sees and does".
- Use React Testing Library -- query by role, label, text, not by test IDs.
- Avoid testing internal state or hook implementation details.
- Test user interactions: click, type, submit.

## Project Structure

```
src/
├── components/          # Shared/reusable components
│   ├── Button/
│   │   ├── Button.tsx
│   │   ├── Button.test.tsx
│   │   └── Button.module.css
│   └── ...
├── features/            # Feature-based modules
│   ├── auth/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── services/
│   │   └── types.ts
│   └── ...
├── hooks/               # Global custom hooks
├── context/             # React contexts
├── services/            # API calls
├── utils/               # Pure utility functions
├── types/               # Shared TypeScript types
├── pages/               # Route-level components (if using file-based routing)
├── App.tsx
└── main.tsx
```

## Questions to Ask

- React version (18, 19)?
- Routing: React Router, TanStack Router, Next.js?
- State management: local only, Zustand, Jotai, Redux Toolkit?
- Styling: CSS Modules, Tailwind, styled-components, Panda CSS?
- Data fetching: TanStack Query, SWR, plain fetch?
- Form handling: React Hook Form, Formik, native?
