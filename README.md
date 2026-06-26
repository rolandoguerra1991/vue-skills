# 🧠 Vue 3 Elite AI-Driven Ecosystem

This repository establishes a high-performance, strictly typed, and scalable architecture for Vue 3 applications, specifically designed for **AI-Assisted Development** (Vibe Coding). By injecting these specialized "skills" into the AI context, we ensure that all generated code adheres to enterprise-grade standards automatically.

## 🚀 Key Features

- **Automated AI Context:** Orchestrated via `AGENTS.md` with a **Skill Index** to guide AI agents through project-specific rules.
- **Enterprise Architecture:** Strict separation between Layouts (structure), Views (orchestration), and Components (presentation).
- **Atomic Design:** Mandatory component decomposition into Atoms, Molecules, and Organisms for maximum reusability.
- **Granular Code:** Function size limits, early returns, and mandatory helper extraction for modular, readable code.
- **Safe API Layer:** Automated request cancellation using `AbortController` hidden behind clean composables.
- **Granular Routing:** Decoupled navigation logic using per-route guards and externalized middleware patterns.
- **BEM Styling:** Mandatory Block-Element-Modifier methodology using local CSS files and Tailwind `@apply`.
- **Pre-configured Environment:** Standardized dependencies and editor alignment for zero-overhead development.
- **Standardized Formatting:** Strict indentation, quote usage, and template structure rules for a cohesive codebase.
- **Bulletproof Typing:** Inferred type safety from API responses directly to UI state.

## 📁 System Architecture (AI Skills)

The intelligence of this repository lies in the `/.agents/skills/` directory. Each skill is now organized as its own folder containing a `SKILL.md` file, which is the format expected by agent-based tooling.

### 🏗️ Structure & Patterns
- **[Project Setup & Editor](.agents/skills/vue-project-setup/SKILL.md)**: Mandatory dependencies and VS Code alignment.
- **[Architectural Roles](.agents/skills/vue-architecture-roles/SKILL.md)**: Defines the Layouts vs. Pages vs. Components hierarchy.
- **[Atomic Design](.agents/skills/vue-atomic-design/SKILL.md)**: Component decomposition into Atoms, Molecules, and Organisms with mandatory extraction rules.
- **[Component Standard](.agents/skills/vue-component-standard/SKILL.md)**: Enforces the 4-file directory structure (`index.vue`, `types.ts`, `styles.css`, `index.spec.ts`).
- **[Coding Standards](.agents/skills/vue-coding-standards/SKILL.md)**: Strict `<script setup>` organization and SOLID principles.
- **[Constants & Messages](.agents/skills/vue-constants-standard/SKILL.md)**: Centralization of strings, magic values, and UI text for better maintainability.
- **[Granular Code](.agents/skills/vue-granular-code/SKILL.md)**: Function size limits (~20 lines), early returns, helper extraction, and composable granularity.
- **[Routing Standards](.agents/skills/vue-routing/SKILL.md)**: Granular per-route guards and rule of separation for views.
- **[Communication Logic](.agents/skills/vue-communication-logic/SKILL.md)**: Guidelines for Props/Emits and parent-child interaction.

### 💡 Logic & State
- **[State Management](.agents/skills/vue-state-management/SKILL.md)**: Modern Pinia usage with Setup Stores and inferred reactivity.
- **[API Integration](.agents/skills/vue-api-integration/SKILL.md)**: Clean API consumption with automated AbortController handling.
- **[Error Handling](.agents/skills/vue-error-handling/SKILL.md)**: Centralized API error handling, toast feedback, and ErrorBoundary component.
- **[Form Validation](.agents/skills/vue-form-validation/SKILL.md)**: Schema-first form validation with Zod and reusable composable.
- **[Composables Standard](.agents/skills/vue-composables-standard/SKILL.md)**: Guidelines for logic reuse and lifecycle-aware cleanup.

### 🎨 Styling & Performance
- [**Styling Standards**](.agents/skills/vue-styling-standards/SKILL.md): BEM priority and Tailwind CSS encapsulation.
- [**UX/UI Standards**](.agents/skills/vue-ux-ui-standards/SKILL.md): Accessibility, visual feedback, and premium animations with Motion.
- [**Formatting & Indentation**](.agents/skills/vue-formatting-standards/SKILL.md): Mandatory rules for clean code and clean diffs.
- [**Performance**](.agents/skills/vue-performance/SKILL.md): Optimization rules for large datasets and heavy components.

### 🛡️ Quality Assurance
- **[TypeScript Rules](.agents/skills/vue-typescript-rules/SKILL.md)**: Externalized interfaces and strict typing.
- **[Unit Testing](.agents/skills/vue-unit-testing/SKILL.md)**: 100% coverage requirement using Vitest.
- **[E2E Testing](.agents/skills/vue-e2e-testing/SKILL.md)**: User flow validation with Playwright/Cypress via `data-testid`.

### 🔗 Integrations & Workflow
- **[Supabase Integration](.agents/skills/vue-supabase-integration/SKILL.md)**: Standards for database, auth, and real-time integration.
- **[Git Standards](.agents/skills/git-standards/SKILL.md)**: Conventional commits, branch naming, and commit body standards.

---

## ⚡ Quick Installation

To fully enable the ecosystem's capabilities, install the core dependencies:

```bash
npm install vue-router@4 pinia motion-v axios zod
npm install -D tailwindcss @tailwindcss/vite vitest @vue/test-utils playwright eslint prettier
```

---

## 🛠️ How to use with AI
When working with an AI Agent in this repository:
1. Ensure the Agent reads `/.ai/AGENTS.md` first.
2. The Agent uses the **Skill Index** in `AGENTS.md` to route directly to the relevant skill without scanning all files.
3. Each skill includes **YAML frontmatter** (`description`, `applies_to`) for fast metadata-based discovery.
4. Experience "Vibe Coding with Responsibility": high-speed development with zero architectural technical debt.

---
> [!TIP]
> **Maintainability First**: Every technical decision in this ecosystem aims to hide infrastructure complexity from the feature-focused component layer.
