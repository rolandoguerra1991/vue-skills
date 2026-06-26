---
description: Refactoring Process — Code analysis, improvement notifications, and implementation plans for maintainability
applies_to: [refactoring, code quality, maintenance]
---
# Skill: Refactoring for Vue/TypeScript

> [!IMPORTANT]
> Refactoring should NOT be done arbitrarily. Apply it only when code complexity increases (e.g., functions >50 lines, high coupling). Keep it simple: refactor incrementally to avoid breaking changes.

## 1. When to Refactor
- **Complexity Indicators**: Functions >50 lines, classes with multiple responsibilities, high cyclomatic complexity, or repeated code.
- **Triggers**: After adding features, when tests fail, or during code reviews.
- **Examples in Vue**: Extract logic from large components to composables, split organisms into smaller parts, or optimize API calls.

## 2. Code Analysis Process
- **Static Analysis**: Use ESLint/Prettier (from [vue-project-setup](../vue-project-setup/SKILL.md)) and TypeScript compiler for errors/warnings.
- **Metrics**: Check line counts, dependencies, and coupling. Tools like SonarQube or VS Code extensions can help.
- **Manual Review**: Read code for smells (e.g., long methods, magic numbers per [vue-constants-standard](../vue-constants-standard/SKILL.md)).

## 3. Notification of Improvements
- **Automated Alerts**: Run linters/tests before commits. If issues arise, notify via console or IDE (e.g., "Function too long: extract to composable").
- **Code Review Feedback**: During PRs, flag areas needing refactor (e.g., "This component violates SRP").
- **Tools Integration**: Use VS Code's problems panel or scripts to highlight issues.

## 4. Implementation Plan
- **Step-by-Step Approach**:
  1. **Identify Scope**: Isolate the code to refactor (e.g., one component).
  2. **Backup**: Ensure tests pass (see [vue-unit-testing](../vue-unit-testing/SKILL.md)).
  3. **Plan Changes**: List steps (e.g., "Extract function X to composable Y").
  4. **Implement Incrementally**: Change small parts, test each.
  5. **Validate**: Run full suite, check performance (see [vue-performance](../vue-performance/SKILL.md)).
- **Risk Mitigation**: Use feature flags or branches. Rollback if issues.
- **Example Plan for a Large Component**:
  - Step 1: Extract API logic to [vue-api-integration](../vue-api-integration/SKILL.md) composable.
  - Step 2: Split UI into atoms/molecules per [vue-atomic-design](../vue-atomic-design/SKILL.md).
  - Step 3: Add tests for new parts.
  - Step 4: Remove old code after validation.

## 5. Integration with Your Skills
- Combine with [vue-coding-standards](../vue-coding-standards/SKILL.md) for style.
- Ensure tests cover refactored code (see below).
- Document changes in commits per [git-standards](../git-standards/SKILL.md).

## 6. Advantages and Disadvantages
- **Advantages**: Improves readability, reduces bugs, eases future changes.
- **Disadvantages**: Time-consuming; risk of introducing bugs. Only justify with clear benefits.