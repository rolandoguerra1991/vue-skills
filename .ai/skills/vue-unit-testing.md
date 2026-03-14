---
description: Unit testing rules with Vitest and Vue Test Utils — shallowMount, mocking, async, coverage
applies_to: [components, composables, utils]
---
# Skill: Vue 3 Unit Testing

> [!IMPORTANT]
> **Dependencies:** `vitest`, `@vue/test-utils` (See [vue-project-setup.md](./vue-project-setup.md))


## 1. Isolation
- Use `shallowMount` ONLY. Do not render child components.
- Each test file must test **one single component or composable**.
- Test files must be co-located with the source: `ComponentName/index.spec.ts`.

## 2. Mocking External Dependencies
- All external functions, services, and modules MUST be mocked with `vi.mock()`.
- Mock at the module level, not inside individual tests.

```typescript
// Good: Module-level mock
import { shallowMount } from '@vue/test-utils';
import { vi, describe, it, expect, beforeEach } from 'vitest';
import UserCard from './index.vue';
import { useAuth } from '@/composables/useAuth';

vi.mock('@/composables/useAuth', () => ({
  useAuth: vi.fn(() => ({
    user: ref({ name: 'John' }),
    isLoggedIn: ref(true),
  })),
}));

describe('UserCard', () => {
  it('renders user name from composable', () => {
    const wrapper = shallowMount(UserCard);
    expect(wrapper.text()).toContain('John');
  });
});
```

## 3. Testing Emits
- Verify emitted events with `wrapper.emitted()`.
- Assert both the event name and its payload.

```typescript
it('emits "update" with the new value on click', async () => {
  const wrapper = shallowMount(MyButton, {
    props: { label: 'Save' },
  });

  await wrapper.find('button').trigger('click');

  const emitted = wrapper.emitted('update');
  expect(emitted).toHaveLength(1);
  expect(emitted![0]).toEqual([expect.any(PointerEvent)]);
});
```

## 4. Testing Composables
- Test composables in isolation, outside of components, by calling them directly.
- Wrap calls in `withSetup` or use a dummy host component if lifecycle hooks are involved.

```typescript
import { describe, it, expect, vi } from 'vitest';
import { useCounter } from './useCounter';

describe('useCounter', () => {
  it('increments the count', () => {
    const { count, increment } = useCounter();

    expect(count.value).toBe(0);
    increment();
    expect(count.value).toBe(1);
  });
});
```

## 5. Async State Updates
- Use `flushPromises()` after triggering async operations.
- Use `await nextTick()` after modifying reactive state that affects the DOM.

```typescript
import { flushPromises } from '@vue/test-utils';

it('shows data after async fetch', async () => {
  const wrapper = shallowMount(UserList);

  await flushPromises(); // Wait for onMounted async calls

  expect(wrapper.findAll('li')).toHaveLength(3);
});
```

## 6. Coverage
- **100% code coverage is mandatory** (branches, lines, and functions).
- Run with: `vitest run --coverage`.