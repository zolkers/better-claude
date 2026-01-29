# Vue.js

Inherits from `_shared/conventions.md` and `javascript/javascript.md` or `typescript/typescript.md` depending on the project.

## Detection

This guide applies when `vue` is found in `package.json` dependencies or `.vue` files are present.

## Conventions

### Components

- Use `<script setup>` syntax (Composition API) -- no Options API for new code.
- One component per `.vue` file. File name in PascalCase: `UserCard.vue`.
- Keep components under 200 lines -- extract sub-components when growing.
- Use SFC order: `<script setup>`, `<template>`, `<style scoped>`.
- Prefix base/layout components: `BaseButton.vue`, `BaseInput.vue`.

### Composition API

- Use `ref()` for primitives, `reactive()` for objects -- prefer `ref()` for consistency.
- Use `computed()` for derived state -- never store computed values in `ref()`.
- Use `watch()` / `watchEffect()` sparingly -- prefer computed or event-driven updates.
- Extract reusable logic into composables prefixed with `use` (e.g., `useFetch`, `useAuth`).
- Always use `toRef()` / `toRefs()` when destructuring reactive objects to maintain reactivity.

```vue
<script setup lang="ts">
import { ref, computed } from "vue";

const count = ref(0);
const doubled = computed(() => count.value * 2);

function increment() {
  count.value++;
}
</script>

<template>
  <button @click="increment">{{ count }} ({{ doubled }})</button>
</template>
```

### Props & Emits

- Always type props with `defineProps<T>()` (TypeScript) or validation objects.
- Always type emits with `defineEmits<T>()`.
- Use `withDefaults()` for default prop values.
- Props are readonly -- never mutate them. Emit events to communicate upward.

```vue
<script setup lang="ts">
interface Props {
  title: string;
  count?: number;
}

const props = withDefaults(defineProps<Props>(), {
  count: 0,
});

const emit = defineEmits<{
  update: [value: number];
}>();
</script>
```

### State Management

- Start with component state + props/emits.
- Use `provide` / `inject` for ancestor-to-descendant data passing.
- Use Pinia for global state -- one store per domain (useAuthStore, useCartStore).
- Pinia stores: prefer setup syntax (`defineStore` with function) over options syntax.
- Never access stores outside of setup or composables.

### Routing

- Use Vue Router with `<RouterView>` and `<RouterLink>`.
- Use lazy loading for routes: `() => import("./views/UserPage.vue")`.
- Use navigation guards for auth: `beforeEach` at router level, not per-component.
- Use `useRoute()` and `useRouter()` composables, not `this.$route`.

### Templates

- Use `v-bind` shorthand `:prop`.
- Use `v-on` shorthand `@event`.
- Use `v-for` with `:key` -- always a unique stable ID, never index.
- Avoid `v-if` and `v-for` on the same element -- use a wrapper or computed filter.
- Use `<component :is="...">` for dynamic components.
- Use `<Teleport>` for modals and overlays.

### Styling

- Always use `<style scoped>` to avoid global CSS leaks.
- Use CSS variables or `:deep()` for styling child components.
- Use `<style module>` for CSS Modules when class names need to be referenced in JS.

### Performance

- Use `defineAsyncComponent` for heavy components.
- Use `v-once` for static content that never changes.
- Use `shallowRef` / `shallowReactive` for large objects that don't need deep reactivity.
- Use `KeepAlive` for tabs/routes that should preserve state.

### Testing

- Use Vitest + Vue Test Utils for unit tests.
- Test component behavior, not internal state.
- Use `mount()` for tests that need child components, `shallowMount()` for isolated tests.
- Test emitted events: `wrapper.emitted("update")`.

## Project Structure

```
src/
├── components/          # Shared components
│   ├── base/            # BaseButton, BaseInput, etc.
│   └── layout/          # AppHeader, AppSidebar, etc.
├── composables/         # Reusable composition functions
│   ├── useFetch.ts
│   └── useAuth.ts
├── views/               # Route-level pages
│   ├── HomePage.vue
│   └── UserPage.vue
├── stores/              # Pinia stores
│   ├── auth.store.ts
│   └── cart.store.ts
├── router/              # Vue Router config
│   └── index.ts
├── services/            # API calls
├── types/               # TypeScript types
├── utils/               # Pure helpers
├── assets/              # Static assets (images, fonts)
├── App.vue
└── main.ts
```

## Questions to Ask

- Vue version (3.3+, 3.4+, latest)?
- TypeScript or plain JavaScript?
- State management: Pinia (recommended) or local only?
- Routing: Vue Router, file-based (Nuxt)?
- Styling: Tailwind, UnoCSS, CSS Modules, scoped CSS?
- SSR needed: Nuxt.js or SPA only?
- Testing: Vitest + Vue Test Utils, Cypress?
