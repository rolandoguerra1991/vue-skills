---
description: v-once, v-memo, shallowRef, async components, computed caching, event modifiers
applies_to: [components, views, composables]
---
# Skill: Vue Performance Standards

## 1. Using v-once and v-memo
- Use `v-once` for static content that never changes during the component's lifecycle.
- Use `v-memo` for large lists or complex components where you want to explicitly define the dependencies that should trigger a re-render.

```html
<!-- Only re-renders if user.id or user.name changes -->
<div v-for="user in users" :key="user.id" v-memo="[user.id, user.name]">
  {{ user.name }}
</div>
```

## 2. shallowRef vs ref
- Use `shallowRef` when dealing with large objects, arrays, or third-party instances (like a map object or a chart instance) where you don't need deep reactivity.
- Deep reactivity (`ref` or `reactive`) on massive arrays (e.g., thousands of rows from an API) can cause significant memory and CPU overhead.

```typescript
// Good: Large dataset where only the entire array replacement matters
import { shallowRef } from 'vue'
const massiveDataList = shallowRef([])

// Reassigning triggers reactivity
massiveDataList.value = newArray
```

## 3. Async Components
- Use `defineAsyncComponent` for heavy components that are not immediately visible (e.g., Modals, Tooltips, or complex charts hidden behind a tab).
- This prevents loading the JavaScript for these components until they are actually needed.

```typescript
import { defineAsyncComponent } from 'vue'

const HeavyChart = defineAsyncComponent(() => import('./components/HeavyChart.vue'))
```

## 4. Computed Properties Caching
- Ensure computed properties do not have side effects.
- Computed properties are cached based on their reactive dependencies. Keep them pure to maximize performance.
- Avoid placing heavy calculations directly inside the `<template>`. Always use a `computed` property.

## 5. Event Modifiers
- Use Vue's built-in event modifiers (`.passive`, `.stop`, `.prevent`) native to the template instead of calling them manually in methods.
- Specifically, use `.passive` on scrolling or touching events (`@scroll.passive`) to improve scroll performance on mobile devices.
