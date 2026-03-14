---
description: Lazy loading, nested routes with layouts, navigation guards, named routes, typing
applies_to: [router, views]
---
# Skill: Vue Routing Standards

> [!IMPORTANT]
> **Dependencies:** `vue-router@4` (See [vue-project-setup.md](./vue-project-setup.md))



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

## 2. Layouts & Nested Routes
Standardize layout management using **Nested Routes** to ensure structural persistence and clean separation.

- **Standard Pattern:** Use a parent route with a `Layout` component and child routes for specific pages.
- **Layout Component:** Must contain a `<router-view />` to render child content.
- **Persistence:** This avoids re-mounting the layout when navigating between sibling child routes.

```typescript
// router/index.ts
const routes = [
  {
    path: '/',
    component: () => import('@/layouts/MainLayout/index.vue'),
    children: [
      {
        path: '',
        name: 'Home',
        component: () => import('@/views/Home/index.vue')
      },
      {
        path: 'profile',
        name: 'Profile',
        component: () => import('@/views/Profile/index.vue'),
        // Child-specific guards
        beforeEnter: [authGuard]
      }
    ]
  }
];
```

## 3. Navigation Guards & Orchestration
To maintain clean and maintainable code, routing logic must be decoupled from the UI.

### 3.1. Rule of Separation
- **Mandatory:** View components (`.vue`) MUST NOT contain navigation hooks (`beforeRouteEnter`, `beforeRouteUpdate`, `beforeRouteLeave`).
- All navigation logic must reside in the router configuration or externalized guard functions.

### 3.2. Granular Guards (Per-Route)
- Preferred Pattern: Use the `beforeEnter` property in route definitions.
- `beforeEnter` should accept an **array of functions** for composability and reuse.

```typescript
// router/guards/auth.ts
export const authGuard = (to, from, next) => {
  const auth = useAuthStore();
  if (!auth.isAuthenticated) return next('/login');
  next();
};

// router/index.ts
{
  path: '/admin',
  component: () => import('@/views/Admin.vue'),
  // Composition: Multiple guards executed sequentially
  beforeEnter: [authGuard, roleGuard('admin')]
}
```

### 3.3. When to use global `beforeEach`
- Use global guards ONLY for logic that applies to **every single route** (e.g., initializing a base store or global analytics).
- For anything specific to a set of routes, use `beforeEnter` arrays.

## 4. Route Naming and Typing
- Always provide a `name` for routes and use `router.push({ name: 'RouteName' })` instead of string paths (`router.push('/path')`).
- This makes routing robust against URL refactoring.
- If using TypeScript, strongly type your route names and parameters if possible (e.g., using `unplugin-vue-router` for typed routing).
