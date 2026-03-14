---
description: Pinia setup stores, storeToRefs, async actions, modular stores, local vs global state
applies_to: [stores, state]
---
# Skill: Vue Pinia State Management

> [!IMPORTANT]
> **Dependencies:** `pinia` (See [vue-project-setup.md](./vue-project-setup.md))


## 1. Setup Stores over Options Stores
- Always write Pinia stores using the [Setup Store](https://pinia.vuejs.org/core-concepts/#Setup-function) syntax (Composition API style).
- **Why:** Better TypeScript inference, identical to component syntax, easier to share composables inside stores.

```typescript
// Good: Setup Store
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'

export const useUserStore = defineStore('user', () => {
  // State
  const userData = ref(null)
  
  // Getters
  const isLoggedIn = computed(() => !!userData.value)
  
  // Actions
  function login(data) {
    userData.value = data
  }

  return { userData, isLoggedIn, login }
})
```

## 2. Ref vs Reactive in Stores
- Use `ref()` for primitives and objects that might be reassigned completely.
- Use `reactive()` ONLY when you need to maintain a constant reference to an object and update its properties mutably.
- Consistency: Defaulting to `ref()` and `.value` is preferred for uniformity unless `reactive` provides a specific benefit.

## 3. Asynchronous Actions
- Handle side effects (API calls, complex async logic) within actions.
- Always use `async/await` and handle errors cleanly (e.g., try/catch blocks).
- State updates reflecting loading status (`isLoading.value = true/false`) should wrap the async call.

## 4. Deconstructing Stores
- Never destructure properties directly from a store without `storeToRefs`, as they will lose reactivity.
- Actions/functions can be destructured directly.

```typescript
import { storeToRefs } from 'pinia'
import { useUserStore } from '@/stores/user'

const userStore = useUserStore()
// State & Getters MUST use storeToRefs
const { userData, isLoggedIn } = storeToRefs(userStore)
// Actions can be destructured directly
const { login } = userStore
```

## 5. Scope and Modularity
- Create one store per domain/feature (e.g., `useAuthStore`, `useCartStore`, `useSettingsStore`).
- Avoid "Monolithic Stores" that contain unrelated data.
- Stores can use other stores simply by importing and calling them inside an action or getter.

## 6. Local State vs Global State
- **Rule of Thumb:** If data is ONLY needed by a component and its immediate children, use local state (`ref`/`reactive` inside the component).
- If data is shared across multiple disconnected components, pages, or persists across navigation, use a Pinia Store.
