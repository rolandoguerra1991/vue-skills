# Skill: Vue Routing Standards

## 1. Lazy Loading Routes
- All top-level page components MUST be lazy-loaded using dynamic imports (`() => import(...)`).
- This is critical for performance, as it splits the application into smaller chunks, reducing the initial bundle size.

```typescript
// Good: Lazy Loading
const routes = [
  {
    path: '/dashboard',
    name: 'Dashboard',
    component: () => import('@/views/DashboardView.vue')
  }
]
```

## 2. Using Meta Fields
- Use the `meta` property on route definitions for declarative configuration.
- Common uses: `requiresAuth: true`, `layout: 'AdminLayout'`, `title: 'Page Title'`, `role: 'admin'`.

```typescript
// Good: Using Meta tags
{
  path: '/settings',
  component: () => import('@/views/Settings.vue'),
  meta: { requiresAuth: true, layout: 'default', title: 'User Settings' }
}
```

## 3. Navigation Guards
- Centralize authentication and authorization logic inside global `router.beforeEach` guards.
- Avoid placing authentication checks inside individual page components' `onMounted` hooks.

```typescript
// Good: Global Auth Guard
router.beforeEach((to, from, next) => {
  const authStore = useAuthStore()
  if (to.meta.requiresAuth && !authStore.isAuthenticated) {
    next('/login')
  } else {
    next()
  }
})
```

## 4. Route Naming and Typing
- Always provide a `name` for routes and use `router.push({ name: 'RouteName' })` instead of string paths (`router.push('/path')`).
- This makes routing robust against URL refactoring.
- If using TypeScript, strongly type your route names and parameters if possible (e.g., using `unplugin-vue-router` for typed routing).
