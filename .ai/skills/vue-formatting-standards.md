---
description: Indentation, quotes, semicolons, line length, attribute order, CSS property order
applies_to: [all]
---
# Skill: Vue Formatting & Indentation Standards

## 1. General Formatting Rules
Consistent formatting is mandatory to maintain readability and clean Git diffs.

- **Indentation:** Use **2 spaces** (no tabs).
- **Line Length:** Maximum **100 characters** per line.
- **Semicolons:** **Mandatory** at the end of statements.
- **Quotes:**
  - **Script (JS/TS):** Use **single quotes** (`'`) unless interpolation is needed (backticks).
  - **Template (HTML):** Use **double quotes** (`"`) for attributes.
  - **CSS:** Use **double quotes** (`"`) for font names or URLs.
- **Trailing Commas:** Use **es5** style (commas in objects and arrays, but not in function parameters if not supported by the environment).

## 2. Vue Template Specifics
- **Attribute Order:** Follow the [Vue.js Style Guide](https://vuejs.org/style-guide/rules-recommended.html#element-attribute-order) (v-for > v-if > attributes > events).
- **Multi-line Attributes:** If a tag has **more than 2 attributes**, each attribute must be on a new line.
- **Closing Bracket:** Place the closing bracket (`>`) of a multi-line tag on a new line.

```vue
<!-- Good -->
<MyComponent
  v-if="isVisible"
  class="my-component"
  :label="buttonLabel"
  @click="handleClick"
/>

<!-- Avoid -->
<MyComponent v-if="isVisible" class="my-component" :label="buttonLabel" @click="handleClick" />
```

## 3. JavaScript / TypeScript Specifics
- **Spacing:**
  - Space before and after keywords (e.g., `if (condition) {`).
  - Space after colons in objects (`{ key: value }`).
  - No space between function name and parentheses (`function name()`).
- **Imports:** Group and sort imports (External Packages > Internal Aliases `@/` > Local Files `./`).

## 4. CSS / Styling Specifics
- **Indentation:** 2 spaces.
- **Declaration Order:** Group related properties (Positioning > Box Model > Typography > Visual > Misc).
- **New Line:** Each selector and property must be on a new line.
