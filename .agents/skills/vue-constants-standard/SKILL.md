---
description: Centralization of constants, UI labels, and long strings into dedicated files
applies_to: [all]
---
# Skill: Constants & Messages Centralization

> [!IMPORTANT]
> To ensure clean code, scalability (i18n readiness), and high maintainability, text strings and magic values MUST NOT be hardcoded inside logic functions or templates.

## 1. Primary Rule: Separation of Text from Logic
- Functions should focus on **logic**.
- Text strings, configuration values, and magic numbers should reside in **static files**.
- Every module or complex component should have its own `constants.ts` or `messages.ts` file.

## 2. Location & Naming
- **Global Constants:** `src/constants/` (e.g., `api.constants.ts`).
- **Domain/Module Specific:** `src/views/[Module]/constants.ts`.
- **Component Specific:** `src/components/[Component]/constants.ts`.

## 3. Implementation Example

### ❌ Bad: Hardcoded strings and logic mixed
```typescript
// RegisterView.vue
function validatePassword(pass: string) {
  if (pass.length < 8) {
    return 'The password must be at least 8 characters long and contain one special character.';
  }
}
```

### ✅ Good: Centralized messages
```typescript
// src/views/Register/messages.ts
export const REGISTER_MESSAGES = {
  ERRORS: {
    PASSWORD_SHORT: 'The password must be at least 8 characters long and contain one special character.',
    EMAIL_INVALID: 'Invalid email address.',
  },
  SUCCESS: {
    ACCOUNT_CREATED: 'Account created successfully!',
  }
} as const;

// RegisterView.vue
import { REGISTER_MESSAGES } from './messages';

function validatePassword(pass: string) {
  if (pass.length < 8) {
    return REGISTER_MESSAGES.ERRORS.PASSWORD_SHORT;
  }
}
```

## 4. UI Text in Templates
- Avoid long paragraphs directly in the `<template>`.
- Use a `constants` object in the script and bind it to the template.

```vue
<script setup lang="ts">
import { PAGE_TEXTS } from './constants';
</script>

<template>
  <section>
    <h1>{{ PAGE_TEXTS.TITLE }}</h1>
    <p>{{ PAGE_TEXTS.DESCRIPTION }}</p>
  </section>
</template>
```

## 5. Using `as const`
- Always export objects using `as const` to ensure TypeScript infer-deep read-only literal types.
- This prevents accidental mutations and provides better autocompletion.

## 6. Preparation for i18n
- Even if the project currently uses only one language, this centralization strategy makes future migration to `vue-i18n` (or similar) a simple find-and-replace operation.
