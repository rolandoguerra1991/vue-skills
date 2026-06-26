---
description: Conventional commits format, types, scopes, commit body with architectural reasoning
applies_to: [git]
---
# Skill: Conventional Commits & Documentation

## 1. Commit Format
- Format: `<type>(<scope>): <short description>`
- **Short description:** Imperative mood, lowercase, no period. Max 72 characters.
- Types: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`, `style`, `perf`.
- **Scope:** Feature area or component name in kebab-case (e.g., `auth`, `user-card`, `router`).

## 2. Commit Body (Mandatory for non-trivial changes)
Detail the **"Why"** behind the change, not the "What" (the diff already shows that).

### Full Example
```
feat(user-card): extract avatar into BaseAvatar atom

Applying Atomic Design decomposition. The avatar was repeated across
UserCard, CommentHeader, and ProfileSidebar with identical markup.

Architecture: Extracted to src/components/atoms/BaseAvatar/ following
vue-atomic-design.md mandatory extraction rule.

SRP: BaseAvatar accepts only `src`, `alt`, and `size` props.
Parent components no longer manage avatar styling.
```

### Quick Example (trivial changes)
```
fix(router): correct guard redirect path for unauthenticated users
```

## 3. Branch Naming
- Format: `<type>/<scope>-<short-description>`
- Examples: `feat/auth-login-flow`, `fix/user-card-avatar-size`, `refactor/api-error-handling`