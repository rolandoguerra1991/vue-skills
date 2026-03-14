---
description: Emits (direct neighbor), Provide/Inject with typed InjectionKeys (deep trees), Pinia (cross-domain)
applies_to: [components, views]
---
# Skill: Component Communication Threshold

## 1. Emits (Direct Neighbor Rule)
- Use ONLY between direct Parent and Child.
- **Prohibition:** "Event Bubbling" (B emitting C's events to A) is forbidden.

```vue
<!-- Good: Direct parent-child emit -->
<script setup lang="ts">
import type { ButtonEmits } from './types';

const emit = defineEmits<ButtonEmits>();

function handleClick(event: PointerEvent) {
  emit('press', event);
}
</script>
```

## 2. Provide / Inject (Ancestry Communication)
- Use for deep trees (Grandparent to Grandchild) to avoid Prop-Drilling.
- Ideal for UI Contexts (e.g., TabGroups, Modals, Themes).
- **Always use typed `InjectionKey`** to guarantee type safety and prevent runtime key collisions.

### 2.1 Defining the Key
Create a shared key file so both provider and consumer import from the same source.

```typescript
// src/injection-keys/tabGroup.ts
import type { InjectionKey, Ref } from 'vue';

export interface TabGroupContext {
  activeTab: Ref<string>;
  setActiveTab: (id: string) => void;
}

export const TAB_GROUP_KEY: InjectionKey<TabGroupContext> = Symbol('TabGroupContext');
```

### 2.2 Providing (Ancestor)
```vue
<!-- TabGroup/index.vue (Organism) -->
<script setup lang="ts">
import { provide, ref } from 'vue';
import { TAB_GROUP_KEY } from '@/injection-keys/tabGroup';
import type { TabGroupContext } from '@/injection-keys/tabGroup';

const activeTab = ref('tab-1');

function setActiveTab(id: string) {
  activeTab.value = id;
}

provide<TabGroupContext>(TAB_GROUP_KEY, {
  activeTab,
  setActiveTab,
});
</script>
```

### 2.3 Injecting (Descendant)
```vue
<!-- TabItem/index.vue (Atom) -->
<script setup lang="ts">
import { inject } from 'vue';
import { TAB_GROUP_KEY } from '@/injection-keys/tabGroup';

const tabGroup = inject(TAB_GROUP_KEY);

if (!tabGroup) {
  throw new Error('TabItem must be used inside a TabGroup');
}

const { activeTab, setActiveTab } = tabGroup;
</script>
```

## 3. Pinia
- Use for cross-domain state or business logic that persists across views.
- **Implementation:** Follow the "Setup Store" conventions defined in [vue-state-management.md](./vue-state-management.md).

## Decision Matrix

| Scenario | Mechanism | Skill Reference |
|---|---|---|
| Parent ↔ Direct Child | Props + Emits | [vue-typescript-rules.md](./vue-typescript-rules.md) |
| Grandparent → Deep Descendant | Provide / Inject with `InjectionKey` | This file §2 |
| Shared across unrelated views | Pinia Store | [vue-state-management.md](./vue-state-management.md) |
| Cross-component UI context (Tabs, Modals) | Provide / Inject | This file §2 |