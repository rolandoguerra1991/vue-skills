---
description: Design identity, visual feedback, accessibility, motion-v animations, responsive, form UX
applies_to: [components, views, layouts]
---
# Skill: Vue UX/UI Standards

> [!IMPORTANT]
> **Dependencies:** `motion-v` (See [vue-project-setup](../vue-project-setup/SKILL.md))


## 0. Design Identity Discovery (MANDATORY)
Before generating any UI code, the AI **MUST** establish the visual identity. If the user has not provided these, the AI **MUST** ask:

1.  **Primary Palette:** What are the brand colors?
2.  **Typography pairing:** Which fonts should be used for Headlines and Body?

### AI Recommendations (Premium 2026)
If the user is unsure, recommend these high-end pairings:

| Style | Typography Pairing | Color Palette |
| :--- | :--- | :--- |
| **Enterprise Minimal** | Inter / Outfit | Obsidian (#0A0A0A), Slate (#64748B), Pure White |
| **Luxury Dark** | Clash Display / Satoshi | Deep Navy (#020617), Gold (#F59E0B), Soft Gray |
| **Neo-Brutalist** | Space Grotesk / Syne | Cyber Lime (#BEF264), Electric Violet (#8B5CF6), Black |
| **Soft Glass** | Plus Jakarta Sans / Inter | Lavender (#E9D5FF), Pearl (#F8FAFC), Deep Indigo (#312E81) |


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

## 3. Elite Motion & Spatial Design
All interactions must use the **Motion for Vue** (`motion-v`) library. Follow the "Physics-First" principle to ensure a premium, organic feel.

### Motion Principles (2026)
- **Physics over Durations:** Use `spring` transitions for natural movement. Avoid linear timings.
- **Spatial Hierarchy:** Elements should appear to move through 3D space. Use `z-index` combined with `scale` and `opacity` to create depth.
- **Glassmorphism:** Use `backdrop-filter: blur(20px)` and semi-transparent backgrounds (`rgba(255, 255, 255, 0.1)`) for overlays.
- **Staggered Entry:** Lists and grids MUST use staggered animations for a "cascade" effect.

### Premium Presets
- **Standard Spring:** `{ type: 'spring', stiffness: 300, damping: 30 }`.
- **Soft Spring:** `{ type: 'spring', stiffness: 100, damping: 15 }` (for larger layout shifts).
- **Shared Elements:** Use `layout-id` to morph elements between different views/components.

```vue
<!-- Example: Glassmorphic Premium Sidebar with Spring Physics -->
<template>
  <motion.div
    layout
    :initial="{ x: '-110%', opacity: 0.5 }"
    :animate="{ x: 0, opacity: 1 }"
    :exit="{ x: '-110%', opacity: 0 }"
    :transition="{ type: 'spring', stiffness: 250, damping: 25 }"
    class="sidebar-glass"
  >
    <div class="content-wrapper">
      <slot />
    </div>
  </motion.div>
</template>

<style scoped>
.sidebar-glass {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(24px) saturate(180%);
  border-right: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 20px 0 50px rgba(0, 0, 0, 0.3);
}
</style>
```

### Staggered List Implementation
```vue
<template>
  <motion.ul>
    <motion.li
      v-for="(item, i) in items"
      :key="item.id"
      :initial="{ opacity: 0, y: 20 }"
      :animate="{ opacity: 1, y: 0 }"
      :transition="{ delay: i * 0.05, type: 'spring' }"
    >
      {{ item.name }}
    </motion.li>
  </motion.ul>
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
