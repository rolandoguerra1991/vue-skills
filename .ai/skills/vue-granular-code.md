# Skill: Granular & Modular Code

> [!IMPORTANT]
> This skill complements [vue-coding-standards.md](./vue-coding-standards.md) (SOLID) and [vue-composables-standard.md](./vue-composables-standard.md) (SRP for composables). Those files define **what** to separate; this file defines **how small** each unit must be.

## 1. Function Size & Single Purpose
- Every function must do **one thing only**. If a function name requires "and" to describe it, split it.
- Target a maximum of **~20 lines** per function (excluding type declarations). If a function exceeds this, extract helper functions.
- Prefer **pure functions** (no side effects, deterministic output) whenever possible. Isolate side effects into their own clearly-named functions.

## 2. Early Returns & Flat Logic
- Use **early returns / guard clauses** instead of deeply nested `if-else` blocks.
- Maximum nesting depth: **2 levels**. Beyond that, extract the nested logic into a named helper.

```typescript
// ❌ Bad: deep nesting
function process(user) {
  if (user) {
    if (user.isActive) {
      if (user.hasPermission) {
        // ...logic
      }
    }
  }
}

// ✅ Good: guard clauses
function process(user) {
  if (!user) return
  if (!user.isActive) return
  if (!user.hasPermission) return

  // ...logic
}
```

## 3. Extract Reusable Helpers
- Repeated logic (≥ 2 occurrences) across files MUST be extracted into a utility or helper.
- **Location:** `src/utils/` for pure functions, `src/helpers/` for context-dependent logic.
- Each utility file should export **closely related** functions only, not become a catch-all.

## 4. Composable Granularity
- Refer to [vue-composables-standard.md](./vue-composables-standard.md) for composable conventions.
- A composable that manages more than one concern (e.g., fetching + transforming + caching) MUST be split into smaller composables that compose together.

## 5. Computed & Watchers
- Complex computed properties (> 5 lines of logic) must delegate their logic to an extracted function.
- Watchers should call a named handler function, not contain inline logic.

```typescript
// ❌ Bad: inline logic in watcher
watch(query, (newVal) => {
  // ...15 lines of filtering, sorting, mapping
})

// ✅ Good: delegated to named function
watch(query, (newVal) => handleQueryChange(newVal))

function handleQueryChange(value: string) {
  const filtered = filterItems(value)
  const sorted = sortByRelevance(filtered)
  items.value = sorted
}
```

## 6. Template Expressions
- Template expressions must be **simple**. Any expression that involves more than a single method call or ternary MUST be extracted to a `computed` property or method in `<script setup>`.
