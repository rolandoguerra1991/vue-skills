---
description: Atomic Design — Atoms, Molecules, Organisms hierarchy, mandatory extraction, naming
applies_to: [components]
---
# Skill: Atomic Design for Vue Components

> [!IMPORTANT]
> This skill extends [vue-architecture-roles](../vue-architecture-roles/SKILL.md) and [vue-component-standard](../vue-component-standard/SKILL.md). Those files define component roles and file structure; this file defines **when and how to decompose** components into reusable atoms.

## 1. Component Decomposition Levels

| Level | Role | Location | Examples |
|---|---|---|---|
| **Atoms** | Smallest UI unit, no business logic | `src/components/atoms/` | `BaseButton`, `BaseIcon`, `BaseInput`, `BaseTag` |
| **Molecules** | Group of atoms working together | `src/components/molecules/` | `SearchBar` (Input + Button), `FormField` (Label + Input + Error) |
| **Organisms** | Complex sections with business context | `src/components/organisms/` | `UserCard`, `NavigationBar`, `CommentThread` |
| **Templates** | Page structure composed of organisms | Defined inside Layouts or Pages | Structural wrappers without data fetching |
| **Pages** | Orchestrators that inject data | `src/views/` or `src/pages/` | Follow [vue-architecture-roles](../vue-architecture-roles/SKILL.md) |

## 2. Mandatory Extraction Rule
- **If a piece of UI could logically be used in another component, it MUST be extracted into its own component.** Do not embed reusable UI inline.
- When in doubt, **extract**. It is always cheaper to merge two small components than to split a monolith later.
- Indicators that extraction is needed:
  - The fragment has its own visual identity (e.g., a badge, a tag, an avatar).
  - The fragment accepts its own data (could be modeled as props).
  - The fragment appears (or could appear) in more than one context.

## 3. Naming Conventions
- **Atoms:** Prefix with `Base` (e.g., `BaseButton`, `BaseAvatar`). This signals they are foundational and context-free.
- **Molecules & Organisms:** Use descriptive, domain-aware names (e.g., `SearchBar`, `UserProfileCard`).
- All components follow the directory structure defined in [vue-component-standard](../vue-component-standard/SKILL.md).

## 4. Props & Slots Design
- **Atoms** must be highly configurable via props and slots. They MUST NOT contain business logic or API calls.
- **Molecules** compose atoms and may add minimal coordination logic. They communicate upward via **emits only**.
- **Organisms** may use composables and stores but must follow [vue-coding-standards](../vue-coding-standards/SKILL.md) SOLID rules.
- Use **slots** to make components open for extension without modification (Open/Closed Principle).

## 5. Proactive Decomposition
- When developing a new feature, the agent MUST:
  1. **Identify atoms** first (buttons, inputs, icons, labels).
  2. **Compose molecules** from those atoms.
  3. **Assemble organisms** from molecules.
  4. **Integrate into pages/views** via layouts.
- This is a top-down design but **bottom-up implementation**: build atoms first, then compose upward.

## 6. Folder Organization
```
src/components/
├── atoms/
│   ├── BaseButton/
│   │   ├── index.vue
│   │   ├── types.ts
│   │   ├── styles.css
│   │   └── index.spec.ts
│   └── BaseIcon/
│       └── ...
├── molecules/
│   └── SearchBar/
│       └── ...
└── organisms/
    └── UserCard/
        └── ...
```
> Each component directory follows [vue-component-standard](../vue-component-standard/SKILL.md).
