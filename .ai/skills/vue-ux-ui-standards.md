# Skill: Vue UX/UI Standards

> [!IMPORTANT]
> **Dependencies:** `motion`


## 1. Visual Feedback
Every asynchronous action or state change must provide immediate visual feedback.

- **Loading States:** Use **Skeletons** for initial page loads and content blocks. Use small spinners or progress bars for sub-actions (e.g., button loading).
- **Empty States:** Always provide a clear message and a "Call to Action" (CTA) when a list or page is empty.
- **Success/Error:** Use toast notifications or localized alerts for operation feedback. Errors must be human-readable and provide a way to retry if possible.

## 2. Accessibility (A11y)
The application must be usable by everyone.

- **Semantic HTML:** Use `<main>`, `<header>`, `<footer>`, `<nav>`, and `<button>` instead of generic `<div>` or `<a>` for actions.
- **Focus Management:** Visible focus indicators for keyboard navigation. Restore focus after closing modals or drawers.
- **ARIA Labels:** Use `aria-label` or `aria-labelledby` for interactive elements without visible text (e.g., icon-only buttons).

## 3. Premium Animations with Motion
Use the **Motion** library as the standard for creating fluid, high-quality transitions.

- **Principle:** Animations should feel natural and assist the user's focus, never distract or cause fatigue.
- **Standard Presets:**
  - **Enter/Exit:** Duration `0.3s`, Easing `easeOut`.
  - **Layout Transitions:** Use the `layout` prop for elements that change size or position smoothly.
- **Presence:** Use `<AnimatePresence>` for exit animations (e.g., removing items from a list or closing a modal).

```vue
<!-- Example: Animated Sidebar -->
<template>
  <Motion
    :initial="{ x: '-100%' }"
    :animate="{ x: 0 }"
    :exit="{ x: '-100%' }"
    :transition="{ duration: 0.3, ease: 'easeOut' }"
    class="sidebar"
  >
    <slot />
  </Motion>
</template>
```

## 4. Responsive Design (Mobile-First)
- **Strategy:** Design for the smallest screen first and add enhancements as the viewport increases.
- **Breakpoints:** Use standard Tailwind breakpoints (`sm`, `md`, `lg`, `xl`).
- **Touch Targets:** Minimum touch area of **44x44px** for mobile interactions.

## 5. Form UX & Interaction
- **Inline Validation:** Show validation errors as the user types (with a slight debounce) or on blur, not only on submit.
- **Button States:** Disable primary buttons during processing and show a loader.
- **Micro-interactions:** Add subtle scale effects (`scale: 0.98`) on button click/tap to provide tactile feedback.
