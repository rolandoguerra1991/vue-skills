---
description: Form validation with Zod schemas — composable pattern, inline errors, type inference
applies_to: [components, composables, views]
---
# Skill: Form Validation with Zod

> [!IMPORTANT]
> **Dependencies:** `zod` (See [vue-project-setup](../vue-project-setup/SKILL.md))


## 1. Schema-First Approach
- Every form MUST define its validation schema using **Zod** before any UI code is written.
- The schema is the **single source of truth** for both validation rules and TypeScript types.
- Schemas must be co-located with the form component in its `types.ts` file.

```typescript
// src/views/Register/types.ts
import { z } from 'zod';

export const RegisterSchema = z.object({
  email: z
    .string()
    .min(1, 'Email is required')
    .email('Invalid email address'),
  password: z
    .string()
    .min(8, 'Password must be at least 8 characters')
    .regex(/[A-Z]/, 'Must contain at least one uppercase letter')
    .regex(/[0-9]/, 'Must contain at least one number'),
  confirmPassword: z.string(),
}).refine((data) => data.password === data.confirmPassword, {
  message: 'Passwords do not match',
  path: ['confirmPassword'],
});

// Infer the TypeScript type directly from the schema
export type RegisterForm = z.infer<typeof RegisterSchema>;

// Props and Emits for the component
export interface RegisterViewEmits {
  (e: 'submit', payload: RegisterForm): void;
}
```

## 2. Validation Composable
- Create a reusable `useFormValidation` composable that accepts any Zod schema.
- It manages field errors and provides a `validate` function.

```typescript
// src/composables/useFormValidation.ts
import { ref, type Ref } from 'vue';
import type { ZodSchema, ZodError } from 'zod';

export function useFormValidation<T>(schema: ZodSchema<T>) {
  const errors = ref<Record<string, string>>({}) as Ref<Record<string, string>>;

  function validate(formData: unknown): formData is T {
    try {
      schema.parse(formData);
      errors.value = {};
      return true;
    } catch (err) {
      const zodError = err as ZodError;
      errors.value = zodError.issues.reduce((acc, issue) => {
        const field = issue.path.join('.');
        if (!acc[field]) {
          acc[field] = issue.message;
        }
        return acc;
      }, {} as Record<string, string>);
      return false;
    }
  }

  function clearError(field: string) {
    delete errors.value[field];
  }

  function clearAllErrors() {
    errors.value = {};
  }

  return { errors, validate, clearError, clearAllErrors };
}
```

## 3. Usage in Components

```vue
<!-- src/views/Register/index.vue -->
<script setup lang="ts">
import { reactive } from 'vue';
import { RegisterSchema, type RegisterForm } from './types';
import { useFormValidation } from '@/composables/useFormValidation';

const form = reactive<RegisterForm>({
  email: '',
  password: '',
  confirmPassword: '',
});

const { errors, validate, clearError } = useFormValidation(RegisterSchema);

function handleSubmit() {
  if (!validate(form)) return;

  // form is now fully typed and validated
  console.log('Valid data:', form);
}
</script>

<template>
  <form class="register" @submit.prevent="handleSubmit">
    <div class="register__field">
      <label for="email">Email</label>
      <input
        id="email"
        v-model="form.email"
        type="email"
        @input="clearError('email')"
      />
      <span v-if="errors.email" class="register__error">
        {{ errors.email }}
      </span>
    </div>

    <div class="register__field">
      <label for="password">Password</label>
      <input
        id="password"
        v-model="form.password"
        type="password"
        @input="clearError('password')"
      />
      <span v-if="errors.password" class="register__error">
        {{ errors.password }}
      </span>
    </div>

    <div class="register__field">
      <label for="confirmPassword">Confirm Password</label>
      <input
        id="confirmPassword"
        v-model="form.confirmPassword"
        type="password"
        @input="clearError('confirmPassword')"
      />
      <span v-if="errors.confirmPassword" class="register__error">
        {{ errors.confirmPassword }}
      </span>
    </div>

    <button type="submit">Register</button>
  </form>
</template>
```

## 4. Rules
- **Schema is the type source.** Never define a separate interface when a Zod schema exists. Use `z.infer<typeof Schema>`.
- **Validate on submit, clear on input.** Validate the entire form on submit. Clear individual field errors on `@input` for a responsive UX.
- **Custom error messages are mandatory.** Never use default Zod messages like `"String must contain at least 8 character(s)"`. Write human-friendly messages.
- **API response validation (optional):** Zod can also validate API responses for runtime safety. Define response schemas in service type files.
