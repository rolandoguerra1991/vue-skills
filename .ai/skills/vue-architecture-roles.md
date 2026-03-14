---
description: Layouts vs Pages vs Components — responsibilities, locations, strict rules
applies_to: [architecture, components, views, layouts]
---
# Skill: Architectural Roles (Layouts, Pages & Components)

## 1. Layout (Structural Wrapper)
- **Location:** `src/layouts/`.
- **Responsibility:** Structural skeleton (Header, Sidebar, Footer) and rendering children via `<router-view />`.
- **Naming:** Must end with `Layout` (e.g., `MainLayout`).
- **Standard:** Follow the directory structure defined in `vue-component-standard.md`.

## 2. Page / View (Orchestrator)
- **Location:** `src/views/` or `src/pages/`.
- **Responsibility:** Orchestration, data fetching, and passing data to children.
- **Strict Rule:** ALL Views MUST follow the exact same file convention as components defined in `vue-component-standard.md` (directory, `index.vue`, `styles.css`, `types.ts`, `index.spec.ts`).


## 3. Component (Presentation/UI)
- **Location:** `src/components/`.
- **Responsibility:** Rendering data and emitting UI events.
- **Decomposition:** Components MUST follow the Atomic Design hierarchy (Atoms → Molecules → Organisms) defined in [vue-atomic-design.md](./vue-atomic-design.md).
- **Strict Prohibition:** MUST NOT import API services, perform direct fetch calls, or manage infrastructure controllers (e.g., `AbortController`). Refer to `vue-api-integration.md` for automated handling patterns.