---
description: Script setup order, SOLID principles (SRP, Open/Closed, ISP, DI), naming conventions
applies_to: [components, views, composables]
---
# Skill: Vue 3 Code Style & SOLID Standards

> [!IMPORTANT]
> All code must adhere to the [Vue Formatting Standards](./vue-formatting-standards.md).


## 1. Script Setup Organization
Maintain this strict order:
1. Imports (External > Internal @/ > Local ./).
2. Type definitions (must follow [vue-component-standard.md](./vue-component-standard.md) and [vue-typescript-rules.md](./vue-typescript-rules.md)).
3. Macros (`defineProps`, `defineEmits`).
4. Reactive & Non-reactive data (Constants > Refs > Stores).
5. Computed properties.
6. Watchers.
7. Functions (Event handlers like `handleSubmit`).
8. Lifecycle hooks (`onMounted`).

## 2. SOLID Principles
- **SRP:** Components must NOT contain infrastructure logic (API calls). Use Composables and Services as defined in [vue-api-integration.md](./vue-api-integration.md) and [vue-composables-standard.md](./vue-composables-standard.md). For function-level granularity rules, follow [vue-granular-code.md](./vue-granular-code.md).
- **Open/Closed:** Use Slots for extensibility.
- **Interface Segregation:** Pass granular props, not large objects. Follow [vue-typescript-rules.md](./vue-typescript-rules.md) for externalized prop interfaces.
- **Dependency Inversion:** Components depend on Props/Emits, not global instances. Follow [vue-communication-logic.md](./vue-communication-logic.md) for hierarchy rules.

## 3. Naming Conventions
- Boolean: Use prefixes like `is`, `has`, `can` (e.g., `isLoading`).
- Events: Use `handle` prefix for functions (e.g., `handleUpdate`).
- **CSS Classes:** Strictly follow the BEM methodology defined in [vue-styling-standards.md](./vue-styling-standards.md).

