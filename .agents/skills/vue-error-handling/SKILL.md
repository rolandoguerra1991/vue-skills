---
description: Centralized error handling — useApi errors, toast notifications, error boundaries
applies_to: [composables, components, views, services]
---
# Skill: Error Handling Standard

> [!IMPORTANT]
> This skill complements [vue-api-integration](../vue-api-integration/SKILL.md) (useApi error state) and [vue-ux-ui-standards](../vue-ux-ui-standards/SKILL.md) (visual feedback).

## 1. Error Categories

| Category | Example | Handling |
|---|---|---|
| **Network/API** | 500, 404, timeout | Toast notification + optional retry |
| **Validation** | Invalid form input | Inline field error (see [vue-form-validation](../vue-form-validation/SKILL.md)) |
| **Auth** | 401, expired session | Redirect to login via router guard |
| **Unexpected** | Runtime exception | Error boundary component + logging |

## 2. API Error Handling (in Composables)
- API errors from `useApi` MUST be caught and transformed into **user-friendly messages**.
- Never expose raw error objects, stack traces, or HTTP status codes to the UI.
- Use a centralized `handleApiError` utility.
- **Mandatory Separation:** Messages must reside in a dedicated `constants.ts` or `messages.ts` file (see [vue-constants-standard](../vue-constants-standard/SKILL.md)).

```typescript
// src/utils/error-messages.ts
export const API_ERROR_MESSAGES = {
  400: 'The request was invalid. Please check your input.',
  401: 'Your session has expired. Please log in again.',
  403: 'You do not have permission to perform this action.',
  404: 'The requested resource was not found.',
  500: 'An unexpected server error occurred. Please try again later.',
  DEFAULT: 'Something went wrong. Please try again.',
} as const;

// src/utils/handleApiError.ts
import { useToast } from '@/composables/useToast';
import { API_ERROR_MESSAGES } from './error-messages';

export function handleApiError(error: unknown): void {
  const { showToast } = useToast();

  if (error instanceof Error && 'status' in error) {
    const status = (error as any).status as number;
    const message = API_ERROR_MESSAGES[status as keyof typeof API_ERROR_MESSAGES] 
                    ?? API_ERROR_MESSAGES.DEFAULT;
    
    showToast({ type: 'error', message });
    return;
  }

  showToast({ type: 'error', message: API_ERROR_MESSAGES.DEFAULT });
}
```

## 3. Usage in Views/Composables
```typescript
import { handleApiError } from '@/utils/handleApiError';
import { useApi } from '@/composables/useApi';
import { UserService } from '@/services/UserService';

const { data: user, execute: fetchUser } = useApi(UserService.fetchProfile);

async function loadUser(id: string) {
  try {
    await fetchUser(id);
  } catch (error) {
    handleApiError(error);
  }
}
```

## 4. Error Boundary Component
- Create a global `ErrorBoundary` component that catches unexpected rendering errors via `onErrorCaptured`.
- Place it at the layout level to wrap page content.

```vue
<!-- src/components/organisms/ErrorBoundary/index.vue -->
<script setup lang="ts">
import { ref, onErrorCaptured } from 'vue';

const hasError = ref(false);
const errorMessage = ref('');

onErrorCaptured((error: Error) => {
  hasError.value = true;
  errorMessage.value = error.message;
  console.error('[ErrorBoundary]', error);
  return false; // Prevent propagation
});

function retry() {
  hasError.value = false;
  errorMessage.value = '';
}
</script>

<template>
  <div v-if="hasError" class="error-boundary">
    <p>{{ errorMessage }}</p>
    <button @click="retry">Retry</button>
  </div>
  <slot v-else />
</template>
```

## 5. Rules
- **Never swallow errors silently.** Every `catch` must either display feedback or re-throw.
- **Never use `console.log` for error logging in production.** Use a structured logger or reporting service.
- **Auth errors (401)** must redirect to login. Handle globally in an Axios interceptor or in `useApi`.
