---
description: Naming conventions for functions, composables, events, and utilities to make logic readable at a glance
applies_to: [components, composables, utilities]
---
# Skill: Naming Conventions for Vue/TypeScript

> [!IMPORTANT]
> Names should make logic intuitive. Apply naming conventions to improve readability, but avoid over-engineering names; keep them concise and meaningful.

## 1. Purpose
- A well-chosen name should let a reader understand the intent without reading the implementation.
- Good names reduce cognitive load and help maintainers identify behavior quickly.

## 2. General Naming Rules
- Use verbs for actions: `fetchUser`, `saveDraft`, `toggleModal`.
- Use nouns for data and state: `userProfile`, `formState`, `errorMessage`.
- Prefix boolean variables with `is`, `has`, or `can`: `isLoading`, `hasPermission`, `canSubmit`.
- Avoid vague names like `doStuff`, `processData`, `handleThing`.
- If a name requires `and` or multiple verbs, split the function into smaller pieces.

## 3. Function Naming Patterns
- `fetch*` for async retrieval: `fetchUserProfile`, `fetchOrderDetails`.
- `get*` for derived read-only values: `getUserName`, `getCartTotal`.
- `set*` for explicit mutation: `setPage`, `setErrorMessage`.
- `create*` for factory or builder functions: `createValidator`, `createApiClient`.
- `calculate*` / `compute*` for derived results: `calculateTotalPrice`, `computeRemainingTime`.
- `validate*` for validation logic: `validateEmail`, `validateFormData`.

## 4. Composable Naming
- Always prefix composables with `use`: `useAuth`, `useFetch`, `useWindowSize`.
- The filename should exactly match the exported composable name.
- Name composables by their domain or capability: `useFormValidation`, `useThemeSwitcher`, `useTodoFilters`.

## 5. Event Handlers and Callbacks
- Use `handle*` for UI event handlers: `handleSubmit`, `handleInputChange`, `handleUpdate`.
- Use `on*` for lifecycle or callback hooks: `onMouseEnter`, `onRouteChange`, `onSaveSuccess`.
- Reserve `handle*` for internal behavior and `on*` for externally consumed callbacks.

## 6. Utilities and Helpers
- Keep utility names simple and explicit: `formatCurrency`, `normalizeSearchTerm`, `mergeFormPermissions`.
- Prefer local helper names when the function is component-specific.
- Use consistent verbs for repeated behavior across the codebase.

## 7. When to Apply These Rules
- Use them whenever you write new functions or refactor existing ones.
- Do not rename without purpose. If the current name already communicates intent, keep it.
- Apply stricter naming when the code is shared or reused by multiple features.

## 8. Integration with Existing Skills
- This skill complements [vue-coding-standards.md](./vue-coding-standards.md) and [vue-composables-standard.md](./vue-composables-standard.md).
- It supports refactoring by making naming decisions explicit and consistent.
