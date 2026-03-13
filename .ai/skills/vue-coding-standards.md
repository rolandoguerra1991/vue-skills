# Skill: Vue 3 Code Style & SOLID Standards

## 1. Script Setup Organization
Maintain this strict order:
1. Imports (External > Internal @/ > Local ./).
2. Type definitions (if not in `types.ts`).
3. Macros (`defineProps`, `defineEmits`).
4. Reactive & Non-reactive data (Constants > Refs > Stores).
5. Computed properties.
6. Watchers.
7. Functions (Event handlers like `handleSubmit`).
8. Lifecycle hooks (`onMounted`).

## 2. SOLID Principles
- **SRP:** Components must NOT contain infrastructure logic (API calls). Use Composables.
- **Open/Closed:** Use Slots for extensibility.
- **Interface Segregation:** Pass granular props, not large objects.
- **Dependency Inversion:** Components depend on Props/Emits, not global instances.

## 3. Naming Conventions
- Boolean: Use prefixes like `is`, `has`, `can` (e.g., `isLoading`).
- Events: Use `handle` prefix for functions (e.g., `handleUpdate`).