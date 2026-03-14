---
description: Composable naming, reactivity returns, cleanup, flexible arguments, SRP
applies_to: [composables]
---
# Skill: Vue 3 Composables Standard

## 1. Naming Conventions
- Always prefix composables with `use` (e.g., `useAuth`, `useFetch`, `useWindowSize`).
- The filename should exactly match the name of the exported composable function (e.g., `useFetch.ts`).

## 2. Returning Reactivity
- Always return an object containing refs (`ref`) or reactive variables, along with functions.
- If you use `reactive()` internally for grouping state, use `toRefs()` when returning it so the consuming component can safely destructure it without losing reactivity.

```typescript
// Good: Safe to destructure
import { reactive, toRefs } from 'vue'

export function useUserStatus() {
  const state = reactive({
    isActive: false,
    lastSeen: Date.now()
  })

  function toggleStatus() {
    state.isActive = !state.isActive
  }

  // toRefs ensures properties remain reactive when destructured
  return {
    ...toRefs(state),
    toggleStatus
  }
}
```

## 3. Managing Side Effects and Cleanup
- Composables that set up event listeners, timers, subscriptions, or **async API requests** MUST clean them up when the component is unmounted.
- Use `onUnmounted` inside the composable to guarantee cleanup (e.g., removing listeners or calling `abort()` on an `AbortController`).

```typescript
// Good: Self-cleaning composable
import { ref, onMounted, onUnmounted } from 'vue'

export function useEventListener(target, event, callback) {
  onMounted(() => target.addEventListener(event, callback))
  onUnmounted(() => target.removeEventListener(event, callback))
}
```

## 4. Flexible Arguments
- Composables should gracefully accept both plain values and `ref` / `ComputedRef` as arguments if they depend on reactive inputs.
- Use `toValue` (Vue 3.3+) or `unref()` to unwrap these arguments internally.

```typescript
// Good: Accepts plain strings or refs
import { ref, watchEffect, toValue, MaybeRefOrGetter } from 'vue'

export function useFeatureFlag(flagName: MaybeRefOrGetter<string>) {
  const isEnabled = ref(false)

  watchEffect(() => {
    // toValue unwraps refs/getters, or returns the primitive
    const name = toValue(flagName)
    isEnabled.value = checkFlag(name)
  })

  return { isEnabled }
}
```

## 5. Single Responsibility Principle
- A composable should do one thing and do it well. 
- Avoid creating "God Composables" that manage fetching, global state, and DOM manipulation all at once. Break them down into smaller, composable pieces.
- Composables can and should call other composables if necessary to build complex logic out of smaller blocks.
