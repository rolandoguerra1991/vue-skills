# Skill: Vue 3 API Integration Standard

> [!IMPORTANT]
> **Dependencies:** `axios`


## 1. Primary Principle: Legibility over Complexity
- Code must be "self-documenting". An engineer reading the component should understand the data flow in seconds.
- Technical boilerplate (cancellation, loading states, error handling) must be encapsulated.
- **True Source of Typing:** The API response type dictates the type of the reactive variable.

## 2. Separation of Concerns (Repository Pattern)
- Define services as simple objects. Always externalize types in `./types.ts` as per `vue-component-standard.md`.

```typescript
// Good: types.ts
export interface UserProfile {
  id: string
  userName: string
  email: string
}

// Good: user.service.ts
import type { UserProfile } from './types'

export const UserService = {
  fetchProfile(userId: string, signal?: AbortSignal): Promise<UserProfile> {
    return api.get(`/users/${userId}/profile`, { signal }).then(res => res.data)
  }
}
```

## 3. Automated Request Cancellation (useApi)
- Cancellation logic is internal. The response type is inferred from the service method.

```typescript
// Good: Generic useApi Composable
import { onUnmounted, ref, shallowRef, type ShallowRef } from 'vue'

export function useApi<T, Args extends any[]>(
  apiCall: (...args: [...Args, AbortSignal?]) => Promise<T>
) {
  let controller: AbortController | null = null
  const data = shallowRef<T | null>(null)
  const isLoading = ref(false)
  const error = ref<Error | null>(null)

  const execute = async (...args: Args) => {
    if (controller) controller.abort()
    controller = new AbortController()
    isLoading.value = true
    
    try {
      const result = await apiCall(...args, controller.signal)
      data.value = result // Type T is preserved here
      return result
    } catch (err: any) {
      if (err.name !== 'AbortError') throw err
    } finally {
      isLoading.value = false
    }
  }

  onUnmounted(() => controller?.abort())

  return { data, isLoading, error, execute }
}
```

## 4. Industry Standard: Multiple Parallel Calls
- Use parallelism to avoid waterfalls. Keep the component logic high-level.

```vue
<!-- Good: UserProfileView.vue -->
<script setup lang="ts">
import { onMounted } from 'vue'
import { UserService } from './services/user.service'
import { PostService } from './services/post.service'
import { useApi } from '@/composables/useApi'

// Types are automatically inferred from the services
const { data: user, execute: fetchUser } = useApi(UserService.fetchProfile)
const { data: posts, execute: fetchPosts } = useApi(PostService.fetchUserPosts)

async function loadPageData(id: string) {
  // Parallel execution following KISS principle
  await Promise.all([
    fetchUser(id),
    fetchPosts(id)
  ])
}

onMounted(() => loadPageData('user-123'))
</script>
```

## 5. Types and Interfaces
- **Mandatory:** Do not use `any`.
- If a variable uses data from an API, it must share the same interface defined in the service's response.
