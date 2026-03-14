---
description: Component/view directory structure — index.vue, types.ts, styles.css, index.spec.ts
applies_to: [components, views, layouts]
---
# Skill: Vue 3 Component & View Standard

## Standard Alignment
- **Architecture:** Components/Views must follow the [Architectural Roles](./vue-architecture-roles.md).
- **Styles:** Follow the [Styling Standards](./vue-styling-standards.md) (BEM + Tailwind v4).
- **Testing:** Follow the [Unit Testing Standards](./vue-unit-testing.md) (Vitest).
- **Coding:** Follow the [Coding Standards](./vue-coding-standards.md) (<script setup> + SOLID).


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