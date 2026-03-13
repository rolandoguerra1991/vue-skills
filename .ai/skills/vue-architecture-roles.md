# Skill: Architectural Roles (Pages vs. Components)

## 1. Page / View (Orchestrator)
- **Location:** `src/views/` or `src/pages/`.
- **Responsibility:** Orchestration, data fetching, and passing data to children.
- **Strict Rule:** ALL Views MUST follow the exact same file convention as components defined in `vue-component-standard.md` (directory, `index.vue`, `styles.css`, `types.ts`, `index.spec.ts`).


## 2. Component (Presentation/UI)
- **Location:** `src/components/`.
- **Responsibility:** Rendering data and emitting UI events.
- **Strict Prohibition:** MUST NOT import API services, perform direct fetch calls, or manage infrastructure controllers (e.g., `AbortController`). Refer to `vue-api-integration.md` for automated handling patterns.