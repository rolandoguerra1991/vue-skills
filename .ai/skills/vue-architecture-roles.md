# Skill: Architectural Roles (Pages vs. Components)

## 1. Page / View (Orchestrator)
- **Location:** `src/views/` or `src/pages/`.
- **Responsibility:** Data fetching, Pinia store management, and passing data to children via Props.
- **Allowed:** Direct use of API services/composables.

## 2. Component (Presentation/UI)
- **Location:** `src/components/`.
- **Responsibility:** Rendering data and emitting UI events.
- **Strict Prohibition:** MUST NOT import API services or perform direct fetch calls.