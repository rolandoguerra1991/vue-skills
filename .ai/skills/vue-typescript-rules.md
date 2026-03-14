---
description: Props/Emits type-based syntax, template refs, generic components, strict mode
applies_to: [components, composables, types]
---
# Skill: Vue TypeScript Standards

## 1. Typing Props and Emits
- Prefer the **type-based** declaration syntax over the runtime declaration for clearer, interface-driven components.
- Avoid using `any`. If a type is unknown, use `unknown`.
- Use `withDefaults` to provide default values to type-based props.
- **Organization:** Always follow the block order defined in `vue-coding-standards.md`.

```vue
<!-- Good: Type-based props with defaults from external types -->
<script setup lang="ts">
import type { UserProps, UserEmits } from './types'

const props = withDefaults(defineProps<UserProps>(), {
  isActive: false,
  tags: () => [] // Always use a factory function for object/array defaults
})

const emit = defineEmits<UserEmits>()
</script>
```

## 2. Template Refs Typing
- Type your template refs explicitly using standard DOM types (e.g., `HTMLInputElement`) or component instance types using `InstanceType<typeof Component>`.

```typescript
// Good: Typing DOM elements
import { ref } from 'vue'

const inputRef = ref<HTMLInputElement | null>(null)

function focusInput() {
  inputRef.value?.focus()
}
```

## 3. Generic Components
- Use the `<script setup lang="ts" generic="...">` feature (Vue 3.3+) for components that work with dynamic types, such as generic tables, lists, or selects.

```vue
<!-- Good: Generic List Component using external generic types -->
<script setup lang="ts" generic="T extends { id: string | number }">
import type { ListProps } from './types'

defineProps<ListProps<T>>()
</script>
```

## 4. Avoiding Implicit Any
- Keep `strict: true` and `noImplicitAny: true` in your `tsconfig.json`.
- Provide return types for composables and complex computed properties if the inference is unclear or to explicitly document intent.
