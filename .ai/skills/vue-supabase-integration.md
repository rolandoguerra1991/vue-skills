# Skill: Supabase Integration (Vue 3)

## 1. Client Configuration
Supabase must be initialized as a singleton in `src/lib/supabase.ts` (or `src/services/supabase.ts`) to be reused across the application.

```typescript
import { createClient } from '@supabase/supabase-js'
import type { Database } from '@/types/supabase' // Generated types

const supabaseUrl = import.meta.env.VITE_SUPABASE_URL
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY

export const supabase = createClient<Database>(supabaseUrl, supabaseAnonKey)
```

## 2. Authentication
Use a dedicated composable `useAuth.ts` to manage the session and user state.

- **Listen to Auth Changes:** Use `supabase.auth.onAuthStateChange` in the root component or a global store.
- **Rules:** Never store passwords in memory. Use `supabase.auth.signInWithPassword` or OAuth providers.

## 3. Service Pattern (Data Access Layer)
All Supabase logic must be encapsulated in **Services**. A service is a collection of functions with explicit types for parameters and responses.

### 3.1. Service Definition
Create services in `src/services/[Feature]Service.ts`.

```typescript
// src/services/ProductService.ts
import { supabase } from '@/lib/supabase'
import type { Product, CreateProductParams } from '@/types/products'

export const ProductService = {
  async getAll(): Promise<Product[]> {
    const { data, error } = await supabase
      .from('products')
      .select('id, name, price, stock')
      .order('name')
      
    if (error) throw error
    return data as Product[]
  },

  async getById(id: string): Promise<Product> {
    const { data, error } = await supabase
      .from('products')
      .select('id, name, price, stock')
      .eq('id', id)
      .single()
      
    if (error) throw error
    return data as Product
  },

  async create(params: CreateProductParams): Promise<Product> {
    const { data, error } = await supabase
      .from('products')
      .insert(params)
      .select()
      .single()
      
    if (error) throw error
    return data as Product
  }
}
```

## 4. Composables Pattern (Component Logic)
Composables act as the bridge between the **Service** and the **Component**, managing reactive state.

```typescript
// src/composables/useProducts.ts
import { ref } from 'vue'
import { ProductService } from '@/services/ProductService'
import type { Product } from '@/types/products'

export const useProducts = () => {
  const products = ref<Product[]>([]) // Reactive state with explicit type
  const isLoading = ref(false)

  const fetchProducts = async () => {
    isLoading.value = true
    try {
      products.value = await ProductService.getAll()
    } finally {
      isLoading.value = false
    }
  }

  return { products, isLoading, fetchProducts }
}
```

## 5. Usage in Components
Components consume the composable and use the typed variables directly in the template.

```vue
<!-- src/views/ProductList/index.vue -->
<script setup lang="ts">
import { onMounted } from 'vue'
import { useProducts } from '@/composables/useProducts'

const { products, isLoading, fetchProducts } = useProducts()

onMounted(fetchProducts)
</script>

<template>
  <div class="product-list">
    <div v-if="isLoading">Loading...</div>
    <ul v-else>
      <!-- 'product' inherits the 'Product' type from the reactive 'products' ref -->
      <li v-for="product in products" :key="product.id" class="product-item">
        {{ product.name }} - ${{ product.price }}
      </li>
    </ul>
  </div>
</template>
```

## 6. Realtime Subscriptions
Realtime should be used for UI updates only, not for critical logic.

- **Cleanup:** Always unsubscribe from channels when the component is unmounted using `onUnmounted`.

```typescript
const channel = supabase
  .channel('public:products')
  .on('postgres_changes', { event: 'INSERT', schema: 'public', table: 'products' }, payload => {
    // Cast payload explicitly if possible
    console.log('New product added!', payload)
  })
  .subscribe()

onUnmounted(() => {
  supabase.removeChannel(channel)
})
```

## 7. Security (RLS)
- **Mandatory:** All tables must have **Row Level Security (RLS)** enabled.
- All requests from the client must be assumed to be potentially malicious. Rely on Supabase policies for data protection, never on client-side logic only.
