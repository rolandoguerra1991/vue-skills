# 🧠 Vue 3 Elite AI-Driven Ecosystem

This repository establishes a high-performance, strictly typed, and scalable architecture for Vue 3 applications, specifically designed for **AI-Assisted Development** (Vibe Coding). By injecting these specialized "skills" into the AI context, we ensure that all generated code adheres to enterprise-grade standards automatically.

## 🚀 Key Features

- **Automated AI Context:** Orchestrated via `.ai/AGENTS.md` to guide AI agents through project-specific rules.
- **Enterprise Architecture:** Strict separation between orchestration (Views) and presentation (Components).
- **Safe API Layer:** Automated request cancellation using `AbortController` hidden behind clean composables.
- **BEM Styling:** Mandatory Block-Element-Modifier methodology using local CSS files and Tailwind `@apply`.
- **Bulletproof Typing:** Inferred type safety from API responses directly to UI state.

## 📁 System Architecture (AI Skills)

The intelligence of this repository lies in the `/.ai/skills/` directory. Each file defines a non-negotiable standard for the AI:

### 🏗️ Structure & Patterns
- **[Architectural Roles](.ai/skills/vue-architecture-roles.md)**: Defines the Pages vs. Components hierarchy.
- **[Component Standard](.ai/skills/vue-component-standard.md)**: Enforces the 4-file directory structure (`index.vue`, `types.ts`, `styles.css`, `index.spec.ts`).
- **[Coding Standards](.ai/skills/vue-coding-standards.md)**: Strict `<script setup>` organization and SOLID principles.

### 💡 Logic & State
- **[State Management](.ai/skills/vue-state-management.md)**: Modern Pinia usage with Setup Stores and inferred reactivity.
- **[API Integration](.ai/skills/vue-api-integration.md)**: Clean API consumption with automated AbortController handling.
- **[Composables Standard](.ai/skills/vue-composables-standard.md)**: Guidelines for logic reuse and lifecycle-aware cleanup.

### 🎨 Styling & Performance
- **[Styling Standards](.ai/skills/vue-styling-standards.md)**: BEM priority and Tailwind CSS encapsulation.
- **[Performance](.ai/skills/vue-performance.md)**: Optimization rules for large datasets and heavy components.

### 🛡️ Quality Assurance
- **[TypeScript Rules](.ai/skills/vue-typescript-rules.md)**: Externalized interfaces and strict typing.
- **[Unit Testing](.ai/skills/vue-unit-testing.md)**: 100% coverage requirement using Vitest.
- **[E2E Testing](.ai/skills/vue-e2e-testing.md)**: User flow validation with Playwright/Cypress via `data-testid`.

---

## 🛠️ How to use with AI
When working with an AI Agent in this repository:
1. Ensure the Agent reads `/.ai/AGENTS.md` first.
2. The Agent will automatically scan and apply the rules in `/.ai/skills/`.
3. Experience "Vibe Coding with Responsibility": high-speed development with zero architectural technical debt.

---
> [!TIP]
> **Maintainability First**: Every technical decision in this ecosystem aims to hide infrastructure complexity from the feature-focused component layer.
