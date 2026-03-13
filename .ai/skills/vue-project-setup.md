# Skill: Vue Project Setup & Editor Alignment

## 1. Mandatory Core Dependencies
All projects in this ecosystem must have the following core libraries installed:

- **Routing:** `vue-router@4`
- **State Management:** `pinia`
- **Animation:** `motion-v`
- **Icons:** `lucide-vue-next` (Recommended)
- **API Clients:** `axios`

## 2. Styling (Tailwind CSS v4)
We use the modern Tailwind v4 stack integrated with Vite.

- **Dependencies:** `tailwindcss`, `@tailwindcss/vite`
- **Vite Config:**
```typescript
// vite.config.ts
import tailwindcss from '@tailwindcss/vite';
import { defineConfig } from 'vite';

export default defineConfig({
  plugins: [
    tailwindcss(),
  ],
});
```

## 3. Editor Alignment (VS Code)
To ensure the AI and the developer are in sync with formatting and linting, use the following configuration.

### Recommended Extensions
- **Vue Official (Volar):** Language support for Vue 3.
- **Tailwind CSS IntelliSense:** Standard for v4 utility classes.
- **ESLint / Prettier:** Mandatory for auto-formatting.

### Suggested `.vscode/settings.json`
```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },
  "javascript.preferences.quoteStyle": "single",
  "typescript.preferences.quoteStyle": "single",
  "editor.insertSpaces": true,
  "editor.tabSize": 2,
  "files.trimTrailingWhitespace": true
}
```

### Suggested `.editorconfig`
```ini
[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.vue]
indent_size = 2
```

## 4. Linting & Formatting
- **Standard:** Rules defined in `vue-formatting-standards.md`.
- **Prettier Config:** Ensure it matches the skill (trailing commas, single quotes, semicolons).
