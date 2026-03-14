---
description: Component/view directory structure — index.vue, types.ts, styles.css, index.spec.ts
applies_to: [components, views, layouts]
---
# Skill: Vue 3 Component & View Standard

## Tech Stack
- Framework: Vue 3 (Composition API with `<script setup lang="ts">`).
- Styles: Pure CSS with **BEM** methodology — see [vue-styling-standards.md](./vue-styling-standards.md) (canonical).
- Testing: Vitest + Vue Test Utils.

## File Structure
Every component or view/page must reside in its own directory: `[Name]/`
1. `index.vue`: Template and logic.
2. `types.ts`: Exported interfaces for Props, Emits, and local types.
3. `styles.css`: Scoped styles following BEM.
4. `index.spec.ts`: Unit tests.

## Implementation Rules
- **Template:** Use a single root (Fragment) when possible. 
- **Async:** If the component uses top-level await, ensure it's wrapped in a `<Suspense>` boundary in its parent.
- **BEM:** The main block class must match the component name in kebab-case. See [vue-styling-standards.md](./vue-styling-standards.md) for full BEM rules.