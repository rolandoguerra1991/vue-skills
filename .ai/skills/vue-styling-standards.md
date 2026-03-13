# Skill: Vue Styling Standards

## 1. BEM Methodology (Priority)
- **Mandatory:** All components must use the **BEM (Block Element Modifier)** methodology for class naming.
- The block name must match the component name in kebab-case (e.g., `user-profile`).
- Styles must be defined in the component's specialized `styles.css` file, fulfilling the directory structure specified in `vue-component-standard.md`.

```css
/* Good: styles.css */
@reference "tailwindcss";

.user-profile {
  @apply flex flex-col gap-4 p-4 border border-gray-200 rounded-lg;
}

.user-profile__avatar {
  @apply w-12 h-12 rounded-full;
}

.user-profile__button--active {
  @apply bg-blue-500 text-white;
}
```

## 2. Tailwind CSS with @apply
- Do **not** use long utility-class strings directly in the HTML template.
- Use Tailwind classes via the `@apply` directive inside the component's `styles.css`.
- **Mandatory (Tailwind v4):** You MUST include `@reference "tailwindcss";` at the top of any `styles.css` file that uses `@apply` to ensure utility classes are available during compilation.
- This keeps the templates clean and adheres to the BEM structure while leveraging Tailwind's utility system.

```vue
<!-- Good: index.vue -->
<template>
  <div class="user-profile">
    <img class="user-profile__avatar" :src="avatarUrl" />
    <button :class="['user-profile__button', { 'user-profile__button--active': isActive }]">
      Click Me
    </button>
  </div>
</template>
```

## 3. Scoped Styles and Encapsulation
- By default, styles are encapsulated by the project structure where `index.vue` and `styles.css` coexist in the component folder.
- Avoid leaking global CSS. Any truly global styles must reside in `assets/base.css` or `assets/main.css`.

## 4. Class and Style Bindings
- Use the object or array syntax for dynamic classes following BEM naming.
- Avoid inline `style="..."` attributes unless the value is strictly dynamic (e.g., a background color or position based on JS variables).

## 5. CSS Custom Properties (Theming)
- Use CSS Custom Properties (`--primary-color`) for theming and dark mode, declared in the `:root`.
- These can be referenced inside `@apply` or directly in CSS.
